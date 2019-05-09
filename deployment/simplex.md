# Provisioning the platform

## Set the ntp server

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ntp-modify ntpservers=0.pool.ntp.org,1.pool.ntp.org
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| uuid         | 978b9229-2f96-46ef-b6f0-8af592069f39 |
| enabled      | True                                 |
| ntpservers   | 0.pool.ntp.org,1.pool.ntp.org        |
| isystem_uuid | 24d8d673-632c-456d-b932-1945f3b25e4c |
| created_at   | 2019-05-08T16:22:10.970718+00:00     |
| updated_at   | None                                 |
+--------------+--------------------------------------+
```

## Configure data interfaces

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0IF=eth1000
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1IF=eth1001
[wrsroot@controller-0 ~(keystone_admin)]$ export COMPUTE=controller-0
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET0='physnet0'
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET1='physnet1'
[wrsroot@controller-0 ~(keystone_admin)]$ SPL=/tmp/tmp-system-port-list
[wrsroot@controller-0 ~(keystone_admin)]$ SPIL=/tmp/tmp-system-host-if-list
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-port-list ${COMPUTE} --nowrap > ${SPL}
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-list -a ${COMPUTE} --nowrap > ${SPIL}
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0PCIADDR=$(cat $SPL | grep $DATA0IF |awk '{print $8}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1PCIADDR=$(cat $SPL | grep $DATA1IF |awk '{print $8}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0PORTUUID=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $2}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1PORTUUID=$(cat $SPL | grep ${DATA1PCIADDR} | awk '{print $2}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0PORTNAME=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $4}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1PORTNAME=$(cat  $SPL | grep ${DATA1PCIADDR} | awk '{print $4}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0IFUUID=$(cat $SPIL | awk -v DATA0PORTNAME=$DATA0PORTNAME '($12 ~ DATA0PORTNAME) {print $2}')
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1IFUUID=$(cat $SPIL | awk -v DATA1PORTNAME=$DATA1PORTNAME '($12 ~ DATA1PORTNAME) {print $2}')
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET0} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 1                                    |
| uuid         | 72f97333-c418-4a02-87e2-816726589d99 |
| name         | physnet0                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET1} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 2                                    |
| uuid         | 75b8665c-8b3a-4722-b04b-28eca9f43170 |
| name         | physnet1                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -m 1500 -n data0 -d ${PHYSNET0} -c data ${COMPUTE} ${DATA0IFUUID}
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:0c:f0:13                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | b0aba6bb-ce85-4182-9013-4fcc2d50c6ff |
| ihost_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-08T16:23:02.382655+00:00     |
| updated_at   | 2019-05-08T16:40:10.321939+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -m 1500 -n data1 -d ${PHYSNET1} -c data ${COMPUTE} ${DATA1IFUUID}
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:35:a0:fe                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2f90ccdd-2665-4862-ab63-a9e813b634ad |
| ihost_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-08T16:23:02.584817+00:00     |
| updated_at   | 2019-05-08T16:40:31.296334+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
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
| uuid        | a4663f03-2d6c-4b33-9c58-de004d247558 |
| host_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openstack-compute-node=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | b9c3ad6b-dfd3-40a0-8d10-64a865569a70 |
| host_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openvswitch=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 21e0ee31-26fa-4033-9409-2c25816846eb |
| host_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 sriov=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 197d3c75-31cc-49e3-9872-a6863138ec99 |
| host_uuid   | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

## Setup partitions for Controller-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ export COMPUTE=controller-0
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>> Getting root disk info"
>>> Getting root disk info
[wrsroot@controller-0 ~(keystone_admin)]$ ROOT_DISK=$(system host-show ${COMPUTE} | grep rootfs | awk '{print $4}')
[wrsroot@controller-0 ~(keystone_admin)]$ ROOT_DISK_UUID=$(system host-disk-list ${COMPUTE} --nowrap | grep ${ROOT_DISK} | awk '{print $2}')
[wrsroot@controller-0 ~(keystone_admin)]$ echo "Root disk: $ROOT_DISK, UUID: $ROOT_DISK_UUID"
Root disk: /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0, UUID: fe191367-e34b-4d7d-ad83-8a442f5d0a2b
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>>> Configuring nova-local"
>>>> Configuring nova-local
[wrsroot@controller-0 ~(keystone_admin)]$ NOVA_SIZE=24
[wrsroot@controller-0 ~(keystone_admin)]$ NOVA_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${NOVA_SIZE})
[wrsroot@controller-0 ~(keystone_admin)]$ NOVA_PARTITION_UUID=$(echo ${NOVA_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add ${COMPUTE} nova-local
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 86b8fe9e-121e-4d13-a173-2f0dd6d6cfce                              |
| ihost_uuid            | c56bb8fd-b67f-479e-97dc-65d383aaa47e                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-08T16:43:19.315909+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add ${COMPUTE} nova-local ${NOVA_PARTITION_UUID}
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | c4c02cd7-c27b-4959-8a81-668e7d7096ea             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | bc15fecc-f0f7-4c71-90b8-a0bf14d7bd15             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | c56bb8fd-b67f-479e-97dc-65d383aaa47e             |
| created_at               | 2019-05-08T16:43:24.785164+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ sleep 2
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>> Wait for partition $NOVA_PARTITION_UUID to be ready."
>>> Wait for partition bc15fecc-f0f7-4c71-90b8-a0bf14d7bd15 to be ready.
[wrsroot@controller-0 ~(keystone_admin)]$ while true; do system host-disk-partition-list $COMPUTE --nowrap | grep $NOVA_PARTITION_UUID | grep Ready; if [ $? -eq 0 ]; then break; fi; sleep 1; done
| bc15fecc-f0f7-4c71-90b8-a0bf14d7bd15 | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 | /dev/sda5   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 24.0     | Ready  |
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

## Extend cgts-vg (config_controller method only)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>>> Extending cgts-vg"
>>>> Extending cgts-vg
[wrsroot@controller-0 ~(keystone_admin)]$ PARTITION_SIZE=6
[wrsroot@controller-0 ~(keystone_admin)]$ CGTS_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${PARTITION_SIZE})
[wrsroot@controller-0 ~(keystone_admin)]$ CGTS_PARTITION_UUID=$(echo ${CGTS_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>> Wait for partition $CGTS_PARTITION_UUID to be ready"
>>> Wait for partition a240e889-5483-4c94-9781-1509e00ac738 to be ready
[wrsroot@controller-0 ~(keystone_admin)]$ while true; do system host-disk-partition-list $COMPUTE --nowrap | grep $CGTS_PARTITION_UUID | grep Ready; if [ $? -eq 0 ]; then break; fi; sleep 1; done
| a240e889-5483-4c94-9781-1509e00ac738 | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | /dev/sda6   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 6.0      | Ready  |
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add ${COMPUTE} cgts-vg ${CGTS_PARTITION_UUID}
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 42940321-3703-4b2c-a3bb-ed48278d6903             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | a240e889-5483-4c94-9781-1509e00ac738             |
| disk_or_part_device_node | /dev/sda6                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 |
| lvm_pv_name              | /dev/sda6                                        |
| lvm_vg_name              | cgts-vg                                          |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | c56bb8fd-b67f-479e-97dc-65d383aaa47e             |
| created_at               | 2019-05-08T16:47:09.408330+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ sleep 2
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>> Waiting for cgts-vg to be ready"
>>> Waiting for cgts-vg to be ready
[wrsroot@controller-0 ~(keystone_admin)]$ while true; do system host-pv-list ${COMPUTE} | grep cgts-vg | grep adding; if [ $? -ne 0 ]; then break; fi; sleep 1; done
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | adding (on unlock) | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | adding (on unlock) | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | adding (on unlock) | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | adding (on unlock) | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | adding (on unlock) | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-list ${COMPUTE} 
+--------------------------------------+-------------+--------------------------------------+--------------------------+--------------------------------------------------+--------------------+-----------+-------------+--------------------------------------+
| uuid                                 | lvm_pv_name | disk_or_part_uuid                    | disk_or_part_device_node | disk_or_part_device_path                         | pv_state           | pv_type   | lvm_vg_name | ihost_uuid                           |
+--------------------------------------+-------------+--------------------------------------+--------------------------+--------------------------------------------------+--------------------+-----------+-------------+--------------------------------------+
| 42940321-3703-4b2c-a3bb-ed48278d6903 | /dev/sda6   | a240e889-5483-4c94-9781-1509e00ac738 | /dev/sda6                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 | provisioned        | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| c4c02cd7-c27b-4959-8a81-668e7d7096ea | /dev/sda5   | bc15fecc-f0f7-4c71-90b8-a0bf14d7bd15 | /dev/sda5                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 | adding (on unlock) | partition | nova-local  | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| cbddb621-ae1f-4a17-be21-4f730b9648d4 | /dev/sda4   | a0f7a02e-2cf0-43c7-8661-df12d3786e9b | /dev/sda4                | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part4 | provisioned        | partition | cgts-vg     | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
+--------------------------------------+-------------+--------------------------------------+--------------------------+--------------------------------------------------+--------------------+-----------+-------------+--------------------------------------+
```

## Configure Ceph for Controller-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ echo ">>> Add OSDs to primary tier"
>>> Add OSDs to primary tier
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                                |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                                            |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------------------+
| fe191367-e34b-4d7d-ad83-8a442f5d0a2b | /dev/sda  | 2048    | HDD     | 600.0 | 330.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| dbead93e-fe90-4b2b-a3c4-ee631249f289 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0 |
| 6476973d-2e8b-42bf-962a-ba504f95fe4c | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000:00:1f.2-ata-3.0 |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+--------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0 | awk '/\/dev\/sdb/{print $2}' | xargs -i system host-stor-add controller-0 {}
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | 7ee4693b-958b-486b-a38d-fe8cc55d4e6e             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 7ee4693b-958b-486b-a38d-fe8cc55d4e6e             |
| ihost_uuid       | c56bb8fd-b67f-479e-97dc-65d383aaa47e             |
| idisk_uuid       | dbead93e-fe90-4b2b-a3c4-ee631249f289             |
| tier_uuid        | 60458248-233a-4a0e-960f-935d4d955a47             |
| tier_name        | storage                                          |
| created_at       | 2019-05-08T16:49:09.536966+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list controller-0
+--------------------------------------+----------+-------+-----------------------+--------------------------------------+-----------------------------+------------+----------+----------+
| uuid                                 | function | osdid | state                 | idisk_uuid                           | journal_path                | journal_no | journal_ | tier_nam |
|                                      |          |       |                       |                                      |                             | de         | size_gib | e        |
+--------------------------------------+----------+-------+-----------------------+--------------------------------------+-----------------------------+------------+----------+----------+
| 7ee4693b-958b-486b-a38d-fe8cc55d4e6e | osd      | 0     | configuring-on-unlock | dbead93e-fe90-4b2b-a3c4-ee631249f289 | /dev/disk/by-path/pci-0000: | /dev/sdb2  | 1        | storage  |
|                                      |          |       |                       |                                      | 00:1f.2-ata-2.0-part2       |            |          |          |
|                                      |          |       |                       |                                      |                             |            |          |          |
+--------------------------------------+----------+-------+-----------------------+--------------------------------------+-----------------------------+------------+----------+---------
```

## Unlock the controller

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
| config_applied      | 54cdb304-1880-4329-a719-6ecf792f089e       |
| config_status       | Config out-of-date                         |
| config_target       | e11bea52-985e-42c3-91be-cd3d0abb041d       |
| console             | tty0                                       |
| created_at          | 2019-05-08T16:23:00.603259+00:00           |
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
| updated_at          | 2019-05-08T16:49:52.639058+00:00           |
| uptime              | 2121                                       |
| uuid                | c56bb8fd-b67f-479e-97dc-65d383aaa47e       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

## After the host unlocks, test that the ceph cluster is operational 

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool set {} size 1
set pool 1 size to 1
set pool 2 size to 1
set pool 3 size to 1
set pool 4 size to 1
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     1c820e4d-5d2a-42b3-ab19-78e823dde62b
    health: HEALTH_OK
 
  services:
    mon: 1 daemons, quorum controller-0
    mgr: controller-0(active)
    osd: 1 osds: 1 up, 1 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   115 MiB used, 199 GiB / 199 GiB avail
    pgs:     256 active+clean
```

```sh
user@workstation:~/stx-tools/deployment/libvirt$ #wget http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/latest_docker_image_build/outputs/helm-charts/stx-openstack-1.0-11-centos-stable-latest.tgz
user@workstation:~/stx-tools/deployment/libvirt$ #scp stx-openstack-1.0-11-centos-stable-latest.tgz wrsroot@10.10.10.3:/home/wrsroot
```

## Stage application for deployment

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ls
stx-openstack-1.0-11-centos-stable-latest.tgz
````

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-upload stx-openstack-1.0-11-centos-stable-latest.tgz
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| app_version   | 1.0-11-centos-stable-latest      |
| created_at    | 2019-05-08T17:02:48.062194+00:00 |
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

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ watch -n1 system application-list
```

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
| created_at    | 2019-05-08T17:02:48.062194+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | applying                         |
| updated_at    | 2019-05-08T17:03:03.774569+00:00 |
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
Every 5.0s: system application-list                                                                                                                                   Wed May  8 17:31:42 2019

+---------------+-----------------------------+-----------------+---------------+----------+--------------------------------------------------------------------+
| application   | version                     | manifest name   | manifest file | status   | progress                                                           |
+---------------+-----------------------------+-----------------+---------------+----------+--------------------------------------------------------------------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | applying | processing chart: osh-openstack-neutron, overall completion: 63.0% |
+---------------+-----------------------------+-----------------+---------------+----------+--------------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list                                                                                                                             
+---------------+-----------------------------+-----------------+---------------+---------+-----------+
| application   | version                     | manifest name   | manifest file | status  | progress  |
+---------------+-----------------------------+-----------------+---------------+---------+-----------+
| stx-openstack | 1.0-11-centos-stable-latest | armada-manifest | manifest.yaml | applied | completed |
+---------------+-----------------------------+-----------------+---------------+---------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service tail -f stx-openstack-apply.log
Password: 
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-horizon took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-cinder took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-aodh took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-gnocchi took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-panko took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Chart osh-openstack-ceilometer took action: install
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Did not perform chart upgrade(s)
2019-05-08 17:48:33.364 38 INFO armada.cli [-] No release changes detected
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Did not perform chart purge(s)
2019-05-08 17:48:33.364 38 INFO armada.cli [-] Did not perform chart protected(s)
```

## Update Ceph pool replication (AIO-SX only)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool set {} size 1
set pool 1 size to 1
set pool 2 size to 1
set pool 3 size to 1
set pool 4 size to 1
set pool 5 size to 1
set pool 6 size to 1
set pool 7 size to 1
set pool 8 size to 1
set pool 9 size to 1
```

## Verify the cluster endpoints

```sh
controller-0:/home/wrsroot# mkdir -p /etc/openstack
controller-0:/home/wrsroot# tee /etc/openstack/clouds.yaml << EOF
> clouds:
>   openstack_helm:
>     region_name: RegionOne
>     identity_api_version: 3
>     endpoint_type: internalURL
>     auth:
>       username: 'admin'
>       password: 'Madawaska1*'
>       project_name: 'admin'
>       project_domain_name: 'default'
>       user_domain_name: 'default'
>       auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
> EOF
clouds:
  openstack_helm:
    region_name: RegionOne
    identity_api_version: 3
    endpoint_type: internalURL
    auth:
      username: 'admin'
      password: 'Madawaska1*'
      project_name: 'admin'
      project_domain_name: 'default'
      user_domain_name: 'default'
      auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
controller-0:/home/wrsroot# 
```

```sh
controller-0:/home/wrsroot# openstack endpoint list | more
+----------------------------------+-----------+--------------+----------------+---------+-----------+----------------------------------------------------------------------
-----+
| ID                               | Region    | Service Name | Service Type   | Enabled | Interface | URL                                                                  
     |
+----------------------------------+-----------+--------------+----------------+---------+-----------+----------------------------------------------------------------------
-----+
| 04efc994bdf54ef9945e1ff6e791b267 | RegionOne | nova         | compute        | True    | admin     | http://nova-api-proxy.openstack.svc.cluster.local:8774/v2.1/%(tenant_
id)s |
| 07b16c7e20e34b33bd904ed7a4d5e0d5 | RegionOne | glance       | image          | True    | internal  | http://glance-api.openstack.svc.cluster.local:9292/                  
     |
| 0b8c138946604cbeb9e836d7496956c4 | RegionOne | glance       | image          | True    | admin     | http://glance-api.openstack.svc.cluster.local:9292/                  
     |
| 13923f3f35bf4c18a5fde31c9898de45 | RegionOne | keystone     | identity       | True    | admin     | http://keystone.openstack.svc.cluster.local:80/v3                    
     |
| 1c4431e293e24b45ba0d26a061612c77 | RegionOne | cinderv3     | volumev3       | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v3/%(tenant_id)s  
     |
| 1de75d5dd3844d1ebb19ac3a66a9f687 | RegionOne | heat-cfn     | cloudformation | True    | public    | http://cloudformation.openstack.svc.cluster.local:80/v1              
     |
| 27488bcb5a59438fa803f33580550cfa | RegionOne | panko        | event          | True    | public    | http://panko.openstack.svc.cluster.local:80/                         
     |
| 33e9d1218e354de98e7198adf7bc52b6 | RegionOne | panko        | event          | True    | internal  | http://panko-api.openstack.svc.cluster.local:8977/                   
     |
| 3545083e8d7d42f795cda08fe65b5e17 | RegionOne | keystone     | identity       | True    | internal  | http://keystone-api.openstack.svc.cluster.local:5000/v3              
     |
| 37f95ee5810e459e90d9174761f4bf64 | RegionOne | placement    | placement      | True    | admin     | http://placement-api.openstack.svc.cluster.local:8778/               
     |
| 3fa1440bb4d24efb96d654c49922722b | RegionOne | glance       | image          | True    | public    | http://glance.openstack.svc.cluster.local:80/                        
     |
| 43dbd248591c4d168c52f516132475cc | RegionOne | aodh         | alarming       | True    | public    | http://aodh.openstack.svc.cluster.local:80/                          
     |
| 4c3a61941df04455bac03b63a5cb9020 | RegionOne | heat         | orchestration  | True    | internal  | http://heat-api.openstack.svc.cluster.local:8004/v1/%(project_id)s   
     |
| 4fa2fa0f343d4ccea19ef7380982028b | RegionOne | nova         | compute        | True    | internal  | http://nova-api-proxy.openstack.svc.cluster.local:8774/v2.1/%(tenant_
id)s |
| 550c473984e04fe4be0d8f00ae8473b5 | RegionOne | barbican     | key-manager    | True    | public    | http://barbican.openstack.svc.cluster.local:80/                      
     |
| 55f49b839b79428ea02fbd6aa6477dae | RegionOne | cinderv2     | volumev2       | True    | public    | http://cinder.openstack.svc.cluster.local:80/v2/%(tenant_id)s        
     |
| 62bc536115bf45f0817c70c6a69296fc | RegionOne | gnocchi      | metric         | True    | internal  | http://gnocchi-api.openstack.svc.cluster.local:8041/                 
     |
| 6511c91db77c451bbd9088dc571e01aa | RegionOne | heat-cfn     | cloudformation | True    | internal  | http://heat-cfn.openstack.svc.cluster.local:8000/v1                  
     |
| 6b5454cae00d4f2c96d4b7cae727888c | RegionOne | gnocchi      | metric         | True    | admin     | http://gnocchi-api.openstack.svc.cluster.local:8041/                 
     |
| 6db3d80a19174a978c3829b2c97dab49 | RegionOne | cinder       | volume         | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v1/%(tenant_id)s  
     |
| 6db3d80a19174a978c3829b2c97dab49 | RegionOne | cinder       | volume         | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v1/%(tenant_id)s  
     |
| 7038cab730ad45378e1bce4ae687a98b | RegionOne | nova         | compute        | True    | public    | http://nova.openstack.svc.cluster.local:80/v2.1/%(tenant_id)s        
     |
| 83d15864cca9484daaf558317435935b | RegionOne | cinderv3     | volumev3       | True    | public    | http://cinder.openstack.svc.cluster.local:80/v3/%(tenant_id)s        
     |
| 885ef35cee6e4c44af6fca24e71a06fc | RegionOne | keystone     | identity       | True    | public    | http://keystone.openstack.svc.cluster.local:80/v3                    
     |
| 8be778d03f9644b886b6e3851a709dd6 | RegionOne | placement    | placement      | True    | internal  | http://placement-api.openstack.svc.cluster.local:8778/               
     |
| 918cc9bd8b5a4693a5bba2bf4ea70a60 | RegionOne | barbican     | key-manager    | True    | internal  | http://barbican-api.openstack.svc.cluster.local:9311/                
     |
| 93ce5b166a1446d2b6fb708fa56f274e | RegionOne | aodh         | alarming       | True    | internal  | http://aodh-api.openstack.svc.cluster.local:8042/                    
     |
| 955c25df446e46899bae84e0da4104cc | RegionOne | heat         | orchestration  | True    | public    | http://heat.openstack.svc.cluster.local:80/v1/%(project_id)s         
     |
| 9692af3e29844ffaa3c248cc5cdaf1a3 | RegionOne | aodh         | alarming       | True    | admin     | http://aodh-api.openstack.svc.cluster.local:8042/                    
     |
| 9931e9de7e06407b94ed4c01954cb21a | RegionOne | neutron      | network        | True    | public    | http://neutron.openstack.svc.cluster.local:80/                       
     |
| ac615e4d390f4509914433a8ffb4e8f5 | RegionOne | placement    | placement      | True    | public    | http://placement.openstack.svc.cluster.local:80/                     
     |
| b175f65cf6714171ae21341224b30013 | RegionOne | cinderv2     | volumev2       | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v2/%(tenant_id)s  
     |
| b379a1060dab4ca7978c8acafcdd4a8f | RegionOne | cinderv3     | volumev3       | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v3/%(tenant_id)s  
     |
| b6149f6af98c4be5a8d1e1b5607b70c6 | RegionOne | neutron      | network        | True    | internal  | http://neutron-server.openstack.svc.cluster.local:9696/              
     |
| be913c0ed53a42febfdb020eb3ff86aa | RegionOne | barbican     | key-manager    | True    | admin     | http://barbican-api.openstack.svc.cluster.local:9311/                
     |
| c8f3257a81cf4abe918459fc1851ffcb | RegionOne | cinder       | volume         | True    | public    | http://cinder.openstack.svc.cluster.local:80/v1/%(tenant_id)s        
     |
| c9e5d202852045439cdb858814a71473 | RegionOne | panko        | event          | True    | admin     | http://panko-api.openstack.svc.cluster.local:8977/                   
     |
     |
| ca190c1fffff43718bd46edf4aeb4f21 | RegionOne | cinderv2     | volumev2       | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v2/%(tenant_id)s  
     |
| d23c4192961f42c5b2adb897c585cbc1 | RegionOne | neutron      | network        | True    | admin     | http://neutron-server.openstack.svc.cluster.local:9696/              
     |
| d6062e18b21f4471892d934fd85239d6 | RegionOne | gnocchi      | metric         | True    | public    | http://gnocchi.openstack.svc.cluster.local:80/                       
     |
| dd4fbd8136604945981bd7b8841a02c0 | RegionOne | heat-cfn     | cloudformation | True    | admin     | http://heat-cfn.openstack.svc.cluster.local:8000/v1                  
     |
| df3dd8fa62aa4c4a9349bf4d4e145e89 | RegionOne | heat         | orchestration  | True    | admin     | http://heat-api.openstack.svc.cluster.local:8004/v1/%(project_id)s   
     |
| f735be1248c546f3acc0e2fa00d2cf5a | RegionOne | cinder       | volume         | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v1/%(tenant_id)s  
     |
+----------------------------------+-----------+--------------+----------------+---------+-----------+----------------------------------------------------------------------
-----+
```


## Provider/tenant networking setup

```sh
controller-0:/home/wrsroot# ADMINID=`openstack project list | grep admin | awk '{print $2}'`
controller-0:/home/wrsroot# PHYSNET0='physnet0'
controller-0:/home/wrsroot# PHYSNET1='physnet1'
```

```sh
controller-0:/home/wrsroot# openstack network segment range create ${PHYSNET0}-a --network-type vlan --physical-network ${PHYSNET0}  --minimum 400 --maximum 499 --private --project ${ADMINID}

+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['400-499']                                                                                                                                                                                               |
| default          | False                                                                                                                                                                                                     |
| id               | 9b740164-c0a3-4daa-ad76-082faafa4733                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 499                                                                                                                                                                                                       |
| minimum          | 400                                                                                                                                                                                                       |
| name             | physnet0-a                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet0                                                                                                                                                                                                  |
| project_id       | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| shared           | False                                                                                                                                                                                                     |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack network segment range create  ${PHYSNET0}-b --network-type vlan  --physical-network ${PHYSNET0}  --minimum 10 --maximum 10 --shared
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['10']                                                                                                                                                                                                    |
| default          | False                                                                                                                                                                                                     |
| id               | 5884f40e-aa4e-4826-92a2-1422db2daa5f                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 10                                                                                                                                                                                                        |
| minimum          | 10                                                                                                                                                                                                        |
| name             | physnet0-b                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet0                                                                                                                                                                                                  |
| project_id       | None                                                                                                                                                                                                      |
| shared           | True                                                                                                                                                                                                      |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack network segment range create ${PHYSNET1}-a --network-type vlan  --physical-network  ${PHYSNET1} --minimum 500 --maximum 599  --private --project ${ADMINID}
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['500-599']                                                                                                                                                                                               |
| default          | False                                                                                                                                                                                                     |
| id               | 6cabe140-53e7-4ed7-a186-4de7331ef1c0                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 599                                                                                                                                                                                                       |
| minimum          | 500                                                                                                                                                                                                       |
| name             | physnet1-a                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet1                                                                                                                                                                                                  |
| project_id       | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| shared           | False                                                                                                                                                                                                     |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

## Tenant Networking setup

```sh
controller-0:/home/wrsroot# ADMINID=`openstack project list | grep admin | awk '{print $2}'`
controller-0:/home/wrsroot# PHYSNET0='physnet0'
controller-0:/home/wrsroot# PHYSNET1='physnet1'
controller-0:/home/wrsroot# PUBLICNET='public-net0'
controller-0:/home/wrsroot# PRIVATENET='private-net0'
controller-0:/home/wrsroot# INTERNALNET='internal-net0'
controller-0:/home/wrsroot# EXTERNALNET='external-net0'
controller-0:/home/wrsroot# PUBLICSUBNET='public-subnet0'
controller-0:/home/wrsroot# PRIVATESUBNET='private-subnet0'
controller-0:/home/wrsroot# INTERNALSUBNET='internal-subnet0'
controller-0:/home/wrsroot# EXTERNALSUBNET='external-subnet0'
controller-0:/home/wrsroot# PUBLICROUTER='public-router0'
controller-0:/home/wrsroot# PRIVATEROUTER='private-router0'
```

```sh
controller-0:/home/wrsroot# openstack network create --project ${ADMINID} --provider-network-type=vlan --provider-physical-network=${PHYSNET0} --provider-segment=10 --share
 --external ${EXTERNALNET}
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------+
| Field                     | Value                                                                                                                                         
                                                            |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------+
| admin_state_up            | UP                                                                                                                                            
                                                            |
| availability_zone_hints   |                                                                                                                                               
                                                            |
| availability_zones        |                                                                                                                                               
                                                            |
| created_at                | 2019-05-08T18:35:13Z                                                                                                                          
                                                            |
| description               |                                                                                                                                               
                                                            |
| dns_domain                | None                                                                                                                                          
                                                            |
| id                        | 6a1d94cb-325e-4f22-8564-835a7c9c8602                                                                                                          
                                                            |
| ipv4_address_scope        | None                                                                                                                                          
                                                            |
| ipv6_address_scope        | None                                                                                                                                          
                                                            |
| is_default                | False                                                                                                                                         
                                                            |
| is_vlan_transparent       | False                                                                                                                                         
                                                            |
| location                  | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': $
openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| mtu                       | 1500                                                                                                                                          
                                                            |
| name                      | external-net0                                                                                                                                 
                                                            |
| port_security_enabled     | True                                                                                                                                          
                                                            |
| project_id                | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                              
                                                            |
| provider:network_type     | vlan                                                                                                                                          
                                                            |
| provider:physical_network | physnet0                                                                                                                                      
                                                            |
| provider:segmentation_id  | 10                                                                                                                                            
                                                            |
| qos_policy_id             | None                                                                                                                                          
                                                            |
| revision_number           | 1                                                                                                                                             
                                                            |
| router:external           | External                                                                                                                                      
                                                            |
| segments                  | None                                                                                                                                          
                                                            |
| shared                    | True                                                                                                                                          
                                                            |
| status                    | ACTIVE                                                                                                                                        
                                                            |
| subnets                   |                                                                                                                                               
                                                            |
| tags                      |                                                                                                                                               
                                                            |
| updated_at                | 2019-05-08T18:35:13Z                                                                                                                          
                                                            |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack network create --project ${ADMINID} --provider-network-type=vlan --provider-physical-network=${PHYSNET0} --provider-segment=400 ${PUBLICNET}
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| Field                     | Value                                                                                                                                                           
                                          |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| admin_state_up            | UP                                                                                                                                                                                                        |
| availability_zone_hints   |                                                                                                                                                                                                           |
| availability_zones        |                                                                                                                                                                                                           |
| created_at                | 2019-05-08T18:36:54Z                                                                                                                                                                                      |
| description               |                                                                                                                                                                                                           |
| dns_domain                | None                                                                                                                                                                                                      |
| id                        | 2d6b387c-58b8-4a7e-ba22-7085def824bd                                                                                                                                                                      |
| ipv4_address_scope        | None                                                                                                                                                                                                      |
| ipv6_address_scope        | None                                                                                                                                                                                                      |
| is_default                | False                                                                                                                                                                                                     |
| is_vlan_transparent       | False                                                                                                                                                                                                     |
| location                  | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', $region_name': 'RegionOne', 'zone': None}) |
| mtu                       | 1500                                                                                                                                                                                                      |
| name                      | public-net0                                                                                                                                                                                               |
| port_security_enabled     | True                                                                                                                                                                                                      |
| project_id                | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| provider:network_type     | vlan                                                                                                                                                                                                      |
| provider:physical_network | physnet0                                                                                                                                                                                                  |
| provider:segmentation_id  | 400                                                                                                                                                                                                       |
| qos_policy_id             | None                                                                                                                                                            
|
| revision_number           | 1                                                                                                                                                                                                         |
| router:external           | Internal                                                                                                                                                                                                  |
| segments                  | None                                                                                                                                                                                                      |
| shared                    | False                                                                                                                                                                                                     |
| status                    | ACTIVE                                                                                                                                                                                                    |
| subnets                   |                                                                                                                                                                                                           |
| tags                      |                                                                                                                                                                                                           |
| updated_at                | 2019-05-08T18:36:54Z                                                                                                                                                                                      |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack network create --project ${ADMINID} --provider-network-type=vlan --provider-physical-network=${PHYSNET1} --provider-segment=500 ${PRIVATENET}
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| Field                     | Value                                                                                                                                                           
                                          |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| admin_state_up            | UP                                                                                                                                                              
                                          |
| availability_zone_hints   |                                                                                                                                                                                                           |
| availability_zones        |                                                                                                                                                                                                           |
| created_at                | 2019-05-08T18:38:38Z                                                                                                                                                                                      |
| description               |                                                                                                                                                                                                           |
| dns_domain                | None                                                                                                                                                                                                      |
| id                        | e23245d4-f5cb-4b9e-ad3e-888fb104a30b                                                                                                                                                                      |
| ipv4_address_scope        | None                                                                                                                                                                                                      |
| ipv6_address_scope        | None                                                                                                                                                                                                      |
| is_default                | False                                                                                                                                                                                                     |
| is_vlan_transparent       | False                                                                                                                                                                                                     |
| location                  | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| mtu                       | 1500                                                                                                                                                                                                      |
| name                      | private-net0                                                                                                                                                                                              |
| port_security_enabled     | True                                                                                                                                                                                                      |
| project_id                | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| provider:network_type     | vlan                                                                                                                                                                                                      |
| provider:physical_network | physnet1                                                                                                                                                                                                  |
| provider:segmentation_id  | 500                                                                                                                                                                                                       |
| qos_policy_id             | None                                                                                                                                                                                                      |
| revision_number           | 1                                                                                                                                                                                                         |
| router:external           | Internal                                                                                                                                                                                                  |
| segments                  | None                                                                                                                                                                                                      |
| shared                    | False                                                                                                                                                                                                     |
| status                    | ACTIVE                                                                                                                                                                                                    |
| subnets                   |                                                                                                                                                                                                           |
| tags                      |                                                                                                                                                                                                           |
| updated_at                | 2019-05-08T18:38:38Z                                                                                                                                                                                      |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack network create --project ${ADMINID} ${INTERNALNET}
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| Field                     | Value                                                                                                                                                           
                                          |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------+
| admin_state_up            | UP                                                                                                                                                                                                        |
| availability_zone_hints   |                                                                                                                                                                                                           |
| availability_zones        |                                                                                                                                                                                                           |
| created_at                | 2019-05-08T18:40:07Z                                                                                                                                                                                      |
| description               |                                                                                                                                                                                                           |
| dns_domain                | None                                                                                                                                                                                                      |
| id                        | 82cb9679-045d-4cca-a788-55e2442a7fc1                                                                                                                                                                      |
| ipv4_address_scope        | None                                                                                                                                                                                                      |
| ipv6_address_scope        | None                                                                                                                                                                                                      |
| is_default                | False                                                                                                                                                                                                     |
| is_vlan_transparent       | False                                                                                                                                                                                                     |
| location                  | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', $region_name': 'RegionOne', 'zone': None}) |
| mtu                       | 1500                                                                                                                                                                                                      |
| name                      | internal-net0                                                                                                                                                                                             |
| port_security_enabled     | True                                                                                                                                                                                                      |
| project_id                | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| provider:network_type     | vlan                                                                                                                                                                                                      |
| provider:physical_network | physnet1                                                                                                                                                                                                  |
| provider:segmentation_id  | 512                                                                                                                                                                                                       |
| qos_policy_id             | None                                                                                                                                                                                                      |
| revision_number           | 1                                                                                                                                                                                                         |
| router:external           | Internal                                                                                                                                                                                                  |
| segments                  | None                                                                                                                                                            
|
| shared                    | False                                                                                                                                                                                                     |
| status                    | ACTIVE                                                                                                                                                                                                    |
| subnets                   |                                                                                                                                                                                                           |
| tags                      |                                                                                                                                                                                                           |
| updated_at                | 2019-05-08T18:40:07Z                                                                                                                                                                                      |
+---------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# PUBLICNETID=`openstack network list | grep ${PUBLICNET} | awk '{print $2}'`
controller-0:/home/wrsroot# PRIVATENETID=`openstack network list | grep ${PRIVATENET} | awk '{print $2}'`
controller-0:/home/wrsroot# INTERNALNETID=`openstack network list | grep ${INTERNALNET} | awk '{print $2}'`
controller-0:/home/wrsroot# EXTERNALNETID=`openstack network list | grep ${EXTERNALNET} | awk '{print $2}'`
```

```sh
controller-0:/home/wrsroot# openstack subnet create --project ${ADMINID} ${PUBLICSUBNET} --network ${PUBLICNET} --subnet-range 192.168.101.0/24
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                                                                                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| allocation_pools  | 192.168.101.2-192.168.101.254                                                                                                                                                                             |
| cidr              | 192.168.101.0/24                                                                                                                                                                                          |
| created_at        | 2019-05-08T18:41:28Z                                                                                                                                                                                      |
| description       |                                                                                                                                                                                                           |
| dns_nameservers   |                                                                                                                                                                                                           |
| enable_dhcp       | True                                                                                                                                                                                                      |
| gateway_ip        | 192.168.101.1                                                                                                                                                                                             |
| host_routes       |                                                                                                                                                                                                           |
| id                | d88b4952-7060-4e33-9a4e-ea0f86b16a16                                                                                                                                                                      |
| ip_version        | 4                                                                                                                                                                                                         |
| ipv6_address_mode | None                                                                                                                                                                                                      |
| ipv6_ra_mode      | None                                                                                                                                                                                                      |
| location          | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name              | public-subnet0                                                                                                                                                                                            |
| network_id        | 2d6b387c-58b8-4a7e-ba22-7085def824bd                                                                                                                                                                      |
| prefix_length     | None                                                                                                                                                                                                      |
| project_id        | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number   | 0                                                                                                                                                                                                         |
| segment_id        | None                                                                                                                                                                                                      |
| service_types     |                                                                                                                                                                                                           |
| subnetpool_id     | None                                                                                                                                                                                                      |
| tags              |                                                                                                                                                                                                           |
| updated_at        | 2019-05-08T18:41:28Z                                                                                                                                                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack subnet create --project ${ADMINID} ${PRIVATESUBNET} --network ${PRIVATENET} --subnet-range 192.168.201.0/24
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                                                                                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| allocation_pools  | 192.168.201.2-192.168.201.254                                                                                                                                                                             |
| cidr              | 192.168.201.0/24                                                                                                                                                                                          |
| created_at        | 2019-05-08T18:42:00Z                                                                                                                                                                                      |
| description       |                                                                                                                                                                                                           |
| dns_nameservers   |                                                                                                                                                                                                           |
| enable_dhcp       | True                                                                                                                                                                                                      |
| gateway_ip        | 192.168.201.1                                                                                                                                                                                             |
| host_routes       |                                                                                                                                                                                                           |
| id                | 122b53e8-0de4-4a86-ad43-ef82ec259c12                                                                                                                                                                      |
| ip_version        | 4                                                                                                                                                                                                         |
| ipv6_address_mode | None                                                                                                                                                                                                      |
| ipv6_ra_mode      | None                                                                                                                                                                                                      |
| location          | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name              | private-subnet0                                                                                                                                                                                           |
| network_id        | e23245d4-f5cb-4b9e-ad3e-888fb104a30b                                                                                                                                                                      |
| prefix_length     | None                                                                                                                                                                                                      |
| project_id        | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number   | 0                                                                                                                                                                                                         |
| segment_id        | None                                                                                                                                                                                                      |
| service_types     |                                                                                                                                                                                                           |
| subnetpool_id     | None                                                                                                                                                                                                      |
| tags              |                                                                                                                                                                                                           |
| updated_at        | 2019-05-08T18:42:00Z                                                                                                                                                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack subnet create --project ${ADMINID} ${INTERNALSUBNET} --gateway none --network ${INTERNALNET} --subnet-range 10.1.1.0/24
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                                                                                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| allocation_pools  | 10.1.1.1-10.1.1.254                                                                                                                                                                                       |
| cidr              | 10.1.1.0/24                                                                                                                                                                                               |
| created_at        | 2019-05-08T18:42:19Z                                                                                                                                                                                      |
| description       |                                                                                                                                                                                                           |
| dns_nameservers   |                                                                                                                                                                                                           |
| enable_dhcp       | True                                                                                                                                                                                                      |
| gateway_ip        | None                                                                                                                                                                                                      |
| host_routes       |                                                                                                                                                                                                           |
| id                | 9f5f6e53-0f00-416f-8e15-5ac066af48a5                                                                                                                                                                      |
| ip_version        | 4                                                                                                                                                                                                         |
| ipv6_address_mode | None                                                                                                                                                                                                      |
| ipv6_ra_mode      | None                                                                                                                                                                                                      |
| location          | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name              | internal-subnet0                                                                                                                                                                                          |
| network_id        | 82cb9679-045d-4cca-a788-55e2442a7fc1                                                                                                                                                                      |
| prefix_length     | None                                                                                                                                                                                                      |
| project_id        | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number   | 0                                                                                                                                                                                                         |
| segment_id        | None                                                                                                                                                                                                      |
| service_types     |                                                                                                                                                                                                           |
| subnetpool_id     | None                                                                                                                                                                                                      |
| tags              |                                                                                                                                                                                                           |
| updated_at        | 2019-05-08T18:42:19Z                                                                                                                                                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack subnet create --project ${ADMINID} ${EXTERNALSUBNET} --gateway 192.168.1.1 --no-dhcp --network ${EXTERNALNET} --subnet-range 192.168.51.0/24 --ip-version 4
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field             | Value                                                                                                                                                                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| allocation_pools  | 192.168.51.1-192.168.51.254                                                                                                                                                                               |
| cidr              | 192.168.51.0/24                                                                                                                                                                                           |
| created_at        | 2019-05-08T18:42:40Z                                                                                                                                                                                      |
| description       |                                                                                                                                                                                                           |
| dns_nameservers   |                                                                                                                                                                                                           |
| enable_dhcp       | False                                                                                                                                                                                                     |
| gateway_ip        | 192.168.1.1                                                                                                                                                                                               |
| host_routes       |                                                                                                                                                                                                           |
| id                | 5021df1d-ae71-41bd-856e-3177f31a0a5c                                                                                                                                                                      |
| ip_version        | 4                                                                                                                                                                                                         |
| ipv6_address_mode | None                                                                                                                                                                                                      |
| ipv6_ra_mode      | None                                                                                                                                                                                                      |
| location          | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name              | external-subnet0                                                                                                                                                                                          |
| network_id        | 6a1d94cb-325e-4f22-8564-835a7c9c8602                                                                                                                                                                      |
| prefix_length     | None                                                                                                                                                                                                      |
| project_id        | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number   | 0                                                                                                                                                                                                         |
| segment_id        | None                                                                                                                                                                                                      |
| service_types     |                                                                                                                                                                                                           |
| subnetpool_id     | None                                                                                                                                                                                                      |
| tags              |                                                                                                                                                                                                           |
| updated_at        | 2019-05-08T18:42:40Z                                                                                                                                                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack router create ${PUBLICROUTER}
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field                   | Value                                                                                                                                                                                                     |
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| admin_state_up          | UP                                                                                                                                                                                                        |
| availability_zone_hints |                                                                                                                                                                                                           |
| availability_zones      |                                                                                                                                                                                                           |
| created_at              | 2019-05-08T18:43:08Z                                                                                                                                                                                      |
| description             |                                                                                                                                                                                                           |
| distributed             | False                                                                                                                                                                                                     |
| external_gateway_info   | None                                                                                                                                                                                                      |
| flavor_id               | None                                                                                                                                                                                                      |
| ha                      | False                                                                                                                                                                                                     |
| id                      | b7e48744-5372-4a73-9faa-3ba3678ee28d                                                                                                                                                                      |
| location                | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name                    | public-router0                                                                                                                                                                                            |
| project_id              | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number         | 1                                                                                                                                                                                                         |
| routes                  |                                                                                                                                                                                                           |
| status                  | ACTIVE                                                                                                                                                                                                    |
| tags                    |                                                                                                                                                                                                           |
| updated_at              | 2019-05-08T18:43:08Z                                                                                                                                                                                      |
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# openstack router create ${PRIVATEROUTER}
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field                   | Value                                                                                                                                                                                                     |
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| admin_state_up          | UP                                                                                                                                                                                                        |
| availability_zone_hints |                                                                                                                                                                                                           |
| availability_zones      |                                                                                                                                                                                                           |
| created_at              | 2019-05-08T18:43:29Z                                                                                                                                                                                      |
| description             |                                                                                                                                                                                                           |
| distributed             | False                                                                                                                                                                                                     |
| external_gateway_info   | None                                                                                                                                                                                                      |
| flavor_id               | None                                                                                                                                                                                                      |
| ha                      | False                                                                                                                                                                                                     |
| id                      | 70c0292d-a686-49c7-9b10-ce1f61540400                                                                                                                                                                      |
| location                | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'17ac3fb8b9b24081a1aaa95cd1fa2d74'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| name                    | private-router0                                                                                                                                                                                           |
| project_id              | 17ac3fb8b9b24081a1aaa95cd1fa2d74                                                                                                                                                                          |
| revision_number         | 1                                                                                                                                                                                                         |
| routes                  |                                                                                                                                                                                                           |
| status                  | ACTIVE                                                                                                                                                                                                    |
| tags                    |                                                                                                                                                                                                           |
| updated_at              | 2019-05-08T18:43:29Z                                                                                                                                                                                      |
+-------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:/home/wrsroot# PRIVATEROUTERID=`openstack router list | grep ${PRIVATEROUTER} | awk '{print $2}'`
controller-0:/home/wrsroot# PUBLICROUTERID=`openstack router list | grep ${PUBLICROUTER} | awk '{print $2}'`
```

```sh
controller-0:/home/wrsroot# openstack router set ${PUBLICROUTER} --external-gateway ${EXTERNALNETID} --disable-snat
controller-0:/home/wrsroot# openstack router set ${PRIVATEROUTER} --external-gateway ${EXTERNALNETID} --disable-snat
```

```sh
controller-0:/home/wrsroot# openstack router add subnet ${PUBLICROUTER} ${PUBLICSUBNET}
controller-0:/home/wrsroot# openstack router add subnet ${PRIVATEROUTER} ${PRIVATESUBNET}
```

## Additional Setup Instructions

None

## Horizon access


```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services -n openstack | grep horizon
horizon                       ClusterIP   10.108.237.164   <none>        80/TCP,443/TCP                 7m33s
horizon-int                   NodePort    10.98.52.151     <none>        80:31000/TCP                   7m33s
[wrsroot@controller-0 ~(keystone_admin)]$ curl -L http://10.10.10.3:8080 -so - | egrep '(PlugIn|<title>)'
    <title>Login - StarlingX</title>
    global.horizonPlugInModules = ['horizon.dashboard.project', 'horizon.dashboard.dc_admin', 'horizon.dashboard.fault_management', 'horizon.dashboard.identity'];
[wrsroot@controller-0 ~(keystone_admin)]$ curl -L http://10.10.10.3:31000 -so - | egrep '(PlugIn|<title>)'
    <title>Login - StarlingX</title>
    global.horizonPlugInModules = ['horizon.dashboard.project', 'horizon.dashboard.identity'];
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

## Others

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services -n openstack
NAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
aodh                          ClusterIP   10.99.130.210    <none>        80/TCP,443/TCP                 7m1s
aodh-api                      ClusterIP   10.102.253.11    <none>        8042/TCP                       7m1s
barbican                      ClusterIP   10.99.204.151    <none>        80/TCP,443/TCP                 22m
barbican-api                  ClusterIP   10.99.104.218    <none>        9311/TCP                       22m
ceilometer                    ClusterIP   10.105.241.28    <none>        80/TCP,443/TCP                 2m5s
cinder                        ClusterIP   10.109.240.235   <none>        80/TCP,443/TCP                 9m51s
cinder-api                    ClusterIP   10.98.239.62     <none>        8776/TCP                       9m51s
cloudformation                ClusterIP   10.106.72.46     <none>        80/TCP,443/TCP                 13m
glance                        ClusterIP   10.106.170.140   <none>        80/TCP,443/TCP                 21m
glance-api                    ClusterIP   10.106.243.38    <none>        9292/TCP                       21m
gnocchi                       ClusterIP   10.108.73.208    <none>        80/TCP,443/TCP                 5m26s
gnocchi-api                   ClusterIP   10.103.64.185    <none>        8041/TCP                       5m26s
heat                          ClusterIP   10.97.160.185    <none>        80/TCP,443/TCP                 13m
heat-api                      ClusterIP   10.98.64.161     <none>        8004/TCP                       13m
heat-cfn                      ClusterIP   10.102.40.48     <none>        8000/TCP                       13m
horizon                       ClusterIP   10.108.237.164   <none>        80/TCP,443/TCP                 11m
horizon-int                   NodePort    10.98.52.151     <none>        80:31000/TCP                   11m
ingress                       ClusterIP   10.99.185.95     <none>        80/TCP,443/TCP,18080/TCP       25m
ingress-error-pages           ClusterIP   None             <none>        80/TCP                         25m
ingress-exporter              ClusterIP   10.103.126.95    <none>        10254/TCP                      25m
keystone                      ClusterIP   10.101.229.41    <none>        80/TCP,443/TCP                 23m
keystone-api                  ClusterIP   10.109.41.93     <none>        5000/TCP                       23m
mariadb                       ClusterIP   10.102.69.71     <none>        3306/TCP                       25m
mariadb-discovery             ClusterIP   None             <none>        3306/TCP,4567/TCP              25m
mariadb-ingress-error-pages   ClusterIP   None             <none>        80/TCP                         25m
mariadb-server                ClusterIP   10.106.77.2      <none>        3306/TCP                       25m
memcached                     ClusterIP   10.102.73.230    <none>        11211/TCP                      24m
metadata                      ClusterIP   10.104.161.229   <none>        80/TCP,443/TCP                 19m
neutron                       ClusterIP   10.99.23.230     <none>        80/TCP,443/TCP                 19m
neutron-server                ClusterIP   10.97.170.104    <none>        9696/TCP                       19m
nova                          ClusterIP   10.100.226.236   <none>        80/TCP,443/TCP                 19m
nova-api                      ClusterIP   10.104.159.200   <none>        8774/TCP                       19m
nova-api-proxy                ClusterIP   10.99.153.99     <none>        8774/TCP                       19m
nova-metadata                 ClusterIP   10.98.152.111    <none>        8775/TCP                       19m
nova-novncproxy               ClusterIP   10.105.141.147   <none>        6080/TCP                       19m
novncproxy                    ClusterIP   10.99.109.60     <none>        80/TCP,443/TCP                 19m
osh-openstac-dsv-e50215       ClusterIP   None             <none>        5672/TCP,25672/TCP,15672/TCP   24m
osh-openstac-mgr-e50215       ClusterIP   10.101.251.195   <none>        80/TCP,443/TCP                 24m
panko                         ClusterIP   10.111.200.92    <none>        80/TCP,443/TCP                 3m13s
panko-api                     ClusterIP   10.107.233.52    <none>        8977/TCP                       3m13s
placement                     ClusterIP   10.106.63.152    <none>        80/TCP,443/TCP                 19m
placement-api                 ClusterIP   10.96.99.35      <none>        8778/TCP                       19m
rabbitmq                      ClusterIP   10.103.171.246   <none>        5672/TCP,25672/TCP,15672/TCP   24m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get configmaps -n openstack
NAME                                                          DATA   AGE
aodh-bin                                                      13     7m57s
barbican-bin                                                  9      23m
ceilometer-bin                                                13     3m1s
ceph-etc                                                      1      26m
ceph-pools-bin                                                1      26m
cinder-bin                                                    19     10m
config-rbd-provisioner                                        2      26m
glance-bin                                                    15     22m
gnocchi-bin                                                   16     6m22s
heat-bin                                                      17     14m
horizon-bin                                                   6      12m
ingress-bin                                                   2      26m
ingress-conf                                                  4      26m
ingress-services-tcp                                          0      26m
ingress-services-udp                                          0      26m
keystone-bin                                                  13     24m
libvirt-bin                                                   1      20m
mariadb-bin                                                   7      26m
mariadb-etc                                                   5      26m
mariadb-services-tcp                                          1      26m
neutron-bin                                                   22     20m
nova-api-proxy-bin                                            1      20m
nova-api-proxy-etc                                            3      20m
nova-bin                                                      32     20m
openvswitch-bin                                               3      20m
osh-openstack-ingress-nginx                                   0      26m
osh-openstack-mariadb-mariadb-state                           5      25m
osh-openstack-mariadb-osh-openstack-mariadb-mariadb-ingress   0      26m
osh-openstack-memcached-memcached-bin                         1      25m
osh-openstack-rabbitmq-rabbitmq-bin                           7      25m
osh-openstack-rabbitmq-rabbitmq-etc                           2      25m
panko-bin                                                     9      4m10s
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods -n openstack -o wide
NAME                                                 READY   STATUS      RESTARTS   AGE     IP               NODE           NOMINATED NODE   READINESS GATES
aodh-api-998c87b4-lt8zc                              1/1     Running     0          8m36s   172.16.192.100   controller-0   <none>           <none>
aodh-db-init-dvf8t                                   0/1     Completed   0          8m36s   172.16.192.101   controller-0   <none>           <none>
aodh-db-sync-tks7p                                   0/1     Completed   0          8m36s   172.16.192.116   controller-0   <none>           <none>
aodh-evaluator-5bc6698954-bpxnx                      1/1     Running     0          8m36s   172.16.192.106   controller-0   <none>           <none>
aodh-ks-endpoints-whp6q                              0/3     Completed   0          8m36s   172.16.192.115   controller-0   <none>           <none>
aodh-ks-service-rfkgb                                0/1     Completed   0          8m36s   172.16.192.73    controller-0   <none>           <none>
aodh-ks-user-nf4xw                                   0/1     Completed   0          8m36s   172.16.192.80    controller-0   <none>           <none>
aodh-listener-55c56fbc8f-w24rn                       1/1     Running     0          8m36s   172.16.192.118   controller-0   <none>           <none>
aodh-notifier-58bd5cbfff-5797k                       1/1     Running     0          8m36s   172.16.192.94    controller-0   <none>           <none>
aodh-rabbit-init-wt8lz                               0/1     Completed   0          8m35s   172.16.192.120   controller-0   <none>           <none>
barbican-api-6f979757c8-2nbxp                        1/1     Running     0          23m     172.16.192.86    controller-0   <none>           <none>
barbican-db-init-7qmg6                               0/1     Completed   0          23m     172.16.192.69    controller-0   <none>           <none>
barbican-db-sync-fdzm8                               0/1     Completed   0          23m     172.16.192.74    controller-0   <none>           <none>
barbican-ks-endpoints-zz27s                          0/3     Completed   0          23m     172.16.192.73    controller-0   <none>           <none>
barbican-ks-service-xdhvt                            0/1     Completed   0          23m     172.16.192.71    controller-0   <none>           <none>
barbican-ks-user-7bh27                               0/1     Completed   0          23m     172.16.192.75    controller-0   <none>           <none>
barbican-rabbit-init-cnxh8                           0/1     Completed   0          23m     172.16.192.77    controller-0   <none>           <none>
ceilometer-central-5d56f6f5bc-6qpkd                  1/1     Running     0          3m40s   172.16.192.109   controller-0   <none>           <none>
ceilometer-compute-ppvqc                             1/1     Running     0          3m40s   192.168.204.3    controller-0   <none>           <none>
ceilometer-db-sync-8zp5l                             0/1     Completed   0          3m40s   172.16.192.65    controller-0   <none>           <none>
ceilometer-ks-service-fv2qq                          0/1     Completed   0          3m40s   172.16.192.121   controller-0   <none>           <none>
ceilometer-ks-user-kjjjg                             0/1     Completed   0          3m40s   172.16.192.122   controller-0   <none>           <none>
ceilometer-notification-5c5f7f9f54-56425             1/1     Running     0          3m40s   172.16.192.117   controller-0   <none>           <none>
ceilometer-rabbit-init-bpf8z                         0/1     Completed   0          3m40s   172.16.192.101   controller-0   <none>           <none>
ceph-pools-audit-1557342300-ngfth                    0/1     Completed   0          10m     172.16.192.82    controller-0   <none>           <none>
ceph-pools-audit-1557342600-52cwd                    0/1     Completed   0          5m32s   172.16.192.123   controller-0   <none>           <none>
ceph-pools-audit-1557342900-f4jc8                    0/1     Completed   0          31s     172.16.192.120   controller-0   <none>           <none>
cinder-api-78f99795b5-9wg6n                          1/1     Running     0          11m     172.16.192.98    controller-0   <none>           <none>
cinder-backup-669856cbcb-8gnhc                       1/1     Running     0          11m     172.16.192.124   controller-0   <none>           <none>
cinder-backup-storage-init-zf6px                     0/1     Completed   0          11m     172.16.192.79    controller-0   <none>           <none>
cinder-bootstrap-tjfs9                               0/1     Completed   0          11m     172.16.192.117   controller-0   <none>           <none>
cinder-db-init-95fb8                                 0/1     Completed   0          11m     172.16.192.76    controller-0   <none>           <none>
cinder-db-sync-sgk8v                                 0/1     Completed   0          11m     172.16.192.109   controller-0   <none>           <none>
cinder-ks-endpoints-qp5cl                            0/9     Completed   0          11m     172.16.192.65    controller-0   <none>           <none>
cinder-ks-service-x2pws                              0/3     Completed   0          11m     172.16.192.119   controller-0   <none>           <none>
cinder-ks-user-l4hzl                                 0/1     Completed   0          11m     172.16.192.103   controller-0   <none>           <none>
cinder-rabbit-init-bmnfh                             0/1     Completed   0          11m     172.16.192.85    controller-0   <none>           <none>
cinder-scheduler-5979cb95c8-88tsh                    1/1     Running     0          11m     172.16.192.104   controller-0   <none>           <none>
cinder-storage-init-klxsb                            0/1     Completed   0          11m     172.16.192.72    controller-0   <none>           <none>
cinder-volume-5b5f7667c9-ch98q                       1/1     Running     0          11m     172.16.192.66    controller-0   <none>           <none>
cinder-volume-usage-audit-1557342300-bqjmn           0/1     Completed   0          10m     172.16.192.121   controller-0   <none>           <none>
cinder-volume-usage-audit-1557342600-pv6gz           0/1     Completed   0          5m32s   172.16.192.88    controller-0   <none>           <none>
cinder-volume-usage-audit-1557342900-mzcwc           0/1     Completed   0          31s     172.16.192.73    controller-0   <none>           <none>
glance-api-55c46b4b7f-hv9gg                          1/1     Running     0          22m     172.16.192.111   controller-0   <none>           <none>
glance-db-init-w7qdx                                 0/1     Completed   0          22m     172.16.192.90    controller-0   <none>           <none>
glance-db-sync-4kkbk                                 0/1     Completed   0          22m     172.16.192.123   controller-0   <none>           <none>
glance-ks-endpoints-jgbbv                            0/3     Completed   0          22m     172.16.192.104   controller-0   <none>           <none>
glance-ks-service-ghxhg                              0/1     Completed   0          22m     172.16.192.125   controller-0   <none>           <none>
glance-ks-user-8ddfh                                 0/1     Completed   0          22m     172.16.192.79    controller-0   <none>           <none>
glance-rabbit-init-mzgpm                             0/1     Completed   0          22m     172.16.192.108   controller-0   <none>           <none>
glance-storage-init-l2mj6                            0/1     Completed   0          22m     172.16.192.65    controller-0   <none>           <none>
gnocchi-api-697d9ccc46-t6hsg                         1/1     Running     0          7m1s    172.16.192.108   controller-0   <none>           <none>
gnocchi-db-init-pw7jv                                0/1     Completed   0          7m1s    172.16.192.84    controller-0   <none>           <none>
gnocchi-db-sync-qbgxj                                0/1     Completed   0          7m1s    172.16.192.74    controller-0   <none>           <none>
gnocchi-ks-endpoints-crwxn                           0/3     Completed   0          7m1s    172.16.192.71    controller-0   <none>           <none>
gnocchi-ks-service-j9z9w                             0/1     Completed   0          7m1s    172.16.192.89    controller-0   <none>           <none>
gnocchi-ks-user-d82sg                                0/1     Completed   0          7m1s    172.16.192.77    controller-0   <none>           <none>
gnocchi-metricd-9h2bz                                1/1     Running     0          7m1s    172.16.192.75    controller-0   <none>           <none>
gnocchi-storage-init-58xck                           0/1     Completed   0          7m1s    172.16.192.69    controller-0   <none>           <none>
heat-api-869b69d8b4-4cb26                            1/1     Running     0          14m     172.16.192.97    controller-0   <none>           <none>
heat-bootstrap-xsn8v                                 0/1     Completed   0          14m     172.16.192.89    controller-0   <none>           <none>
heat-cfn-867858f558-dczlb                            1/1     Running     0          14m     172.16.192.91    controller-0   <none>           <none>
heat-db-init-5248s                                   0/1     Completed   0          14m     172.16.192.73    controller-0   <none>           <none>
heat-db-sync-zh27j                                   0/1     Completed   0          14m     172.16.192.69    controller-0   <none>           <none>
heat-domain-ks-user-qn96z                            0/1     Completed   0          14m     172.16.192.80    controller-0   <none>           <none>
heat-engine-6cf47797b-l5plz                          1/1     Running     0          14m     172.16.192.92    controller-0   <none>           <none>
heat-engine-cleaner-1557342300-qf5n5                 0/1     Completed   0          10m     172.16.192.122   controller-0   <none>           <none>
heat-engine-cleaner-1557342600-ljhk9                 0/1     Completed   0          5m32s   172.16.192.90    controller-0   <none>           <none>
heat-engine-cleaner-1557342900-bz7sg                 1/1     Running     0          31s     172.16.192.116   controller-0   <none>           <none>
heat-ks-endpoints-x6x2z                              0/6     Completed   0          14m     172.16.192.75    controller-0   <none>           <none>
heat-ks-service-bwb6h                                0/2     Completed   0          14m     172.16.192.71    controller-0   <none>           <none>
heat-ks-user-m7gp7                                   0/1     Completed   0          14m     172.16.192.74    controller-0   <none>           <none>
heat-rabbit-init-kxjzv                               0/1     Completed   0          14m     172.16.192.84    controller-0   <none>           <none>
heat-trustee-ks-user-h472d                           0/1     Completed   0          14m     172.16.192.77    controller-0   <none>           <none>
heat-trusts-5hrgh                                    0/1     Completed   0          14m     172.16.192.88    controller-0   <none>           <none>
horizon-7bf458fbb7-zg5s9                             1/1     Running     0          13m     172.16.192.125   controller-0   <none>           <none>
horizon-db-init-c8wpc                                0/1     Completed   0          13m     172.16.192.123   controller-0   <none>           <none>
horizon-db-sync-x85nc                                0/1     Completed   0          13m     172.16.192.90    controller-0   <none>           <none>
ingress-6ff8cbb99-mrnpt                              1/1     Running     0          27m     172.16.192.78    controller-0   <none>           <none>
ingress-error-pages-d48db556-cfm2x                   1/1     Running     0          27m     172.16.192.113   controller-0   <none>           <none>
keystone-api-799bcb4959-m7rtn                        1/1     Running     0          25m     172.16.192.87    controller-0   <none>           <none>
keystone-bootstrap-2qsr7                             0/1     Completed   0          25m     172.16.192.88    controller-0   <none>           <none>
keystone-credential-setup-wwg4w                      0/1     Completed   0          25m     172.16.192.115   controller-0   <none>           <none>
keystone-db-init-4sj4z                               0/1     Completed   0          25m     172.16.192.80    controller-0   <none>           <none>
keystone-db-sync-jstxj                               0/1     Completed   0          25m     172.16.192.92    controller-0   <none>           <none>
keystone-domain-manage-j64br                         0/1     Completed   0          25m     172.16.192.97    controller-0   <none>           <none>
keystone-fernet-setup-tpbdv                          0/1     Completed   0          25m     172.16.192.89    controller-0   <none>           <none>
keystone-rabbit-init-v72vx                           0/1     Completed   0          25m     172.16.192.91    controller-0   <none>           <none>
libvirt-libvirt-default-pxkvx                        1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
mariadb-ingress-5f6547cb4-2qvzq                      1/1     Running     0          26m     172.16.192.114   controller-0   <none>           <none>
mariadb-ingress-error-pages-fbfb8b56b-2tqkv          1/1     Running     0          26m     172.16.192.112   controller-0   <none>           <none>
mariadb-server-0                                     1/1     Running     0          26m     172.16.192.107   controller-0   <none>           <none>
neutron-db-init-kwj9v                                0/1     Completed   0          21m     172.16.192.72    controller-0   <none>           <none>
neutron-db-sync-5mdw8                                0/1     Completed   0          21m     172.16.192.118   controller-0   <none>           <none>
neutron-dhcp-agent-controller-0-a762cb46-42vxr       1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-ks-endpoints-l77gp                           0/3     Completed   0          21m     172.16.192.120   controller-0   <none>           <none>
neutron-ks-service-qdhdj                             0/1     Completed   0          21m     172.16.192.82    controller-0   <none>           <none>
neutron-ks-user-mn8h4                                0/1     Completed   0          21m     172.16.192.117   controller-0   <none>           <none>
neutron-l3-agent-controller-0-a762cb46-g4kn6         1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-metadata-agent-controller-0-a762cb46-wxh47   1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-ovs-agent-controller-0-a762cb46-xp8d6        1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-rabbit-init-cdw69                            0/1     Completed   0          21m     172.16.192.76    controller-0   <none>           <none>
neutron-server-7795f76bc6-p6fkt                      1/1     Running     0          21m     172.16.192.102   controller-0   <none>           <none>
neutron-sriov-agent-controller-0-a762cb46-g7494      1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-api-metadata-b456bc9b7-wbbbl                    1/1     Running     1          21m     172.16.192.93    controller-0   <none>           <none>
nova-api-osapi-7bf769b779-m5cjb                      1/1     Running     0          21m     172.16.192.83    controller-0   <none>           <none>
nova-api-proxy-59c95df5b4-lq466                      1/1     Running     0          21m     172.16.192.70    controller-0   <none>           <none>
nova-bootstrap-lx8qj                                 0/1     Completed   0          21m     172.16.192.106   controller-0   <none>           <none>
nova-cell-setup-bpthm                                0/1     Completed   0          21m     172.16.192.116   controller-0   <none>           <none>
nova-compute-controller-0-9626473e-vmz4q             2/2     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-conductor-7d479f4746-g9c76                      1/1     Running     0          21m     172.16.192.105   controller-0   <none>           <none>
nova-consoleauth-75bb89d84b-rfmv8                    1/1     Running     0          21m     172.16.192.95    controller-0   <none>           <none>
nova-db-init-qh5x5                                   0/3     Completed   0          21m     172.16.192.119   controller-0   <none>           <none>
nova-db-sync-vgh9n                                   0/1     Completed   0          21m     172.16.192.94    controller-0   <none>           <none>
nova-ks-endpoints-d992f                              0/3     Completed   0          21m     172.16.192.100   controller-0   <none>           <none>
nova-ks-service-qs8nr                                0/1     Completed   0          21m     172.16.192.98    controller-0   <none>           <none>
nova-ks-user-7fqzc                                   0/1     Completed   0          21m     172.16.192.121   controller-0   <none>           <none>
nova-novncproxy-79664ffdd8-vktn8                     1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-placement-api-75f6866c4-nzfkz                   1/1     Running     0          21m     172.16.192.96    controller-0   <none>           <none>
nova-rabbit-init-dww7b                               0/1     Completed   0          21m     172.16.192.122   controller-0   <none>           <none>
nova-scheduler-768dcb6699-9nndq                      1/1     Running     0          21m     172.16.192.81    controller-0   <none>           <none>
nova-storage-init-9lcr8                              0/1     Completed   0          21m     172.16.192.85    controller-0   <none>           <none>
openvswitch-db-n7nh2                                 1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
openvswitch-vswitchd-z8mdb                           1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
osh-openstack-memcached-memcached-5f9c796767-28nqz   1/1     Running     0          26m     172.16.192.110   controller-0   <none>           <none>
osh-openstack-rabbitmq-cluster-wait-q64cx            0/1     Completed   0          25m     172.16.192.82    controller-0   <none>           <none>
osh-openstack-rabbitmq-rabbitmq-0                    1/1     Running     0          25m     172.16.192.127   controller-0   <none>           <none>
panko-api-787d9c8b69-m7qng                           1/1     Running     0          4m48s   172.16.192.76    controller-0   <none>           <none>
panko-db-init-6b6hq                                  0/1     Completed   0          4m48s   172.16.192.85    controller-0   <none>           <none>
panko-db-sync-8tl8n                                  0/1     Completed   0          4m48s   172.16.192.72    controller-0   <none>           <none>
panko-ks-endpoints-kv9p5                             0/3     Completed   0          4m48s   172.16.192.79    controller-0   <none>           <none>
panko-ks-service-588nw                               0/1     Completed   0          4m48s   172.16.192.119   controller-0   <none>           <none>
panko-ks-user-8zszn                                  0/1     Completed   0          4m48s   172.16.192.82    controller-0   <none>           <none>
placement-ks-endpoints-nmrnz                         0/3     Completed   0          21m     172.16.192.101   controller-0   <none>           <none>
placement-ks-service-nqxgd                           0/1     Completed   0          21m     172.16.192.124   controller-0   <none>           <none>
placement-ks-user-k5q9d                              0/1     Completed   0          21m     172.16.192.66    controller-0   <none>           <none>
rbd-provisioner-569784655f-bdt66                     1/1     Running     0          26m     172.16.192.99    controller-0   <none>           <none>
rbd-provisioner-storage-init-d996n                   0/1     Completed   0          26m     172.16.192.120   controller-0   <none>           <none>
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pvc --all-namespaces
NAMESPACE   NAME                                              STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
openstack   mysql-data-mariadb-server-0                       Bound    pvc-e4a64525-71c1-11e9-a337-5254000cf013   5Gi        RWO            general        30m
openstack   rabbitmq-data-osh-openstack-rabbitmq-rabbitmq-0   Bound    pvc-0adb5131-71c2-11e9-a337-5254000cf013   1Gi        RWO            general        29m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pv --all-namespaces
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                       STORAGECLASS   REASON   AGE
pvc-0adb5131-71c2-11e9-a337-5254000cf013   1Gi        RWO            Delete           Bound    openstack/rabbitmq-data-osh-openstack-rabbitmq-rabbitmq-0   general                 30m
pvc-e4a64525-71c1-11e9-a337-5254000cf013   5Gi        RWO            Delete           Bound    openstack/mysql-data-mariadb-server-0                       general                 31m
```
