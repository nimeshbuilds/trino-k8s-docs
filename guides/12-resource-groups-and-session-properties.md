# 12. Resource groups and session properties

Admission control and per-workload defaults: who may run how many queries, with how much memory, and with which settings. Trino 484, chart 1.45.0.

Upstream pages: [admin/resource-groups.md](../upstream/trino/docs/admin/resource-groups.md), [admin/session-property-managers.md](../upstream/trino/docs/admin/session-property-managers.md).

## Why you need this

Without resource groups, every query is admitted at once. One user with a script can queue hundreds of scans and starve everyone else. Resource groups put a concurrency and memory budget on each class of workload. Session property managers then attach different limits and behaviour to each class.

A typical split:

| Class | Example sources | Goal |
|---|---|---|
| Interactive | BI tools, CLI, notebooks | Low latency, small concurrency per user, short time limit |
| Scheduled | Orchestrators, cron jobs | Steady throughput, larger memory, long time limit |
| Automation | Services, AI agents | High request count, tight per-query limits, scan caps |
| Admin | Operators | Always admitted |

## How resource groups work

- Groups form a tree. A query is assigned to exactly one leaf group by the first matching selector.
- A query runs when its group and every ancestor have a free running slot and are under their memory limit. Otherwise it queues. If the queue of the group is full, it is rejected with `QUERY_QUEUE_FULL`.
- Limits apply to admission. A running query is never paused or killed by a resource group. Memory limits are soft: they stop new queries from starting.
- Evaluation happens on the coordinator.

Group properties:

| Property | Required | Meaning |
|---|---|---|
| `name` | yes | Name, or a template such as `${USER}` or `${SOURCE}` |
| `maxQueued` | yes | Queued queries allowed before rejection |
| `hardConcurrencyLimit` | yes | Running queries allowed |
| `softConcurrencyLimit` | no | Above this, the group only gets slots that peers do not want |
| `softMemoryLimit` | no | Distributed memory in use above which new queries queue. Absolute (`100GB`) or percent of cluster memory (`40%`). |
| `softCpuLimit`, `hardCpuLimit` | no | CPU time per `cpuQuotaPeriod` |
| `hardPhysicalDataScanLimit` | no | Bytes scanned per `physicalDataScanQuotaPeriod` |
| `schedulingPolicy` | no | `fair` (default), `weighted`, `weighted_fair`, `query_priority` |
| `schedulingWeight` | no | Weight among siblings |
| `jmxExport` | no | Export group statistics to JMX |
| `subGroups` | no | Children |

Selector fields, all optional except `group`: `user`, `originalUser`, `authenticatedUser`, `userGroup`, `source`, `queryText`, `queryType` (`SELECT`, `EXPLAIN`, `DESCRIBE`, `INSERT`, `UPDATE`, `MERGE`, `DELETE`, `ANALYZE`, `DATA_DEFINITION`, `ALTER_TABLE_EXECUTE`), `clientTags`, `group`. Regular expressions are Java regex and must match the whole value.

Selectors are checked in order. Put specific rules first and a catch-all last. A query that matches no selector is rejected.

## Example

[examples/resource-groups.json](examples/resource-groups.json):

```json
{
  "rootGroups": [
    {
      "name": "global",
      "softMemoryLimit": "90%",
      "hardConcurrencyLimit": 100,
      "maxQueued": 2000,
      "schedulingPolicy": "weighted",
      "jmxExport": true,
      "subGroups": [
        {
          "name": "interactive",
          "softMemoryLimit": "50%",
          "hardConcurrencyLimit": 40,
          "maxQueued": 400,
          "schedulingWeight": 4,
          "jmxExport": true,
          "subGroups": [
            {
              "name": "${USER}",
              "softMemoryLimit": "20%",
              "hardConcurrencyLimit": 5,
              "maxQueued": 20
            }
          ]
        },
        {
          "name": "scheduled",
          "softMemoryLimit": "60%",
          "hardConcurrencyLimit": 30,
          "maxQueued": 1000,
          "schedulingWeight": 2,
          "jmxExport": true
        },
        {
          "name": "automation",
          "softMemoryLimit": "30%",
          "hardConcurrencyLimit": 30,
          "maxQueued": 500,
          "schedulingWeight": 2,
          "jmxExport": true,
          "subGroups": [
            {
              "name": "${SOURCE}",
              "softMemoryLimit": "15%",
              "hardConcurrencyLimit": 10,
              "maxQueued": 100
            }
          ]
        }
      ]
    },
    {
      "name": "admin",
      "softMemoryLimit": "100%",
      "hardConcurrencyLimit": 20,
      "maxQueued": 50
    }
  ],
  "selectors": [
    {"userGroup": "admins", "group": "admin"},
    {"clientTags": ["scheduled"], "group": "global.scheduled"},
    {"source": "(?i).*(airflow|scheduler|cron).*", "group": "global.scheduled"},
    {"clientTags": ["agent"], "group": "global.automation.${SOURCE}"},
    {"source": "(?i).*(agent|service).*", "group": "global.automation.${SOURCE}"},
    {"group": "global.interactive.${USER}"}
  ],
  "cpuQuotaPeriod": "1h"
}
```

Read it as: the cluster runs at most 100 queries. Interactive users get up to 40 slots, 5 each. Scheduled work gets 30. Automation gets 30, at most 10 per calling service. Admins bypass all of it. Under contention, interactive work is preferred two to one over each of the others.

The percentages of sibling groups may add up to more than 100. They are caps, not reservations.

## Deploy with the chart

```yaml
resourceGroups:
  type: configmap
  resourceGroupsConfig: |-
    {
      "rootGroups": [ ... ],
      "selectors": [ ... ]
    }
```

The chart writes `resource-groups.properties` with the `file` manager and mounts the JSON at `/etc/trino/resource-groups/resource-groups.json` on the coordinator. A change to the JSON changes the coordinator ConfigMap checksum, so `helm upgrade` restarts the coordinator. The file manager does not reload.

To change limits without a restart, use the database manager:

```yaml
resourceGroups:
  type: properties
  properties: |
    resource-groups.configuration-manager=db
    resource-groups.config-db-url=jdbc:mysql://mysql.example.internal:3306/resource_groups
    resource-groups.config-db-user=${ENV:RG_DB_USER}
    resource-groups.config-db-password=${ENV:RG_DB_PASSWORD}
```

The database manager polls its tables (`resource_groups_global_properties`, `resource_groups`, `selectors`) every `resource-groups.refresh-interval`. MySQL, PostgreSQL and Oracle are supported. Schema and examples are in the upstream page.

## Getting clients into the right group

Selectors can only use what the client sends.

| Field | CLI | JDBC | Python client | HTTP header |
|---|---|---|---|---|
| Source | `--source` | `source` property | `source=` | `X-Trino-Source` |
| Client tags | `--client-tags a,b` | `clientTags` property | `client_tags=[...]` | `X-Trino-Client-Tags` |
| User | `--user` | `user` property | `user=` | `X-Trino-User`, or the authenticated principal |
| Groups | | | | From the group provider. See guide [11](11-security.md). |

Source and tags are self-declared. A client can claim any value. Match on the authenticated user or group when the class decides how much a tenant may consume, and treat source and tags as hints.

## Session property managers

Rules that set default session properties for matching queries. Evaluated on the coordinator for each query. Users can still override a default with `SET SESSION` unless access control forbids it.

Match fields: `user`, `source`, `queryType`, `clientTags`, `group` (the resource group the query was routed to). All matching rules apply, later rules override earlier ones.

[examples/session-property-config.json](examples/session-property-config.json):

```json
[
  {
    "group": "global\\.interactive\\..*",
    "sessionProperties": {
      "query_max_execution_time": "15m",
      "query_max_scan_physical_bytes": "5TB"
    }
  },
  {
    "group": "global\\.scheduled",
    "sessionProperties": {
      "query_max_execution_time": "4h"
    }
  },
  {
    "group": "global\\.automation\\..*",
    "sessionProperties": {
      "query_max_execution_time": "5m",
      "query_max_scan_physical_bytes": "500GB",
      "query_max_planning_time": "1m"
    }
  }
]
```

All values are strings. Catalog session properties use the `catalog.property` form, for example `"hive.max_split_size": "128MB"`.

```yaml
sessionProperties:
  type: configmap
  sessionPropertiesConfig: |-
    [ ... ]
```

The chart writes `session-property-config.properties` and `session-property-config.json` into the coordinator ConfigMap.

To stop users from raising their own limits, add `system_session_properties` rules to file-based access control. See the session property rules section of [security/file-system-access-control.md](../upstream/trino/docs/security/file-system-access-control.md).

Useful session properties for workload shaping:

| Session property | Effect |
|---|---|
| `query_max_execution_time`, `query_max_run_time`, `query_max_planning_time` | Time limits |
| `query_max_memory`, `query_max_total_memory` | Memory limits, can only lower the cluster setting |
| `query_max_scan_physical_bytes` | Scan cap |
| `query_max_cpu_time` | CPU cap |
| `query_priority` | Used by the `query_priority` scheduling policy |
| `retry_policy` | Fault tolerance per workload |
| `join_distribution_type`, `join_reordering_strategy` | Planner behaviour |
| `task_concurrency` | Per-query local parallelism |

Every session property: [reference/session-properties-from-source.md](reference/session-properties-from-source.md).

## Observe

```sql
SELECT resource_group_id, state, count(*)
FROM system.runtime.queries
GROUP BY 1, 2
ORDER BY 1, 2;

SELECT query_id, user, source, resource_group_id, queued_time_ms
FROM system.runtime.queries
WHERE state = 'QUEUED'
ORDER BY created;
```

The Web UI shows the resource group of each query and a per-group view. With `jmxExport: true`, each group publishes running and queued counts under the `trino.execution.resourcegroups` JMX domain. Add that domain to your metrics configuration as described in guide [13](13-observability.md).

## Pitfalls

| Pitfall | Result |
|---|---|
| No catch-all selector | Unmatched queries are rejected |
| Regex without anchors assumed to be a substring match | The pattern must match the full string. Use `.*name.*`. |
| `softMemoryLimit` taken as a hard cap | Running queries can exceed it. Use query memory limits for hard caps. |
| `hardConcurrencyLimit` sized for the average query | Heavy queries admitted together exhaust memory. Combine with `softMemoryLimit` and per-query memory limits. |
| Dots in group names inside session property rules | `group` is a regex. Escape dots. |
| Expecting DDL or `SHOW` statements to bypass groups | They are queries too. Leave room, or route them with `queryType`. |
