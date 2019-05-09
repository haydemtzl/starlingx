# Provisioning controller-0

## Set the ntp server

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

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

Documentation! Only in Bare Metal

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

Once all nodes have been installed and rebooted:

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

Once rebooted

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

CEPH with non ok status, due to missing Ceph monitor

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     d2143f4f-b092-4ba7-8d91-81c942a4c6ee
    health: HEALTH_WARN
            Reduced data availability: 64 pgs inactive
 
  services:
    mon: 2 daemons, quorum controller-0,controller-1
    mgr: controller-0(active), standbys: controller-1
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   1 pools, 64 pgs
    objects: 0  objects, 0 B
    usage:   0 B used, 0 B / 0 B avail
    pgs:     100.000% pgs unknown
             64 unknown
```

# Provisioning computes

## Add the third Ceph monitor to a compute node (Standard Only)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ceph-mon-add compute-0
+--------------+------------------------------------------------------------------+
| Property     | Value                                                            |
+--------------+------------------------------------------------------------------+
| uuid         | 5810c712-555a-490d-9b03-d0840a92e236                             |
| ceph_mon_gib | 20                                                               |
| created_at   | 2019-05-09T10:06:04.494153+00:00                                 |
| updated_at   | None                                                             |
| state        | configuring                                                      |
| task         | {u'controller-1': 'configuring', u'controller-0': 'configuring'} |
+--------------+------------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ceph-mon-list
+--------------------------------------+-------+--------------+------------+------+
| uuid                                 | ceph_ | hostname     | state      | task |
|                                      | mon_g |              |            |      |
|                                      | ib    |              |            |      |
+--------------------------------------+-------+--------------+------------+------+
| 0320914f-195f-4776-a51c-4570dfa0791d | 20    | controller-1 | configured | None |
| 2bbedca6-859b-4291-a425-05901aac51f1 | 20    | controller-0 | configured | None |
| 5810c712-555a-490d-9b03-d0840a92e236 | 20    | compute-0    | configured | None |
+--------------------------------------+-------+--------------+------------+------+
```

# Create the volume group for nova

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   echo "Configuring nova local for: $COMPUTE"
>   set -ex
>   ROOT_DISK=$(system host-show ${COMPUTE} | grep rootfs | awk '{print $4}')
>   ROOT_DISK_UUID=$(system host-disk-list ${COMPUTE} --nowrap | awk /${ROOT_DISK}/'{print $2}')
>   PARTITION_SIZE=10
>   NOVA_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${PARTITION_SIZE})
>   NOVA_PARTITION_UUID=$(echo ${NOVA_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
>   system host-lvg-add ${COMPUTE} nova-local
>   system host-pv-add ${COMPUTE} nova-local ${NOVA_PARTITION_UUID}
>   system host-lvg-modify -b image ${COMPUTE} nova-local
>   set +ex
> done
Configuring nova local for: compute-0
++ system host-show compute-0
++ grep --color=auto rootfs
++ awk '{print $4}'
+ ROOT_DISK=sda
++ system host-disk-list compute-0 --nowrap
++ awk '/sda/{print $2}'
+ ROOT_DISK_UUID=63941904-3a7d-406d-aead-9cfb474a3c61
+ PARTITION_SIZE=10
++ system host-disk-partition-add -t lvm_phys_vol compute-0 63941904-3a7d-406d-aead-9cfb474a3c61 10
+ NOVA_PARTITION='+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| device_node | /dev/sda5                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 10240                                            |
| uuid        | c0cb34c6-7b66-4fcb-b22d-becc8efefbc4             |
| ihost_uuid  | f4675450-cf5e-4797-a8c9-3238ed5a1da9             |
| idisk_uuid  | 63941904-3a7d-406d-aead-9cfb474a3c61             |
| ipv_uuid    | None                                             |
| status      | Creating (on unlock)                             |
| created_at  | 2019-05-09T10:07:19.280703+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+'
++ grep --color=auto -ow '| uuid | [a-z0-9\-]* |'
++ awk '{print $4}'
++ echo +-------------+--------------------------------------------------+ '|' Property '|' Value '|' +-------------+--------------------------------------------------+ '|' device_path '|' /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 '|' '|' device_node '|' /dev/sda5 '|' '|' type_guid '|' ba5eba11-0000-1111-2222-000000000001 '|' '|' type_name '|' None '|' '|' start_mib '|' None '|' '|' end_mib '|' None '|' '|' size_mib '|' 10240 '|' '|' uuid '|' c0cb34c6-7b66-4fcb-b22d-becc8efefbc4 '|' '|' ihost_uuid '|' f4675450-cf5e-4797-a8c9-3238ed5a1da9 '|' '|' idisk_uuid '|' 63941904-3a7d-406d-aead-9cfb474a3c61 '|' '|' ipv_uuid '|' None '|' '|' status '|' Creating '(on' 'unlock)' '|' '|' created_at '|' 2019-05-09T10:07:19.280703+00:00 '|' '|' updated_at '|' None '|' +-------------+--------------------------------------------------+
+ NOVA_PARTITION_UUID=c0cb34c6-7b66-4fcb-b22d-becc8efefbc4
+ system host-lvg-add compute-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 5f1fa392-ac46-40f9-a186-fd44ca1d0f99                              |
| ihost_uuid            | f4675450-cf5e-4797-a8c9-3238ed5a1da9                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-09T10:07:20.641046+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+ system host-pv-add compute-0 nova-local c0cb34c6-7b66-4fcb-b22d-becc8efefbc4
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | d7d7268b-9ad5-4a62-96d5-071f7fe13898             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | c0cb34c6-7b66-4fcb-b22d-becc8efefbc4             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | f4675450-cf5e-4797-a8c9-3238ed5a1da9             |
| created_at               | 2019-05-09T10:07:21.867072+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+ system host-lvg-modify -b image compute-0 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 5f1fa392-ac46-40f9-a186-fd44ca1d0f99                              |
| ihost_uuid            | f4675450-cf5e-4797-a8c9-3238ed5a1da9                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-09T10:07:20.641046+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+ set +ex
Configuring nova local for: compute-1
++ system host-show compute-1
++ grep --color=auto rootfs
++ awk '{print $4}'
+ ROOT_DISK=sda
++ system host-disk-list compute-1 --nowrap
++ awk '/sda/{print $2}'
+ ROOT_DISK_UUID=10d0f01e-0b46-425f-b89a-ecb98e99a98a
+ PARTITION_SIZE=10
++ system host-disk-partition-add -t lvm_phys_vol compute-1 10d0f01e-0b46-425f-b89a-ecb98e99a98a 10
+ NOVA_PARTITION='+-------------+--------------------------------------------------+
| Property    | Value                                            |
+-------------+--------------------------------------------------+
| device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| device_node | /dev/sda5                                        |
| type_guid   | ba5eba11-0000-1111-2222-000000000001             |
| type_name   | None                                             |
| start_mib   | None                                             |
| end_mib     | None                                             |
| size_mib    | 10240                                            |
| uuid        | 9cf5de98-d3a4-46f1-84c6-66a4a83c77a1             |
| ihost_uuid  | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3             |
| idisk_uuid  | 10d0f01e-0b46-425f-b89a-ecb98e99a98a             |
| ipv_uuid    | None                                             |
| status      | Creating (on unlock)                             |
| created_at  | 2019-05-09T10:07:26.717719+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+'
++ grep --color=auto -ow '| uuid | [a-z0-9\-]* |'
++ awk '{print $4}'
++ echo +-------------+--------------------------------------------------+ '|' Property '|' Value '|' +-------------+--------------------------------------------------+ '|' device_path '|' /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 '|' '|' device_node '|' /dev/sda5 '|' '|' type_guid '|' ba5eba11-0000-1111-2222-000000000001 '|' '|' type_name '|' None '|' '|' start_mib '|' None '|' '|' end_mib '|' None '|' '|' size_mib '|' 10240 '|' '|' uuid '|' 9cf5de98-d3a4-46f1-84c6-66a4a83c77a1 '|' '|' ihost_uuid '|' 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 '|' '|' idisk_uuid '|' 10d0f01e-0b46-425f-b89a-ecb98e99a98a '|' '|' ipv_uuid '|' None '|' '|' status '|' Creating '(on' 'unlock)' '|' '|' created_at '|' 2019-05-09T10:07:26.717719+00:00 '|' '|' updated_at '|' None '|' +-------------+--------------------------------------------------+
+ NOVA_PARTITION_UUID=9cf5de98-d3a4-46f1-84c6-66a4a83c77a1
+ system host-lvg-add compute-1 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | f2786398-480d-4d0e-88a2-6057b3ddf3f2                              |
| ihost_uuid            | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-09T10:07:27.978481+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+ system host-pv-add compute-1 nova-local 9cf5de98-d3a4-46f1-84c6-66a4a83c77a1
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 7cf0b6f3-ade0-4cbd-9e2f-733d97b80386             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 9cf5de98-d3a4-46f1-84c6-66a4a83c77a1             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3             |
| created_at               | 2019-05-09T10:07:29.407066+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+ system host-lvg-modify -b image compute-1 nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | f2786398-480d-4d0e-88a2-6057b3ddf3f2                              |
| ihost_uuid            | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-09T10:07:27.978481+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+ set +ex
```

# Configure data interfaces for computes

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0IF=eth1000
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1IF=eth1001
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET0='physnet0'
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET1='physnet1'
[wrsroot@controller-0 ~(keystone_admin)]$ SPL=/tmp/tmp-system-port-list
[wrsroot@controller-0 ~(keystone_admin)]$ SPIL=/tmp/tmp-system-host-if-list
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET0} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 1                                    |
| uuid         | 110cd57b-6da7-4356-9e9f-651b0a86b56a |
| name         | physnet0                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET1} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 2                                    |
| uuid         | d45f30bf-40da-483c-aeac-5d1386ad3063 |
| name         | physnet1                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   echo "Configuring interface for: $COMPUTE"
>   set -ex
>   system host-port-list ${COMPUTE} --nowrap > ${SPL}
>   system host-if-list -a ${COMPUTE} --nowrap > ${SPIL}
>   DATA0PCIADDR=$(cat $SPL | grep $DATA0IF |awk '{print $8}')
>   DATA1PCIADDR=$(cat $SPL | grep $DATA1IF |awk '{print $8}')
>   DATA0PORTUUID=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $2}')
>   DATA1PORTUUID=$(cat $SPL | grep ${DATA1PCIADDR} | awk '{print $2}')
>   DATA0PORTNAME=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $4}')
>   DATA1PORTNAME=$(cat  $SPL | grep ${DATA1PCIADDR} | awk '{print $4}')
>   DATA0IFUUID=$(cat $SPIL | awk -v DATA0PORTNAME=$DATA0PORTNAME '($12 ~ DATA0PORTNAME) {print $2}')
>   DATA1IFUUID=$(cat $SPIL | awk -v DATA1PORTNAME=$DATA1PORTNAME '($12 ~ DATA1PORTNAME) {print $2}')
>   system host-if-modify -m 1500 -n data0 -d ${PHYSNET0} -c data ${COMPUTE} ${DATA0IFUUID}
>   system host-if-modify -m 1500 -n data1 -d ${PHYSNET1} -c data ${COMPUTE} ${DATA1IFUUID}
>   set +ex
> done
Configuring interface for: compute-0
+ system host-port-list compute-0 --nowrap
+ system host-if-list -a compute-0 --nowrap
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1000
++ awk '{print $8}'
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $2}'
+ DATA0PORTUUID=adecc221-3922-45e8-b97a-d31f5ef47f2e
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'
+ DATA1PORTUUID=3b3a67b7-1a2a-4b2e-8167-1bf5d8a295ac
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'
+ DATA0IFUUID=b0e22fe3-92c4-4546-9eb9-288745fe446c
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
+ DATA1IFUUID=7d2eb5e7-d215-4174-9828-ffb9c654680a
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-0 b0e22fe3-92c4-4546-9eb9-288745fe446c
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:36:f5:16                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | b0e22fe3-92c4-4546-9eb9-288745fe446c |
| ihost_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:17.631463+00:00     |
| updated_at   | 2019-05-09T10:08:46.528133+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-0 7d2eb5e7-d215-4174-9828-ffb9c654680a
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:4b:54:59                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 7d2eb5e7-d215-4174-9828-ffb9c654680a |
| ihost_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:17.762022+00:00     |
| updated_at   | 2019-05-09T10:08:48.917231+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
Configuring interface for: compute-1
+ system host-port-list compute-1 --nowrap
+ system host-if-list -a compute-1 --nowrap
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1000
++ awk '{print $8}'
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ cat /tmp/tmp-system-port-list
++ awk '{print $2}'
++ grep --color=auto 0000:02:03.0
+ DATA0PORTUUID=17c1a546-dd82-4e0e-8db2-9b574092199a
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'
+ DATA1PORTUUID=fe8e14da-20a8-4ba5-bf8f-23acba767cf6
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'
+ DATA0IFUUID=d15265ea-2293-4b71-9e57-cca8afd21128
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
+ DATA1IFUUID=ef4f6a0b-a739-4d51-b6f1-5c07f0e2b9c3
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-1 d15265ea-2293-4b71-9e57-cca8afd21128
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:15:dc:4a                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | d15265ea-2293-4b71-9e57-cca8afd21128 |
| ihost_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:44.070835+00:00     |
| updated_at   | 2019-05-09T10:08:53.643109+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-1 ef4f6a0b-a739-4d51-b6f1-5c07f0e2b9c3
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:73:c1:cd                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | ef4f6a0b-a739-4d51-b6f1-5c07f0e2b9c3 |
| ihost_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:44.192263+00:00     |
| updated_at   | 2019-05-09T10:08:55.861330+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
```

# Setup the cluster-host interfaces on the computes to the management network (enp0s8)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>    system host-if-modify $COMPUTE mgmt0 --networks cluster-host
> done
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:6a:8c:f7                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 4d8b713e-9035-48b5-9907-9820fbbf1841 |
| ihost_uuid   | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:17.353137+00:00     |
| updated_at   | 2019-05-09T10:09:39.981778+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:77:42:81                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 0587198d-f5c8-4c5c-bddd-73f76ca5bd1e |
| ihost_uuid   | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-09T08:47:43.891565+00:00     |
| updated_at   | 2019-05-09T10:09:42.555785+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

# Unlock compute nodes

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>    system host-unlock $COMPUTE
> done
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
| config_applied      | 54ed0e3f-ae77-44a1-8b07-ecacce217add |
| config_status       | Config out-of-date                   |
| config_target       | 13599fe0-f486-4927-9551-0048df787956 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-09T08:35:40.579739+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.186                      |
| mgmt_mac            | 52:54:00:6a:8c:f7                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-09T10:11:44.873146+00:00     |
| uptime              | 5117                                 |
| uuid                | f4675450-cf5e-4797-a8c9-3238ed5a1da9 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
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
| config_applied      | 649c9d18-d854-44a5-bfa5-c99724b3ccd0 |
| config_status       | Config out-of-date                   |
| config_target       | 13599fe0-f486-4927-9551-0048df787956 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-09T08:35:44.749879+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.252                      |
| mgmt_mac            | 52:54:00:77:42:81                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-09T10:11:49.753304+00:00     |
| uptime              | 5083                                 |
| uuid                | 6ff0a563-422e-4bdd-8b89-9386cedc3fa3 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Once rebooted... after some time

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | compute-0    | worker      | unlocked       | enabled     | degraded     |
| 4  | compute-1    | worker      | unlocked       | enabled     | degraded     |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     d2143f4f-b092-4ba7-8d91-81c942a4c6ee
    health: HEALTH_WARN
            Reduced data availability: 64 pgs inactive
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,compute-0
    mgr: controller-0(active), standbys: controller-1
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   1 pools, 64 pgs
    objects: 0  objects, 0 B
    usage:   0 B used, 0 B / 0 B avail
    pgs:     100.000% pgs unknown
             64 unknown
```

# Add Ceph OSDs to controllers

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ HOST=controller-0
[wrsroot@controller-0 ~(keystone_admin)]$ DISKS=$(system host-disk-list ${HOST})
[wrsroot@controller-0 ~(keystone_admin)]$ TIERS=$(system storage-tier-list ceph_cluster)
[wrsroot@controller-0 ~(keystone_admin)]$ OSDs="/dev/sdb"
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for OSD in $OSDs; do
>     system host-stor-add ${HOST} $(echo "$DISKS" | grep "$OSD" | awk '{print $2}') --tier-uuid $(echo "$TIERS" | grep storage | awk '{print $2}')
>     while true; do system host-stor-list ${HOST} | grep ${OSD} | grep configuring; if [ $? -ne 0 ]; then break; fi; sleep 1; done
> done
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| state            | configuring                                      |
| journal_location | 15db1dc9-5ce7-40db-915e-8375c7b13317             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 15db1dc9-5ce7-40db-915e-8375c7b13317             |
| ihost_uuid       | dd87579c-9945-4ae1-8de0-ce8cb3622eeb             |
| idisk_uuid       | d425149a-c53a-4251-92ff-f2389ef033d8             |
| tier_uuid        | afa14fa8-7105-4793-979b-2a70b33c9f2a             |
| tier_name        | storage                                          |
| created_at       | 2019-05-09T10:45:20.234727+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
| 15db1dc9-5ce7-40db-915e-8375c7b13317 | osd      | 0     | configuring | d425149a-c53a-4251-92ff-f2389ef033d8 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
...
...
| 15db1dc9-5ce7-40db-915e-8375c7b13317 | osd      | 0     | configuring | d425149a-c53a-4251-92ff-f2389ef033d8 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list $HOST
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 15db1dc9-5ce7-40db-915e-8375c7b13317 | osd      | 0     | configured | d425149a-c53a-4251-92ff-f2389ef033d8 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ HOST=controller-1
[wrsroot@controller-0 ~(keystone_admin)]$ DISKS=$(system host-disk-list ${HOST})
[wrsroot@controller-0 ~(keystone_admin)]$ TIERS=$(system storage-tier-list ceph_cluster)
[wrsroot@controller-0 ~(keystone_admin)]$ OSDs="/dev/sdb"
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for OSD in $OSDs; do
>     system host-stor-add ${HOST} $(echo "$DISKS" | grep "$OSD" | awk '{print $2}') --tier-uuid $(echo "$TIERS" | grep storage | awk '{print $2}')
>     while true; do system host-stor-list ${HOST} | grep ${OSD} | grep configuring; if [ $? -ne 0 ]; then break; fi; sleep 1; done
> done
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 1                                                |
| function         | osd                                              |
| state            | configuring                                      |
| journal_location | 4fea2e08-c416-44b3-a50a-a8de4e81bedb             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 4fea2e08-c416-44b3-a50a-a8de4e81bedb             |
| ihost_uuid       | ab52f735-0833-457c-a94f-777c12d103d9             |
| idisk_uuid       | 0093e9c8-a9b9-446c-9e9e-5110fd6c2fad             |
| tier_uuid        | afa14fa8-7105-4793-979b-2a70b33c9f2a             |
| tier_name        | storage                                          |
| created_at       | 2019-05-09T10:47:18.255989+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
| 4fea2e08-c416-44b3-a50a-a8de4e81bedb | osd      | 1     | configuring | 0093e9c8-a9b9-446c-9e9e-5110fd6c2fad | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
...
...
| 4fea2e08-c416-44b3-a50a-a8de4e81bedb | osd      | 1     | configuring | 0093e9c8-a9b9-446c-9e9e-5110fd6c2fad | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list $HOST
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 4fea2e08-c416-44b3-a50a-a8de4e81bedb | osd      | 1     | configured | 0093e9c8-a9b9-446c-9e9e-5110fd6c2fad | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     d2143f4f-b092-4ba7-8d91-81c942a4c6ee
    health: HEALTH_WARN
            Degraded data redundancy: 233/2264 objects degraded (10.292%), 13 pgs degraded
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,compute-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   224 MiB used, 398 GiB / 398 GiB avail
    pgs:     233/2264 objects degraded (10.292%)
             242 active+clean
             12  active+recovery_wait+degraded
             1   active+recovery_wait
             1   active+recovering+degraded
 
  io:
    recovery: 0 B/s, 17 objects/s
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd tree
ID CLASS WEIGHT  TYPE NAME                 STATUS REWEIGHT PRI-AFF 
-1       0.38840 root storage-tier                                 
-2       0.38840     chassis group-0                               
-4       0.19420         host controller-0                         
 0   hdd 0.19420             osd.0             up  1.00000 1.00000 
-3       0.19420         host controller-1                         
 1   hdd 0.19420             osd.1             up  1.00000 1.00000 
```

After some time...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     d2143f4f-b092-4ba7-8d91-81c942a4c6ee
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,compute-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   226 MiB used, 398 GiB / 398 GiB avail
    pgs:     256 active+clean
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd tree
ID CLASS WEIGHT  TYPE NAME                 STATUS REWEIGHT PRI-AFF 
-1       0.38840 root storage-tier                                 
-2       0.38840     chassis group-0                               
-4       0.19420         host controller-0                         
 0   hdd 0.19420             osd.0             up  1.00000 1.00000 
-3       0.19420         host controller-1                         
 1   hdd 0.19420             osd.1             up  1.00000 1.00000 
```

# Using sysinv to bring up/down the containerized services

```sh
user@workstation:~/stx-tools/deployment/libvirt$ scp stx-openstack-1.0-11-centos-stable-latest.tgz wrsroot@10.10.10.3:~/
wrsroot@10.10.10.3's password: 
stx-openstack-1.0-11-centos-stable-latest.tgz                                                                                                               100% 1120KB   1.1MB/s   00:00    
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ls
stx-openstack-1.0-11-centos-stable-latest.tgz
```

## Stage application for deployment

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-upload stx-openstack-1.0-11-centos-stable-latest.tgz
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| app_version   | 1.0-11-centos-stable-latest      |
| created_at    | 2019-05-09T10:51:42.591439+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | uploading                        |
| updated_at    | None                             |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show stx-openstack' to view the current progress.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------+-----------------------------+-----------------+---------------+-----------+---------------------------------+
| application   | version                     | manifest name   | manifest file | status    | progress                        |
+---------------+-----------------------------+-----------------+---------------+-----------+---------------------------------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | uploading | validating and uploading charts |
+---------------+-----------------------------+-----------------+---------------+-----------+---------------------------------+
```

After some time...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------+-----------------------------+-----------------+---------------+----------+-----------+
| application   | version                     | manifest name   | manifest file | status   | progress  |
+---------------+-----------------------------+-----------------+---------------+----------+-----------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | uploaded | completed |
+---------------+-----------------------------+-----------------+---------------+----------+-----------+
```

## Bring Up Services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-apply stx-openstack
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| app_version   | 1.0-11-centos-stable-latest      |
| created_at    | 2019-05-09T10:51:42.591439+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | applying                         |
| updated_at    | 2019-05-09T10:52:02.557043+00:00 |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show stx-openstack' to view the current progress.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------+-----------------------------+-----------------+---------------+----------+--------------------------+
| application   | version                     | manifest name   | manifest file | status   | progress                 |
+---------------+-----------------------------+-----------------+---------------+----------+--------------------------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | applying | retrieving docker images |
+---------------+-----------------------------+-----------------+---------------+----------+--------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ watch -n 5 system application-list
```

```sh
Every 5.0s: system application-list                                                                                      Thu May  9 11:39:51 2019

+---------------+---------------------------+-----------------+-----------+----------+-----------------------------------------------------+
| application   | version                   | manifest name   | manifest  | status   | progress                                            |
|               |                           |                 | file      |          |                                                     |
+---------------+---------------------------+-----------------+-----------+----------+-----------------------------------------------------+
| stx-openstack | 1.0-11-centos-stable-     | armada-manifest | manifest. | applying | processing chart: osh-openstack-neutron, overall    |
|               | latest                    |                 | yaml	  |          | completion: 67.0%                                   |
|               |                           |                 |           |          |                                                     |
+---------------+---------------------------+-----------------+-----------+----------+-----------------------------------------------------+
```

```sh
Every 5.0s: system application-list                                                                                      Thu May  9 11:47:21 2019

+---------------+-----------------------------+-----------------+---------------+--------------+------------------------------------------+
| application   | version                     | manifest name   | manifest file | status       | progress                                 |
+---------------+-----------------------------+-----------------+---------------+--------------+------------------------------------------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | apply-failed | operation aborted, check logs for detail |
+---------------+-----------------------------+-----------------+---------------+--------------+------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service tail -f stx-openstack-apply.log
Password: 
2019-05-09 11:45:15.771 42 ERROR armada.cli   File "/usr/lib/python3.6/concurrent/futures/_base.py", line 384, in __get_result
2019-05-09 11:45:15.771 42 ERROR armada.cli     raise self._exception
2019-05-09 11:45:15.771 42 ERROR armada.cli   File "/usr/lib/python3.6/concurrent/futures/thread.py", line 56, in run
2019-05-09 11:45:15.771 42 ERROR armada.cli     result = self.fn(*self.args, **self.kwargs)
2019-05-09 11:45:15.771 42 ERROR armada.cli   File "/usr/local/lib/python3.6/dist-packages/armada/cli/apply.py", line 252, in handle
2019-05-09 11:45:15.771 42 ERROR armada.cli     return armada.sync()
2019-05-09 11:45:15.771 42 ERROR armada.cli   File "/usr/local/lib/python3.6/dist-packages/armada/handlers/armada.py", line 249, in sync
2019-05-09 11:45:15.771 42 ERROR armada.cli     raise armada_exceptions.ChartDeployException(failures)
2019-05-09 11:45:15.771 42 ERROR armada.cli armada.exceptions.armada_exceptions.ChartDeployException: Exception deploying charts: ['openvswitch']
2019-05-09 11:45:15.771 42 ERROR armada.cli 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system modify --vswitch_type none
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
| updated_at           | 2019-05-09T08:15:59.828113+00:00     |
| uuid                 | 57b9f9a3-5840-4a6d-89fe-e4c6aa956b47 |
| vswitch_type         | none                                 |
+----------------------+--------------------------------------+
```

