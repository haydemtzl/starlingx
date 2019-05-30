# STOR_CORE_014

- Virtual
- Bare Metal __Failed__

## Virtual

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
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | locked         | disabled    | online       |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

2. Initiate a host re-install

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
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | locked         | disabled    | offline      |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            1 osds down
            1 host (1 osds) down
            Degraded data redundancy: 1409/2818 objects degraded (50.000%), 147 pgs degraded, 856 pgs undersized
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 1 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.41 k objects, 750 MiB
    usage:   1.6 GiB used, 396 GiB / 398 GiB avail
    pgs:     1409/2818 objects degraded (50.000%)
             709 active+undersized
             147 active+undersized+degraded
 
  io:
    client:   458 KiB/s wr, 0 op/s rd, 88 op/s wr
```

3. Ensure the host comes online after reinstall.

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | locked         | disabled    | online       |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            Degraded data redundancy: 1409/2818 objects degraded (50.000%), 147 pgs degraded, 856 pgs undersized
            1/3 mons down, quorum controller-0,controller-1
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1, out of quorum: storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 1 up, 1 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.41 k objects, 752 MiB
    usage:   805 MiB used, 198 GiB / 199 GiB avail
    pgs:     1409/2818 objects degraded (50.000%)
             709 active+undersized
             147 active+undersized+degraded
 
  io:
    client:   852 B/s rd, 222 KiB/s wr, 0 op/s rd, 51 op/s wr

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
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | unlocked       | enabled     | available    |
| 7  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

6. Check that ceph reports HEALTH_OK via

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            Degraded data redundancy: 1207/2818 objects degraded (42.832%), 121 pgs degraded, 121 pgs undersized
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in; 121 remapped pgs
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.41 k objects, 754 MiB
    usage:   982 MiB used, 397 GiB / 398 GiB avail
    pgs:     1207/2818 objects degraded (42.832%)
             735 active+clean
             95  active+recovery_wait+undersized+degraded+remapped
             25  active+undersized+degraded+remapped+backfill_wait
             1   active+recovering+undersized+degraded+remapped
 
  io:
    client:   182 KiB/s wr, 0 op/s rd, 37 op/s wr
```

7. Ensure the weights look accurate in

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

8. Ensure there are no unexpected alarms or events

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ fm alarm-list 
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| Alarm | Reason Text                                                                          | Entity ID                            | Severity | Time Stamp     |
| ID    |                                                                                      |                                      |          |                |
+-------+--------------------------------------------------------------------------------------+--------------------------------------+----------+----------------+
| 800.  | Storage Alarm Condition: HEALTH_WARN [PGs are degraded/stuck or undersized]. Please  | cluster=e8ab94c1-8878-40bd-          | warning  | 2019-05-29T16: |
| 001   | check 'ceph -s' for more details.                                                    | ac05-381366f91e97                    |          | 59:29.062697   |
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
```

9. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.

```sh
In Progress
```

10. Repeat test for the other system configuration types

```sh
In Progress
```

## Bare Metal

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

8. Ensure there are no unexpected alarms or events

9. Perform basic actions to ensure the system is working properly, e.g. create some volumes, import some images, launch VMs from volume.

10. Repeat test for the other system configuration types
