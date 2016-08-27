# shinken-pack-collectd-base

## Description

This Shinken monitoring pack is used to manage services with our standard
deployment of Collectd.

We request Collectd data using unixsock plugin and collectd-nagios script from
collecd-utils package.

All other services will be managed by sub packs.

## Objects

### Templates

#### Host templates

* collectd-base: Manage all default thresholds and values for services
* collectd-cpu-jitter: Manage CPU check with jitter values (CPU idle
* collectd-cpu-percent: Manage CPU check with percent values (CPU percent idle
aggregation service)

#### Service templates

* collectd-base-service: Linked to all services of this pack. Disable perfadata

### Commands

* check_collectd_one_ds: Manage checks with one ds information
* check_collectd_without_ds: Manage checks without ds information

### Services

#### Network

| Service name    | Description                              | Value specification    | DS    | Consolidation | Warning variable                       | Critical variable                      | Duplicate_foreach variable |
|-----------------|------------------------------------------|------------------------|-------|---------------|----------------------------------------|----------------------------------------|----------------------------|
| Conntrack usage | Check contrack percentage used           | conntrack/percent-used | value | none          | $_HOSTCOLLECTD_CONNTRACK_PERCENT_WARN$ | $_HOSTCOLLECTD_CONNTRACK_PERCENT_CRIT$ | N/A                        |
| $KEY$ - Errors  | Check total of errors for each interface | $VALUE1$/if_errors     | N/A   | sum           | $VALUE2$                               | $VALUE3$                               | _interfaces                |

#### System/perfs

| Service name                 | Description                                               | Value specification                  | DS       | Consolidation | Warning variable                                  | Critical variable                                 | Duplicate_foreach variable |
|------------------------------|-----------------------------------------------------------|--------------------------------------|----------|---------------|---------------------------------------------------|---------------------------------------------------|----------------------------|
| CPU idle aggregation         | Check average cpu-idle for all cpu (need aggregation)     | aggregation-cpu-average/cpu-idle     | value    | none          | $_HOSTCOLLECTD_AGGREGATION_CPU_IDLE_WARN$         | $_HOSTCOLLECTD_AGGREGATION_CPU_IDLE_CRIT$         | N/A                        |
| CPU percent idle aggregation | Check average percent-idle for all cpu (need aggregation) | aggregation-cpu-average/percent-idle | value    | none          | $_HOSTCOLLECTD_AGGREGATION_CPU_PERCENT_IDLE_WARN$ | $_HOSTCOLLECTD_AGGREGATION_CPU_PERCENT_IDLE_CRIT$ | N/A                        |
| $KEY$                        | Check load value foreach term                             | load/load                            | $VALUE1$ | none          | $VALUE2$                                          | $VALUE3$                                          |  _load                     |

#### System/partitions

| Service name               | Description                 | Value specification          | DS    | Consolidation | Warning variable | Critical variable | Duplicate_foreach variable |
|----------------------------|-----------------------------|------------------------------|-------|---------------|------------------|-------------------|----------------------------|
| $KEY$ - Percent bytes free | Check free space percentage | $VALUE1$/percent_bytes-free  | value | none          | $VALUE2$         | $VALUE3$          | _partitions                |
| $KEY$- Percent inodes free | Check free inode percentage | $VALUE1$/percent_inodes-free | value | none          | $VALUE4$         | $VALUE5$          | _partitions                |

#### System

| Service name           | Description                          | Value specification         | DS        | Consolidation | Warning variable                         | Critical variable                        | Duplicate_foreach variable |
|------------------------|--------------------------------------|-----------------------------|-----------|---------------|------------------------------------------|------------------------------------------|----------------------------|
| Free memory percentage | Check free memory percentage         | memory/percent-free         | value     | none          | $_HOSTCOLLECTD_MEMORY_FREE_PERCENT_WARN$ | $_HOSTCOLLECTD_MEMORY_FREE_PERCENT_CRIT$ | N/A                        |
| Used memory percentage | Check used memory percentage         | memory/percent-used         | value     | none          | $_HOSTCOLLECTD_MEMORY_USED_PERCENT_WARN$ | $_HOSTCOLLECTD_MEMORY_USED_PERCENT_CRIT$ | N/A                        |
| Processes - Stopped    | Number of processes in stopped state | processes/ps_state-stopped  | value     | none          | $_HOSTCOLLECTD_PROCESSES_ZOMBIES_WARN$   | $_HOSTCOLLECTD_PROCESSES_ZOMBIES_CRIT$   | N/A                        |
| Processes - Zombies    | Number of processes in zombie state  | processes/ps_state-zombies  | value     | none          | $_HOSTCOLLECTD_PROCESSES_ZOMBIES_WARN$   | $_HOSTCOLLECTD_PROCESSES_ZOMBIES_CRIT$   | N/A                        |
| $KEY$ processes        | Number of processes maching spec     | processes-$VALUE1$/ps_count | processes | none          | $VALUE2$                                 | $VALUE3$                                 | _processes                 |
| Uptime                 | Uptime in seconds                    | uptime/uptime               | value     | none          | $_HOSTCOLLECTD_UPTIME_WARN$              | $_HOSTCOLLECTD_UPTIME_CRIT$              | N/A                        |
| Users                  | Number of connected users            | users/users                 | value     | none          | $_HOSTCOLLECTD_USERS_WARN$               | $_HOSTCOLLECTD_USERS_CRIT$               | N/A                        |

## Default values

    _COLLECTD_CONNTRACK_PERCENT_WARN                    75
    _COLLECTD_CONNTRACK_PERCENT_CRIT                    90
    _COLLECTD_AGGREGATION_CPU_IDLE_WARN                 35:
    _COLLECTD_AGGREGATION_CPU_IDLE_CRIT                 20:
    _HOSTCOLLECTD_AGGREGATION_CPU_PERCENT_IDLE_WARN     35:
    _HOSTCOLLECTD_AGGREGATION_CPU_PERCENT_IDLE_CRIT     20:
    _COLLECTD_MEMORY_FREE_PERCENT_WARN                  10
    _COLLECTD_MEMORY_FREE_PERCENT_CRIT                  15
    _COLLECTD_MEMORY_USED_PERCENT_WARN                  75
    _COLLECTD_MEMORY_USED_PERCENT_CRIT                  90
    _COLLECTD_PROCESSES_STOPPED_WARN                    5
    _COLLECTD_PROCESSES_STOPPED_CRIT                    10
    _COLLECTD_PROCESSES_ZOMBIES_WARN                    1
    _COLLECTD_PROCESSES_ZOMBIES_CRIT                    5
    _COLLECTD_UPTIME_WARN                               600:
    _COLLECTD_UPTIME_CRIT                               300:
    _COLLECTD_USERS_WARN                                2
    _COLLECTD_USERS_CRIT                                4

    _interfaces     Eth0 $(netlink-eth0)$$(1)$$(1)$,Eth1 $(netlink-eth1)$$(1)$$(1)$
    _load           Load 1m $(shortterm)$$(8)$$(16)$,Load 5m $(midterm)$$(6)$$(12)$,Load 15m $(longterm)$$(4)$$(8)$
    _partitions     / $(df-root)$$(20:)$$(10:)$$(20:)$$(10:)$
    _processes      Cron $(cron)$$(1:)$$(1:)$,SSHd $(sshd)$$(1:)$$(1:)$,NTPd $(ntpd)$$(1:1)$$(1:1)$,Collectd $(collectd)$$(1:1)$$(1:1)$
