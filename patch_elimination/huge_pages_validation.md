```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-show controller-0 0
+-------------------------------------+--------------------------------------+
| Property                            | Value                                |
+-------------------------------------+--------------------------------------+
| Memory: Usable Total (MiB)          | 10258                                |
|         Platform     (MiB)          | 7600                                 |
|         Available    (MiB)          | 10258                                |
| Huge Pages Configured               | True                                 |
| vSwitch Huge Pages: Size (MiB)      | 2                                    |
|                     Total           | 0                                    |
|                     Available       | 0                                    |
|                     Required        | None                                 |
| Application  Pages (4K): Total      | 2626048                              |
| Application  Huge Pages (2M): Total | 0                                    |
|                 Available           | 0                                    |
| Application  Huge Pages (1G): Total | 0                                    |
|                 Available           | None                                 |
| uuid                                | a68ebc55-f62a-4510-bfdc-b3b4c3e257b9 |
| ihost_uuid                          | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| inode_uuid                          | d41a9a42-4e10-41de-b0a2-791664422ce1 |
| created_at                          | 2019-05-08T16:23:03.328445+00:00     |
| updated_at                          | 2019-05-08T19:22:32.798550+00:00     |
+-------------------------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-list controller-0
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
| processor | mem_tot | mem_platfo | mem_ava | hugepages(hp)_ | vs_hp_ | vs_hp_ | vs_hp_ | vs_hp | vm_total | vm_hp_ | vm_hp_ | vm_hp_pe | vm_hp_ | vm_hp_ | vm_hp_pen | vm_hp_use_1G |
|           | al(MiB) | rm(MiB)    | il(MiB) | configured     | size(M | total  | avail  | _reqd | _4K      | total_ | avail_ | nding_2M | total_ | avail_ | ding_1G   |              |
|           |         |            |         |                | iB)    |        |        |       |          | 2M     | 2M     |          | 1G     | 1G     |           |              |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
| 0         | 10258   | 7600       | 10258   | True           | 2      | 0      | 0      | None  | 2626048  | 0      | 0      | None     | 0      | None   | None      | False        |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-modify controller-0 0 -2M 1
Host must be locked.
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock controller-0
+---------------------+--------------------------------------------+
| Property            | Value                                      |
+---------------------+--------------------------------------------+
| action              | none                                       |
| administrative      | unlocked                                   |
| availability        | available                                  |
| bm_ip               | None                                       |
| bm_type             | None                                       |
| bm_username         | None                                       |
| boot_device         | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| capabilities        | {u'stor_function': u'monitor'}             |
| config_applied      | 6628f085-b413-4dcd-9b06-8d3fb040a166       |
| config_status       | None                                       |
| config_target       | 6628f085-b413-4dcd-9b06-8d3fb040a166       |
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
| operational         | enabled                                    |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| subfunction_avail   | available                                  |
| subfunction_oper    | enabled                                    |
| subfunctions        | controller,worker                          |
| task                | Locking                                    |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-05-08T19:32:33.680854+00:00           |
| uptime              | 9353                                       |
| uuid                | c56bb8fd-b67f-479e-97dc-65d383aaa47e       |
| vim_progress_status | services-enabled                           |
+---------------------+--------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-modify controller-0 0 -2M 1
+-------------------------------------+--------------------------------------+
| Property                            | Value                                |
+-------------------------------------+--------------------------------------+
| Memory: Usable Total (MiB)          | 10258                                |
|         Platform     (MiB)          | 7600                                 |
|         Available    (MiB)          | 10258                                |
| Huge Pages Configured               | True                                 |
| vSwitch Huge Pages: Size (MiB)      | 2                                    |
|                     Total           | 0                                    |
|                     Available       | 0                                    |
|                     Required        | None                                 |
| Application  Pages (4K): Total      | 2626048                              |
| Application  Huge Pages (2M): Total | 0                                    |
|                 Total Pending       | 1                                    |
|                 Available           | 0                                    |
| Application  Huge Pages (1G): Total | 0                                    |
|                 Available           | None                                 |
| uuid                                | a68ebc55-f62a-4510-bfdc-b3b4c3e257b9 |
| ihost_uuid                          | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| inode_uuid                          | d41a9a42-4e10-41de-b0a2-791664422ce1 |
| created_at                          | 2019-05-08T16:23:03.328445+00:00     |
| updated_at                          | 2019-05-08T19:33:15.416682+00:00     |
+-------------------------------------+--------------------------------------+
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
| config_applied      | 6628f085-b413-4dcd-9b06-8d3fb040a166       |
| config_status       | None                                       |
| config_target       | 6628f085-b413-4dcd-9b06-8d3fb040a166       |
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
| updated_at          | 2019-05-08T19:34:34.632154+00:00           |
| uptime              | 9733                                       |
| uuid                | c56bb8fd-b67f-479e-97dc-65d383aaa47e       |
| vim_progress_status | services-disabled                          |
+---------------------+--------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-show controller-0 0
+-------------------------------------+--------------------------------------+
| Property                            | Value                                |
+-------------------------------------+--------------------------------------+
| Memory: Usable Total (MiB)          | 10258                                |
|         Platform     (MiB)          | 7600                                 |
|         Available    (MiB)          | 10258                                |
| Huge Pages Configured               | True                                 |
| vSwitch Huge Pages: Size (MiB)      | 2                                    |
|                     Total           | 0                                    |
|                     Available       | 0                                    |
|                     Required        | None                                 |
| Application  Pages (4K): Total      | 2329856                              |
| Application  Huge Pages (2M): Total | 0                                    |
|                 Total Pending       | 1                                    |
|                 Available           | 0                                    |
| Application  Huge Pages (1G): Total | 0                                    |
|                 Available           | None                                 |
| uuid                                | a68ebc55-f62a-4510-bfdc-b3b4c3e257b9 |
| ihost_uuid                          | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| inode_uuid                          | d41a9a42-4e10-41de-b0a2-791664422ce1 |
| created_at                          | 2019-05-08T16:23:03.328445+00:00     |
| updated_at                          | 2019-05-08T19:34:53.915214+00:00     |
+-------------------------------------+--------------------------------------+
```


```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-list controller-0
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
| processor | mem_tot | mem_platfo | mem_ava | hugepages(hp)_ | vs_hp_ | vs_hp_ | vs_hp_ | vs_hp | vm_total | vm_hp_ | vm_hp_ | vm_hp_pe | vm_hp_ | vm_hp_ | vm_hp_pen | vm_hp_use_1G |
|           | al(MiB) | rm(MiB)    | il(MiB) | configured     | size(M | total  | avail  | _reqd | _4K      | total_ | avail_ | nding_2M | total_ | avail_ | ding_1G   |              |
|           |         |            |         |                | iB)    |        |        |       |          | 2M     | 2M     |          | 1G     | 1G     |           |              |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
| 0         | 10258   | 7600       | 10258   | True           | 2      | 0      | 0      | None  | 2329856  | 0      | 0      | 1        | 0      | None   | None      | False        |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+-----------+--------------+
```


```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-show controller-0 0
+-------------------------------------+--------------------------------------+
| Property                            | Value                                |
+-------------------------------------+--------------------------------------+
| Memory: Usable Total (MiB)          | 9103                                 |
|         Platform     (MiB)          | 7600                                 |
|         Available    (MiB)          | 9103                                 |
| Huge Pages Configured               | True                                 |
| vSwitch Huge Pages: Size (MiB)      | 2                                    |
|                     Total           | 0                                    |
|                     Available       | 0                                    |
|                     Required        | None                                 |
| Application  Pages (4K): Total      | 2329856                              |
| Application  Huge Pages (2M): Total | 1                                    |
|                 Available           | 1                                    |
| Application  Huge Pages (1G): Total | 0                                    |
|                 Available           | None                                 |
| uuid                                | a68ebc55-f62a-4510-bfdc-b3b4c3e257b9 |
| ihost_uuid                          | c56bb8fd-b67f-479e-97dc-65d383aaa47e |
| inode_uuid                          | d41a9a42-4e10-41de-b0a2-791664422ce1 |
| created_at                          | 2019-05-08T16:23:03.328445+00:00     |
| updated_at                          | 2019-05-09T04:28:22.383138+00:00     |
+-------------------------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-memory-list controller-0
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+----------+--------------+
| processor | mem_tot | mem_platfo | mem_ava | hugepages(hp)_ | vs_hp_ | vs_hp_ | vs_hp_ | vs_hp | vm_total | vm_hp_ | vm_hp_ | vm_hp_pe | vm_hp_ | vm_hp_ | vm_hp_pe | vm_hp_use_1G |
|           | al(MiB) | rm(MiB)    | il(MiB) | configured     | size(M | total  | avail  | _reqd | _4K      | total_ | avail_ | nding_2M | total_ | avail_ | nding_1G |              |
|           |         |            |         |                | iB)    |        |        |       |          | 2M     | 2M     |          | 1G     | 1G     |          |              |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+----------+--------------+
| 0         | 9103    | 7600       | 9103    | True           | 2      | 0      | 0      | None  | 2329856  | 1      | 1      | None     | 0      | None   | None     | False        |
+-----------+---------+------------+---------+----------------+--------+--------+--------+-------+----------+--------+--------+----------+--------+--------+----------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ grep Huge /proc/meminfo
HugePages_Total:       1
HugePages_Free:        1
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
```

```sh

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ grep GRUB_CMDLINE_LINUX /etc/default/grub
GRUB_CMDLINE_LINUX="security_profile=standard module_blacklist=integrity,ima audit=0 tboot=false crashkernel=auto biosdevname=0 console=tty0 iommu=pt usbcore.autosuspend=-1 hugepagesz=2M hugepages=0 default_hugepagesz=2M isolcpus=2,3 rcu_nocbs=2-5 kthread_cpus=0,1 irqaffinity=0,1 selinux=0 enforcing=0 nmi_watchdog=panic,1 softlockup_panic=1 intel_iommu=on user_namespace.enable=1"
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ mount | grep huge
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
none on /dev/huge-2048kB type hugetlbfs (rw,relatime,pagesize=2048kB)
none on /mnt/huge-2048kB type hugetlbfs (rw,relatime,pagesize=2048kB)
none on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
```

