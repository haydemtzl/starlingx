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

# Compute Host Provision

```
controller-0:~$ source /etc/nova/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | worker-0     | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

## Provisioning Network Interfaces on a Compute Host

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list worker-0
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
| uuid                                 | name    | type     | pci address  | device | processor | accelerated | device type      |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
| e3f63aec-cb1c-40ec-ab72-92785ce98761 | enp2s1  | ethernet | 0000:02:01.0 | 0      | -1        | False       | 82540EM Gigabit  |
|                                      |         |          |              |        |           |             | Ethernet         |
|                                      |         |          |              |        |           |             | Controller       |
|                                      |         |          |              |        |           |             |                  |
| 7601d9ad-0e15-43f8-b486-d7128a0c90da | enp2s2  | ethernet | 0000:02:02.0 | 0      | -1        | False       | 82540EM Gigabit  |
|                                      |         |          |              |        |           |             | Ethernet         |
|                                      |         |          |              |        |           |             | Controller       |
|                                      |         |          |              |        |           |             |                  |
| a5cc71bd-cf5f-4788-beda-9b3ee27eeac2 | eth1000 | ethernet | 0000:02:03.0 | 0      | -1        | True        | Virtio network   |
|                                      |         |          |              |        |           |             | device           |
|                                      |         |          |              |        |           |             |                  |
| bcd56c79-8c22-4350-8667-51d8e1bb3923 | eth1001 | ethernet | 0000:02:04.0 | 0      | -1        | True        | Virtio network   |
|                                      |         |          |              |        |           |             | device           |
|                                      |         |          |              |        |           |             |                  |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -p providernet-a -c data worker-0 eth1000
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:8d:50:94                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | e2fc6f03-e63f-46d6-839b-8f5ea4df72c9 |
| ihost_uuid       | d5fb7325-858e-48da-96b2-9edbddaf6e6b |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-17T15:17:09.519543+00:00     |
| updated_at       | 2019-01-17T15:20:03.332844+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
```

## VSwitch Virtual Environment

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify worker-0 -f vswitch -p0 1
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| uuid                                 | log_c | processor | phy_c | thread | processor_model                           | assigned_function |
|                                      | ore   |           | ore   |        |                                           |                   |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| e65b51ad-2e2f-4852-9d6c-755eae262a5a | 0     | 0         | 0     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | Platform          |
| b9c54670-622f-42a0-a447-3e9eb478f2fd | 1     | 0         | 1     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | vSwitch           |
| 1ec2fb0b-8272-4375-97c6-a4305b074c6f | 2     | 0         | 2     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | Applications      |
| 1a38067d-4ec5-4020-9a4a-b26f561fb378 | 3     | 0         | 3     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | Applications      |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
```

## Provisioning Storage on a Compute Host

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list worker-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                    |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------+
| ab578f38-0967-4778-9046-c7837fdde66a | /dev/sda  | 2048    | HDD     | 200.0 | 120.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00: |
|                                      |           |         |         |       |            |              |         | 1f.2-ata-1.0                   |
|                                      |           |         |         |       |            |              |         |                                |
| 6f6aef17-ab13-4efa-9b90-369208395203 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00: |
|                                      |           |         |         |       |            |              |         | 1f.2-ata-2.0                   |
|                                      |           |         |         |       |            |              |         |                                |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add worker-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 3f01467b-141a-4bd9-b4ea-2370d17f5ddf                              |
| ihost_uuid            | d5fb7325-858e-48da-96b2-9edbddaf6e6b                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-01-17T15:21:35.305010+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add worker-0 nova-local 6f6aef17-ab13-4efa-9b90-369208395203 
+--------------------------+--------------------------------------------+
| Property                 | Value                                      |
+--------------------------+--------------------------------------------+
| uuid                     | e63e8e73-011d-4d64-9366-2ebdcfec979c       |
| pv_state                 | adding                                     |
| pv_type                  | disk                                       |
| disk_or_part_uuid        | 6f6aef17-ab13-4efa-9b90-369208395203       |
| disk_or_part_device_node | /dev/sdb                                   |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0 |
| lvm_pv_name              | /dev/sdb                                   |
| lvm_vg_name              | nova-local                                 |
| lvm_pv_uuid              | None                                       |
| lvm_pv_size_gib          | 0.0                                        |
| lvm_pe_total             | 0                                          |
| lvm_pe_alloced           | 0                                          |
| ihost_uuid               | d5fb7325-858e-48da-96b2-9edbddaf6e6b       |
| created_at               | 2019-01-17T15:22:42.599119+00:00           |
| updated_at               | None                                       |
+--------------------------+--------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-modify -b image -s 10240 worker-0 nova-local
usage: system [--version] [--debug] [-v] [-k] [--cert-file CERT_FILE]
              [--key-file KEY_FILE] [--ca-file CA_FILE] [--timeout TIMEOUT]
              [--os-username OS_USERNAME] [--os-password OS_PASSWORD]
              [--os-tenant-id OS_TENANT_ID] [--os-tenant-name OS_TENANT_NAME]
              [--os-auth-url OS_AUTH_URL] [--os-region-name OS_REGION_NAME]
              [--os-auth-token OS_AUTH_TOKEN] [--system-url SYSTEM_URL]
              [--system-api-version SYSTEM_API_VERSION]
              [--os-service-type OS_SERVICE_TYPE]
              [--os-endpoint-type OS_ENDPOINT_TYPE]
              [--os-user-domain-id OS_USER_DOMAIN_ID]
              [--os-user-domain-name OS_USER_DOMAIN_NAME]
              [--os-project-id OS_PROJECT_ID]
              [--os-project-name OS_PROJECT_NAME]
              [--os-project-domain-id OS_PROJECT_DOMAIN_ID]
              [--os-project-domain-name OS_PROJECT_DOMAIN_NAME]
              <subcommand> ...
system: error: unrecognized arguments: -s nova-local
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-modify -b image worker-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 3f01467b-141a-4bd9-b4ea-2370d17f5ddf                              |
| ihost_uuid            | d5fb7325-858e-48da-96b2-9edbddaf6e6b                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-01-17T15:21:35.305010+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock worker-0
+---------------------+--------------------------------------+
| Property            | Value                                |[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-modify -b image -s 10240 worker-0 nova-local
usage: system [--version] [--debug] [-v] [-k] [--cert-file CERT_FILE]
              [--key-file KEY_FILE] [--ca-file CA_FILE] [--timeout TIMEOUT]
              [--os-username OS_USERNAME] [--os-password OS_PASSWORD]
              [--os-tenant-id OS_TENANT_ID] [--os-tenant-name OS_TENANT_NAME]
              [--os-auth-url OS_AUTH_URL] [--os-region-name OS_REGION_NAME]
              [--os-auth-token OS_AUTH_TOKEN] [--system-url SYSTEM_URL]
              [--system-api-version SYSTEM_API_VERSION]
              [--os-service-type OS_SERVICE_TYPE]
              [--os-endpoint-type OS_ENDPOINT_TYPE]
              [--os-user-domain-id OS_USER_DOMAIN_ID]
              [--os-user-domain-name OS_USER_DOMAIN_NAME]
              [--os-project-id OS_PROJECT_ID]
              [--os-project-name OS_PROJECT_NAME]
              [--os-project-domain-id OS_PROJECT_DOMAIN_ID]
              [--os-project-domain-name OS_PROJECT_DOMAIN_NAME]
              <subcommand> ...
system: error: unrecognized arguments: -s nova-local
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | 030b8c31-6eb8-4c4f-83da-1a9902a22166 |
| config_status       | None                                 |
| config_target       | 030b8c31-6eb8-4c4f-83da-1a9902a22166 |
| console             | ttyS0,115200                         |
| created_at          | 2019-01-17T14:40:34.573926+00:00     |
| hostname            | worker-0                             |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.248                      |
| mgmt_mac            | 52:54:00:f0:16:c9                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-01-17T15:23:54.399639+00:00     |
| uptime              | 614                                  |
| uuid                | d5fb7325-858e-48da-96b2-9edbddaf6e6b |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

# System Health Check

## Listing StarlingX Nodes

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | worker-0     | worker      | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list
+----------+--------------------------------------------------------------------------+----------------------+----------+---------------+
| Alarm ID | Reason Text                                                              | Entity ID            | Severity | Time Stamp    |
+----------+--------------------------------------------------------------------------+----------------------+----------+---------------+
| 400.002  | Service group cloud-services loss of redundancy; expected 1 standby      | service_domain=      | major    | 2019-01-17T14 |
|          | member but no standby members available                                  | controller.          |          | :27:35.427254 |
|          |                                                                          | service_group=cloud- |          |               |
|          |                                                                          | services             |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group controller-services loss of redundancy; expected 1 standby | service_domain=      | major    | 2019-01-17T14 |
|          | member but no standby members available                                  | controller.          |          | :26:17.214315 |
|          |                                                                          | service_group=       |          |               |
|          |                                                                          | controller-services  |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group vim-services loss of redundancy; expected 1 standby member | service_domain=      | major    | 2019-01-17T14 |
|          | but no standby members available                                         | controller.          |          | :26:17.127586 |
|          |                                                                          | service_group=vim-   |          |               |
|          |                                                                          | services             |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group oam-services loss of redundancy; expected 1 standby member | service_domain=      | major    | 2019-01-17T14 |
|          | but no standby members available                                         | controller.          |          | :26:04.373822 |
|          |                                                                          | service_group=oam-   |          |               |
|          |                                                                          | services             |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group patching-services loss of redundancy; expected 1 standby   | service_domain=      | major    | 2019-01-17T14 |
|          | member but no standby members available                                  | controller.          |          | :26:03.995340 |
|          |                                                                          | service_group=       |          |               |
|          |                                                                          | patching-services    |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group directory-services loss of redundancy; expected 2 active   | service_domain=      | major    | 2019-01-17T14 |
|          | members but only 1 active member available                               | controller.          |          | :26:03.907343 |
|          |                                                                          | service_group=       |          |               |
|          |                                                                          | directory-services   |          |               |
|          |                                                                          |                      |          |               |
| 400.002  | Service group web-services loss of redundancy; expected 2 active members | service_domain=      | major    | 2019-01-17T14 |
|          | but only 1 active member available                                       | controller.          |          | :26:03.801357 |
|          |                                                                          | service_group=web-   |          |               |
|          |                                                                          | services             |          |               |
|          |                                                                          |                      |          |               |
| 400.005  | Communication failure detected with peer over port enp2s2 on host        | host=controller-0.   | major    | 2019-01-17T14 |
|          | controller-0                                                             | network=mgmt         |          | :26:03.714416 |
|          |                                                                          |                      |          |               |
| 400.005  | Communication failure detected with peer over port enp2s1 on host        | host=controller-0.   | major    | 2019-01-17T14 |
|          | controller-0                                                             | network=oam          |          | :26:01.384896 |
|          |                                                                          |                      |          |               |
+----------+--------------------------------------------------------------------------+----------------------+----------+---------------+
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
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | worker-0     | worker      | unlocked       | enabled     | available    |
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
| created_at          | 2019-01-17T14:40:03.388786+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:dc:12:2f                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | b335b224-f3a3-47f5-8219-e52daf3c1549 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show controller-1 | grep install
| install_output      | text                                    |
| install_state       | None                                    |
| install_state_info  | None                                    |
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | worker-0     | worker      | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```

```
