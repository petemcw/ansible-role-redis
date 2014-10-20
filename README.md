# Redis Role for Ansible

This role installs [Redis](http://redis.io/) which is an open source, BSD licensed,
advanced key-value store. The role can deploy a Redis master or a replication
slave server.

## Requirements

This role requires [Ansible](http://www.ansibleworks.com/) version 1.4 or higher
and the Debian/Ubuntu platform.

## Role Variables

The variables that can be passed to this role and a brief description about
them are as follows (additional variables are available in the source):

```yaml
# The default flag for whether to run Redis as a daemon
redis_daemonize: 'yes'

# The default port for Redis to accept connections on
redis_port: 6379

# The default interfaces Redis will listen for connections on
redis_bind:
  - '127.0.0.1'

# The default flag for whether data should be sent to syslog
redis_syslog_enabled: 'yes'

# The default number of databases
redis_databases: 2

# The default number of save points
redis_save_points:
  - '900 1'
  - '300 10'
  - '60 10000'

# The default role for Redis to assume. Options are 'master' or 'slave'
redis_role: 'master'

# The default filename for where to dump the database data
redis_dbfilename: 'dump.rdb'

# The default path for Redis data
redis_dir: '/var/lib/redis'

# The default maximum number of connected clients at the same time
redis_maxclients: 128

# The default amount of memory Redis will reserve
redis_maxmemory: '256MB'

# The default policy Redis will use to remove data when 'maxmemory' is reached
redis_maxmemory_policy: 'volatile-lru'

# The default  operation for how to write data on disk
redis_appendfsync: 'everysec'

# Replication variables. If `redis_role` variable is 'slave', set at least these
# additional variables
redis_slaveof: '192.168.1.1'
redis_masterauth: 'None'
redis_slave_serve_stale_data: 'yes'
```

## Examples

1. Install Redis as a master server

    ```yaml
    ---
    # This playbook installs Redis

    - name: Apply Redis role
      hosts: redis
      roles:
        - redis
    ```

2. Install Redis as a replication slave server

    ```yaml
    ---
    # This playbook installs Redis

    - name: Apply Redis role
      hosts: redis
      roles:
        - { role: redis,
            redis_role: 'slave',
            redis_slaveof: '192.168.1.1',
            redis_masterauth: 'Secret!',
            redis_serve_stale_data: 'yes'
          }
    ```

## Dependencies

The following packages may be required for Debian derivatives:

- `python-apt`

## License

MIT.
