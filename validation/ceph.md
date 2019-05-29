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

### STOR_CORE_*

> __STOR_CORE_014__ The objective of this test is to ensure that host reinstall of nodes running ceph-mon works properly on all supported configs.

> __STOR_CORE_015__ The objective of this test is to ensure that host delete and reprovision of nodes running ceph-mon works properly on all supported configs.

> __STOR_CORE_016__ The objective of this test is to ensure that semantic checks with respect to node lock, work properly on nodes running ceph monitors.

#### STOR_CORE_014

1. Lock one of the nodes that are part of a ceph-system. e.g. controller-0 on an All-in-One Duplex system, controller-0 on a standard system, or storage-0 on a ceph storage system.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 6  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock storage-0                                                                                                            
```

1. Initiate a host re-install

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-reinstall storage-0
```

In Horizon: Pre-Install ... Installing Packages (23%) ... Post-Install

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            Reduced data availability: 284 pgs stale
            1/3 mons down, quorum controller-0,cont  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            Reduced data availability: 284 pgs stale
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 2 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.82 k objects, 1.7 GiB
    usage:   2.0 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   1023 B/s wr, 0 op/s rd, 0 op/s wr
 
```

3. Ensure the host comes online after reinstall.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.82 k objects, 1.7 GiB
    usage:   1.4 GiB used, 890 GiB / 892 GiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   341 B/s rd, 0 op/s rd, 0 op/s wr

```

4. Unlock the host

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
```

5. Ensure the host eventually becomes available

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

But controller-0 is degraded. Alarms?

```sh
200.006	controller-1 is degraded due to the failure of its 'ceph' process. Auto recovery of this major process is in progress.	host=controller-1.process=ceph	major	2019-05-28T16:17:55.608239
```

```sh
800.001	Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please check 'ceph -s' for more details.	cluster=ea5b5cfa-c8f4-454f-9b8a-92afb56973ac	warning	2019-05-28T16:05:05.486930
```

Trying to recover controller-1 by locking and unlocking controller-1:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-1
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock controller-1
```

```
400.002	Service group cloud-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=cloud-services	major	2019-05-28T16:47:39.731109	
400.002	Service group controller-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=controller-services	major	2019-05-28T16:47:39.895027	
400.002	Service group directory-services loss of redundancy; expected 2 active members but only 1 active member available	service_domain=controller.service_group=directory-services	major	2019-05-28T16:47:39.242037	
400.002	Service group oam-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=oam-services	major	2019-05-28T16:47:40.057033	
400.002	Service group patching-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=patching-services	major	2019-05-28T16:47:39.406200	
400.002	Service group storage-monitoring-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=storage-monitoring-services	major	2019-05-28T16:47:38.754022	
400.002	Service group storage-services loss of redundancy; expected 2 active members but only 1 active member available	service_domain=controller.service_group=storage-services	major	2019-05-28T16:47:38.916208	
400.002	Service group vim-services loss of redundancy; expected 1 standby member but no standby members available	service_domain=controller.service_group=vim-services	major	2019-05-28T16:47:39.569023	
400.002	Service group web-services loss of redundancy; expected 2 active members but only 1 active member available	service_domain=controller.service_group=web-services	major	2019-05-28T16:47:39.079025	
800.001	Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please check 'ceph -s' for more details.	cluster=ea5b5cfa-c8f4-454f-9b8a-92afb56973ac	warning	2019-05-28T16:05:05.486930
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | disabled    | offline      |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list[527199.052290] drbd drbd-extension: PingAck did not arrive in time.
[527199.078255] drbd drbd-cgcs: PingAck did not arrive in time.
[527199.080234] drbd drbd-platform: PingAck did not arrive in time.
[527202.998096] drbd drbd-pgsql: PingAck did not arrive in time.
[527204.723966] drbd drbd-dockerdistribution: PingAck did not arrive in time.
[527204.724969] drbd drbd-rabbit: PingAck did not arrive in time.
[527205.528082] drbd drbd-etcd: PingAck did not arrive in time.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
            1/3 mons down, quorum controller-0,storage-0
 
  services:
    mon: 3 daemons, quorum controller-0,storage-0, out of quorum: controller-1
    mgr: controller-0(active)
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.82 k objects, 1.7 GiB
    usage:   1.5 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   85 B/s rd, 0 op/s rd, 0 op/s wr
 
```

Under Horizon, Testing... After ~30 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  dat           Reduced data availability: 284 pgs stale
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.82 k objects, 1.7 GiB
    usage:   1.5 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
```

```sh
800.001	Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please check 'ceph -s' for more details.	cluster=ea5b5cfa-c8f4-454f-9b8a-92afb56973ac	warning	2019-05-28T16:05:05.486930
```

After 30 minutes:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 32afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.82 k objects, 1.7 GiB
    usage:   1.5 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   170 B/s rd, 0 op/s rd, 0 op/s wr
 
```

- http://docs.ceph.com/docs/mimic/rados/troubleshooting/troubleshooting-pg/

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd lspools
1 .rgw.root
2 kube-rbd
3 default.rgw.control
4 default.rgw.meta
5 default.rgw.log
6 images
7 ephemeral
8 cinder-volumes
9 gnocchi.metrics
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ rados df
POOL_NAME              USED OBJECTS CLONES COPIES MISSING_ON_PRIMARY UNFOUND DEGRADED  RD_OPS      RD   WR_OPS      WR 
.rgw.root           1.1 KiB       4      0      4                  0       0        0      18  12 KiB        4   4 KiB 
cinder-volumes          0 B       0      0      0                  0       0        0       0     0 B        0     0 B 
default.rgw.control     0 B       8      0      8                  0       0        0       0     0 B        0     0 B 
default.rgw.log         0 B    1152      0   1152                  0       0        0 5469977 5.2 GiB  3646523     0 B 
default.rgw.meta        0 B       0      0      0                  0       0        0       0     0 B        0     0 B 
ephemeral               0 B       0      0      0                  0       0        0       0     0 B        0     0 B 
gnocchi.metrics     382 KiB      63      0     63                  0       0        0  661146 590 MiB   333669 218 MiB 
images                 19 B       2      0      2                  0       0        0     224 182 KiB       55  12 MiB 
kube-rbd            1.7 GiB     588      0    588                  0       0        0    3353  67 MiB 40847794 198 GiB 

total_objects    1817
total_used       1.5 GiB
total_avail      1.3 TiB
total_space      1.3 TiB
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck inactive
ok
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck unclean
ok
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck stale
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg dump_stuck stale | more
PG_STAT STATE              UP  UP_PRIMARY ACTING ACTING_PRIMARY 
7.1eb   stale+active+clean [0]          0    [0]              0 
7.1e2   stale+active+clean [0]          0    [0]              0 
...
...
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph health detail
HEALTH_WARN Reduced data availability: 284 pgs stale
PG_AVAILABILITY Reduced data availability: 284 pgs stale
    pg 1.2f is stuck stale for 6725.774777, current state stale+active+clean, last acting [0]
    pg 1.30 is stuck stale for 6725.774778, current state stale+active+clean, last acting [0]
    pg 1.36 is stuck stale for 6725.774784, current state stale+active+clean, last acting [0]
    pg 1.37 is stuck stale for 6725.774785, current state stale+active+clean, last acting [0]
    pg 1.3a is stuck stale for 6725.774788, current state stale+active+clean, last acting [0]
    pg 1.3b is stuck stale for 6725.774790, current state stale+active+clean, last acting [0]
    ...
    ...
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph pg 1.2 query                                                                                                                     
{
    "state": "active+clean",
    "snap_trimq": "[]",
    "snap_trimq_len": 0,
    "epoch": 96,
    "up": [
        1
    ],
...
...
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph health detail | grep stale
HEALTH_WARN Reduced data availability: 284 pgs stale
PG_AVAILABILITY Reduced data availability: 284 pgs stale
    pg 1.2f is stuck stale for 7110.113774, current state stale+active+clean, last acting [0]
    pg 1.30 is stuck stale for 7110.113774, current state stale+active+clean, last acting [0]
    pg 1.36 is stuck stale for 7110.113780, current state stale+active+clean, last acting [0]
    pg 1.37 is stuck stale for 7110.113781, current state stale+active+clean, last acting [0]
    pg 1.3a is stuck stale for 7110.113784, current state stale+active+clean, last acting [0]
...
...
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock storage-0
[wrsroot@controller-0 ~(keystone_admin)]$ system host-reboot storage-0
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool get default.rgw.log pg_num
pg_num: 64
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph osd lspools
1 .rgw.root
2 kube-rbd
3 default.rgw.control
4 default.rgw.meta
5 default.rgw.log
6 images
7 ephemeral
8 cinder-volumes
9 gnocchi.metrics
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} size
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool set {} size 5
set pool 1 size to 10
set pool 2 size to 10
set pool 3 size to 10
set pool 4 size to 10
set pool 5 size to 10
set pool 6 size to 10
Error ERANGE: pool id 7 pg_num 512 size 10 would mean 8416 total pgs, which exceeds max 6144 (mon_max_pg_per_osd 2048 * num_in_osds 3)
set pool 8 size to 10
set pool 9 size to 10
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} pg_num
pg_num: 64
pg_num: 64
pg_num: 64
pg_num: 64
pg_num: 64
pg_num: 8
pg_num: 512
pg_num: 8
pg_num: 8
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool set {} pg_num 128
set pool 1 pg_num to 128
set pool 2 pg_num to 128
set pool 3 pg_num to 128
set pool 4 pg_num to 128
set pool 5 pg_num to 128
Error E2BIG: specified pg_num 128 is too large (creating 120 new PGs on ~3 OSDs exceeds per-OSD max with mon_osd_max_split_count of 32)
Error EEXIST: specified pg_num 128 <= current 512
Error E2BIG: specified pg_num 128 is too large (creating 120 new PGs on ~3 OSDs exceeds per-OSD max with mon_osd_max_split_count of 32)
Error E2BIG: specified pg_num 128 is too large (creating 120 new PGs on ~3 OSDs exceeds per-OSD max with mon_osd_max_split_count of 32)
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool get {} pgp_num
pgp_num: 64
pgp_num: 64
pgp_num: 64
pgp_num: 64
pgp_num: 64
pgp_num: 8
pgp_num: 512
pgp_num: 8
pgp_num: 8
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool ls | xargs -i ceph osd pool set {} pgp_num 128
set pool 1 pgp_num to 128
set pool 2 pgp_num to 128
set pool 3 pgp_num to 128
set pool 4 pgp_num to 128
set pool 5 pgp_num to 128
Error EINVAL: specified pgp_num 128 > pg_num 8
set pool 7 pgp_num to 128
Error EINVAL: specified pgp_num 128 > pg_num 8
Error EINVAL: specified pgp_num 128 > pg_num 8
```

```sh
200.004	compute-1 experienced a service-affecting failure. Auto-recovery in progress. Manual Lock and Unlock may be required if auto-recovery is unsuccessful.	host=compute-1	critical	2019-05-28T18:31:01.747027	
200.005	compute-1 experienced a persistent critical 'Management Network' communication failure.	host=compute-1.network=Management	critical	2019-05-28T18:31:01.733037	
270.001	Host compute-1 compute services failure, failed to disable nova services	host=compute-1.services=compute	critical	2019-05-28T16:07:35.022505	
800.011	Loss of replication in replication group group-0: OSDs are down	cluster=ea5b5cfa-c8f4-454f-9b8a-92afb56973ac.peergroup=group-0.host=storage-1	major	2019-05-28T18:51:07.786088	
800.011	Loss of replication in replication group group-1: peer host down	cluster=ea5b5cfa-c8f4-454f-9b8a-92afb56973ac.peergroup=group-1	major	2019-05-28T16:04:04.757143	
200.006	compute-1 is degraded due to the failure of its 'pci-irq-affinity-agent' process. Auto recovery of this major process is in progress.	host=compute-1.process=pci-irq-affinity-agent	major	2019-05-28T18:50:32.852847
```

Tried:

- Lock (Fail), Unlock Nodes

Reboot controller-0

```sh
[578813.090734] watchdog: watchdog0: watchdog did not stop!
[578816.471283] libceph: connect 10.10.58.4:6789 error -101
[578820.477902] libceph: connect 10.10.58.3:6789 error -101
[578821.458981] libceph: connect 10.10.58.3:6789 error -101
[578822.456698] libceph: connect 10.10.58.3:6789 error -101
[578824.451842] libceph: connect 10.10.58.3:6789 error -101
```

Lock storage-1, storage-2, from UI:

```sh
Error: Unable to force lock host: storage-1. Reason/Action: Remote error: CephMonRestfulListKeysError Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1 [u'Traceback (most recent call last):\n', u' File "/usr/lib64/python2.7/site-packages/sysinv/openstack/common/rpc/amqp.py", line 438, in _process_data\n **args)\n', u' File "/usr/lib64/python2.7/site-packages/sysinv/openstack/common/rpc/dispatcher.py", line 172, in dispatch\n result = getattr(proxyobj, method)(ctxt, **kwargs)\n', u' File "/usr/lib64/python2.7/site-packages/sysinv/conductor/manager.py", line 5280, in get_ceph_pools_df_stats\n return self._ceph.get_pools_df_stats()\n', u' File "/usr/lib64/python2.7/site-packages/sysinv/conductor/ceph.py", line 944, in get_pools_df_stats\n timeout=timeout)\n', u' File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 1391, in df\n return self._request(\'df\', **kwargs)\n', u' File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 162, in _request\n self._get_password()\n', u' File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 86, in _get_password\n raise CephMonRestfulListKeysError(str(e))\n', u"CephMonRestfulListKeysError: Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1\n"].
```

From command line:

```sh
[wrsroot@controller-1 ~(keystone_admin)]$ system host-lock storage-1
Cannot lock a storage node when ceph pools are not empty and replication is lost. This may result in data loss. 
```

```sh
[wrsroot@controller-1 ~(keystone_admin)]$ system host-lock storage-2
Remote error: CephMonRestfulListKeysError Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1
[u'Traceback (most recent call last):\n', u'  File "/usr/lib64/python2.7/site-packages/sysinv/openstack/common/rpc/amqp.py", line 438, in _process_data\n    **args)\n', u'  File "/usr/lib64/python2.7/site-packages/sysinv/openstack/common/rpc/dispatcher.py", line 172, in dispatch\n    result = getattr(proxyobj, method)(ctxt, **kwargs)\n', u'  File "/usr/lib64/python2.7/site-packages/sysinv/conductor/manager.py", line 5280, in get_ceph_pools_df_stats\n    return self._ceph.get_pools_df_stats()\n', u'  File "/usr/lib64/python2.7/site-packages/sysinv/conductor/ceph.py", line 944, in get_pools_df_stats\n    timeout=timeout)\n', u'  File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 1391, in df\n    return self._request(\'df\', **kwargs)\n', u'  File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 162, in _request\n    self._get_password()\n', u'  File "/usr/lib/python2.7/site-packages/cephclient/client.py", line 86, in _get_password\n    raise CephMonRestfulListKeysError(str(e))\n', u"CephMonRestfulListKeysError: Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1\n"].
```

Alarms:

```sh
270.001	Host compute-1 compute services failure,	host=compute-1.services=compute	critical	2019-05-29T07:40:21.667039	
250.001	compute-1 Configuration is out-of-date.	host=compute-1	major	2019-05-29T07:40:44.070493	
200.006	compute-1 is degraded due to the failure of its 'pci-irq-affinity-agent' process. Auto recovery of this major process is in progress.	host=compute-1.process=pci-irq-affinity-agent	major	2019-05-29T07:23:41.163381	
100.119	compute-1 is not locked to remote PTP Grand Master	host=compute-1.ptp=no-lock	major	2019-05-29T07:29:29.147505
```

Force Lock compute-0

```sh
services-disable-failed
```

CEPH?

```sh
Cluster connection interrupted or timed out
[wrsroot@controller-1 ~(keystone_admin)]$ ceph status
Cluster connection interrupted or timed out
[wrsroot@controller-1 ~(keystone_admin)]$ ceph health
```

Locked, Reinstall storage-0

```sh
Error: Unable to reinstall host: storage-0. Reason/Action: Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1
```

```sh
[wrsroot@controller-1 ~(keystone_admin)]$ system host-reinstall storage-0
Failed to get ceph-mgr restful plugin keys. Command 'ceph restful list-keys --connect-timeout 5' returned non-zero exit status 1
```

6. Check that ceph reports HEALTH_OK via

```sh
ceph -s
```

7. Ensure the weights look accurate in

```sh
ceph osd tree
```

#### STOR_CORE_016

1. storage-0: Pass but additional steps involved to recover healthiness. Then another cycle was executed and it succedded without those extra steps involved.
2. controller-1: Pass

##### controller-1

See steps under section storage-0 fir details, this output is a summary of them.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock controller-1
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
```

```
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock controller-1
[wrsroot@controller-0 ~(keystone_admin)]$ [523649.791656] drbd drbd-rabbit: PingAck did not arrive in time.
[523652.389547] drbd drbd-dockerdistribution: PingAck did not arrive in time.
[523652.554152] drbd drbd-platform: PingAck did not arrive in time.
[523655.425493] drbd drbd-extension: PingAck did not arrive in time.
[523655.427394] drbd drbd-cgcs: PingAck did not arrive in time.
[523656.111783] drbd drbd-etcd: PingAck did not arrive in time.
[523656.432034] drbd drbd-pgsql: PingAck did not arrive in time.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active)
    osd: 3 osds: 3 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.81 k objects, 1.7 GiB
    usage:   2.1 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     856 active+clean
 
  io:
    client:   26 KiB/s rd, 80 KiB/s wr, 26 op/s rd, 36 op/s wr
 
```

##### storage-0

1. Lock one of the ceph monitor nodes in the system being tested

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
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
    client:   88 KiB/s rd, 463 KiB/s wr, 88 op/s rd, 146 op/s wr
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock storage-0
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

2. Ensure `ceph -s` reports HEALTH_WARN with one of the monitor's listed as being down:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 2 up, 3 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.81 k objects, 1.7 GiB
    usage:   2.1 GiB used, 1.3 TiB / 1.3 TiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   10 KiB/s wr, 0 op/s rd, 2 op/s wr
 
```

3. Attempt to lock another one of the ceph monitors (if applies).

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock controller-1
Only 2 storage monitor available. At least 2 unlocked and enabled hosts with monitors are required. Please ensure hosts with monitors are unlocked and enabled.
```

4. Ensure this is rejected.

This is rejected

```sh
Only 2 storage monitor available. At least 2 unlocked and enabled hosts with monitors are required. Please ensure hosts with monitors are unlocked and enabled.
```

However the following messages are shown:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ [519825.439055] INFO: task jbd2/rbd0-8:801166 blocked for more than 120 seconds.
[519825.446180] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.454236] INFO: task mysqld:810948 blocked for more than 120 seconds.
[519825.460932] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.460977] INFO: task mysqld:810952 blocked for more than 120 seconds.
[519825.460978] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461083] INFO: task mysqld:810973 blocked for more than 120 seconds.
[519825.461085] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461148] INFO: task mysqld:1018507 blocked for more than 120 seconds.
[519825.461150] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461566] INFO: task 10_dirty_io_sch:859628 blocked for more than 120 seconds.
[519825.461566] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461954] INFO: task pidof:1231546 blocked for more than 120 seconds.
[519825.461955] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461970] INFO: task ps:1232857 blocked for more than 120 seconds.
[5sage.
[519825.461954] INFO: task pidof:1231546 blocked for more than 120 seconds.
[519825.461955] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519825.461970] INFO: task ps:1232857 blocked for more than 120 seconds.
[519825.461970] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
```

5. Unlock the ceph monitor that was locked in step 1.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0

<No output message for some seconds, then>

Timeout while waiting on RPC response - topic: "sysinv.conductor_manager", RPC method: "configure_ihost" info: "<unknown>"
[wrsroot@controller-0 ~(keystone_admin)]$ 
[wrsroot@controller-0 ~(keystone_admin)]$ [519945.180175] INFO: task kubelet:90685 blocked for more than 120 seconds.
[519945.186870] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.
[519945.195464] INFO: task jbd2/rbd0-8:801166 blocked for more than 120 seconds.
[519945.202579] "echo 0 > /proc/sys/kernel/hung_task_timeout_secs" disables this message.

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

After some seconds:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
Restore Ceph config failed. Retry unlocking storage node.
```

Another retry:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
Restore Ceph config failed. Retry unlocking storage node.
```

Failed! Checking again status:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     ea5b5cfa-c8f4-454f-9b8a-92afb56973ac
    health: HEALTH_WARN
            Reduced data availability: 284 pgs stale
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.81 k objects, 1.7 GiB
    usage:   1.5 GiB used, 890 GiB / 892 GiB avail
    pgs:     572 active+clean
             284 stale+active+clean
 
  io:
    client:   170 B/s rd, 0 op/s rd, 0 op/s wr
 
```

5. __Recover__ Reboot storage-0 and then unlock

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-reboot storage-0
```

Check storage-0 is offline, reboot in progress

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | degraded     |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

After storage-0 is up again

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | degraded     |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

Unlock it, storage-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
```
storage-0 host unlock is succesful! Check storage-0 is offline, reboot in progress

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
|
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | unlocked       | disabled    | offline      |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

6. Ensure ceph becomes healthy again.

- Failure in Step 6 with regular process
- Success in Step 6 after reboot and unlock of storage-0 was performed

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | compute-0    | worker      | unlocked       | enabled     | available    |
| 3  | compute-1    | worker      | unlocked       | enabled     | available    |
| 4  | controller-1 | controller  | unlocked       | enabled     | available    |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
| 7  | storage-2    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
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
    client:   4.6 KiB/s rd, 473 KiB/s wr, 5 op/s rd, 93 op/s wr

```

7. Repeat this for each node type, e.g. on a 2+X system, try this by locking the controller, and then do another test to lock the worker that is running the ceph monitor.
8. Repeat test for each system type, e.g. 2+X, All-in-One Duplex, Storage, All-in-One Simplex.

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

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-show shared_services
+--------------+----------------------------------+
| Property     | Value                            |
+--------------+----------------------------------+
| backend      | external                         |
| name         | shared_services                  |
| state        | configured                       |
| task         | None                             |
| services     | glance                           |
| capabilities |                                  |
| created_at   | 2019-05-22T12:34:57.446723+00:00 |
| updated_at   | None                             |
+--------------+----------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-show ceph-store
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| backend              | ceph                                 |
| name                 | ceph-store                           |
| state                | configured                           |
| task                 | reconfig-controller                  |
| services             | None                                 |
| capabilities         | min_replication: 1                   |
|                      | replication: 2                       |
| object_gateway       | False                                |
| ceph_total_space_gib | 890                                  |
| object_pool_gib      | None                                 |
| cinder_pool_gib      | None                                 |
| kube_pool_gib        | None                                 |
| glance_pool_gib      | None                                 |
| ephemeral_pool_gib   | None                                 |
| tier_name            | storage                              |
| tier_uuid            | aae84629-2518-4b0a-a7e0-0fc3e759b918 |
| created_at           | 2019-05-22T12:35:44.884703+00:00     |
| updated_at           | None                                 |
+----------------------+--------------------------------------+
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
[wrsroot@controller-0 ~(keystone_admin)]$ sudo sm-dump
Password: 

-Service_Groups------------------------------------------------------------------------
oam-services                     active               active                         
controller-services              active               active                         
cloud-services                   active               active                         
patching-services                active               active                         
directory-services               active               active                         
web-services                     active               active                         
storage-services                 active               active                         
storage-monitoring-services      active               active                         
vim-services                     active               active                         
---------------------------------------------------------------------------------------

-Services------------------------------------------------------------------------------
oam-ip                           enabled-active       enabled-active                  
management-ip                    enabled-active       enabled-active                  
drbd-pg                          enabled-active       enabled-active                  
drbd-rabbit                      enabled-active       enabled-active                  
drbd-cgcs                        enabled-active       enabled-active                  
drbd-platform                    enabled-active       enabled-active                  
pg-fs                            enabled-active       enabled-active                  
rabbit-fs                        enabled-active       enabled-active                  
nfs-mgmt                         enabled-active       enabled-active                  
cgcs-fs                          enabled-active       enabled-active                  
platform-fs                      enabled-active       enabled-active                  
postgres                         enabled-active       enabled-active                  
rabbit                           enabled-active       enabled-active                  
cgcs-export-fs                   enabled-active       enabled-active                  
platform-export-fs               enabled-active       enabled-active                  
cgcs-nfs-ip                      enabled-active       enabled-active                  
platform-nfs-ip                  enabled-active       enabled-active                  
sysinv-inv                       enabled-active       enabled-active                  
sysinv-conductor                 enabled-active       enabled-active                  
mtc-agent                        enabled-active       enabled-active                  
hw-mon                           enabled-active       enabled-active                  
dnsmasq                          enabled-active       enabled-active                  
fm-mgr                           enabled-active       enabled-active                  
keystone                         enabled-active       enabled-active                  
open-ldap                        enabled-active       enabled-active                  
snmp                             enabled-active       enabled-active                  
lighttpd                         enabled-active       enabled-active                  
horizon                          enabled-active       enabled-active                  
patch-alarm-manager              enabled-active       enabled-active                  
mgr-restful-plugin               enabled-active       enabled-active                  
ceph-manager                     enabled-active       enabled-active                  
vim                              enabled-active       enabled-active                  
vim-api                          enabled-activSTOR_CORE_14e       enabled-active                  
vim-webserver                    enabled-active       enabled-active                  
guest-agent                      enabled-active       enabled-active                  
haproxy                          enabled-active       en                 
vim-webserver                    enabled-active       enabled-active                  
guest-agent                      enabled-active       enabled-active                  
haproxy                          enabled-active       enabled-active                  
pxeboot-ip                       enabled-active       enabled-active                  
ceph-radosgw                     enabled-active       enabled-active                  
drbd-extension                   enabled-active       enabled-active                  
extension-fs                     enabled-active       enabled-active                  
extension-export-fs              enabled-active       enabled-active                  
etcd                             enabled-active       enabled-active                  
drbd-etcd                        enabled-active       enabled-active                  
etcd-fs                          enabled-active       enabled-active                  
barbican-api                     enabled-active       enabled-active                  
barbican-keystone-listener       enabled-active       enabled-active                  
barbican-worker                  enabled-active       enabled-active                  
cluster-host-ip                  enabled-active       enabled-active                  
docker-distribution              enabled-active       enabled-active                  
dockerdistribution-fs            enabled-active       enabled-active                  
drbd-dockerdistribution          enabled-active       enabled-active                  
helmrepository-fs                enabled-active       enabled-active                  
registry-token-server            enabled-active       enabled-active                  
------------------------------------------------------------------------------
```

```sh
wrsroot@controller-0 ~(keystone_admin)]$ ceph osd pool get cinder-volumes pg_num
pg_num: 8
```
