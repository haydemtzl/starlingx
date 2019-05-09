# Provisioning controller-0

## Set the ntp server

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

(reverse-i-search)`': ^C
controller-0:~$ source /etc/platform/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ntp-modify ntpservers=0.pool.ntp.org,1.pool.ntp.org
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| uuid         | f220ebcc-874d-4d47-91cd-5c922b83b416 |
| enabled      | True                                 |
| ntpservers   | 0.pool.ntp.org,1.pool.ntp.org        |
| isystem_uuid | 57b9f9a3-5840-4a6d-89fe-e4c6aa956b47 |
| created_at   | 2019-05-09T07:57:52.887066+00:00     |
| updated_at   | None                                 |
+--------------+--------------------------------------+
```

## Configure the vswitch type (optional)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system modify --vswitch_type ovs-dpdk
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| contact              | None                                 |
| created_at           | 2019-05-09T07:57:52.857427+00:00     |
| description          | None                                 |
| https_enabled        | False                                |
| location             | None                                 |
| name                 | 5873e93f-e035-4d22-8c2e-7dacf7c78003 |
| region_name          | RegionOne                            |
| sdn_enabled          | False                                |
| security_feature     | spectre_meltdown_v1                  |
| service_project_name | services                             |
| software_version     | 19.01                                |
| system_mode          | duplex                               |
| system_type          | Standard                             |
| timezone             | UTC                                  |
| updated_at           | 2019-05-09T07:58:41.681534+00:00     |
| uuid                 | 57b9f9a3-5840-4a6d-89fe-e4c6aa956b47 |
| vswitch_type         | ovs-dpdk                             |
+----------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify -f vswitch -p0 1 controller-0
Can only modify worker node cores.
```

## Prepare the host for running the containerized services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 41f98a54-2058-4f2e-ae0b-cb8b303fac9b |
| host_uuid   | dd87579c-9945-4ae1-8de0-ce8cb3622eeb |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

## Unlock controller-0

```sh
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
| config_applied      | 6cb8ff6a-bf51-4998-89ea-f17b4e369b76       |
| config_status       | Config out-of-date                         |
| config_target       | ecb8ff6a-bf51-4998-89ea-f17b4e369b76       |
| console             | tty0                                       |
| created_at          | 2019-05-09T07:58:44.594232+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:ca:3f:93                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-05-09T08:22:55.842887+00:00           |
| uptime              | 8602                                       |
| uuid                | dd87579c-9945-4ae1-8de0-ce8cb3622eeb       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

# Install remaining hosts

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ source /etc/platform/openrc
Openstack Admin credentials can only be loaded from the active controller.
controller-0:~$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

## PXE boot hosts

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

Power-on, the remaining hosts, they should PXEboot from the controller. 

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

controller-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 2 personality=controller
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
| created_at          | 2019-05-09T08:35:05.749607+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:38:9f:39                    |
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
| uuid                | ab52f735-0833-457c-a94f-777c12d103d9 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

compute-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 3 personality=worker hostname=compute-0
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
| created_at          | 2019-05-09T08:35:40.579739+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.186                      |
| mgmt_mac            | 52:54:00:6a:8c:f7                    |
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
| uuid                | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

compute-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 4 personality=worker hostname=compute-1
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
| created_at          | 2019-05-09T08:35:44.749879+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.252                      |
| mgmt_mac            | 52:54:00:77:42:81                    |
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
| uuid                | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

Once all installed and rebooted:

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | online       |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

# Prepare the remaining hosts for running the containerized services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-1 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | a491e6f7-3755-4434-975f-e2c704d11cf4 |
| host_uuid   | ab52f735-0833-457c-a94f-777c12d103d9 |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ for NODE in compute-0 compute-1; do
>   system host-label-assign $NODE  openstack-compute-node=enabled
>   system host-label-assign $NODE  openvswitch=enabled
>   system host-label-assign $NODE  sriov=enabled
> done
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | e914be43-bccd-4979-80b9-cecaad3a1037 |
| host_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | f7a37ecd-ab8d-4af0-a021-a170f55506c9 |
| host_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 0e5e08d1-5bc1-4d3a-8352-5165881988c3 |
| host_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 3f3728c8-4814-4e71-9481-a3a2a5c9ceca |
| host_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 20b6f029-9dc8-44aa-9610-b632ebd01513 |
| host_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | bc5f9409-4481-4ab1-8faa-079e78815f94 |
| host_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

# Provisioning controller-1

## Add interfaces on Controller-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -n oam0 -c platform --networks oam controller-1 $(system host-if-list -a controller-1 | awk '/enp2s1/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | oam0                                 |
| iftype       | ethernet                             |
| ports        | [u'enp2s1']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:41:8b:f9                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | oam                                  |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 43fb55eb-4003-4c7b-8ff6-909d6829dcdd |
| ihost_uuid   | ab52f735-0833-457c-a94f-777c12d103d9 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:44:56.402178+00:00     |
| updated_at   | 2019-05-09T09:41:25.945307+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-1 mgmt0 --networks cluster-host
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:38:9f:39                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2743ab72-10aa-4c09-bb37-6962a7ea780b |
| ihost_uuid   | ab52f735-0833-457c-a94f-777c12d103d9 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:44:56.602348+00:00     |
| updated_at   | 2019-05-09T09:41:46.334541+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

## Unlock Controller-1

```sh
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
| created_at          | 2019-05-09T08:35:05.749607+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:38:9f:39                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-09T09:41:58.491275+00:00     |
| uptime              | 3438                                 |
| uuid                | ab52f735-0833-457c-a94f-777c12d103d9 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```
