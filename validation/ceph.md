# CEPH Validation

## Resources

- [Feature Testing - stx.2.0](https://docs.google.com/spreadsheets/d/15us6HWgcb0dmHHZe2SyOR_UI5BvVpoU8TCST0rQd4zg)

## StarlingX Cluster

1. Access StarlingX Console
2. And access StarlingX UI

```sh
$ google-chrome --no-proxy-server &
```

## StarlingX CEPH Test Cases

### STOR_CORE_*

> __STOR_CORE_014__ The objective of this test is to ensure that host reinstall of nodes running ceph-mon works properly on all supported configs.

> __STOR_CORE_015__ The objective of this test is to ensure that host delete and reprovision of nodes running ceph-mon works properly on all supported configs.

> __STOR_CORE_016__ The objective of this test is to ensure that semantic checks with respect to node lock, work properly on nodes running ceph monitors.

### STOR_TIER_005

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system cluster-list
+--------------------------------------+--------------------------------------+------+--------------+------------------+
| uuid                                 | cluster_uuid                         | type | name         | deployment_model |
+--------------------------------------+--------------------------------------+------+--------------+------------------+
| 1093b1fd-99f0-496d-bfd8-e751b98b04bb | ea5b5cfa-c8f4-454f-9b8a-92afb56973ac | ceph | ceph_cluster | storage-nodes    |
+--------------------------------------+--------------------------------------+------+--------------+------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-tier-list 1093b1fd-99f0-496d-bfd8-e751b98b04bb
+--------------------------------------+---------+--------+--------------------------------------+
| uuid                                 | name    | status | backend_using                        |
+--------------------------------------+---------+--------+--------------------------------------+
| aae84629-2518-4b0a-a7e0-0fc3e759b918 | storage | in-use | 6ae8265d-40cc-4e30-83b9-7bf188e3a59b |
+--------------------------------------+---------+--------+--------------------------------------+
```

### STOR_TIER_006


#### storage-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-0
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_p------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 00ab78be-2555-4e4c-88ac-01fb712fa912 | osd      | 0     | configured----------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 00ab78be-2555-4e4c-88ac-01fb712fa912 | osd      | 0     | configured | b47d3cdb-95d1-425f-9969-58a623ab618f | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
[wrsroot@co               | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-0
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| 3a23f96f-5a2f-4d5d-9ea6-520722d930bc | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CU0480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| b47d3cdb-95d1-425f-9969-58a623ab618f | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CUP480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 20550907-f594-4290-a0a3-de2740702835 | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYS8164013J480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| 0e7470c2-066a-42d6-aea9-ee44e9ac4da3 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTLT70400321480EGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### storage-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-1
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 193d22b4-99a3-4203-9aa7-93ec87ff4828 | osd      | 1     | configured | 878661cc-1081-4923-a563-b55990488ebb | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-1
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| b917b5eb-f0e1-4d69-a04f-0612fac5802f | /dev/sda    | 2048       | SSD         | 447.13 -------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| b917b5eb-f0e1-4d69-a04f-0612fac5802f | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CU4480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| 878661cc-1081-4923-a563-b55990488ebb | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81460H0P480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 25e86c9e-ac05-4e3f-8285-b6c5ae9dad56 | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYF83150D4L480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| edeb5f5a-a297-4d88-8df7-b903963ff1a2 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTYS818001DG480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### storage-2

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-2
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 52ae1c0a-78b6-4d09-9cbb-612470f0da7c | osd      | 2     | configured | a3012a1c-cf66-4fc4-9fe5-6520fd135c23 | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-2
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| d34ee920-f5ef-4e75-a689-8461f98244af | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81800LTF480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| a3012a1c-cf66-4fc4-9fe5-6520fd135c23 | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81800LSV480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 781dde8f-5c34-4d14-8152-a489a55376ce | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYF83200B5F480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| 8de1a787-eef4-4604-9210-75744a389a17 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTYF82910BWY480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### Storage Backend List

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
| uuid                                 | name            | backend  | state      | task                | services | capabilities       |
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
| 53eb253b-effb-4977-930c-d887ba58edc7 | shared_services | external | configured | None                | glance   |                    |
| 6ae8265d-40cc-4e30-83b9-7bf188e3a59b | ceph-store      | ceph     | configured | reconfig-controller | None     | min_replication: 1 |
|                                      |                 |          |            |                     |          | replication: 2     |
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-show shared_services
+--------------+----------------------------------+
| Property     | Value                            |
+--------------+----------------------------------+
| backend      | external                         |
| name         | shared_services                  |
| state        | configured                       |
| task         | None                             |
| services     | glance                           |
| capabilities |                                  |
| created_at   | 2019-05-22T12:34:57.446723+00:00 |
| updated_at   | None                             |
+--------------+----------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-show ceph-store
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| backend              | ceph                                 |
| name                 | ceph-store                           |
| state                | configured                           |
| task                 | reconfig-controller                  |
| services             | None                                 |
| capabilities         | min_replication: 1                   |
|                      | replication: 2                       |
| object_gateway       | False                                |
| ceph_total_space_gib | 890                                  |
| object_pool_gib      | None                                 |
| cinder_pool_gib      | None                                 |
| kube_pool_gib        | None                                 |
| glance_pool_gib      | None                                 |
| ephemeral_pool_gib   | None                                 |
| tier_name            | storage                              |
| tier_uuid            | aae84629-2518-4b0a-a7e0-0fc3e759b918 |
| created_at           | 2019-05-22T12:35:44.884703+00:00     |
| updated_at           | None                                 |
+----------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph df
GLOBAL:
    SIZE        AVAIL       RAW USED     %RAW USED 
    1.3 TiB     1.3 TiB      2.1 GiB          0.16 
POOLS:
    NAME                    ID     USED        %USED     MAX AVAIL     OBJECTS 
    .rgw.root               1      1.1 KiB         0       1.2 TiB           4 
    kube-rbd                2      1.7 GiB      0.13       1.2 TiB         584 
    default.rgw.control     3          0 B         0       1.2 TiB           8 
    default.rgw.meta        4          0 B         0       1.2 TiB           0 
    default.rgw.log         5          0 B         0       1.2 TiB        1152 
    images                  6         19 B         0       1.2 TiB           2 
    ephemeral               7          0 B         0       1.2 TiB           0 
    cinder-volumes          8          0 B         0       1.2 TiB           0 
    gnocchi.metrics         9      378 KiB         0       1.2 TiB          63 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo sm-dump
Password: 

-Service_Groups------------------------------------------------------------------------
oam-services                     active               active                         
controller-services              active               active                         
cloud-services                   active               active                         
patching-services                active               active                         
directory-services               active               active                         
web-services                     active               active                         
storage-services                 active               active                         
storage-monitoring-services      active               active                         
vim-services                     active               active                         
---------------------------------------------------------------------------------------

-Services------------------------------------------------------------------------------
oam-ip                           enabled-active       enabled-active                  
management-ip                    enabled-active       enabled-active                  
drbd-pg                          enabled-active       enabled-active                  
drbd-rabbit                      enabled-active       enabled-active                  
drbd-cgcs                        enabled-active       enabled-active                  
drbd-platform                    enabled-active       enabled-active                  
pg-fs                            enabled-active       enabled-active                  
rabbit-fs                        enabled-active       enabled-active                  
nfs-mgmt                         enabled-active       enabled-active                  
cgcs-fs                          enabled-active       enabled-active                  
platform-fs                      enabled-active       enabled-active                  
postgres                         enabled-active       enabled-active                  
rabbit                           enabled-active       enabled-active                  
cgcs-export-fs                   enabled-active       enabled-active                  
platform-export-fs               enabled-active       enabled-active                  
cgcs-nfs-ip                      enabled-active       enabled-active                  
platform-nfs-ip                  enabled-active       enabled-active                  
sysinv-inv                       enabled-active       enabled-active                  
sysinv-conductor                 enabled-active       enabled-active                  
mtc-agent                        enabled-active       enabled-active                  
hw-mon                           enabled-active       enabled-active                  
dnsmasq                          enabled-active       enabled-active                  
fm-mgr                           enabled-active       enabled-active                  
keystone                         enabled-active       enabled-active                  
open-ldap                        enabled-active       enabled-active                  
snmp                             enabled-active       enabled-active                  
lighttpd                         enabled-active       enabled-active                  
horizon                          enabled-active       enabled-active                  
patch-alarm-manager              enabled-active       enabled-active                  
mgr-restful-plugin               enabled-active       enabled-active                  
ceph-manager                     enabled-active       enabled-active                  
vim                              enabled-active       enabled-active                  
vim-api                          enabled-activSTOR_CORE_14e       enabled-active                  
vim-webserver                    enabled-active       enabled-active                  
guest-agent                      enabled-active       enabled-active                  
haproxy                          enabled-active       en                 
vim-webserver                    enabled-active       enabled-active                  
guest-agent                      enabled-active       enabled-active                  
haproxy                          enabled-active       enabled-active                  
pxeboot-ip                       enabled-active       enabled-active                  
ceph-radosgw                     enabled-active       enabled-active                  
drbd-extension                   enabled-active       enabled-active                  
extension-fs                     enabled-active       enabled-active                  
extension-export-fs              enabled-active       enabled-active                  
etcd                             enabled-active       enabled-active                  
drbd-etcd                        enabled-active       enabled-active                  
etcd-fs                          enabled-active       enabled-active                  
barbican-api                     enabled-active       enabled-active                  
barbican-keystone-listener       enabled-active       enabled-active                  
barbican-worker                  enabled-active       enabled-active                  
cluster-host-ip                  enabled-active       enabled-active                  
docker-distribution              enabled-active       enabled-active                  
dockerdistribution-fs            enabled-active       enabled-active                  
drbd-dockerdistribution          enabled-active       enabled-active                  
helmrepository-fs                enabled-active       enabled-active                  
registry-token-server            enabled-active       enabled-active                  
------------------------------------------------------------------------------
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool get cinder-volumes pg_num
pg_num: 8
```
