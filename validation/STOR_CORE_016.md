# STOR_CORE_016

> STOR_CORE_016 The objective of this test is to ensure that semantic checks with respect to node lock, work properly on nodes running ceph monitors.

- Results
  - Bare Metal Passed
- Issues
  - https://bugs.launchpad.net/starlingx/+bug/1829279
- To Do

## Bare Metal

1. storage-0: Pass but additional steps involved to recover healthiness. Then another cycle was executed and it succedded without those extra steps involved.
2. controller-1: Pass

### controller-1

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

### storage-0

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
