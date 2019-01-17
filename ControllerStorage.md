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

## Controller-1 / Compute Hosts Installation

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
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 3 personality=worker hostname=worker-0
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
| created_at          | 2019-01-17T14:40:34.573926+00:00     |
| hostname            | worker-0                             |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.248                      |
| mgmt_mac            | 52:54:00:f0:16:c9                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | d5fb7325-858e-48da-96b2-9edbddaf6e6b |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show worker-0 | grep install
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | worker-0     | worker      | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

# Controller-1 Provisioning

```
```


# Compute Host Provision

```

```
