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
