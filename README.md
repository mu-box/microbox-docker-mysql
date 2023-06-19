# MySQL [![Build Status Image](https://github.com/mu-box/microbox-docker-mysql/actions/workflows/ci.yaml/badge.svg)](https://github.com/mu-box/microbox-docker-mysql/actions)

This is an MySQL Docker image used to launch a MySQL service on Microbox. To use this image, add a data component to your `boxfile.yml` with the `mubox/mysql` image specified:

```yaml
data.db:
  image: mubox/mysql
```

## MySQL Configuration Options
MySQL components are configured in your `boxfile.yml`. All available configuration options are outlined below.

###### Quick Links
[plugins](#plugins)
[event\_scheduler](#event-scheduler)
[max\_connections](#max-connections)
[thread\_stack](#thread-stack)
[myisam\_recover](#myisam-recover)
[max\_allowed\_packet](#max-allowed-packet)
[max\_join\_size](#max-join-size)
[table\_open\_cache](#table-open-cache)
[query\_cache\_limit](#query-cache-limit)
[allow\_suspicious\_udfs](#allow-suspicious-udfs)
[ansi](#ansi)
[audit\_log](#audit-log)
[ft\_max\_word\_len](#fulltext-maximum-word-length)
[ft\_min\_word\_len](#fulltext-minimum-word-length)
[ft\_query\_expansion\_limit](#fulltext-query-expansion-limit)
[ft\_stopword\_file](#fulltext-stopword-file)
[Custom Users/Permissions/Databases](#custom-userspermissionsdatabases)

#### Overview of MySQL Boxfile Settings
```yaml
data.db:
  image: mubox/mysql
  config:
    version: 5.7
    plugins:
      - federated
      - audit_log
    event_scheduler: 'Off'
    max_connections: 1024
    thread_stack: '256K'
    myisam_recover: 'DEFAULT'
    max_allowed_packet:  '16M'
    max_join_size: 9223372036854775807
    table_open_cache: 64
    query_cache_limit: '1M'
    allow_suspicious_udfs: 'Off'
    ansi: 'Off'
    audit_log: 'On'
    ft_max_word_len: 84
    ft_min_word_len: 4
    ft_query_expansion_limit: 20
    ft_stopword_file: ' '
    users:
      - username: root
        meta:
          privileges:
            - privilege: ALL PRIVILEGES
              'on': "*.*"
              with_grant: true
      - username: microbox
        meta:
          privileges:
            - privilege: ALL PRIVILEGES
              'on': gomicro.*
              with_grant: true
            - privilege: ALL PRIVILEGES
              'on': testing.*
              with_grant: true
            - privilege: ALL PRIVILEGES
              'on': blah.*
              with_grant: true
            - privilege: PROCESS
              'on': "*.*"
              with_grant: false
            - privilege: SUPER
              'on': "*.*"
              with_grant: false
          databases:
          - gomicro
          - testing
```

### Version
When configuring MySQL in your Boxfile, you can define which of the following versions you'd like to use.

- 5.5
- 5.6
- 5.7

**Note:** Due to version compatibility constraints, MySQL versions cannot be changed after the service is created. To use a different version, you'll have to create a new MySQL service and manually migrate data.

#### version
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    version: 5.7
```

### Plugins
This allows you to specify what MySQL plugins to load into your database service. The following plugins are available:

- archive
- blackhole
- federated
- audit_log

#### plugins
```yaml
data.db:
  image: mubox/mysql
  config:
    plugins:
      - federated
```

**Note:** When using the `audit_log` plugin, you must also specify a [`audit_log`](#audit-log) setting in your Boxfile.

### Event Scheduler
This enables or disables [MySQL's event scheduler](http://dev.mysql.com/doc/refman/5.6/en/events-overview.html).

**Note:** Even though `event_scheduler`'s default is "Off", it can still be enabled through a SQL query in your database. Setting it to "On" just enables the event scheduler when the database is provisioned.

#### event\_scheduler
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    event_scheduler: 'Off'
```

### Max Connections
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_max_connections) for definition and configuration options.

#### max\_connections
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    max_connections: 1024
```

### Thread Stack
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_thread_stack) for definition and configuration options.

#### thread\_stack
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    thread_stack: '256K'
```

### MyISAM Recover
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-options.html#option_mysqld_myisam-recover-options) for definition and configuration options.

#### myisam\_recover
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    myisam_recover: 'DEFAULT'
```

### Max Allowed Packet
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_max_allowed_packet) for definition and configuration options.

#### myisam\_recover
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    max_allowed_packet:  '16M'
```

### Max Join Size
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_max_join_size) for definition and configuration options.

#### max\_join\_size
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    max_join_size: 9223372036854775807
```

### Table Open Cache
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.1/en/server-system-variables.html#sysvar_table_open_cache) for definition and configuration options.

#### table\_open\_cache
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    table_open_cache: 64
```

### Query Cache Limit
View [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_query_cache_limit) for definition and configuration options.

#### query\_cache\_limit
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    query_cache_limit: '1M'
```

### Allow Suspicious UDFs
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.6/en/server-options.html#option_mysqld_allow-suspicious-udfs) for definition and configuration options.

#### allow\_suspicious\_udfs
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    allow_suspicious_udfs: 'Off'
```

### ANSI
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.6/en/server-options.html#option_mysqld_ansi) for definition and configuration options.

#### ansi
```yaml
# default setting
data.db:
  image: mubox/mysql
  config:
    ansi: 'Off'
```

### Audit Log
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/audit-log-plugin-installation.html) for definition and configuration details. Below are the following options:

- on
- off
- force
- force\_plus\_permanent

**Note:** In order to specify a `audit_log` setting, you must also include the [`audit_log` mysql plugin](#plugins) in your Boxfile.

#### audit\_log
```yaml
data.db:
  image: mubox/mysql
  config:
    audit_log: 'On'
    plugins:
      - audit_log
```

### FULLTEXT Maximum Word Length
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_ft_max_word_len) for definition and configuration options.

### ft\_max\_word\_len
```yaml
data.db:
  image: mubox/mysql
  config:
    ft_max_word_len: 84
```

### FULLTEXT Minimum Word Length
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_ft_min_word_len) for definition and configuration options.

#### ft\_min\_word\_len
```yaml
data.db:
  image: mubox/mysql
  config:
    ft_min_word_len: 4
```

### FULLTEXT Query Expansion Limit
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_ft_query_expansion_limit) for definition and configuration options.

#### ft\_query\_expansion\_limit
```yaml
data.db:
  image: mubox/mysql
  config:
    ft_query_expansion_limit: 20
```

### FULLTEXT Stopword File
View the [dev.mysql.com documentation](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_ft_stopword_file) for definition and configuration options.

#### ft\_stopword\_file
```yaml
data.db:
  image: mubox/mysql
  config:
    ft_stopword_file: ' '
```

### Custom Users/Permissions/Databases
You can create custom users with custom permissions as well as additional databases.

```yaml
data.mysql:
  image: mubox/mysql
  config:
    users:
      - username: root
        meta:
          privileges:
            - privilege: ALL PRIVILEGES
              'on': "*.*"
              with_grant: true
      - username: microbox
        meta:
          privileges:
            - privilege: ALL PRIVILEGES
              'on': gomicro.*
              with_grant: true
            - privilege: ALL PRIVILEGES
              'on': testing.*
              with_grant: true
            - privilege: ALL PRIVILEGES
              'on': blah.*
              with_grant: true
            - privilege: PROCESS
              'on': "*.*"
              with_grant: false
            - privilege: SUPER
              'on': "*.*"
              with_grant: false
          databases:
          - gomicro
          - testing
```

For each custom user specified, Microbox will generate an environment variable for the user's password using the following pattern:

```yaml
# Pattern
COMPONENT_ID_USERNAME_PASS

# Examples

## Custom user config 1
data.mysql:
  config:
    users:
      - username: myuser

## Generated password evar 1
DATA_MYSQL_MYUSER_PASS

## Custom user config 2
data.db:
  config:
    users:
      - username: dbuser

## Generated password evar 2
DATA_DB_DBUSER_PASS
```

## Help & Support
This is a MySQL Docker image provided by [Microbox](http://microbox.cloud). If you need help with this image, you can reach out to us in the [Microbox Discord](https://discord.gg/MCDdHfy). If you are running into an issue with the image, feel free to [create a new issue on this project](https://github.com/mu-box/microbox-docker-mysql/issues/new).

## License
This project is released under [The MIT License](http://opensource.org/licenses/MIT).
