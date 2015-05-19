#Monasca-Schema
Adds schema to mysql and influxdb, creates db users and creates kafka topics.

## Tags
These tags can be used to specify just sections of the role
- kafka_topics
- mysql_schema
- influxdb_schema
- winchester_schema

##Requirements
The monasca services for mysql, influxdb and kafka must be up and running. Influxdb must be version 0.9.
- zookeeper_hosts - comma separated list of host:port pairs.
- influxdb_url - URL of the influxdb server
- mon_influxdb_users - dictionary of user/password pairs
```
    mon_influxdb_users:
      - username: monasca-api
        password: some-password
```
- mon_mysql_users - Array of entries with a username and password, for example:
```
    mon_mysql_users:
      - username: monasca-api
        password: some-password
```

By default, the creation of the kafka topics assume multiple kafka servers. If there is only one, then
kafka_replicas should be set to 1. The default is 3.

The number of partitions for the kafka topics can be controlled with:
- kafka_events_partitions - number of partitions for the various events topics, the default is 12
- kafka_metrics_partitions - number of partitions for the metrics topic, the default is 64
- kafka_retry_notifications_partitions - This should be the number of systems running the Notification Engine, the default is 3

If the kafka topics have been created, neither the number of partitions nor the number of replicas will not be
changed even if the above parameters are changed and the role run again.

## TODO
- The notification engine user could be given readonly access to the db but in the current setup there is no way
  to specify that so it gets full access like the rest of the users.

## Running with Vertica
- To run with vertica make sure that vertica is up and running and change the variable database_type in defaults to vertica.
- Then add in the users you want for your vertica database to the defaults.

For Example:

```
vertica_users:
  - username: monitor
    password: password
    role: monitor
  - username: mon_api
    password: password
    role: monasca_api
  - username: mon_persister
    password: password
    role: monasca_persister
```

##License
Apache

##Author Information
Tim Kuhlman
Monasca Team email monasca@lists.launchpad.net
