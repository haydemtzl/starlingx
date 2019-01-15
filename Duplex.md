# Controller-0 Host Provision

```
controller-0:~$ source /etc/nova/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

## Configuring Provider Networks at Installation

```
[wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-create providernet-a --type=vlan
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Created a new providernet:
+------------------+--------------------------------------+
| Field            | Value                                |
+------------------+--------------------------------------+
| description      |                                      |
| id               | 9a4e7585-9b46-4b1a-860d-e2a00304009d |
| mtu              | 1500                                 |
| name             | providernet-a                        |
| ranges           |                                      |
| status           | DOWN                                 |
| type             | vlan                                 |
| vlan_transparent | False                                |
+------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-range-create --name providernet-a-range1 --range 100-400 providernet-a
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Created a new providernet_range:
+------------------+--------------------------------------+
| Field            | Value                                |
+------------------+--------------------------------------+
| description      |                                      |
| id               | 78a374e6-7235-4c81-a296-be8b1d1bd321 |
| maximum          | 400                                  |
| minimum          | 100                                  |
| name             | providernet-a-range1                 |
| project_id       | 55d931306602491c907e12c91a0b1695     |
| providernet_id   | 9a4e7585-9b46-4b1a-860d-e2a00304009d |
| providernet_name | providernet-a                        |
| providernet_type | vlan                                 |
| shared           | False                                |
| tenant_id        | 55d931306602491c907e12c91a0b1695     |
+------------------+--------------------------------------+
```

## Providing Data Interfaces on Controller-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-list -a controller-0
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| uuid                                 | name    | class    | type     | vlan | ports        | uses | used | attributes | provider networks |
|                                      |         |          |          | id   |              | i/f  | by   |            |                   |
|                                      |         |          |          |      |              |      | i/f  |            |                   |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| 49fe3b1f-4519-4276-bbba-bf4fcd060f55 | enp2s2  | platform | ethernet | None | [u'enp2s2']  | []   | []   | MTU=1500   | None              |
| c6ac5f95-6a58-4f82-8e17-7d328166af5a | eth1000 | None     | ethernet | None | [u'eth1000'] | []   | []   | MTU=1500   | None              |
| c7a19cef-cb05-46a0-9521-193e9e485186 | enp2s1  | platform | ethernet | None | [u'enp2s1']  | []   | []   | MTU=1500   | None              |
| ff0ff048-fdbd-434d-ba78-90fd60008705 | eth1001 | None     | ethernet | None | [u'eth1001'] | []   | []   | MTU=1500   | None              |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c data controller-0 eth1000 -p providernet-a
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:07:03:da                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | c6ac5f95-6a58-4f82-8e17-7d328166af5a |
| ihost_uuid       | 006b8955-68aa-45c2-96e8-510cbf74c03b |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-14T10:13:57.525472+00:00     |
| updated_at       | 2019-01-14T15:39:15.374889+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
```

## Configuring Cinder on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                 |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| 09f40d22-7f86-42e1-a827-2941b2488cfb | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-1.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 6cb523be-b97c-4de9-a974-6de851258359 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-2.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 182c5d4b-2482-44f5-bee7-bf45e8156016 | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-3.0             |
|                                      |           |         |         |       |            |              |         |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 cinder-volumes
+-----------------------+--------------------------------------+
| Property              | Value                                |
+-----------------------+--------------------------------------+
| lvm_vg_name           | cinder-volumes                       |
| vg_state              | adding                               |
| uuid                  | dadc4fec-9e60-44a6-a54a-832af252e9b3 |
| ihost_uuid            | 006b8955-68aa-45c2-96e8-510cbf74c03b |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2019-01-14T15:40:15.519664+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 6cb523be-b97c-4de9-a974-6de851258359 199 -t lvm_phys_vol
+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| device_node | /dev/sdb1                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 203776                                           |
| uuid        | ab3beaf8-484c-4b2e-9317-59377220a73d             |
| ihost_uuid  | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| idisk_uuid  | 6cb523be-b97c-4de9-a974-6de851258359             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-14T15:41:07.634942+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 6cb523be-b97c-4de9-a974-6de851258359
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| ab3beaf8-484c-4b2e-9317-59377220a73d | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       |          |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 6cb523be-b97c-4de9-a974-6de851258359
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| uuid                                 | device_path                                      | device_node | type_guid                            | type_name           | size_gib | status |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| ab3beaf8-484c-4b2e-9317-59377220a73d | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 | /dev/sdb1   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 199.0    | Ready  |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 cinder-volumes ab3beaf8-484c-4b2e-9317-59377220a73d
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 4b60934d-5718-4fc7-bd52-da3092b0dab0             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | ab3beaf8-484c-4b2e-9317-59377220a73d             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| created_at               | 2019-01-14T15:43:13.764346+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

## Adding an LVM Storage Backend at Installation

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add lvm -s cinder

WARNING : THIS OPERATION IS NOT REVERSIBLE AND CANNOT BE CANCELLED. 

By confirming this operation, the LVM backend will be created.

Please refer to the system admin guide for minimum spec for LVM
storage. Set the 'confirmed' field to execute this operation
for the lvm backend.
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add lvm -s cinder --confirmed

System configuration has changed.
Please follow the administrator guide to complete configuring the system.

+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| uuid                                 | name       | backend | state       | task                           | services | capabilities |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| 8256b3d5-df46-4e1c-81b5-4bc3bd17b070 | file-store | file    | configured  | None                           | glance   |              |
| 8980cb1f-9144-4544-8733-353fcddde1f6 | lvm-store  | lvm     | configuring | {u'controller-0':              | cinder   |              |
|                                      |            |         |             | 'configuring'}                 |          |              |
|                                      |            |         |             |                                |          |              |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| uuid                                 | name       | backend | state       | task                           | services | capabilities |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| 8256b3d5-df46-4e1c-81b5-4bc3bd17b070 | file-store | file    | configured  | None                           | glance   |              |
| 8980cb1f-9144-4544-8733-353fcddde1f6 | lvm-store  | lvm     | configuring | {u'controller-0':              | cinder   |              |
|                                      |            |         |             | 'configuring'}                 |          |              |
|                                      |            |         |             |                                |          |              |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+------------+------+----------+--------------+
| uuid                                 | name       | backend | state      | task | services | capabilities |
+--------------------------------------+------------+---------+------------+------+----------+--------------+
| 8256b3d5-df46-4e1c-81b5-4bc3bd17b070 | file-store | file    | configured | None | glance   |              |
| 8980cb1f-9144-4544-8733-353fcddde1f6 | lvm-store  | lvm     | configured | None | cinder   |              |
+--------------------------------------+------------+---------+------------+------+----------+--------------+
```

## Configuring VM Local Storage on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                 |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| 09f40d22-7f86-42e1-a827-2941b2488cfb | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-1.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 6cb523be-b97c-4de9-a974-6de851258359 | /dev/sdb  | 2064    | HDD     | 200.0 | 0.997      | Undetermined | QM00003 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-2.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 182c5d4b-2482-44f5-bee7-bf45e8156016 | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-3.0             |
|                                      |           |         |         |       |            |              |         |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 56892b5a-9008-4b84-98cf-e775accac65c                              |
| ihost_uuid            | 006b8955-68aa-45c2-96e8-510cbf74c03b                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-01-14T15:46:35.630358+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 182c5d4b-2482-44f5-bee7-bf45e8156016 199 -t lvm_phys_vol
+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 |
| device_node | /dev/sdc1                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 203776                                           |
| uuid        | 6a8b0103-7057-4113-b4d8-5a2d8bb5e106             |
| ihost_uuid  | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| idisk_uuid  | 182c5d4b-2482-44f5-bee7-bf45e8156016             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-14T15:48:39.825789+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 182c5d4b-2482-44f5-bee7-bf45e8156016
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| uuid                                 | device_path                                      | device_node | type_guid                            | type_name           | size_gib | status |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| 6a8b0103-7057-4113-b4d8-5a2d8bb5e106 | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 | /dev/sdc1   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 199.0    | Ready  |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 nova-local 6a8b0103-7057-4113-b4d8-5a2d8bb5e106
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 68d51625-96c5-4689-861d-39615d62f286             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 6a8b0103-7057-4113-b4d8-5a2d8bb5e106             |
| disk_or_part_device_node | /dev/sdc1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 |
| lvm_pv_name              | /dev/sdc1                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| created_at               | 2019-01-14T16:00:09.254448+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

## Unlocking Controller-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-0
+---------------------+--------------------------------------------+
| Property            | Value                                      |
+---------------------+--------------------------------------------+
| action              | none                                       |
| administrative      | locked                                     |
| availability        | online                                     |
| bm_ip               | None                                       |
| bm_type             | None                                       |
| bm_username         | None                                       |
| boot_device         | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| capabilities        | {}                                         |
| config_applied      | 74a09109-1f3f-443b-90d2-f17418aea237       |
| config_status       | None                                       |
| config_target       | 74a09109-1f3f-443b-90d2-f17418aea237       |
| console             | tty0                                       |
| created_at          | 2019-01-14T10:13:26.105067+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:f0:c1:cf                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| subfunction_avail   | online                                     |
| subfunction_oper    | disabled                                   |
| subfunctions        | controller,worker                          |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-01-14T15:59:22.071034+00:00           |
| uptime              | 26051                                      |
| uuid                | 006b8955-68aa-45c2-96e8-510cbf74c03b       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

## Verifying the Controller-0 Configuration

```
controller-0:~$ source /etc/nova/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system service-list
+-----+-------------------------------+--------------+----------------+
| id  | service_name                  | hostname     | state          |
+-----+-------------------------------+--------------+----------------+
| 68  | aodh-api                      | controller-0 | enabled-active |
| 69  | aodh-evaluator                | controller-0 | enabled-active |
| 70  | aodh-listener                 | controller-0 | enabled-active |
| 71  | aodh-notifier                 | controller-0 | enabled-active |
| 102 | barbican-api                  | controller-0 | enabled-active |
| 103 | barbican-keystone-listener    | controller-0 | enabled-active |
| 104 | barbican-worker               | controller-0 | enabled-active |
| 43  | ceilometer-agent-notification | controller-0 | enabled-active |
| 14  | cgcs-export-fs                | controller-0 | enabled-active |
| 10  | cgcs-fs                       | controller-0 | enabled-active |
| 16  | cgcs-nfs-ip                   | controller-0 | enabled-active |
| 35  | cinder-api                    | controller-0 | enabled-active |
| 38  | cinder-backup                 | controller-0 | enabled-active |
| 58  | cinder-ip                     | controller-0 | enabled-active |
| 56  | cinder-lvm                    | controller-0 | enabled-active |
| 36  | cinder-scheduler              | controller-0 | enabled-active |
| 37  | cinder-volume                 | controller-0 | enabled-active |
| 23  | dnsmasq                       | controller-0 | enabled-active |
| 5   | drbd-cgcs                     | controller-0 | enabled-active |
| 55  | drbd-cinder                   | controller-0 | enabled-active |
| 75  | drbd-extension                | controller-0 | enabled-active |
| 3   | drbd-pg                       | controller-0 | enabled-active |
| 6   | drbd-platform                 | controller-0 | enabled-active |
| 4   | drbd-rabbit                   | controller-0 | enabled-active |
| 77  | extension-export-fs           | controller-0 | enabled-active |
| 76  | extension-fs                  | controller-0 | enabled-active |
| 24  | fm-mgr                        | controller-0 | enabled-active |
| 27  | glance-api                    | controller-0 | enabled-active |
| 26  | glance-registry               | controller-0 | enabled-active |
| 108 | gnocchi-api                   | controller-0 | enabled-active |
| 109 | gnocchi-metricd               | controller-0 | enabled-active |
| 62  | guest-agent                   | controller-0 | enabled-active |
| 64  | haproxy                       | controller-0 | enabled-active |
| 45  | heat-api                      | controller-0 | enabled-active |
| 46  | heat-api-cfn                  | controller-0 | enabled-active |
| 47  | heat-api-cloudwatch           | controller-0 | enabled-active |
| 44  | heat-engine                   | controller-0 | enabled-active |
| 51  | horizon                       | controller-0 | enabled-active |
| 22  | hw-mon                        | controller-0 | enabled-active |
| 57  | iscsi                         | controller-0 | enabled-active |
| 25  | keystone                      | controller-0 | enabled-active |
| 50  | lighttpd                      | controller-0 | enabled-active |
| 2   | management-ip                 | controller-0 | enabled-active |
| 20  | mtc-agent                     | controller-0 | enabled-active |
| 28  | neutron-server                | controller-0 | enabled-active |
| 9   | nfs-mgmt                      | controller-0 | enabled-active |
| 29  | nova-api                      | controller-0 | enabled-active |
| 63  | nova-api-proxy                | controller-0 | enabled-active |
| 31  | nova-conductor                | controller-0 | enabled-active |
| 33  | nova-console-auth             | controller-0 | enabled-active |
| 34  | nova-novnc                    | controller-0 | enabled-active |
| 83  | nova-placement-api            | controller-0 | enabled-active |
| 30  | nova-scheduler                | controller-0 | enabled-active |
| 1   | oam-ip                        | controller-0 | enabled-active |
| 48  | open-ldap                     | controller-0 | enabled-active |
| 82  | panko-api                     | controller-0 | enabled-active |
| 52  | patch-alarm-manager           | controller-0 | enabled-active |
| 7   | pg-fs                         | controller-0 | enabled-active |
| 15  | platform-export-fs            | controller-0 | enabled-active |
| 11  | platform-fs                   | controller-0 | enabled-active |
| 17  | platform-nfs-ip               | controller-0 | enabled-active |
| 12  | postgres                      | controller-0 | enabled-active |
| 65  | pxeboot-ip                    | controller-0 | enabled-active |
| 13  | rabbit                        | controller-0 | enabled-active |
| 8   | rabbit-fs                     | controller-0 | enabled-active |
| 49  | snmp                          | controller-0 | enabled-active |
| 19  | sysinv-conductor              | controller-0 | enabled-active |
| 18  | sysinv-inv                    | controller-0 | enabled-active |
| 59  | vim                           | controller-0 | enabled-active |
| 60  | vim-api                       | controller-0 | enabled-active |
| 61  | vim-webserver                 | controller-0 | enabled-active |
+-----+-------------------------------+--------------+----------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show 1 | grep subfunctions
| subfunctions        | controller,worker                          |
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

# Controller-1 Host Installation

```
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/nova/openrc
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 2 personality=controller hostname=controller-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-01-14T16:30:06.590410+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:5f:6a:ee                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| subfunction_avail   | not-installed                        |
| subfunction_oper    | disabled                             |
| subfunctions        | controller,worker                    |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 9c2f618c-75d1-4d21-af30-eb058cc46a64 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

## Monitoring Controller-1 Host

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show controller-1 | grep install
| install_output      | text                                    |
| install_state       | None                                    |
| install_state_info  | None                                    |
| subfunction_avail   | not-installed                           |
```

## Listing Controller-1 Host

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

# Controller-1 Provisioning

```
controller-0:~$ source /etc/nova/openrc
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list controller-1

+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
| uuid                                 | name    | type     | pci address  | device | processor | accelerated | device type      |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
| d9e32e19-5e02-4d93-bdce-df7c6aeb0bc2 | enp2s1  | ethernet | 0000:02:01.0 | 0      | -1        | False       | 82540EM Gigabit  |
|                                      |         |          |              |        |           |             | Ethernet         |
|                                      |         |          |              |        |           |             | Controller       |
|                                      |         |          |              |        |           |             |                  |
| 4d0b8c46-bc7d-4b1e-8b69-3b5f85f0d8f6 | enp2s2  | ethernet | 0000:02:02.0 | 0      | -1        | False       | 82540EM Gigabit  |
|                                      |         |          |              |        |           |             | Ethernet         |
|                                      |         |          |              |        |           |             | Controller       |
|                                      |         |          |              |        |           |             |                  |
| 384817e0-8f73-4c0f-9199-16846b67e446 | eth1000 | ethernet | 0000:02:03.0 | 0      | -1        | True        | Virtio network   |
|                                      |         |          |              |        |           |             | device           |
|                                      |         |          |              |        |           |             |                  |
| fe109a21-d3c1-42aa-bbae-da92c7c8bf2e | eth1001 | ethernet | 0000:02:04.0 | 0      | -1        | True        | Virtio network   |
|                                      |         |          |              |        |           |             | device           |
|                                      |         |          |              |        |           |             |                  |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -n enp2s1 -c platform --networks oam controller-1 enp2s1
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | enp2s1                               |
| iftype           | ethernet                             |
| ports            | [u'enp2s1']                          |
| providernetworks | None                                 |
| imac             | 52:54:00:cd:21:78                    |
| imtu             | 1500                                 |
| ifclass          | platform                             |
| networks         | oam                                  |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | d8add911-0139-48f3-b676-4a1210e3612d |
| ihost_uuid       | 9c2f618c-75d1-4d21-af30-eb058cc46a64 |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-14T17:09:51.865649+00:00     |
| updated_at       | 2019-01-15T08:40:54.293652+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | static                               |
| ipv6_mode        | disabled                             |
| accelerated      | [False]                              |
+------------------+--------------------------------------+
```

## Providing Data Interfaces on Controller-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-list -a controller-1
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| uuid                                 | name    | class    | type     | vlan | ports        | uses | used | attributes | provider networks |
|                                      |         |          |          | id   |              | i/f  | by   |            |                   |
|                                      |         |          |          |      |              |      | i/f  |            |                   |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| 97174eaf-0923-4161-b7d3-2b8ca0b16556 | mgmt0   | platform | ethernet | None | [u'enp2s2']  | []   | []   | MTU=1500   | None              |
| d137e367-fd84-4323-b303-9b2406110215 | eth1000 | None     | ethernet | None | [u'eth1000'] | []   | []   | MTU=1500   | None              |
| d8add911-0139-48f3-b676-4a1210e3612d | enp2s1  | platform | ethernet | None | [u'enp2s1']  | []   | []   | MTU=1500   | None              |
| fd67b82d-2c69-431f-9f4d-15f12c950a99 | eth1001 | None     | ethernet | None | [u'eth1001'] | []   | []   | MTU=1500   | None              |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -p providernet-a -c data controller-1 eth1000
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:8e:ef:d8                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | d137e367-fd84-4323-b303-9b2406110215 |
| ihost_uuid       | 9c2f618c-75d1-4d21-af30-eb058cc46a64 |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-14T17:09:53.753060+00:00     |
| updated_at       | 2019-01-15T08:41:56.663906+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
```

## Provisioning Storage on Controller-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-1
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                   |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                               |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
| 01879566-c03e-47ae-bda6-45fc9f7f72ac | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-1.0                 |
|                                      |           |         |         |       |            |              |         |                               |
| 4b3a36de-c6aa-4e19-954e-fde5cecaf10e | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-2.0                 |
|                                      |           |         |         |       |            |              |         |                               |
| 975604a9-204e-4d50-ba7e-05cd29269c6e | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-3.0                 |
|                                      |           |         |         |       |            |              |         |                               |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-1 cinder-volumes
+-----------------------+--------------------------------------+
| Property              | Value                                |
+-----------------------+--------------------------------------+
| lvm_vg_name           | cinder-volumes                       |
| vg_state              | adding                               |
| uuid                  | ee59d06e-5ab3-4a47-b509-fca46c7f1c4b |
| ihost_uuid            | 9c2f618c-75d1-4d21-af30-eb058cc46a64 |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2019-01-15T08:43:23.780441+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-1 4b3a36de-c6aa-4e19-954e-fde5cecaf10e 199 -t lvm_phys_vol
+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| device_node | /dev/sdb1                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 203776                                           |
| uuid        | 0978e551-6494-47cf-b662-8c745339d12a             |
| ihost_uuid  | 9c2f618c-75d1-4d21-af30-eb058cc46a64             |
| idisk_uuid  | 4b3a36de-c6aa-4e19-954e-fde5cecaf10e             |
| ipv_uuid    | None                                             |
| status      | Creating (on unlock)                             |
| created_at  | 2019-01-15T08:44:20.901677+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-1 --disk 4b3a36de-c6aa-4e19-954e-fde5cecaf10e
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| 0978e551-6494-47cf-b662-8c745339d12a | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       | (on      |
|                                      |                             |            |                                      |       |       | unlock)  |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-1 cinder-volumes 0978e551-6494-47cf-b662-8c745339d12a
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | d5cf7179-0d99-4179-b4b8-0c0e02f49f11             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 0978e551-6494-47cf-b662-8c745339d12a             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 9c2f618c-75d1-4d21-af30-eb058cc46a64             |
| created_at               | 2019-01-15T08:47:41.629932+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

## Configuring VM Local Storage on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-1
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                   |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                               |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
| 01879566-c03e-47ae-bda6-45fc9f7f72ac | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-1.0                 |
|                                      |           |         |         |       |            |              |         |                               |
| 4b3a36de-c6aa-4e19-954e-fde5cecaf10e | /dev/sdb  | 2064    | HDD     | 200.0 | 0.997      | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-2.0                 |
|                                      |           |         |         |       |            |              |         |                               |
| 975604a9-204e-4d50-ba7e-05cd29269c6e | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000:00 |
|                                      |           |         |         |       |            |              |         | :1f.2-ata-3.0                 |
|                                      |           |         |         |       |            |              |         |                               |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-1 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 63073145-04cd-4d33-be48-3cb2b7e875d0                              |
| ihost_uuid            | 9c2f618c-75d1-4d21-af30-eb058cc46a64                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-01-15T08:48:30.938737+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-1 975604a9-204e-4d50-ba7e-05cd29269c6e 199 -t lvm_phys_vol
+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 |
| device_node | /dev/sdc1                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 203776                                           |
| uuid        | 0ae3a9cd-0b2b-438b-80f6-e0305bdf5635             |
| ihost_uuid  | 9c2f618c-75d1-4d21-af30-eb058cc46a64             |
| idisk_uuid  | 975604a9-204e-4d50-ba7e-05cd29269c6e             |
| ipv_uuid    | None                                             |
| status      | Creating (on unlock)                             |
| created_at  | 2019-01-15T08:49:25.692581+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-1
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| 0978e551-6494-47cf-b662-8c745339d12a | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       | (on      |
|                                      |                             |            |                                      |       |       | unlock)  |
|                                      |                             |            |                                      |       |       |          |
| 0ae3a9cd-0b2b-438b-80f6-e0305bdf5635 | /dev/disk/by-path/pci-0000: | /dev/sdc1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-3.0-part1       |            |                                      |       |       | (on      |
|                                      |                             |            |                                      |       |       | unlock)  |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-1 nova-local 0ae3a9cd-0b2b-438b-80f6-e0305bdf5635
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 432876f8-c763-42e0-9a42-6ed1b748b87f             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 0ae3a9cd-0b2b-438b-80f6-e0305bdf5635             |
| disk_or_part_device_node | /dev/sdc1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 |
| lvm_pv_name              | /dev/sdc1                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 9c2f618c-75d1-4d21-af30-eb058cc46a64             |
| created_at               | 2019-01-15T08:51:05.649266+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

## Unlocking Controller-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | Config out-of-date                   |
| config_target       | 34b87eb9-da47-45cb-94de-d11ae4920b9a |
| console             | ttyS0,115200                         |
| created_at          | 2019-01-14T16:30:06.590410+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:5f:6a:ee                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| subfunction_avail   | online                               |
| subfunction_oper    | disabled                             |
| subfunctions        | controller,worker                    |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-01-15T08:50:02.330310+00:00     |
| uptime              | 56353                                |
| uuid                | 9c2f618c-75d1-4d21-af30-eb058cc46a64 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```
