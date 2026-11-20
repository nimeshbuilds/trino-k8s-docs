# Configuration properties: `lib/trino-hdfs`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [HdfsAuthenticationConfig](#hdfsauthenticationconfig), [HdfsConfig](#hdfsconfig), [HdfsKerberosConfig](#hdfskerberosconfig)

## HdfsAuthenticationConfig

Source: `lib/trino-hdfs/src/main/java/io/trino/hdfs/authentication/HdfsAuthenticationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.hdfs.authentication.type` | HdfsAuthenticationType | `HdfsAuthenticationType.NONE` | HDFS authentication type |  |
| `hive.hdfs.impersonation.enabled` | boolean | `false` | Should Trino user be impersonated when communicating with HDFS |  |

## HdfsConfig

Source: `lib/trino-hdfs/src/main/java/io/trino/hdfs/HdfsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.config.resources` | List<String> | `ImmutableList.of()` |  |  |
| `hive.dfs-timeout` | Duration | `new Duration(60, TimeUnit.SECONDS)` |  |  |
| `hive.dfs.connect.max-retries` | int | `5` |  |  |
| `hive.dfs.connect.timeout` | Duration | `new Duration(500, TimeUnit.MILLISECONDS)` |  |  |
| `hive.dfs.domain-socket-path` | String |  |  |  |
| `hive.dfs.ipc-ping-interval` | Duration | `new Duration(10, TimeUnit.SECONDS)` |  |  |
| `hive.dfs.key-provider.cache-ttl` | Duration | `new Duration(30, TimeUnit.MINUTES)` |  |  |
| `hive.dfs.replication` | Integer |  | Hadoop FileSystem replication factor |  |
| `hive.dfs.verify-checksum` | boolean | `true` |  |  |
| `hive.fs.cache.max-size` | int | `1000` | Hadoop FileSystem cache size |  |
| `hive.fs.new-directory-permissions` | String | `"0777"` | File system permissions for new directories |  |
| `hive.hdfs.socks-proxy` | HostAndPort |  |  |  |
| `hive.hdfs.wire-encryption.enabled` | boolean | `false` | Should be turned on when HDFS wire encryption is enabled |  |

Removed properties, rejected at startup if still set: `hive.fs.new-file-inherit-ownership`

## HdfsKerberosConfig

Source: `lib/trino-hdfs/src/main/java/io/trino/hdfs/authentication/HdfsKerberosConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.hdfs.trino.credential-cache.location` | String |  | Trino credential-cache location used to access HDFS |  |
| `hive.hdfs.trino.keytab` | String |  | Trino keytab used to access HDFS | Legacy names: `hive.hdfs.presto.keytab` |
| `hive.hdfs.trino.principal` | String |  | Trino principal used to access HDFS | Legacy names: `hive.hdfs.presto.principal` |
