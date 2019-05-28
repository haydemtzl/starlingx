# CEPH Validation

## Resources

- [Feature Testing - stx.2.0](https://docs.google.com/spreadsheets/d/15us6HWgcb0dmHHZe2SyOR_UI5BvVpoU8TCST0rQd4zg)

## StarlingX Cluster

1. Access StarlingX Console
2. And access StarlingX UI

```sh
$ google-chrome --no-proxy-server &
```

## CEPH Basics

- http://docs.ceph.com/docs/mimic/rados/operations/monitoring/

### Monitoring a Cluster

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph health
HEALTH_OK
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph status
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.81 k objects, 1.7 GiB
    usage:   2.1 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     856 active+clean
 
  io:
    client:   228 KiB/s wr, 0 op/s rd, 50 op/s wr

```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph quorum_status | python -m json.tool
{
    "election_epoch": 8,
    "monmap": {
        "created": "2019-05-22 12:38:07.817475",
        "epoch": 2,
        "features": {
            "optional": [],
            "persistent": [
                "kraken",
                "luminous",
                "mimic",
                "osdmap-prune"
            ]
        },
        "fsid": "ea5b5cfa-c8f4-454f-9b8a-92afb56973ac",
        "modified": "2019-05-22 14:51:15.519776",
        "mons": [
            {
                "addr": "10.10.58.3:6789/0",
                "name": "controller-0",
                "public_addr": "10.10.58.3:6789/0",
                "rank": 0
            },
            {
                "addr": "10.10.58.4:6789/0",
                "name": "controller-1",
                "public_addr": "10.10.58.4:6789/0",
                "rank": 1
            },
            {
                "addr": "10.10.58.125:6789/0",
                "name": "storage-0",
                "public_addr": "10.10.58.125:6789/0",
                "rank": 2
            }
        ]
    },
    "quorum": [
        0,
        1,
        2
    ],
    "quorum_leader_name": "controller-0",
    "quorum_names": [
        "controller-0",
        "controller-1",
        "storage-0"
    ]
}
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon_status | python -m json.tool                                                                                        [44/1298]
{
    "election_epoch": 8,
    "extra_probe_peers": [
        "10.10.58.125:6789/0"
    ],
    "feature_map": {
        "client": [
            {
                "features": "0x3ffddff8ffa4fffb",
                "num": 9,
                "release": "luminous"
            }
        ],
        "mgr": [
            {
                "features": "0x3ffddff8ffa4fffb",
                "num": 1,
                "release": "luminous"
            }
        ],
        "mon": [
            {
                "features": "0x3ffddff8ffa4fffb",
                "num": 1,
                "release": "luminous"
            }
        ]
    },
    "features": {
        "quorum_con": "4611087854031142907",
        "quorum_mon": [
            "kraken",
            "luminous",
            "mimic",
            "osdmap-prune"
        ],
        "required_con": "144115738102218752",
        "required_mon": [
            "kraken",
            "luminous",
            "mimic",
            "osdmap-prune"
        ]
    },
    "monmap": {
        "created": "2019-05-22 12:38:07.817475",
        "epoch": 2,
        "features": {
            "optional": [],
            "persistent": [
                "kraken",
                "luminous",
                "mimic",
                "osdmap-prune"
            ]
        },
        "fsid": "ea5b5cfa-c8f4-454f-9b8a-92afb56973ac",
        "modified": "2019-05-22 14:51:15.519776",
        "mons": [
            {
                "addr": "10.10.58.3:6789/0",
                "name": "controller-0",
                "public_addr": "10.10.58.3:6789/0",
                "rank": 0
            },
            {
                "addr": "10.10.58.4:6789/0",
                "name": "controller-1",
                "public_addr": "10.10.58.4:6789/0",
                "rank": 1
            },
            {
                "addr": "10.10.58.125:6789/0",
                "name": "storage-0",
                "public_addr": "10.10.58.125:6789/0",
                "rank": 2
            }
        ]
    },
    "name": "controller-0",
    "outside_quorum": [],
    "quorum": [
        0,
        1,
        2
    ],
    "rank": 0,
    "state": "leader",
    "sync_provider": []
}
```

#### Checking a Cluster’s Usage Stats

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph df
GLOBAL:
    SIZE        AVAIL       RAW USED     %RAW USED 
    1.3 TiB     1.3 TiB      2.1 GiB          0.16 
POOLS:
    NAME                    ID     USED        %USED     MAX AVAIL     OBJECTS 
    .rgw.root               1      1.1 KiB         0       1.2 E        AVAIL       RAW USED     %RAW USED 
    1.3 TiB     1.3 TiB      2.1 GiB          0.16 
POOLS:
    NAME                    ID     USED        %USED     MAX AVAIL     OBJECTS 
    .rgw.root               1      1.1 KiB         0       1.2 TiB           4 
    kube-rbd                2      1.7 GiB      0.13       1.2 TiB         584 
    default.rgw.control     3          0 B         0       1.2 TiB           8 
    default.rgw.meta        4          0 B         0       1.2 TiB           0 
    default.rgw.log         5          0 B         0       1.2 TiB        1152 
    images                  6         19 B         0       1.2 TiB           2 
    ephemeral               7          0 B         0       1.2 TiB           0 
    cinder-volumes          8          0 B         0       1.2 TiB           0 
    gnocchi.metrics         9      378 KiB         0       1.2 TiB          63 
```

#### Checking OSD Status

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd stat
3 osds: 3 up, 3 in; epoch: e83
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd dump
epoch 83
fsid ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
created 2019-05-22 14:36:59.614395
modified 2019-05-28 13:47:50.832249
flags sortbitwise,recovery_deletes,purged_snapdirs
crush_version 12
full_ratio 0.95
backfillfull_ratio 0.9
nearfull_ratio 0.85
require_min_compat_client jewel
min_compat_client firefly
require_osd_release mimic
pool 1 '.rgw.root' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 53 flags hashpspool stripe_width 0 application rgw
pool 2 'kube-rbd' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 54 flags hashpspool,selfmanaged_snaps stripe_width 0 application rbd
        removed_snaps [1~5]
pool 3 'default.rgw.control' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 55 flags hashpspool stripe_width 0 application rgw
pool 4 'default.rgw.meta' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 56 flags hashpspool stripe_width 0 application rgw
pool 5 'default.rgw.log' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 57 flags hashpspool stripe_width 0 application rgw
pool 6 'images' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 8 pgp_num 8 last_change 82 flags hashpspool,selfmanaged_snaps ect_hash rjenkins pg_num 64 pgp_num 64 last_change 57 flags hashpspool stripe_width 0 application rgw
pool 6 'images' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 8 pgp_num 8 last_change 82 flags hashpspool,selfmanaged_snaps stripe_width 0 application glance-image
        removed_snaps [1~5]
pool 7 'ephemeral' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 512 pgp_num 512 last_change 59 flags hashpspool stripe_width 0 application nova-ephemeral
pool 8 'cinder-volumes' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 8 pgp_num 8 last_change 60 flags hashpspool stripe_width 0 application cinder-volume
pool 9 'gnocchi.metrics' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 8 pgp_num 8 last_change 61 flags hashpspool stripeash rjenkins pg_num 8 pgp_num 8 last_change 60 flags hashpspool stripe_width 0 application cinder-volume
pool 9 'gnocchi.metrics' replicated size 1 min_size 1 crush_rule 0 object_hash rjenkins pg_num 8 pgp_num 8 last_change 61 flags hashpspool stripe_width 0 application gnocchi-metrics
max_osd 3
osd.0 up   in  weight 1 up_from 12 up_thru 75 down_at 0 last_clean_interval [0,0) 10.10.58.125:6800/37513 10.10.58.125:6801/37513 10.10.58.125:6802/37513 10.10.58.125:6803/37513 exists,up 00ab78be-2555-_width 0 application gnocchi-metrics
max_osd 3
osd.0 up   in  weight 1 up_from 12 up_thru 75 down_at 0 last_clean_interval [0,0) 10.10.58.125:6800/37513 10.10.58.125:6801/37513 10.10.58.125:6802/37513 10.10.58.125:6803/37513 exists,up 00ab78be-2555-4e4c-88ac-01fb712fa912
osd.1 up   in  weight 1 up_from 25 up_thru 75 down_at 0 last_clean_interval [0,0) 10.10.58.12:6800/36072 10.10.58.12:6801/36072 10.10.58.12:6802/36072 10.10.58.12:6803/36072 exists,up 193d22b4-99a3-4203-9aa7-93ec87ff4828
osd.24e4c-88ac-01fb712fa912
osd.1 up   in  weight 1 up_from 25 up_thru 75 down_at 0 last_clean_interval [0,0) 10.10.58.12:6800/36072 10.10.58.12:6801/36072 10.10.58.12:6802/36072 10.10.58.12:6803/36072 exists,up 193d22b4-99a3-4203-9aa7-93ec87ff4828
osd.2 up   in  weight 1 up_from 70 up_thru 76 down_at 0 last_clean_interval [0,0) 10.10.58.36:6800/33272 10.10.58.36:6801/33272 10.10.58.36:6802/33272 10.10.58.36:6803/33272 exists,up 52ae1c0a-78b6-4d09-9cbb-612470f0da7c
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd tree
ID  CLASS WEIGHT  TYPE NAME              STATUS REWEIGHT PRI-AFF 
 -1       1.30646 root storage-tier                              
 -3       0.87097     chassis group-0                            
 -4       0.43549         host storage-0                         
  0   ssd 0.43549             osd.0          up  1.00000 1.00000 
 -5       0.43549         host storage-1                         
  1   ssd 0.43549             osd.1          up  1.00000 1.00000 
-10       0.43549     chassis group-1                            
 -9       0.43549         host storage-2                         
  2   ssd 0.43549             osd.2          up  1.00000 1.00000 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls
.rgw.root
kube-rbd
default.rgw.control
default.rgw.meta
default.rgw.log
images
ephemeral
cinder-volumes
gnocchi.metrics
```

#### Checking Monitor Status

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon stat
e2: 3 mons at {controller-0=10.10.58.3:6789/0,controller-1=10.10.58.4:6789/0,storage-0=10.10.58.125:6789/0}, election epoch 8, leader 0 controller-0, quorum 0,1,2 controller-0,controller-1,storage-0
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon dump
dumped monmap epoch 2
epoch 2
fsid ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
last_changed 2019-05-22 14:51:15.519776
created 2019-05-22 12:38:07.817475
0: 10.10.58.3:6789/0 mon.controller-0
1: 10.10.58.4:6789/0 mon.controller-1
2: 10.10.58.125:6789/0 mon.storage-0
```

```sh
olrsroot@controller-0 ~(keystone_admin)]$ ceph quorum_status | python -m json.too
{
    "election_epoch": 8,
    "monmap": {
        "created": "2019-05-22 12:38:07.817475",
        "epoch": 2,
        "features": {
            "optional": [],
            "persistent": [
                "kraken",
                "luminous",
                "mimic",
                "osdmap-prune"
            ]
        },
        "fsid": "ea5b5cfa-c8f4-454f-9b8a-92afb56973ac",
        "modified": "2019-05-22 14:51:15.519776",
        "mons": [
            {
                "addr": "10.10.58.3:6789/0",
                "name": "controller-0",
                "public_addr": "10.10.58.3:6789/0",
                "rank": 0
            },
            {
                "addr": "10.10.58.4:6789/0",
                "name": "controller-1",
                "public_addr": "10.10.58.4:6789/0",
                "rank": 1
            },
            {
                "addr": "10.10.58.125:6789/0",
                "name": "storage-0",
                "public_addr": "10.10.58.125:6789/0",
                "rank": 2
            }
        ]
    },
    "quorum": [
        0,
        1,
        2
    ],
    "quorum_leader_name": "controller-0",
    "quorum_names": [
        "controller-0",
        "controller-1",
        "storage-0"
    ]
}
```

#### Checking MDS Status

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mds stat
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph fs dump
dumped fsmap epoch 1
e1
enable_multiple, ever_enabled_multiple: 0,0
compat: compat={},rocompat={},incompat={1=base v0.20,2=client writeable ranges,3=default file layouts on dirs,4=dir inode in separate object,5=mds uses versioned encoding,6=dirfrag is stored in omap,8=no anchor table,9=file layout v2,10=snaprealm v2}
legacy client fscid: -1
 
No filesystems configured
```

## StarlingX CEPH Basics

1. Upload Cirros Image

## StarlingX CEPH Test Cases

### STOR_CORE_015

### STOR_TIER_005

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system cluster-list
+--------------------------------------+--------------------------------------+------+--------------+------------------+
| uuid                                 | cluster_uuid                         | type | name         | deployment_model |
+--------------------------------------+--------------------------------------+------+--------------+------------------+
| 1093b1fd-99f0-496d-bfd8-e751b98b04bb | ea5b5cfa-c8f4-454f-9b8a-92afb56973ac | ceph | ceph_cluster | storage-nodes    |
+--------------------------------------+--------------------------------------+------+--------------+------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-tier-list 1093b1fd-99f0-496d-bfd8-e751b98b04bb
+--------------------------------------+---------+--------+--------------------------------------+
| uuid                                 | name    | status | backend_using                        |
+--------------------------------------+---------+--------+--------------------------------------+
| aae84629-2518-4b0a-a7e0-0fc3e759b918 | storage | in-use | 6ae8265d-40cc-4e30-83b9-7bf188e3a59b |
+--------------------------------------+---------+--------+--------------------------------------+
```

### STOR_TIER_006


#### storage-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-0
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_p------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 00ab78be-2555-4e4c-88ac-01fb712fa912 | osd      | 0     | configured----------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 00ab78be-2555-4e4c-88ac-01fb712fa912 | osd      | 0     | configured | b47d3cdb-95d1-425f-9969-58a623ab618f | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
[wrsroot@co               | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-0
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| 3a23f96f-5a2f-4d5d-9ea6-520722d930bc | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CU0480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| b47d3cdb-95d1-425f-9969-58a623ab618f | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CUP480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 20550907-f594-4290-a0a3-de2740702835 | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYS8164013J480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| 0e7470c2-066a-42d6-aea9-ee44e9ac4da3 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTLT70400321480EGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### storage-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-1
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 193d22b4-99a3-4203-9aa7-93ec87ff4828 | osd      | 1     | configured | 878661cc-1081-4923-a563-b55990488ebb | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-1
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| b917b5eb-f0e1-4d69-a04f-0612fac5802f | /dev/sda    | 2048       | SSD         | 447.13 -------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| b917b5eb-f0e1-4d69-a04f-0612fac5802f | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81460CU4480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| 878661cc-1081-4923-a563-b55990488ebb | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81460H0P480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 25e86c9e-ac05-4e3f-8285-b6c5ae9dad56 | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYF83150D4L480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| edeb5f5a-a297-4d88-8df7-b903963ff1a2 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTYS818001DG480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### storage-2

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-list storage-2
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                     | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
| 52ae1c0a-78b6-4d09-9cbb-612470f0da7c | osd      | 2     | configured | a3012a1c-cf66-4fc4-9fe5-6520fd135c23 | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+--------------------------------------------------+--------------+------------------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list storage-2
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm | serial_id          | device_path                                |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
| d34ee920-f5ef-4e75-a689-8461f98244af | /dev/sda    | 2048       | SSD         | 447.13   | 0.0           | N/A | BTYS81800LTF480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-1.0 |
| a3012a1c-cf66-4fc4-9fe5-6520fd135c23 | /dev/sdb    | 2064       | SSD         | 447.13   | 0.0           | N/A | BTYS81800LSV480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-2.0 |
| 781dde8f-5c34-4d14-8152-a489a55376ce | /dev/sdc    | 2080       | SSD         | 447.13   | 447.128       | N/A | BTYF83200B5F480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-3.0 |
| 8de1a787-eef4-4604-9210-75744a389a17 | /dev/sdd    | 2096       | SSD         | 447.13   | 447.128       | N/A | BTYF82910BWY480BGN | /dev/disk/by-path/pci-0000:00:17.0-ata-4.0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+-----+--------------------+--------------------------------------------+
```

#### Storage Backend List

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
| uuid                                 | name            | backend  | state      | task                | services | capabilities       |
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
| 53eb253b-effb-4977-930c-d887ba58edc7 | shared_services | external | configured | None                | glance   |                    |
| 6ae8265d-40cc-4e30-83b9-7bf188e3a59b | ceph-store      | ceph     | configured | reconfig-controller | None     | min_replication: 1 |
|                                      |                 |          |            |                     |          | replication: 2     |
+--------------------------------------+-----------------+----------+------------+---------------------+----------+--------------------+
```

