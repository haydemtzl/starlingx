# STOR_CORE_015

> STOR_CORE_015 The objective of this test is to ensure that host delete and reprovision of nodes running ceph-mon works properly on all supported configs.

- Results
  - Virtual
  - Bare Metal
- Issues

## Virtual

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ cat /etc/build.info 
###
### StarlingX
###     Built from master
###

OS="centos"
SW_VERSION="19.01"
BUILD_TARGET="Host Installer"
BUILD_TYPE="Formal"
BUILD_ID="20190523T013000Z"

JOB="STX_build_master_master"
BUILD_BY="starlingx.build@cengn.ca"
BUILD_NUMBER="113"
BUILD_HOST="starlingx_mirror"
BUILD_DATE="2019-05-23 01:30:00 +0000"
```

1. Lock one of the nodes that are part of a ceph-system. e.g. controller-0 on an All-in-One Duplex system, controller-0 on a standard system, or storage-0 on a ceph storage system.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock storage-0
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                    | Entity ID                            | Severity | Time Stamp     |
| ID    |                                                                                |                                      |          |                |
+-------+--------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Loss of replication in replication group  group-0: OSDs are down               | cluster=e8ab94c1-8878-40bd-          | major    | 2019-05-30T03: |
| 011   |                                                                                | ac05-381366f91e97.peergroup=group-0. |          | 39:42.421320   |
|       |                                                                                | host=storage-0                       |          |                |
|       |                                                                                |                                      |          |                |
| 800.  | Storage Alarm Condition: HEALTH_WARN. Please check 'ceph -s' for more details. | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T03: |
| 001   |                                                                                | ac05-381366f91e97                    |          | 39:35.323709   |
|       |                                                                                |                                      |          |                |
| 200.  | storage-0 was administratively locked to take it out-of-service.               | host=storage-0                       | warning  | 2019-05-30T03: |
| 001   |                                                                                |                                      |          | 39:29.229077   |
|       |                                                                                |                                      |          |                |
+-------+--------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            Degraded data redundancy: 1489/2978 objects degraded (50.000%), 147 pgs degraded, 856 pgs undersized
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 1 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.49 k objects, 952 MiB
    usage:   2.2 GiB used, 396 GiB / 398 GiB avail
    pgs:     1489/2978 objects degraded (50.000%)
             709 active+undersized
             147 active+undersized+degraded
 
  io:
    client:   5.0 KiB/s rd, 314 KiB/s wr, 5 op/s rd, 66 op/s wr
 
```

2. Delete the node

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-delete storage-0
Deleted host storage-0
```

3. Verify the appropriate alarms and events are seen. Verify the ceph status is updated as expected.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |
| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T03: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 40:43.184647   |
|       |                                                                                      |                                      |          |                |
| 800.  | Loss of replication in replication group  group-0: OSDs are down                     | cluster=e8ab94c1-8878-40bd-          | major    | 2019-05-30T03: |
| 011   |                                                                                      | ac05-381366f91e97.peergroup=group-0. |          | 39:42.421320   |
|       |                                                                                      | host=storage-0                       |          |                |
|       |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            Degraded data redundancy: 1489/2978 objects degraded (50.000%), 147 pgs degraded, 856 pgs undersized
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 1 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.49 k objects, 952 MiB
    usage:   2.2 GiB used, 396 GiB / 398 GiB avail
    pgs:     1489/2978 objects degraded (50.000%)
             709 active+undersized
             147 active+undersized+degraded
 
  io:
    client:   595 KiB/s wr, 0 op/s rd, 113 op/s wr

```

4. Re-provision the deleted node

```sh
          │ Waiting for this node to be configured.                  │
          │                                                          │
          │ Please configure the personality for this node from the  │
          │ controller node in order to proceed.                     │
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
| 8  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 8 personality=storage
```

After ~ 15 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
| 8  | storage-0    | storage     | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

After ~ 20 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
| 8  | storage-0    | storage     | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

Add the cluster-host interface on storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-0 $(system host-if-list -a storage-0 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:d2:e5:5b                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2eeeac9f-12f4-4d65-a4cf-5540e3341f3d |
| ihost_uuid   | d171904b-a8a6-47e7-baf1-e6f4a7838a11 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-30T03:54:26.253612+00:00     |
| updated_at   | 2019-05-30T03:55:09.172832+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

Add an OSD to the storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-0 $(system host-disk-list storage-0 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | a13030cb-aff0-41d5-a986-f553d117367c             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | a13030cb-aff0-41d5-a986-f553d117367c             |
| ihost_uuid       | d171904b-a8a6-47e7-baf1-e6f4a7838a11             |
| idisk_uuid       | 3e2d6202-2408-4a6f-8269-a1c92d247e91             |
| tier_uuid        | a9d1d16f-68d6-4aa6-9274-d417da4a4068             |
| tier_name        | storage                                          |
| created_at       | 2019-05-30T03:55:29.592355+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
```

Unlock the storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
```

5. Once the node is available, ensure that ceph recovers.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
| 8  | storage-0    | storage     | unlocked       | disabled    | intest       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            Reduced data availability: 1 pg inactive, 1 pg peering
            Degraded data redundancy: 1482/3054 objects degraded (48.527%), 132 pgs degraded
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in; 132 remapped pgs
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.53 k objects, 981 MiB
    usage:   1.3 GiB used, 397 GiB / 398 GiB avail
    pgs:     0.117% pgs not active
             1482/3054 objects degraded (48.527%)
             723 active+clean
             121 active+undersized+degraded+remapped+backfill_wait
             10  active+recovery_wait+undersized+degraded+remapped
             1   creating+peering
             1   active+recovering+undersized+degraded+remapped
 
  io:
    client:   249 KiB/s rd, 482 KiB/s wr, 251 op/s rd, 257 op/s wr

```

```sh

[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |
| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T03: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 40:43.184647   |
|       |                                                                                      |                                      |          |                |
| 250.  | compute-1 Configuration is out-of-date.                                              | host=compute-1                       | major    | 2019-05-29T16: |
| 001   |                                                                                      |                                      |          | 37:52.318338   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP configuration does not contain any valid or reachable NTP servers.               | host=controller-1.ntp                | major    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.855769   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 129.250.35.250 is not a valid or a reachable NTP server.                 | host=controller-1.ntp=129.250.35.250 | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.812477   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 23.239.24.67 is not a valid or a reachable NTP server.                   | host=controller-1.ntp=23.239.24.67   | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.802781   |
|       |                                                                                      |                                      |          |                |
| 400.  | Communication failure detected with peer over port enp2s1 on host controller-1       | host=controller-1.network=oam        | major    | 2019-05-29T14: |
| 005   |                                                                                      |                                      |          | 33:08.994881   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP configuration does not contain any valid or reachable NTP servers.               | host=controller-0.ntp                | major    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.751106   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 103.105.51.156 is not a valid or a reachable NTP server.                 | host=controller-0.ntp=103.105.51.156 | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.573019   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 72.30.35.89 is not a valid or a reachable NTP server.                    | host=controller-0.ntp=72.30.35.89    | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.519140   |
|       |                                                                                      |                                      |          |                |
| 400.  | Communication failure detected with peer over port enp2s1 on host controller-0       | host=controller-0.network=oam        | major    | 2019-05-29T14: |
| 005   |                                                                                      |                                      |          | 01:01.284789   |
|       |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+

| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T03: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 40:43.184647   |
|       |                                                                                      |                                      |          |                |
| 250.  | compute-1 Configuration is out-of-date.                                              | host=compute-1                       | major    | 2019-05-29T16: |
| 001   |                                                                                      |                                      |          | 37:52.318338   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP configuration does not contain any valid or reachable NTP servers.               | host=controller-1.ntp                | major    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.855769   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 129.250.35.250 is not a valid or a reachable NTP server.                 | host=controller-1.ntp=129.250.35.250 | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.812477   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 23.239.24.67 is not a valid or a reachable NTP server.                   | host=controller-1.ntp=23.239.24.67   | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 47:51.802781   |
|       |                                                                                      |                                      |          |                |
| 400.  | Communication failure detected with peer over port enp2s1 on host controller-1       | host=controller-1.network=oam        | major    | 2019-05-29T14: |
| 005   |                                                                                      |                                      |          | 33:08.994881   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP configuration does not contain any valid or reachable NTP servers.               | host=controller-0.ntp                | major    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.751106   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 103.105.51.156 is not a valid or a reachable NTP server.                 | host=controller-0.ntp=103.105.51.156 | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.573019   |
|       |                                                                                      |                                      |          |                |
| 100.  | NTP address 72.30.35.89 is not a valid or a reachable NTP server.                    | host=controller-0.ntp=72.30.35.89    | minor    | 2019-05-29T14: |
| 114   |                                                                                      |                                      |          | 14:10.519140   |
|       |                                                                                      |                                      |          |                |
| 400.  | Communication failure detected with peer over port enp2s1 on host controller-0       | host=controller-0.network=oam        | major    | 2019-05-29T14: |
| 005   |                                                                                      |                                      |          | 01:01.284789   |
|       |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+

| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T03: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 40:43.184647   |
|       |                                                                                      |                                      |          |                |
|       |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
```

After ~ 20 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            Degraded data redundancy: 152/3100 objects degraded (4.903%), 20 pgs degraded, 20 pgs undersized
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in; 19 remapped pgs
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.55 k objects, 990 MiB
    usage:   2.1 GiB used, 396 GiB / 398 GiB avail
    pgs:     152/3100 objects degraded (4.903%)
             836 active+clean
             19  active+undersized+degraded+remapped+backfill_wait
             1   active+undersized+degraded+remapped+backfilling
 
  io:
    client:   404 KiB/s wr, 0 op/s rd, 75 op/s wr
    recovery: 2.0 MiB/s, 0 keys/s, 0 objects/s

```

And finally:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.55 k objects, 990 MiB
    usage:   2.3 GiB used, 396 GiB / 398 GiB avail
    pgs:     856 active+clean
 
  io:
    client:   3.6 KiB/s rd, 283 KiB/s wr, 4 op/s rd, 62 op/s wr

```

6. Ensure the weights look accurate in

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

7. Ensure there are no unexpected alarms or events

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list

<None related to storage>

```

8. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.
9. Repeat test for the other system configuration types

### Third Storage Node

#### storage-2

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
| 8  | storage-0    | storage     | unlocked       | enabled     | available    |
| 9  | storage-2    | storage     | locked         | disabled    | online       |
| 11 | compute-2    | worker      | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

Add the cluster-host interface on storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-2 $(system host-if-list -a storage-2 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:9d:cf:e2                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 3715d273-74ce-4f99-81f2-c5c9bbf64e6a |
| ihost_uuid   | a8a91b47-67ac-4a00-b43f-21c32cc32049 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-30T04:39:09.651741+00:00     |
| updated_at   | 2019-05-30T04:40:00.270561+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

Add an OSD to the storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-2 $(system host-disk-list storage-2 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 2                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | 60e1eb24-07a9-4c31-ac56-ad9937e83edf             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 60e1eb24-07a9-4c31-ac56-ad9937e83edf             |
| ihost_uuid       | a8a91b47-67ac-4a00-b43f-21c32cc32049             |
| idisk_uuid       | 96126eb2-54b9-455b-b134-239f947b1557             |
| tier_uuid        | a9d1d16f-68d6-4aa6-9274-d417da4a4068             |
| tier_name        | storage                                          |
| created_at       | 2019-05-30T04:42:22.156009+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
```

Unlock the storage hosts.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-2
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |
| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Loss of replication in replication group  group-1: peer host down                    | cluster=e8ab94c1-8878-40bd-          | major    | 2019-05-30T04: |
| 011   |                                                                                      | ac05-381366f91e97.peergroup=group-1  |          | 46:44.987319   |
|       |                                                                                      |                                      |          |                |
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-30T04: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 46:44.631795   |
|       |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
```

Check CEPH status after some time:

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            615/3106 objects misplaced (19.800%)
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in; 296 remapped pgs
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.55 k objects, 1004 MiB
    usage:   2.4 GiB used, 594 GiB / 597 GiB avail
    pgs:     615/3106 objects misplaced (19.800%)
             560 active+clean
             296 active+clean+remapped
 
  io:
    client:   2.0 KiB/s rd, 364 KiB/s wr, 2 op/s rd, 66 op/s wr

```

After ~ 30 minutes... how do we fix this? now we have 3 OSDs instead of 2 OSDs.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            615/3108 objects misplaced (19.788%)
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 3 osds: 3 up, 3 in; 296 remapped pgs
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.55 k objects, 1012 MiB
    usage:   2.4 GiB used, 594 GiB / 597 GiB avail
    pgs:     615/3108 objects misplaced (19.788%)
             560 active+clean
             296 active+clean+remapped
 
  io:
    client:   199 KiB/s wr, 0 op/s rd, 43 op/s wr
 
```

## Bare Metal

1. Lock one of the nodes that are part of a ceph-system. e.g. controller-0 on an All-in-One Duplex system, controller-0 on a standard system, or storage-0 on a ceph storage system.
2. Delete the node
3. Verify the appropriate alarms and events are seen. Verify the ceph status is updated as expected.
4. Re-provision the deleted node
5. Once the node is available, ensure that ceph recovers.
6. Ensure the weights look accurate in
7. Ensure there are no unexpected alarms or events
8. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.
9. Repeat test for the other system configuration types
