# Kthread CPUs Kernel Boot Option

## Background


> From [\[PATCH\] affine compute kernel threads](https://git.openstack.org/cgit/openstack/stx-integ/tree/kernel/kernel-std/centos/patches/affine-compute-kernel-threads.patch) VT: The existing "isolcpus" kernel bootarg, cgroup/cpuset, and taskset might provide the some way to have cpu isolation.  However none of them satisfies the requirements. Replacing spaces with tabs. Combine two calls of set_cpus_allowed_ptr() in kernel_init_freeable() in init/main.c into one.

- [Documentation/admin-guide/kernel-parameters.rst](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/admin-guide/kernel-parameters.txt#n1847)

### isolcpus

```
user@workstation:~/starlingx/kernel/linux.github$ git grep isolcpus
Documentation/admin-guide/kernel-parameters.rst:Some kernel parameters take a list of CPUs as a value, e.g.  isolcpus,
Documentation/admin-guide/kernel-parameters.rst:        isolcpus=1,2,10-20,100-2000:2/25
Documentation/admin-guide/kernel-parameters.txt:        isolcpus=       [KNL,SMP] Isolate CPUs from the general scheduler.
Documentation/cgroup-v1/cpusets.txt:marked isolated using the kernel boot time "isolcpus=" argument. However,
Documentation/cgroup-v1/cpusets.txt:CPUs in "cpuset.isolcpus" were excluded from load balancing by the
Documentation/cgroup-v1/cpusets.txt:isolcpus= kernel boot option, and will never be load balanced regardless
kernel/sched/core.c:    /* May be allocated at isolcpus cmdline parse time */
kernel/sched/rt.c:       * whether they are isolcpus or were isolated via cpusets, lest
kernel/sched/topology.c:                pr_err("sched: Error, all isolcpus= values must be between 0 and %u\n", nr_cpu_ids);
kernel/sched/topology.c:__setup("isolcpus=", isolated_cpu_setup);
```

### cpu_isolated_map

```
user@workstation:~/starlingx/kernel/linux.github$ git grep cpu_isolated_map
drivers/base/cpu.c:     n = scnprintf(buf, len, "%*pbl\n", cpumask_pr_args(cpu_isolated_map));
include/linux/sched.h:extern cpumask_var_t                      cpu_isolated_map;
kernel/cgroup/cpuset.c: cpumask_andnot(non_isolated_cpus, cpu_possible_mask, cpu_isolated_map);
kernel/sched/core.c:cpumask_var_t cpu_isolated_map;
kernel/sched/core.c:    cpumask_andnot(non_isolated_cpus, cpu_possible_mask, cpu_isolated_map);
kernel/sched/core.c:    if (cpu_isolated_map == NULL)
kernel/sched/core.c:            zalloc_cpumask_var(&cpu_isolated_map, GFP_NOWAIT);
kernel/sched/topology.c:        alloc_bootmem_cpumask_var(&cpu_isolated_map);
kernel/sched/topology.c:        ret = cpulist_parse(str, cpu_isolated_map);
kernel/sched/topology.c:        cpumask_andnot(doms_cur[0], cpu_map, cpu_isolated_map);
kernel/sched/topology.c:                        cpumask_andnot(doms_new[0], cpu_active_mask, cpu_isolated_map);
kernel/sched/topology.c:                cpumask_andnot(doms_new[0], cpu_active_mask, cpu_isolated_map);
```
