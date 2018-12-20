# Controller-0 and System Provision

```
[wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-create providernet-a --type=vlan
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Created a new providernet:
+------------------+--------------------------------------+
| Field            | Value                                |
+------------------+--------------------------------------+
| description      |                                      |
| id               | f409b995-8550-43b0-81ca-164ddca54537 |
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
| id               | bae7e12a-fcb3-44f2-9a7b-20af6c1003f1 |
| maximum          | 400                                  |
| minimum          | 100                                  |
| name             | providernet-a-range1                 |
| project_id       | 9dc3492a4dbb4c3281aacbabf56c18af     |
| providernet_id   | f409b995-8550-43b0-81ca-164ddca54537 |
| providernet_name | providernet-a                        |
| providernet_type | vlan                                 |
| shared           | False                                |
| tenant_id        | 9dc3492a4dbb4c3281aacbabf56c18af     |
+------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add ceph -s cinder,glance,swift,nova

WARNING : THIS OPERATION IS NOT REVERSIBLE AND CANNOT BE CANCELLED. 

By confirming this operation, Ceph backend will be created.
A minimum of 2 storage nodes are required to complete the configuration.
Please set the 'confirmed' field to execute this operation for the ceph backend.
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add ceph -s cinder,glance,swift,nova --confirmed

System configuration has changed.
Please follow the administrator guide to complete configuring the system.

+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
| uuid                                 | name       | backend | state       | task | services                 | capabilities       |
+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
| 80943244-6f42-40a4-89aa-055b085dc0e5 | file-store | file    | configured  | None | glance                   |                    |
| bee1e818-4717-459a-987c-9d66d50e2138 | ceph-store | ceph    | configuring | {}   | cinder,glance,swift,nova | min_replication: 1 |
|                                      |            |         |             |      |                          | replication: 2     |
+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
| uuid                                 | name       | backend | state       | task | services                 | capabilities       |
+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
| 80943244-6f42-40a4-89aa-055b085dc0e5 | file-store | file    | configured  | None | glance                   |                    |
| bee1e818-4717-459a-987c-9d66d50e2138 | ceph-store | ceph    | configuring | {}   | cinder,glance,swift,nova | min_replication: 1 |
|                                      |            |         |             |      |                          | replication: 2     |
+--------------------------------------+------------+---------+-------------+------+--------------------------+--------------------+
...
...
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+------------+---------+------------+-------------------+--------------------------+--------------------+
| uuid                                 | name       | backend | state      | task              | services                 | capabilities       |
+--------------------------------------+------------+---------+------------+-------------------+--------------------------+--------------------+
| 80943244-6f42-40a4-89aa-055b085dc0e5 | file-store | file    | configured | None              | glance                   |                    |
| bee1e818-4717-459a-987c-9d66d50e2138 | ceph-store | ceph    | configured | provision-storage | cinder,glance,swift,nova | min_replication: 1 |
|                                      |            |         |            |                   |                          | replication: 2     |
+--------------------------------------+------------+---------+------------+-------------------+--------------------------+--------------------+
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
| capabilities        | {u'stor_function': u'monitor'}             |
| config_applied      | 3e63f993-4b5f-4835-8d85-0783a0bc7545       |
| config_status       | None                                       |
| config_target       | 3e63f993-4b5f-4835-8d85-0783a0bc7545       |
| console             | tty0                                       |
| created_at          | 2018-12-19T15:12:54.402419+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:fe:13:1c                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 18.10                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2018-12-19T15:42:08.715971+00:00           |
| uptime              | 2751                                       |
| uuid                | 9ef881f4-c80d-4d75-86e8-5ab5054fb5c2       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

Reboot

```
controller-0:~$ source /etc/nova/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ system controllerfs-list
+--------------------------------------+-----------------+------+--------------------+------------+-----------+
| UUID                                 | FS Name         | Size | Logical Volume     | Replicated | State     |
|                                      |                 | in   |                    |            |           |
|                                      |                 | GiB  |                    |            |           |
+--------------------------------------+-----------------+------+--------------------+------------+-----------+
| 31bd6740-2562-4dc4-9962-f6f30b36edb6 | backup          | 40   | backup-lv          | False      | available |
| 320d4c82-9d8c-4e56-b951-a70cc97212a1 | database        | 10   | pgsql-lv           | True       | available |
| 6d26b21a-1498-4226-b23e-0ac6fdf12af5 | extension       | 1    | extension-lv       | True       | available |
| 5e570d7f-eb84-4458-9df5-e9b6a8b0d4e2 | glance          | 10   | cgcs-lv            | True       | available |
| 1eca3dbd-eb69-4b8d-94a3-14c69b08d218 | gnocchi         | 5    | gnocchi-lv         | False      | available |
| c0812aba-9b30-472f-8e82-207201c4f430 | img-conversions | 10   | img-conversions-lv | False      | available |
| 152803c3-2f59-4d78-a2a4-8ea82d4639cd | scratch         | 8    | scratch-lv         | False      | available |
+--------------------------------------+-----------------+------+--------------------+------------+-----------+
[wrsroot@controller-0 ~(keystone_admin)]$ system controllerfs-modify backup=42 database=12 img-conversions=12
+--------------------------------------+-----------------+------+--------------------+------------+------------------------------+
| UUID                                 | FS Name         | Size | Logical Volume     | Replicated | State                        |
|                                      |                 | in   |                    |            |                              |
|                                      |                 | GiB  |                    |            |                              |
+--------------------------------------+-----------------+------+--------------------+------------+------------------------------+
| 31bd6740-2562-4dc4-9962-f6f30b36edb6 | backup          | 42   | backup-lv          | False      | available                    |
| 320d4c82-9d8c-4e56-b951-a70cc97212a1 | database        | 12   | pgsql-lv           | True       | drbd_fs_resizing_in_progress |
| 6d26b21a-1498-4226-b23e-0ac6fdf12af5 | extension       | 1    | extension-lv       | True       | available                    |
| 5e570d7f-eb84-4458-9df5-e9b6a8b0d4e2 | glance          | 10   | cgcs-lv            | True       | available                    |
| 1eca3dbd-eb69-4b8d-94a3-14c69b08d218 | gnocchi         | 5    | gnocchi-lv         | False      | available                    |
| c0812aba-9b30-472f-8e82-207201c4f430 | img-conversions | 12   | img-conversions-lv | False      | available                    |
| 152803c3-2f59-4d78-a2a4-8ea82d4639cd | scratch         | 8    | scratch-lv         | False      | available                    |
+--------------------------------------+-----------------+------+--------------------+------------+------------------------------+
```

# Controller-1 / Storage Hosts / Compute Hosts Installation

## Controller-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
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
| created_at          | 2018-12-19T15:52:44.614111+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:da:f4:00                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | a53e1d79-facb-42f5-9322-a99fead0f3da |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show controller-1 | grep install
| install_output      | text                                    |
| install_state       | preinstall                              |
| install_state_info  | None                                    |
```

## Compute-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 3 personality=compute hostname=compute-0
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
| created_at          | 2018-12-19T15:56:36.776266+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.104                      |
| mgmt_mac            | 52:54:00:da:c7:20                    |
| operational         | disabled                             |
| personality         | compute                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | b35edc7d-9268-4514-ac66-f3c3d8571826 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show compute-0 | grep install
| install_output      | text                                 |
| install_state       | preinstall                           |
| install_state_info  | None                                 |
```

## Compute-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | compute     | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 4 personality=compute hostname=compute-1
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
| created_at          | 2018-12-19T15:59:16.815496+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.86                       |
| mgmt_mac            | 52:54:00:a7:a5:b8                    |
| operational         | disabled                             |
| personality         | compute                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show compute-1 | grep install
| install_output      | text                                 |
| install_state       | preinstall                           |
| install_state_info  | None                                 |
```

## Storage-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | compute     | locked         | disabled    | offline      |
| 4  | compute-1    | compute     | locked         | disabled    | offline      |
| 5  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 5 personality=storage hostname=storage-0
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
| created_at          | 2018-12-19T16:01:44.822948+00:00     |
| hostname            | storage-0                            |
| id                  | 5                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.101                      |
| mgmt_mac            | 52:54:00:3a:0e:6f                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 84816687-3c13-4620-9d03-1266ff0c2825 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show storage-0 | grep install
| install_output      | text                                            |
| install_state       | None                                            |
| install_state_info  | None                                            |
```

## Storage-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | compute     | locked         | disabled    | offline      |
| 4  | compute-1    | compute     | locked         | disabled    | offline      |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 6 personality=storage hostname=storage-1
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
| created_at          | 2018-12-19T16:03:24.951145+00:00     |
| hostname            | storage-1                            |
| id                  | 6                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.108                      |
| mgmt_mac            | 52:54:00:8c:ec:e8                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | ce3d948a-56de-4f49-bf4e-002ccdf8b532 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-show storage-1 | grep install
| install_output      | text                                                          |
| install_state       | None                                                          |
| install_state_info  | None                                                          |
```

# Controller-1 Provisioning

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | online       |
| 3  | compute-0    | compute     | locked         | disabled    | online       |
| 4  | compute-1    | compute     | locked         | disabled    | online       |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list controller-1
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| uuid                                 | name    | type     | pci address  | device | processor | accelerated | device type                       |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| 9aa29cf4-0dc7-4971-aa3a-702e21c4c781 | enp2s1  | ethernet | 0000:02:01.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| 697f9ccf-a9ca-4177-bc99-9c8c6c0ffd62 | enp2s2  | ethernet | 0000:02:02.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| 41dc095f-9125-4656-8d1c-72729deb7f05 | eth1000 | ethernet | 0000:02:03.0 | 0      | -1        | False       | Virtio network device             |
| 08a9f308-c27d-43f1-b772-e57d41c53444 | eth1001 | ethernet | 0000:02:04.0 | 0      | -1        | False       | Virtio network device             |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -n enp2s1 -c platform --networks oam controller-1 enp2s1
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | enp2s1                               |
| iftype           | ethernet                             |
| ports            | [u'enp2s1']                          |
| providernetworks | None                                 |
| imac             | 52:54:00:4d:55:56                    |
| imtu             | 1500                                 |
| ifclass          | platform                             |
| networks         | oam                                  |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | ae896743-fc73-4973-b5e5-1e5ccb24c142 |
| ihost_uuid       | a53e1d79-facb-42f5-9322-a99fead0f3da |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2018-12-19T16:03:34.251527+00:00     |
| updated_at       | 2018-12-20T09:01:40.164503+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | static                               |
| ipv6_mode        | disabled                             |
| accelerated      | [False]                              |
+------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-1
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                      |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| 76522847-51db-4c08-951b-df3d50563e3f | /dev/sda  | 2048    | HDD     | 200.0 | 0.0        | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-1.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
| 431d46d7-edeb-44d0-9086-5415a4eec5b7 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-2.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-1 cinder-volumes
+-----------------------+--------------------------------------+
| Property              | Value                                |
+-----------------------+--------------------------------------+
| lvm_vg_name           | cinder-volumes                       |
| vg_state              | adding                               |
| uuid                  | 9b4b5b7a-99f3-4230-9df4-63757280c022 |
| ihost_uuid            | a53e1d79-facb-42f5-9322-a99fead0f3da |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2018-12-20T09:02:08.270093+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-1 431d46d7-edeb-44d0-9086-5415a4eec5b7 199 -t lvm_phys_vol
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
| uuid        | f9ddefd9-fd2c-496b-8821-f03fdd92aecf             |
| ihost_uuid  | a53e1d79-facb-42f5-9322-a99fead0f3da             |
| idisk_uuid  | 431d46d7-edeb-44d0-9086-5415a4eec5b7             |
| ipv_uuid    | None                                             |
| status      | Creating (on unlock)                             |
| created_at  | 2018-12-20T09:02:29.477807+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-1 --disk 431d46d7-edeb-44d0-9086-5415a4eec5b7
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+-------------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status      |
|                                      |                             | e          |                                      | name  | gib   |             |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+-------------+
| f9ddefd9-fd2c-496b-8821-f03fdd92aecf | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating    |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       | (on unlock) |
|                                      |                             |            |                                      |       |       |             |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+-------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-1 cinder-volumes f9ddefd9-fd2c-496b-8821-f03fdd92aecf
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 2fdce4ef-91a3-43bf-b59a-09b13f20f5d7             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | f9ddefd9-fd2c-496b-8821-f03fdd92aecf             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | a53e1d79-facb-42f5-9322-a99fead0f3da             |
| created_at               | 2018-12-20T09:03:09.941695+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
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
| capabilities        | {u'stor_function': u'monitor'}       |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2018-12-19T15:52:44.614111+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:da:f4:00                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2018-12-20T09:03:10.184601+00:00     |
| uptime              | 61106                                |
| uuid                | a53e1d79-facb-42f5-9322-a99fead0f3da |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Reboot

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 3  | compute-0    | compute     | locked         | disabled    | online       |
| 4  | compute-1    | compute     | locked         | disabled    | online       |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```
# Storage Host Provision

## Storage-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                      |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| 6073a6ab-3eac-4fcd-bdf2-8803e0a5bee1 | /dev/sda  | 2048    | HDD     | 200.0 | 0.0        | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-1.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
| 835deb1c-21c0-41aa-9b15-ab3ef59edb47 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-2.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-tier-list ceph_cluster
+--------------------------------------+---------+--------+--------------------------------------+
| uuid                                 | name    | status | backend_using                        |
+--------------------------------------+---------+--------+--------------------------------------+
| e69468d8-934f-43df-943d-4ce228111c5e | storage | in-use | bee1e818-4717-459a-987c-9d66d50e2138 |
+--------------------------------------+---------+--------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-0 835deb1c-21c0-41aa-9b15-ab3ef59edb47
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| journal_location | 7542232a-c8b8-4964-b642-2d6380f9d556             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 7542232a-c8b8-4964-b642-2d6380f9d556             |
| ihost_uuid       | 84816687-3c13-4620-9d03-1266ff0c2825             |
| idisk_uuid       | 835deb1c-21c0-41aa-9b15-ab3ef59edb47             |
| tier_uuid        | e69468d8-934f-43df-943d-4ce228111c5e             |
| tier_name        | storage                                          |
| created_at       | 2018-12-20T14:10:35.369502+00:00                 |
| updated_at       | 2018-12-20T14:11:01.933177+00:00                 |
+------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-0
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | capabilities | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 7542232a-c8b8-4964-b642-2d6380f9d556 | osd      | 0     | {}           | 835deb1c-21c0-41aa-9b15-ab3ef59edb47 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {u'stor_function': u'monitor'}                                |
| config_applied      | None                                                          |
| config_status       | None                                                          |
| config_target       | None                                                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2018-12-19T16:01:44.822948+00:00                              |
| hostname            | storage-0                                                     |
| id                  | 5                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {u'stor_function': u'monitor'}                                |
| config_applied      | None                                                          |
| config_status       | None                                                          |
| config_target       | None                                                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2018-12-19T16:01:44.822948+00:00                              |
| hostname            | storage-0                                                     |
| id                  | 5                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.101                                               |
| mgmt_mac            | 52:54:00:3a:0e:6f                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-0', u'storage-1'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 18.10                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2018-12-20T14:11:18.060933+00:00                              |
| uptime              | 79104                                                         |
| uuid                | 84816687-3c13-4620-9d03-1266ff0c2825                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+

| mgmt_ip             | 192.168.204.101                                               |
| mgmt_mac            | 52:54:00:3a:0e:6f                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-0', u'storage-1'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 18.10                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2018-12-20T14:11:18.060933+00:00                              |
| uptime              | 79104                                                         |
| uuid                | 84816687-3c13-4620-9d03-1266ff0c2825                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

## Storage-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-1
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                      |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| e739079a-6fea-4cf3-8c99-ab3d423cb6b3 | /dev/sda  | 2048    | HDD     | 200.0 | 0.0        | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-1.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
| 5b7e3abb-32e2-4195-acf7-b02b2f1466ab | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-2.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-tier-list ceph_cluster
+--------------------------------------+---------+--------+--------------------------------------+
| uuid                                 | name    | status | backend_using                        |
+--------------------------------------+---------+--------+--------------------------------------+
| e69468d8-934f-43df-943d-4ce228111c5e | storage | in-use | bee1e818-4717-459a-987c-9d66d50e2138 |
+--------------------------------------+---------+--------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-1 5b7e3abb-32e2-4195-acf7-b02b2f1466ab
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 1                                                |
| function         | osd                                              |
| journal_location | 2cfe0331-e658-4298-bce1-27cb4bc32ac8             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 2cfe0331-e658-4298-bce1-27cb4bc32ac8             |
| ihost_uuid       | ce3d948a-56de-4f49-bf4e-002ccdf8b532             |
| idisk_uuid       | 5b7e3abb-32e2-4195-acf7-b02b2f1466ab             |
| tier_uuid        | e69468d8-934f-43df-943d-4ce228111c5e             |
| tier_name        | storage                                          |
| created_at       | 2018-12-20T14:12:37.179923+00:00                 |
| updated_at       | 2018-12-20T14:12:51.103502+00:00                 |
+------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-1
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | capabilities | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 2cfe0331-e658-4298-bce1-27cb4bc32ac8 | osd      | 1     | {}           | 5b7e3abb-32e2-4195-acf7-b02b2f1466ab | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+--------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-1
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {}                                                            |
| config_applied      | None                                                          |
| config_status       | None                                                          |
| config_target       | None                                                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2018-12-19T16:03:24.951145+00:00                              |
| hostname            | storage-1                                                     |
| id                  | 6                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.108                                               |
| mgmt_mac            | 52:54:00:8c:ec:e8                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-0', u'storage-1'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 18.10                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2018-12-20T14:13:18.093001+00:00                              |
| uptime              | 79038                                                         |
| uuid                | ce3d948a-56de-4f49-bf4e-002ccdf8b532                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

# Compute Host Provision

## Compute-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list compute-0
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| uuid                                 | name    | type     | pci address  | device | processor | accelerated | device type                       |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| da80542a-55f8-47ba-abb9-f1f9d7781cc0 | enp2s1  | ethernet | 0000:02:01.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| 7ac23c18-51dc-4e00-bb60-f4fee938dfaf | enp2s2  | ethernet | 0000:02:02.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| 3d3e01f1-4782-4586-8cb1-d6d19ffc3aad | eth1000 | ethernet | 0000:02:03.0 | 0      | -1        | True        | Virtio network device             |
| 78d37173-a86d-47f3-b04a-de1da11cacaf | eth1001 | ethernet | 0000:02:04.0 | 0      | -1        | True        | Virtio network device             |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -p providernet-a -c data compute-0 eth1000
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:ec:e5:dc                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | b6528412-bba2-4b20-adad-c0895446c9a8 |
| ihost_uuid       | b35edc7d-9268-4514-ac66-f3c3d8571826 |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2018-12-19T16:07:13.663986+00:00     |
| updated_at       | 2018-12-20T09:14:10.039961+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify compute-0 -f vswitch -p0 1
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| uuid                                 | log_c | processor | phy_c | thread | processor_model                           | assigned_function |
|                                      | ore   |           | ore   |        |                                           |                   |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| 697d3101-2e6e-4f58-8c22-c30cc004c86b | 0     | 0         | 0     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | Platform          |
| 0897c0ab-1961-4a62-a7c8-e6b516d5221d | 1     | 0         | 1     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | vSwitch           |
| d05f823e-4fe5-4ff1-9df3-ff8f87134867 | 2     | 0         | 2     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | VMs               |
| 029e6d39-72a9-4a6b-8009-c5badff4596d | 3     | 0         | 3     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | VMs               |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list compute-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                      |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| 559e6ad1-776f-4ef8-a741-1bdd5834d539 | /dev/sda  | 2048    | HDD     | 200.0 | 172.164    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-1.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
| d2fae63d-dc84-4564-a8b4-c8c6282e040f | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-2.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add compute-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | fe8e62c2-daec-4ae2-aaa9-bf7840f27ca7                              |
| ihost_uuid            | b35edc7d-9268-4514-ac66-f3c3d8571826                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2018-12-20T09:15:07.308458+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add compute-0 nova-local d2fae63d-dc84-4564-a8b4-c8c6282e040f
+--------------------------+--------------------------------------------+
| Property                 | Value                                      |
+--------------------------+--------------------------------------------+
| uuid                     | dabf783b-db99-488e-91fb-4faf3ba2abd2       |
| pv_state                 | adding                                     |
| pv_type                  | disk                                       |
| disk_or_part_uuid        | d2fae63d-dc84-4564-a8b4-c8c6282e040f       |
| disk_or_part_device_node | /dev/sdb                                   |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0 |
| lvm_pv_name              | /dev/sdb                                   |
| lvm_vg_name              | nova-local                                 |
| lvm_pv_uuid              | None                                       |
| lvm_pv_size_gib          | 0.0                                        |
| lvm_pe_total             | 0                                          |
| lvm_pe_alloced           | 0                                          |
| ihost_uuid               | b35edc7d-9268-4514-ac66-f3c3d8571826       |
| created_at               | 2018-12-20T09:15:29.604701+00:00           |
| updated_at               | None                                       |
+--------------------------+--------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-modify -b remote compute-0 nova-local
+-----------------------+--------------------------------------------------------------------+
| Property              | Value                                                              |
+-----------------------+--------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                         |
| vg_state              | adding                                                             |
| uuid                  | fe8e62c2-daec-4ae2-aaa9-bf7840f27ca7                               |
| ihost_uuid            | b35edc7d-9268-4514-ac66-f3c3d8571826                               |
| lvm_vg_access         | None                                                               |
| lvm_max_lv            | 0                                                                  |
| lvm_cur_lv            | 0                                                                  |
| lvm_max_pv            | 0                                                                  |
| lvm_cur_pv            | 0                                                                  |
| lvm_vg_size_gib       | 0.0                                                                |
| lvm_vg_avail_size_gib | 0.0                                                                |
| lvm_vg_total_pe       | 0                                                                  |
| lvm_vg_free_pe        | 0                                                                  |
| created_at            | 2018-12-20T09:15:07.308458+00:00                                   |
| updated_at            | None                                                               |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'remote'} |
+-----------------------+--------------------------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock compute-0
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
| config_applied      | 1afc406c-85ba-4024-8d40-d14b290e2572 |
| config_status       | Config out-of-date                   |
| config_target       | 041e05f1-b477-4b9e-85d7-6a276f98f661 |
| console             | ttyS0,115200                         |
| created_at          | 2018-12-19T15:56:36.776266+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.104                      |
| mgmt_mac            | 52:54:00:da:c7:20                    |
| operational         | disabled                             |
| personality         | compute                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2018-12-20T14:17:18.700431+00:00     |
| uptime              | 79783                                |
| uuid                | b35edc7d-9268-4514-ac66-f3c3d8571826 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

# Compute-1

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list compute-1
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| uuid                                 | name    | type     | pci address  | device | processor | accelerated | device type                       |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
| 4c21a98c-d508-4232-a5c6-8409e101f169 | enp2s1  | ethernet | 0000:02:01.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| 46c83d92-8e1c-4f5b-82cb-c9fea4ae322b | enp2s2  | ethernet | 0000:02:02.0 | 0      | -1        | False       | 82540EM Gigabit Ethernet          |
|                                      |         |          |              |        |           |             | Controller                        |
|                                      |         |          |              |        |           |             |                                   |
| de31f0ca-6dd6-4885-8255-1eacf9077a7f | eth1000 | ethernet | 0000:02:03.0 | 0      | -1        | True        | Virtio network device             |
| 8378963f-a7e4-40ce-a000-f78876f08d20 | eth1001 | ethernet | 0000:02:04.0 | 0      | -1        | True        | Virtio network device             |
+--------------------------------------+---------+----------+--------------+--------+-----------+-------------+-----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -p providernet-a -c data compute-1 eth1000
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:d5:5e:e1                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | 915d993b-2936-41fd-a473-dbf1bb5e3264 |
| ihost_uuid       | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4 |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2018-12-19T16:09:25.949787+00:00     |
| updated_at       | 2018-12-20T14:18:52.915697+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify compute-1 -f vswitch -p0 1
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| uuid                                 | log_c | processor | phy_c | thread | processor_model                           | assigned_function |
|                                      | ore   |           | ore   |        |                                           |                   |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
| b951c942-f869-4c56-be27-bd23f90f4bb1 | 0     | 0         | 0     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | Platform          |
| 523802c9-f887-421f-8ac8-c12984adc98a | 1     | 0         | 1     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | vSwitch           |
| cacb3a73-3fec-415f-a801-035b6007e4c5 | 2     | 0         | 2     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | VMs               |
| a13d7bed-74c9-4b76-b770-817103a9a990 | 3     | 0         | 3     | 0      | Intel Core i7 9xx (Nehalem Class Core i7) | VMs               |
+--------------------------------------+-------+-----------+-------+--------+-------------------------------------------+-------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list compute-1
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                      |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
| 2e105af1-9976-4020-b82f-ba8e01d812ba | /dev/sda  | 2048    | HDD     | 200.0 | 172.164    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-1.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
| bdac78fe-3a5a-4f6d-b04e-1b92a3c6a443 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f |
|                                      |           |         |         |       |            |              |         | .2-ata-2.0                       |
|                                      |           |         |         |       |            |              |         |                                  |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+----------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add compute-1 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 3e3a9b92-0862-4d83-b7a1-479aa79c67e0                              |
| ihost_uuid            | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2018-12-20T14:19:48.396037+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add compute-1 nova-local bdac78fe-3a5a-4f6d-b04e-1b92a3c6a443
+--------------------------+--------------------------------------------+
| Property                 | Value                                      |
+--------------------------+--------------------------------------------+
| uuid                     | b30630e0-08fb-4b5f-8a3c-f9fc1f921e3d       |
| pv_state                 | adding                                     |
| pv_type                  | disk                                       |
| disk_or_part_uuid        | bdac78fe-3a5a-4f6d-b04e-1b92a3c6a443       |
| disk_or_part_device_node | /dev/sdb                                   |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0 |
| lvm_pv_name              | /dev/sdb                                   |
| lvm_vg_name              | nova-local                                 |
| lvm_pv_uuid              | None                                       |
| lvm_pv_size_gib          | 0.0                                        |
| lvm_pe_total             | 0                                          |
| lvm_pe_alloced           | 0                                          |
| ihost_uuid               | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4       |
| created_at               | 2018-12-20T14:20:15.219772+00:00           |
| updated_at               | None                                       |
+--------------------------+--------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-modify -b remote compute-1 nova-local
+-----------------------+--------------------------------------------------------------------+
| Property              | Value                                                              |
+-----------------------+--------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                         |
| vg_state              | adding                                                             |
| uuid                  | 3e3a9b92-0862-4d83-b7a1-479aa79c67e0                               |
| ihost_uuid            | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4                               |
| lvm_vg_access         | None                                                               |
| lvm_max_lv            | 0                                                                  |
| lvm_cur_lv            | 0                                                                  |
| lvm_max_pv            | 0                                                                  |
| lvm_cur_pv            | 0                                                                  |
| lvm_vg_size_gib       | 0.0                                                                |
| lvm_vg_avail_size_gib | 0.0                                                                |
| lvm_vg_total_pe       | 0                                                                  |
| lvm_vg_free_pe        | 0                                                                  |
| created_at            | 2018-12-20T14:19:48.396037+00:00                                   |
| updated_at            | None                                                               |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'remote'} |
+-----------------------+--------------------------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock compute-1
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
| config_applied      | 74220fd3-1667-4f81-993c-8348b5e910ec |
| config_status       | None                                 |
| config_target       | 74220fd3-1667-4f81-993c-8348b5e910ec |
| console             | ttyS0,115200                         |
| created_at          | 2018-12-19T15:59:16.815496+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.86                       |
| mgmt_mac            | 52:54:00:a7:a5:b8                    |
| operational         | disabled                             |
| personality         | compute                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 18.10                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2018-12-20T14:20:18.333793+00:00     |
| uptime              | 79659                                |
| uuid                | 59da4e4d-ea9a-4bef-bea5-886329b8ceb4 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

# System Health Check

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | compute-0    | compute     | unlocked       | enabled     | available    |
| 4  | compute-1    | compute     | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
    cluster 79280ca9-89b8-4083-a602-a79e148f55f8
     health HEALTH_OK
     monmap e1: 3 mons at {controller-0=192.168.204.3:6789/0,controller-1=192.168.204.4:6789/0,storage-0=192.168.204.101:6789/0}
            election epoch 10, quorum 0,1,2 controller-0,controller-1,storage-0
     osdmap e83: 2 osds: 2 up, 2 in
            flags sortbitwise,require_jewel_osds
      pgmap v128: 2048 pgs, 11 pools, 1588 bytes data, 1116 objects
            95092 kB used, 397 GB / 397 GB avail
                2048 active+clean
```
