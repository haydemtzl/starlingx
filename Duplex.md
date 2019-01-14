# Controller-0 Host Provision

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
| id               | 9a4e7585-9b46-4b1a-860d-e2a00304009d |
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
| id               | 78a374e6-7235-4c81-a296-be8b1d1bd321 |
| maximum          | 400                                  |
| minimum          | 100                                  |
| name             | providernet-a-range1                 |
| project_id       | 55d931306602491c907e12c91a0b1695     |
| providernet_id   | 9a4e7585-9b46-4b1a-860d-e2a00304009d |
| providernet_name | providernet-a                        |
| providernet_type | vlan                                 |
| shared           | False                                |
| tenant_id        | 55d931306602491c907e12c91a0b1695     |
+------------------+--------------------------------------+
```

## Providing Data Interfaces on Controller-0

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-list -a controller-0
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| uuid                                 | name    | class    | type     | vlan | ports        | uses | used | attributes | provider networks |
|                                      |         |          |          | id   |              | i/f  | by   |            |                   |
|                                      |         |          |          |      |              |      | i/f  |            |                   |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
| 49fe3b1f-4519-4276-bbba-bf4fcd060f55 | enp2s2  | platform | ethernet | None | [u'enp2s2']  | []   | []   | MTU=1500   | None              |
| c6ac5f95-6a58-4f82-8e17-7d328166af5a | eth1000 | None     | ethernet | None | [u'eth1000'] | []   | []   | MTU=1500   | None              |
| c7a19cef-cb05-46a0-9521-193e9e485186 | enp2s1  | platform | ethernet | None | [u'enp2s1']  | []   | []   | MTU=1500   | None              |
| ff0ff048-fdbd-434d-ba78-90fd60008705 | eth1001 | None     | ethernet | None | [u'eth1001'] | []   | []   | MTU=1500   | None              |
+--------------------------------------+---------+----------+----------+------+--------------+------+------+------------+-------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c data controller-0 eth1000 -p providernet-a
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| ifname           | eth1000                              |
| iftype           | ethernet                             |
| ports            | [u'eth1000']                         |
| providernetworks | providernet-a                        |
| imac             | 52:54:00:07:03:da                    |
| imtu             | 1500                                 |
| ifclass          | data                                 |
| networks         |                                      |
| aemode           | None                                 |
| schedpolicy      | None                                 |
| txhashpolicy     | None                                 |
| uuid             | c6ac5f95-6a58-4f82-8e17-7d328166af5a |
| ihost_uuid       | 006b8955-68aa-45c2-96e8-510cbf74c03b |
| vlan_id          | None                                 |
| uses             | []                                   |
| used_by          | []                                   |
| created_at       | 2019-01-14T10:13:57.525472+00:00     |
| updated_at       | 2019-01-14T15:39:15.374889+00:00     |
| sriov_numvfs     | 0                                    |
| ipv4_mode        | disabled                             |
| ipv6_mode        | disabled                             |
| accelerated      | [True]                               |
+------------------+--------------------------------------+
```

## Configuring Cinder on Controller Disk

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| uuid                                 | device_no | device_ | device_ | size_ | available_ | rpm          | serial_ | device_path                 |
|                                      | de        | num     | type    | gib   | gib        |              | id      |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
| 09f40d22-7f86-42e1-a827-2941b2488cfb | /dev/sda  | 2048    | HDD     | 600.0 | 360.976    | Undetermined | QM00001 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-1.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 6cb523be-b97c-4de9-a974-6de851258359 | /dev/sdb  | 2064    | HDD     | 200.0 | 199.997    | Undetermined | QM00003 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-2.0             |
|                                      |           |         |         |       |            |              |         |                             |
| 182c5d4b-2482-44f5-bee7-bf45e8156016 | /dev/sdc  | 2080    | HDD     | 200.0 | 199.997    | Undetermined | QM00005 | /dev/disk/by-path/pci-0000: |
|                                      |           |         |         |       |            |              |         | 00:1f.2-ata-3.0             |
|                                      |           |         |         |       |            |              |         |                             |
+--------------------------------------+-----------+---------+---------+-------+------------+--------------+---------+-----------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 cinder-volumes
+-----------------------+--------------------------------------+
| Property              | Value                                |
+-----------------------+--------------------------------------+
| lvm_vg_name           | cinder-volumes                       |
| vg_state              | adding                               |
| uuid                  | dadc4fec-9e60-44a6-a54a-832af252e9b3 |
| ihost_uuid            | 006b8955-68aa-45c2-96e8-510cbf74c03b |
| lvm_vg_access         | None                                 |
| lvm_max_lv            | 0                                    |
| lvm_cur_lv            | 0                                    |
| lvm_max_pv            | 0                                    |
| lvm_cur_pv            | 0                                    |
| lvm_vg_size_gib       | 0.0                                  |
| lvm_vg_avail_size_gib | 0.0                                  |
| lvm_vg_total_pe       | 0                                    |
| lvm_vg_free_pe        | 0                                    |
| created_at            | 2019-01-14T15:40:15.519664+00:00     |
| updated_at            | None                                 |
| parameters            | {u'lvm_type': u'thin'}               |
+-----------------------+--------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 6cb523be-b97c-4de9-a974-6de851258359 199 -t lvm_phys_vol
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
| uuid        | ab3beaf8-484c-4b2e-9317-59377220a73d             |
| ihost_uuid  | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| idisk_uuid  | 6cb523be-b97c-4de9-a974-6de851258359             |
| ipv_uuid    | None                                             |
| status      | Creating                                         |
| created_at  | 2019-01-14T15:41:07.634942+00:00                 |
| updated_at  | None                                             |
+-------------+--------------------------------------------------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 6cb523be-b97c-4de9-a974-6de851258359
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| uuid                                 | device_path                 | device_nod | type_guid                            | type_ | size_ | status   |
|                                      |                             | e          |                                      | name  | gib   |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
| ab3beaf8-484c-4b2e-9317-59377220a73d | /dev/disk/by-path/pci-0000: | /dev/sdb1  | ba5eba11-0000-1111-2222-000000000001 | None  | 199.0 | Creating |
|                                      | 00:1f.2-ata-2.0-part1       |            |                                      |       |       |          |
|                                      |                             |            |                                      |       |       |          |
+--------------------------------------+-----------------------------+------------+--------------------------------------+-------+-------+----------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 6cb523be-b97c-4de9-a974-6de851258359
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| uuid                                 | device_path                                      | device_node | type_guid                            | type_name           | size_gib | status |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
| ab3beaf8-484c-4b2e-9317-59377220a73d | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 | /dev/sdb1   | ba5eba11-0000-1111-2222-000000000001 | LVM Physical Volume | 199.0    | Ready  |
+--------------------------------------+--------------------------------------------------+-------------+--------------------------------------+---------------------+----------+--------+
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 cinder-volumes ab3beaf8-484c-4b2e-9317-59377220a73d
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 4b60934d-5718-4fc7-bd52-da3092b0dab0             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | ab3beaf8-484c-4b2e-9317-59377220a73d             |
| disk_or_part_device_node | /dev/sdb1                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part1 |
| lvm_pv_name              | /dev/sdb1                                        |
| lvm_vg_name              | cinder-volumes                                   |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 006b8955-68aa-45c2-96e8-510cbf74c03b             |
| created_at               | 2019-01-14T15:43:13.764346+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
```

## Adding an LVM Storage Backend at Installation

```

```
