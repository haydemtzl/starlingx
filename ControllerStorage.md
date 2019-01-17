# Controller-0 and System Provision

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
| id               | 263b0847-3820-4ba7-8fe9-ba43b11ad047 |
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
| id               | b10ea37e-3450-4b5c-906d-7f32e16d23af |
| maximum          | 400                                  |
| minimum          | 100                                  |
| name             | providernet-a-range1                 |
| project_id       | a49d11aaafd047fe8efaf5a6ba22f4cb     |
| providernet_id   | 263b0847-3820-4ba7-8fe9-ba43b11ad047 |
| providernet_name | providernet-a                        |
| providernet_type | vlan                                 |
| shared           | False                                |
| tenant_id        | a49d11aaafd047fe8efaf5a6ba22f4cb     |
+------------------+--------------------------------------+
```

Configuring Cinder on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+------------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                        |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                    |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+------------------------------------+
| 46edaea7-ff7a-4f22-92a1-841502bc8c05 | /dev/sda  | 2048    | HDD     | 250.0 | 0.0        | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f.  |
|                                      |           |         |         |       |            |              |         | 2-ata-1.0                          |
|                                      |           |         |         |       |            |              |         |                                    |
| e9e16679-a136-449a-92b1-c00c4612f033 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f.  |
|                                      |           |         |         |       |            |              |         | 2-ata-2.0                          |
|                                      |           |         |         |       |            |              |         |                                    |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 cinder-volumes
+-----------------------+--------------------------------------+
| Property              | Value                                |
+-----------------------+--------------------------------------+
| lvm_vg_name           | cinder-volumes                       |
| vg_state              | adding                               |
| uuid                  | 1c2fee2b-5541-474a-bfcb-ef8769b75d6d |
| ihost_uuid            | f8714e42-2abd-4aa6-adc4-53402f6c4bd0 |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2019-01-17T13:36:02.237978+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 e9e16679-a136-449a-92b1-c00c4612f033 199 -t lvm_phys_vol
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
| uuid        | bf9de972-eacb-485d-ab50-0a77b12dd6f7             |
| ihost_uuid  | f8714e42-2abd-4aa6-adc4-53402f6c4bd0             |
| idisk_uuid  | e9e16679-a136-449a-92b1-c00c4612f033             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-17T13:36:38.118102+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk e9e16679-a136-449a-92b1-c00c4612f033
+--------------------------------------+-----------------------------+------------+--------------------------------------+--------+----------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_n | size_gib | status   |
|                                      |                             | e          |                                      | ame    |          |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+--------+----------+----------+
| bf9de972-eacb-485d-ab50-0a77b12dd6f7 | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None   | 199.0    | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |        |          |          |
|                                      |                             |            |                                      |        |          |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+--------+----------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk e9e16679-a136-449a-92b1-c00c4612f033
+--------------------------------------+-----------------------------+------------+--------------------------------------+-----------+----------+--------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_name | size_gib | status |
|                                      |                             | e          |                                      |           |          |        |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-----------+----------+--------+
| bf9de972-eacb-485d-ab50-0a77b12dd6f7 | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | LVM       | 199.0    | Ready  |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      | Physical  |          |        |
|                                      |                             |            |                                      | Volume    |          |        |
|                                      |                             |            |                                      |           |          |        |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-----------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 cinder-volumes bf9de972-eacb-485d-ab50-0a77b12dd6f7
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | d9ba4769-d1bc-4651-822e-4f590a36feef             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | bf9de972-eacb-485d-ab50-0a77b12dd6f7             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | f8714e42-2abd-4aa6-adc4-53402f6c4bd0             |
| created_at               | 2019-01-17T13:38:29.228938+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add lvm -s cinder --confirmed

System configuration has changed.
Please follow the administrator guide to complete configuring the system.

+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
| uuid                                 | name       | backend | state       | task                             | services | capabilities |
+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
| 66aa8fcb-706a-42cf-9340-e73469c1289d | lvm-store  | lvm     | configuring | {u'controller-0': 'configuring'} | cinder   |              |
| 7373878a-e354-41fc-9d9d-54e4d5d09d48 | file-store | file    | configured  | None                             | glance   |              |
+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
| uuid                                 | name       | backend | state       | task                             | services | capabilities |
+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
| 66aa8fcb-706a-42cf-9340-e73469c1289d | lvm-store  | lvm     | configuring | {u'controller-0': 'configuring'} | cinder   |              |
| 7373878a-e354-41fc-9d9d-54e4d5d09d48 | file-store | file    | configured  | None                             | glance   |              |
+--------------------------------------+------------+---------+-------------+----------------------------------+----------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-0
Lvm is configuring. All storage backends must be configured before operation is allowed.
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+------------+------+----------+--------------+
| uuid                                 | name       | backend | state      | task | services | capabilities |
+--------------------------------------+------------+---------+------------+------+----------+--------------+
| 66aa8fcb-706a-42cf-9340-e73469c1289d | lvm-store  | lvm     | configured | None | cinder   |              |
| 7373878a-e354-41fc-9d9d-54e4d5d09d48 | file-store | file    | configured | None | glance   |              |
+--------------------------------------+------------+---------+------------+------+----------+--------------+
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
| config_applied      | 326fa54e-98ab-4613-a507-13f887d5827b       |
| config_status       | None                                       |
| config_target       | 326fa54e-98ab-4613-a507-13f887d5827b       |
| console             | tty0                                       |
| created_at          | 2019-01-17T10:30:42.079395+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:a5:05:d3                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-01-17T14:03:50.257452+00:00           |
| uptime              | 13785                                      |
| uuid                | f8714e42-2abd-4aa6-adc4-53402f6c4bd0       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```
