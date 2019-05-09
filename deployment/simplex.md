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
