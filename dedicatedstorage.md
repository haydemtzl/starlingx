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

