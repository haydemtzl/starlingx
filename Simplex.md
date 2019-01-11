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
| id               | e65239c9-b425-4863-be69-0b49f7569017 |
| mtu              | 1500                                 |
| name             | providernet-a                        |
| ranges           |                                      |
| status           | DOWN                                 |
| type             | vlan                                 |
| vlan_transparent | False                                |
+------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-range-create --name providernet-a-range1 --range 100-400 providernet-a
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Created a new providernet_range:
+------------------+--------------------------------------+
| Field            | Value                                |
+------------------+--------------------------------------+
| description      |                                      |
| id               | deab598f-0e2d-4b5d-947a-fc91653c58f4 |
| maximum          | 400                                  |
| minimum          | 100                                  |
| name             | providernet-a-range1                 |
| project_id       | 125082a444694b95b246c607805dc65c     |
| providernet_id   | e65239c9-b425-4863-be69-0b49f7569017 |
| providernet_name | providernet-a                        |
| providernet_type | vlan                                 |
| shared           | False                                |
| tenant_id        | 125082a444694b95b246c607805dc65c     |
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
| 3603814b-57e2-4d6a-9f14-1de50721ed0f | lo      | platform | virtual  | None | []           | []   | []   | MTU=1500   | None              |
| 608be5c9-b1f2-47af-99e1-fa952d9a7e2d | eth1000 | None     | ethernet | None | [u'eth1000'] | []   | []   | MTU=1500   | None              |
| 625acbab-a460-4927-90a8-68ce19e61393 | eth1001 | None     | ethernet | None | [u'eth1001'] | []   | []   | MTU=1500   | None              |
| d74d88d5-cbf9-4a0e-baa1-b9ea03c872cb | enp2s2  | None     | ethernet | None | [u'enp2s2']  | []   | []   | MTU=1500   | None              |
| efb44047-4e99-4c78-b864-120254e4ac41 | enp2s1  | platform | ethernet | None | [u'enp2s1']  | []   | []   | MTU=1500   | None              |
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
| imac             | 52:54:00:fa:c0:95                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | 608be5c9-b1f2-47af-99e1-fa952d9a7e2d |
| ihost_uuid       | 8a3d98ed-6832-4c43-ac65-768979f0d648 |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-11T15:12:40.347005+00:00     |
| updated_at       | 2019-01-11T16:12:46.416710+00:00     |
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
| b381bcf2-8a27-4d11-9d53-c1d25cae281c | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-1.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 86224659-75f2-4535-ace7-407b12230a47 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-2.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 89d03b65-a479-44ff-a570-492232c509d8 | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000: |
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
| uuid                  | 6c085ab6-9f13-4217-886c-564bfe52c925 |
| ihost_uuid            | 8a3d98ed-6832-4c43-ac65-768979f0d648 |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2019-01-11T16:14:13.981934+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 86224659-75f2-4535-ace7-407b12230a47 199 -t lvm_phys_vol
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
| uuid        | 291c8b40-1020-48d8-bbb7-30602dac69f5             |
| ihost_uuid  | 8a3d98ed-6832-4c43-ac65-768979f0d648             |
| idisk_uuid  | 86224659-75f2-4535-ace7-407b12230a47             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-11T16:15:10.102407+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 86224659-75f2-4535-ace7-407b12230a47
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| 291c8b40-1020-48d8-bbb7-30602dac69f5 | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       |          |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 86224659-75f2-4535-ace7-407b12230a47
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| uuid                                 | device_path                                      | device_node | type_guid                            | type_name           | size_gib | status |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| 291c8b40-1020-48d8-bbb7-30602dac69f5 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 | /dev/sdb1   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 199.0    | Ready  |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 cinder-volumes 291c8b40-1020-48d8-bbb7-30602dac69f5
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 1b5b208a-b40b-45f9-bd78-949a03235ed3             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 291c8b40-1020-48d8-bbb7-30602dac69f5             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 8a3d98ed-6832-4c43-ac65-768979f0d648             |
| created_at               | 2019-01-11T16:18:01.699046+00:00                 |
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
| c31ee9c5-072e-41a0-8e1e-c0ab6b52eb5d | lvm-store  | lvm     | configuring | {u'controller-0':              | cinder   |              |
|                                      |            |         |             | 'configuring'}                 |          |              |
|                                      |            |         |             |                                |          |              |
| eda59142-6859-4313-8978-988367ac8012 | file-store | file    | configured  | None                           | glance   |              |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| uuid                                 | name       | backend | state       | task                           | services | capabilities |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
| c31ee9c5-072e-41a0-8e1e-c0ab6b52eb5d | lvm-store  | lvm     | configuring | {u'controller-0':              | cinder   |              |
|                                      |            |         |             | 'configuring'}                 |          |              |
|                                      |            |         |             |                                |          |              |
| eda59142-6859-4313-8978-988367ac8012 | file-store | file    | configured  | None                           | glance   |              |
+--------------------------------------+------------+---------+-------------+--------------------------------+----------+--------------+
```

## Configuring VM Local Storage on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                 |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| b381bcf2-8a27-4d11-9d53-c1d25cae281c | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-1.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 86224659-75f2-4535-ace7-407b12230a47 | /dev/sdb  | 2064    | HDD     | 200.0 | 0.997      | Undetermined | QM00003 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-2.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 89d03b65-a479-44ff-a570-492232c509d8 | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000: |
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
| uuid                  | f2fc934a-dc8a-4387-a708-cd6d1bade837                              |
| ihost_uuid            | 8a3d98ed-6832-4c43-ac65-768979f0d648                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-01-11T16:22:50.921005+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 89d03b65-a479-44ff-a570-492232c509d8 199 -t lvm_phys_vol
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
| uuid        | 25fec7c1-6441-440b-843a-38c3c09dad50             |
| ihost_uuid  | 8a3d98ed-6832-4c43-ac65-768979f0d648             |
| idisk_uuid  | 89d03b65-a479-44ff-a570-492232c509d8             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-11T16:23:43.503508+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 89d03b65-a479-44ff-a570-492232c509d8
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| 25fec7c1-6441-440b-843a-38c3c09dad50 | /dev/disk/by-path/pci-0000: | /dev/sdc1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-3.0-part1       |            |                                      |       |       |          |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 89d03b65-a479-44ff-a570-492232c509d8
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| uuid                                 | device_path                                      | device_node | type_guid                            | type_name           | size_gib | status |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| 25fec7c1-6441-440b-843a-38c3c09dad50 | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 | /dev/sdc1   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 199.0    | Ready  |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 nova-local 25fec7c1-6441-440b-843a-38c3c09dad50
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 0d38b489-9f36-4396-aad5-cf0e6d75dffa             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 25fec7c1-6441-440b-843a-38c3c09dad50             |
| disk_or_part_device_node | /dev/sdc1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0-part1 |
| lvm_pv_name              | /dev/sdc1                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 8a3d98ed-6832-4c43-ac65-768979f0d648             |
| created_at               | 2019-01-11T16:34:32.110245+00:00                 |
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
| config_applied      | 0f1d23ef-bb06-482b-97d5-23b7ddb27434       |
| config_status       | None                                       |
| config_target       | 0f1d23ef-bb06-482b-97d5-23b7ddb27434       |
| console             | tty0                                       |
| created_at          | 2019-01-11T15:12:31.428932+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 00:00:00:00:00:00                          |
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
| updated_at          | 2019-01-11T16:34:42.458788+00:00           |
| uptime              | 6988                                       |
| uuid                | 8a3d98ed-6832-4c43-ac65-768979f0d648       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

