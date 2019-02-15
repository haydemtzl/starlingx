# Kthread CPUs Kernel Boot Option

- [\[PATCH\] StarlingX: affine compute kernel threads](https://git.openstack.org/cgit/openstack/stx-integ/tree/kernel/kernel-std/centos/patches/affine-compute-kernel-threads.patch)

## Patch Original

- [\[RFC\] Restrict kernel spawning of threads to a specified set of cpus.](https://lwn.net/Articles/565932/)

### cpu_all_mask 

- [cpu_all_mask](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/include/linux/cpumask.h#n769)

```
/* It's common to want to use cpu_all_mask in struct member initializers,
 * so it has to refer to an address rather than a pointer. */
extern const DECLARE_BITMAP(cpu_all_bits, NR_CPUS);
#define cpu_all_mask to_cpumask(cpu_all_bits)
```

### set_cpus_allowed_ptr? where to place in actual kernel?

> Pay attention to _set_cpus_allowed_ptr_ and _do_basic_setup_

- [0 x86_64_start_kernel](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/x86/kernel/head64.c#n405)
- [1 start_kernel](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n537)
  - [start_kernel](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/include/linux/start_kernel.h)
- [2 arch_call_rest_init](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n739)
  - [arch_call_rest_init](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n532)
  - [rest_init](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n534)
- [3 rest_init](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n397)
  - [kernel_thread(kernel_init, NULL, CLONE_FS);](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n408)
  - [set_cpus_allowed_ptr](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n416)

```c
noinline void __ref rest_init(void)
{
	struct task_struct *tsk;
	int pid;

	rcu_scheduler_starting();
	/*
	 * We need to spawn init first so that it obtains pid 1, however
	 * the init task will end up wanting to create kthreads, which, if
	 * we schedule it before we create kthreadd, will OOPS.
	 */
	pid = kernel_thread(kernel_init, NULL, CLONE_FS);
	/*
	 * Pin init on the boot CPU. Task migration is not properly working
	 * until sched_init_smp() has been run. It will set the allowed
	 * CPUs for init to the non isolated CPUs.
	 */
	rcu_read_lock();
	tsk = find_task_by_pid_ns(pid, &init_pid_ns);
	set_cpus_allowed_ptr(tsk, cpumask_of(smp_processor_id()));
	rcu_read_unlock();

	numa_default_policy();
	pid = kernel_thread(kthreadd, NULL, CLONE_FS | CLONE_FILES);
	rcu_read_lock();
	kthreadd_task = find_task_by_pid_ns(pid, &init_pid_ns);
	rcu_read_unlock();

	/*
	 * Enable might_sleep() and smp_processor_id() checks.
	 * They cannot be enabled earlier because with CONFIG_PREEMPT=y
	 * kernel_thread() would trigger might_sleep() splats. With
	 * CONFIG_PREEMPT_VOLUNTARY=y the init task might have scheduled
	 * already, but it's stuck on the kthreadd_done completion.
	 */
	system_state = SYSTEM_SCHEDULING;

	complete(&kthreadd_done);

	/*
	 * The boot idle thread must execute schedule()
	 * at least once to get things moving:
	 */
	schedule_preempt_disabled();
	/* Call into cpu_idle with preempt disabled */
	cpu_startup_entry(CPUHP_ONLINE);
}
```

- [X kernel_init](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n1050)
- [X kernel_init_freeable](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n1103)
  - [do_basic_setup](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/init/main.c#n1136)

```c
/*
 * Ok, the machine is now initialized. None of the devices
 * have been touched yet, but the CPU subsystem is up and
 * running, and memory and process management works.
 *
 * Now we can finally start doing some real work..
 */
static void __init do_basic_setup(void)
{
	cpuset_init_smp();
	shmem_init();
	driver_init();
	init_irq_proc();
	do_ctors();
	usermodehelper_enable();
	do_initcalls();
}
```

### NR_CPUS

```
config NR_CPUS
        int "Maximum number of CPUs" if SMP && !MAXSMP
        range 2 8 if SMP && X86_32 && !X86_BIGSMP
        range 2 512 if SMP && !MAXSMP && !CPUMASK_OFFSTACK
        range 2 8192 if SMP && !MAXSMP && CPUMASK_OFFSTACK && X86_64
        default "1" if !SMP
        default "8192" if MAXSMP
        default "32" if SMP && X86_BIGSMP
        default "8" if SMP && X86_32
        default "64" if SMP
        ---help---
          This allows you to specify the maximum number of CPUs which this
          kernel will support.  If CPUMASK_OFFSTACK is enabled, the maximum
          supported value is 8192, otherwise the maximum value is 512.  The
          minimum value which makes sense is 2.

          This is purely to save memory - each supported CPU adds
          approximately eight kilobytes to the kernel image.
```

### cpu_kthread_mask

Taking _cpu_active_mask_ as an example

```sh
user@workstation:~/starlingx/kernel/linux.github$ git grep __cpu_active_mask
include/linux/cpumask.h:extern struct cpumask __cpu_active_mask;
include/linux/cpumask.h:#define cpu_active_mask   ((const struct cpumask *)&__cpu_active_mask)
include/linux/cpumask.h:                cpumask_set_cpu(cpu, &__cpu_active_mask);
include/linux/cpumask.h:                cpumask_clear_cpu(cpu, &__cpu_active_mask);
kernel/cpu.c:struct cpumask __cpu_active_mask __read_mostly;
kernel/cpu.c:EXPORT_SYMBOL(__cpu_active_mask);
scripts/gdb/linux/cpus.py:    for cpu in cpu_list("__cpu_active_mask"):
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ git grep __cpu_kthread_mask
include/linux/cpumask.h:extern struct cpumask __cpu_kthread_mask;
include/linux/cpumask.h:#define cpu_kthread_mask  ((const struct cpumask *)&__cpu_kthread_mask)
kernel/cpu.c:struct cpumask __cpu_kthread_mask __read_mostly;
kernel/cpu.c:EXPORT_SYMBOL(__cpu_kthread_mask);
```

### kernel/kmod.c set_cpus_allowed_ptr

Where does _set_cpus_allowed_ptr_ go? In the past it was under _\_\_call_usermodehelper_ _kernel/kmod.c_
Take a look at these changes to understand the new landing place:

```sh
commit 3e63a93b987685f02421e18b2aa452d20553a88b
Author: Oleg Nesterov <oleg@redhat.com>
Date:   Fri Mar 23 15:02:49 2012 -0700

    kmod: introduce call_modprobe() helper
```

```sh
commit 235586939d7fe4833ada9e988f92af543ee6851f
Author: Luis R. Rodriguez <mcgrof@kernel.org>
Date:   Fri Sep 8 16:17:00 2017 -0700

    kmod: split out umh code into its own file
```

```sh
commit f634460c90751da21745eec7a220edf76c7d0c76
Author: Lucas De Marchi <lucas.demarchi@profusion.mobi>
Date:   Tue Apr 30 15:28:03 2013 -0700

    kmod: split call to call_usermodehelper_fns()
```

It all started here for _set_cpus_allowed_ptr_ under _\_\_call_usermodehelper_ _kernel/kmod.c_:

- [log kernel/kmod.c](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/log/kernel/kmod.c)

```
commit f70316dace2bb99730800d47044acb818c6735f6
Author: Mike Travis <travis@sgi.com>
Date:   Fri Apr 4 18:11:06 2008 -0700

    generic: use new set_cpus_allowed_ptr function
```

This patch is very important!

```c
-       set_cpus_allowed_ptr(current, CPU_MASK_ALL_PTR);
+       set_cpus_allowed_ptr(current, cpu_all_mask);
```

- [1a2142afa5646ad5af44bbe1febaa5e0b7e71156](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=1a2142afa5646ad5af44bbe1febaa5e0b7e71156)

```sh
commit 1a2142afa5646ad5af44bbe1febaa5e0b7e71156
Author: Rusty Russell <rusty@rustcorp.com.au>
Date:   Mon Mar 30 22:05:10 2009 -0600

    cpumask: remove dangerous CPU_MASK_ALL_PTR, &CPU_MASK_ALL
```

This is where all legacy code (where StarlingX applied) was replaced!

- [898b374af6f71041bd3bceebe257e564f3f1d458](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=898b374af6f71041bd3bceebe257e564f3f1d458)

```
commit 898b374af6f71041bd3bceebe257e564f3f1d458
Author: Neil Horman <nhorman@tuxdriver.com>
Date:   Wed May 26 14:42:59 2010 -0700

    exec: replace call_usermodehelper_pipe with use of umh init function and resolve limit
```

- [kmod: move call_usermodehelper_fns\(\) to .c file and unexport all it's helpers](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/kernel/kmod.c?id=785042f2e275089e22c36b462f6495ce8d91732d)
- [kmod: add up-to-date explanations on the purpose of each asynchronous levels](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/kernel/kmod.c?id=b639e86bae431db3fbc9fae8d09a9bbf97b74711)

This is the end

- [235586939d7fe4833ada9e988f92af543ee6851f](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=235586939d7fe4833ada9e988f92af543ee6851f)

```
commit 235586939d7fe4833ada9e988f92af543ee6851f
Author: Luis R. Rodriguez <mcgrof@kernel.org>
Date:   Fri Sep 8 16:17:00 2017 -0700

    kmod: split out umh code into its own file
```

## Patch StarlingX

> From [\[PATCH\] StarlingX: affine compute kernel threads](https://git.openstack.org/cgit/openstack/stx-integ/tree/kernel/kernel-std/centos/patches/affine-compute-kernel-threads.patch) VT: The existing "isolcpus" kernel bootarg, cgroup/cpuset, and taskset might provide the some way to have cpu isolation.  However none of them satisfies the requirements. Replacing spaces with tabs. Combine two calls of set_cpus_allowed_ptr() in kernel_init_freeable() in init/main.c into one.

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

But! [Deprecated - use cpusets instead](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/admin-guide/kernel-parameters.txt#n1848)

```sh
user@workstation:~/starlingx/kernel/linux.github$ git grep cpusets
```

## Testing

- https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/admin-guide/kernel-parameters.rst#n73

## Support

Tbd
