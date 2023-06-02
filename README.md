![Current tag](https://img.shields.io/github/tag/thermistor/redis.svg)

# redis

Setup redis. Provides options to make it easy to run a second instance. Eg. one for sidekiq and one for cache. By default it installs exactly as the regular package would, except `ProtectSystem=true` is changed to `ProtectSystem=full` in the service config.

## Vars

Here are some defaults that control behavior, appropriate for sidekiq:

    redis_instance_name: redis
    redis_port: 6379
    redis_maxmemory: ~
    redis_maxmemory_policy: noeviction
    redis_config_file: /etc/redis/redis.conf
    redis_pid_file: /var/run/redis/redis-server.pid
    redis_log_file: /var/log/redis/redis.log
    redis_dir: /var/lib/redis

## Examples

Here is the basic usage:

    - hosts: servers
      roles:
        - role: thermistor.redis

Here is how to run two instances:

    - hosts: servers
      roles:
        - role: thermistor.redis
        - role: thermistor.redis
          redis_instance_name: redis-cache
          redis_port: 6380
          redis_maxmemory: 100M
          redis_maxmemory_policy: allkeys-lfu

Note that although the redis.conf shows it running daemonized and not supervised the command line options in the redis-server.service file override that.

Note how for the 2nd instance the following vars are derived from the instance name:

    redis_config_file: /etc/redis-cache/redis.conf
    redis_pid_file: /var/run/redis-cache/redis-server.pid
    redis_log_file: /var/log/redis-cache/redis.log
    redis_dir: /var/lib/redis-cache
