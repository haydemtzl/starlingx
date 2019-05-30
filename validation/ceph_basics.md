# CEPH Basics

- http://docs.ceph.com/docs/mimic/rados/operations/monitoring/

## Monitoring a Cluster

Bare Metal

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph health
HEALTH_OK
```

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph health
HEALTH_OK
```

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph status
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   225 MiB used, 398 GiB / 398 GiB avail
    pgs:     256 active+clean
```

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph quorum_status | python -m json.tool
{
    "election_epoch": 10,
    "monmap": {
        "created": "2019-05-29 13:57:39.414613",
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
        "fsid": "e8ab94c1-8878-40bd-ac05-381366f91e97",
        "modified": "2019-05-29 14:47:17.152717",
        "mons": [
            {
                "addr": "192.168.204.3:6789/0",
                "name": "controller-0",
                "public_addr": "192.168.204.3:6789/0",
                "rank": 0
            },
            {
                "addr": "192.168.204.4:6789/0",
                "name": "controller-1",
                "public_addr": "192.168.204.4:6789/0",
                "rank": 1
            },
            {
                "addr": "192.168.204.9:6789/0",
                "name": "storage-0",
                "public_addr": "192.168.204.9:6789/0",
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

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon_status | python -m json.tool 
{
    "election_epoch": 10,
    "extra_probe_peers": [
        "192.168.204.9:6789/0"
    ],
    "feature_map": {
        "client": [
            {
                "features": "0x3ffddff8ffa4fffb",
                "num": 2,
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
        "created": "2019-05-29 13:57:39.414613",
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
        "fsid": "e8ab94c1-8878-40bd-ac05-381366f91e97",
        "modified": "2019-05-29 14:47:17.152717",
        "mons": [
            {
                "addr": "192.168.204.3:6789/0",
                "name": "controller-0",
                "public_addr": "192.168.204.3:6789/0",
                "rank": 0
            },
            {
                "addr": "192.168.204.4:6789/0",
                "name": "controller-1",
                "public_addr": "192.168.204.4:6789/0",
                "rank": 1
            },
            {
                "addr": "192.168.204.9:6789/0",
                "name": "storage-0",
                "public_addr": "192.168.204.9:6789/0",
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

### Checking a Cluster’s Usage Stats

Bare Metal

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

Virtual ? Step "(Optional) Setup Remote Storage" Missing

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph df
GLOBAL:
    SIZE        AVAIL       RAW USED     %RAW USED 
    398 GiB     398 GiB      226 MiB          0.06 
POOLS:
    NAME                    ID     USED        %USED     MAX AVAIL     OBJECTS 
    .rgw.root               1      1.1 KiB         0       189 GiB           4 
    default.rgw.control     2          0 B         0       189 GiB           8 
    default.rgw.meta        3          0 B         0       189 GiB           0 
    default.rgw.log         4          0 B         0       189 GiB        1120 
    kube-rbd                5          0 B         0       189 GiB           0 
```

### Checking OSD Status

Bare Metal

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd stat
3 osds: 3 up, 3 in; epoch: e83
```

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd stat
2 osds: 2 up, 2 in; epoch: e26
```

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd dump
epoch 26
fsid e8ab94c1-8878-40bd-ac05-381366f91e97
created 2019-05-29 14:28:33.487653
modified 2019-05-29 14:52:22.725281
flags sortbitwise,recovery_deletes,purged_snapdirs
crush_version 6
full_ratio 0.95
backfillfull_ratio 0.9
nearfull_ratio 0.85
require_min_compat_client jewel
min_compat_client firefly
require_osd_release mimic
pool 1 '.rgw.root' replicated size 2 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 3 flags hashpspool stripe_width 0 application rgw
pool 2 'default.rgw.control' replicated size 2 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 13 flags hashpspool stripe_width 0 application rgw
pool 3 'default.rgw.meta' replicated size 2 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 15 flags hashpspool stripe_width 0 application rgw
pool 4 'default.rgw.log' replicated size 2 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 17 flags hashpspool stripe_width 0 application rgw
pool 5 'kube-rbd' replicated size 2 min_size 1 crush_rule 0 object_hash rjenkins pg_num 64 pgp_num 64 last_change 26 flags hashpspool stripe_width 0 application rbd
max_osd 2
osd.0 up   in  weight 1 up_from 9 up_thru 23 down_at 0 last_clean_interval [0,0) 192.168.204.9:6800/25317 192.168.204.9:6801/25317 192.168.204.9:6802/25317 192.168.204.9:6803/25317 exists,up 620ae832-acf9-4029-8040-ed78ff585da2
osd.1 up   in  weight 1 up_from 21 up_thru 23 down_at 0 last_clean_interval [0,0) 192.168.204.14:6800/23907 192.168.204.14:6801/23907 192.168.204.14:6802/23907 192.168.204.14:6803/23907 exists,up fea553fe-dd9a-4962-baa5-2710ec3c4669
```

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd tree
ID CLASS WEIGHT  TYPE NAME              STATUS REWEIGHT PRI-AFF 
-1       0.38840 root storage-tier                              
-3       0.38840     chassis group-0                            
-4       0.19420         host storage-0                         
 0   hdd 0.19420             osd.0          up  1.00000 1.00000 
-5       0.19420         host storage-1                         
 1   hdd 0.19420             osd.1          up  1.00000 1.00000 
```

In Dedicated Storage, Baremetal:

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

In Simplex, Virtual:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls
.rgw.root
default.rgw.control
default.rgw.meta
default.rgw.log
kube-rbd
```

### Checking Monitor Status

Bare Metal

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon stat
e2: 3 mons at {controller-0=10.10.58.3:6789/0,controller-1=10.10.58.4:6789/0,storage-0=10.10.58.125:6789/0}, election epoch 8, leader 0 controller-0, quorum 0,1,2 controller-0,controller-1,storage-0
```

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon stat
e2: 3 mons at {controller-0=192.168.204.3:6789/0,controller-1=192.168.204.4:6789/0,storage-0=192.168.204.9:6789/0}, election epoch 10, leader 0 controller-0, quorum 0,1,2 controller-0,controller-1,storage-0
```

Bare Metal

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

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon dump
dumped monmap epoch 2
epoch 2
fsid e8ab94c1-8878-40bd-ac05-381366f91e97
last_changed 2019-05-29 14:47:17.152717
created 2019-05-29 13:57:39.414613
0: 192.168.204.3:6789/0 mon.controller-0
1: 192.168.204.4:6789/0 mon.controller-1
2: 192.168.204.9:6789/0 mon.storage-0
```

Bare Metal

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

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph quorum_status | python -m json.tool
{
    "election_epoch": 10,
    "monmap": {
        "created": "2019-05-29 13:57:39.414613",
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
        "fsid": "e8ab94c1-8878-40bd-ac05-381366f91e97",
        "modified": "2019-05-29 14:47:17.152717",
        "mons": [
            {
                "addr": "192.168.204.3:6789/0",
                "name": "controller-0",
                "public_addr": "192.168.204.3:6789/0",
                "rank": 0
            },
            {
                "addr": "192.168.204.4:6789/0",
                "name": "controller-1",
                "public_addr": "192.168.204.4:6789/0",
                "rank": 1
            },
            {
                "addr": "192.168.204.9:6789/0",
                "name": "storage-0",
                "public_addr": "192.168.204.9:6789/0",
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

### Checking MDS Status

Bare Metal

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mds stat
```

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mds stat
```

Bare Metal

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph fs dump
dumped fsmap epoch 1
e1
enable_multiple, ever_enabled_multiple: 0,0
compat: compat={},rocompat={},incompat={1=base v0.20,2=client writeable ranges,3=default file layouts on dirs,4=dir inode in separate object,5=mds uses versioned encoding,6=dirfrag is stored in omap,8=no anchor table,9=file layout v2,10=snaprealm v2}
legacy client fscid: -1
 
No filesystems configured
```

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph fs dump
dumped fsmap epoch 1
e1
enable_multiple, ever_enabled_multiple: 0,0
compat: compat={},rocompat={},incompat={1=base v0.20,2=client writeable ranges,3=default file layouts on dirs,4=dir inode in separate object,5=mds uses versioned encoding,6=dirfrag is stored in omap,8=no anchor table,9=file layout v2,10=snaprealm v2}
legacy client fscid: -1
 
No filesystems configured
```

## Other Checks

Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ rados df
POOL_NAME              USED OBJECTS CLONES COPIES MISSING_ON_PRIMARY UNFOUND DEGRADED RD_OPS      RD WR_OPS    WR 
.rgw.root           1.1 KiB       4      0      8                  0       0        0      0     0 B      4 4 KiB 
default.rgw.control     0 B       8      0     16                  0       0        0      0     0 B      0   0 B 
default.rgw.log         0 B    1120      0   2240                  0       0        0   9651 8.3 MiB   6434   0 B 
default.rgw.meta        0 B       0      0      0                  0       0        0      0     0 B      0   0 B 
kube-rbd                0 B       0      0      0                  0       0        0      0     0 B      0   0 B 

total_objects    1132
total_used       226 MiB
total_avail      398 GiB
total_space      398 GiB


[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck inactive
ok
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck unclean
ok
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck stale
ok


[wrsroot@controller-0 ~(keystone_admin)]$ ceph health detail
HEALTH_OK



[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} size
size: 2
size: 2
size: 2
size: 2
size: 2


[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} pg_num
pg_num: 64
pg_num: 64
pg_num: 64
pg_num: 64
pg_num: 64


[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} pgp_num
pgp_num: 64
pgp_num: 64
pgp_num: 64
pgp_num: 64
pgp_num: 64
```
