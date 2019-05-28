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
[wrsroot@controller-0 ~(keystone_admin)]$ ceph mon_status | python -m json.tool
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

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph df
GLOBAL:
    SIZE        AVAIL       RAW USED     %RAW USED 
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

## StarlingX CEPH Basics

1. Upload Cirros Image

## StarlingX CEPH Test Cases

### STOR_CORE_015

