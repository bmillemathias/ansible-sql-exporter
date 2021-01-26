# sql-exporter

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/sql-exporter/status.svg)](https://drone.osshelp.ru/ansible/sql-exporter)

Ansible role that installs and configures sql-exporter.

## Usage (example)

```yaml
    - role: sql-exporter
      sql_exporter_params:
        max_connections: 2
        max_idle_connections: 2
      sql_exporter_target:
        data_source_name: 'some_connection_string'
        collectors: 'test-collector'
      sql_exporter_collectors:
        - name: some-collector-name
          min_interval: 15s
          metrics:
            - name: mssql_local_time_seconds
              type: gauge
              help: 'Local time in seconds since epoch (Unix time).'
              values: unix_time
              query: "SELECT DATEDIFF(second, '19700101', GETUTCDATE()) AS unix_time"
```

## Available parameters

### sql_exporter_params

Global settings and defaults.

| Param | Description |
| -------- | -------- |
| scrape_timeout_offset | Subtracted from Prometheus' scrape_timeout to give us some headroom and prevent Prometheus from timing out first. |
| min_interval | Minimum interval between collector runs. By default (0s) - collectors are executed on every scrape. |
| max_connections | Maximum number of open connections to any one target. |
| max_idle_connections | Maximum number of idle connections to any one target. |

### sql_exporter_target

The target to monitor and the list of collectors to execute on it.

| Param | Description |
| -------- | -------- |
| data_source_name | Data source name always has a URI schema that matches the driver name. In some cases (e.g. MySQL) the schema gets dropped or replaced to match the driver expected DSN format. |
| collectors | Collectors (referenced by name) to execute on the target. |

### sql_exporter_collectors

Variables for collectors configuration:

| Param | Description |
| -------- | -------- |
| name | With this name the collector will be referenced in the exporter configuration. |
| min_interval | Override of the similar param from global settings (optional). |
| metrics | List of metric specific params (see below). |

Params available for usage in "metrics" list:

| Param | Description |
| -------- | -------- |
| type | Set the type of the gathered metric (e.g. gauge or counter). |
| help | Short description of exported metric. |
| values | The value we'll recieve as a metric. |
| query | The query for metric gathering itself. |

## FAQ

### I have hundreds of metrics to export, playbook becomes huge

Configuration files are being generated via Jinja2, so there is no graceful way to avoid the playbook overgrowth for now, but we'll look into it.

## Useful links

- [Official documentation](https://github.com/burningalchemist/sql_exporter/blob/master/README.md)
- [Our article](https://oss.help/kb3805)

## TODO

- Improvements to params description (more readable and clear)

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
