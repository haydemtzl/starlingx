# Steps

0. Prerequisites __Done__
1. Mainline v4.14 Clone __Done__
2. Mainline v4.14 Compilation Default defconfig __Done__
3. Mainline v4.14 Porting from v3.10 CONFIG_SIGEXIT to v4.4 CONFIG_SIGEXIT __Done__
4. Mainline v4.14 Compilation Default defconfig with CONFIG_SIGEXIT __Done__
5. Mainline v4.14 Run with CONFIG_SIGEXIT __Done__
6. Mainline v4.14 Compilation StarlingX defconfig + Customizations __Done__
7. Mainline v4.14 Run StarlingX defconfig + Customizations __Done__ Issues
8. Mainline v4.14 RT Compilation Default defconfig __Done__
9. Mainline v4.14 RT Run Default defconfig __Done__

# Prerequisites

```sh
$ sudo apt-get install git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache
```

# Mainline v4.14 Clone

```sh
$ git clone https://github.com/xe1gyq/linux.git
```


```sh
user@workstation:~/starlingx/kernel/linux.github$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ git checkout -b v4.14 v4.14
Checking out files: 100% (42539/42539), done.
Switched to a new branch 'v4.14'
```

# Mainline v4.14 Compilation Default defconfig

- From [kernel: config-4.15.0-45-generic ](https://github.com/xe1gyq/starlingx/commit/4bf015bf77563cfe6117d58e9f3d6ad5fe210b95#diff-e661e2a5e23af26038240c07a1fdb56f) to [kernel: config-4.14.0](https://github.com/xe1gyq/starlingx/commit/313e76ecfe4dbe47ae54f726820fce4ec020792c#diff-e661e2a5e23af26038240c07a1fdb56f)

```sh
user@workstation:~/starlingx/kernel/linux.github$ make oldconfig
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j5
```

# Mainline v4.14 Porting from v3.10 CONFIG_SIGEXIT to v4.4 CONFIG_SIGEXIT

- [\[PATCH\] Notification of death of arbitrary processes](https://github.com/xe1gyq/linux/commit/859bf7ecb646201fc5250da1d6b184c6404149e3)

# Mainline v4.14 Compilation Default defconfig with CONFIG_SIGEXIT

> CONFIG_SIGEXIT=y

- Using [kernel: config-4.14.0 CONFIG_SIGEXIT](https://github.com/xe1gyq/starlingx/commit/2848a0dfd9c164af2b38480fb54e0290b1c301bc#diff-e661e2a5e23af26038240c07a1fdb56f)

```
Symbol: SIGEXIT [=y]
Type  : boolean
Prompt: Notification of death of arbitrary processes
  Location:
(1) -> General setup
  Defined at init/Kconfig:1485
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make menuconfig
```

```
    General setup  --->
[*] Notification of death of arbitrary processes
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j5
```

# Mainline v4.14 Run with CONFIG_SIGEXIT

```sh
user@workstation:~/linux$ uname -a
Linux workstation 4.14.0+ #2 SMP Wed Feb 13 21:28:22 CST 2019 x86_64 x86_64 x86_64 GNU/Linux
```

```sh
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_SIGEXIT
CONFIG_SIGEXIT=y
```

```sh
user@workstation:~$ cat /proc/kallsyms | grep do_notify_task_state
0000000000000000 T do_notify_task_state
user@workstation:~$ cat /proc/kallsyms | grep do_notify_others
0000000000000000 T do_notify_others
```

```sh
user@workstation:~/linux$ cat /boot/config-$(uname -r) | grep CONFIG_INTEL_IOMMU_DEFAULT_ON
# CONFIG_INTEL_IOMMU_DEFAULT_ON is not set
user@workstation:~/linux$ cat /boot/config-$(uname -r) | grep CONFIG_E1000E
CONFIG_E1000E=m
CONFIG_E1000E_HWTS=y
```

# Mainline v4.14 Compilation StarlingX defconfig + Customizations

- Taking [kernel-3.10.0-x86_64.config.tis_extra](https://git.openstack.org/cgit/openstack/stx-integ/tree/kernel/kernel-std/centos/patches/kernel-3.10.0-x86_64.config.tis_extra)
- Merging those into [kernel: config-4.14.0 customizations](https://github.com/xe1gyq/starlingx/commit/6ea0ea16ed2b5e006f0e1dada21659ddbd036f62#diff-e661e2a5e23af26038240c07a1fdb56f)

## Analysis

```sh
$ cp /localdisk/loadbuild/builder/starlingx/std/results/builder-starlingx-tis-r5-pike-std/kernel-3.10.0-957.1.3.el7.1.tis/kernel-3.10.0-957.1.3.el7.1.tis.src.rpm .
```

```sh
$ rpm2cpio ./kernel-3.10.0-957.1.3.el7.1.tis.src.rpm | cpio -idmv
```

```sh
[builder@3327e4ee19d7 kernel-3.10.0-957.1.3.el7.1.tis.src]$ ls
affine-compute-kernel-threads.patch                         kernel-3.10.0-s390x.config
Affine-irqs-and-workqueues-with-kthread_cpus.patch          kernel-3.10.0-s390x-debug.config
aic94xx-Skip-reading-user-settings-if-flash-is-not-f.patch  kernel-3.10.0-s390x-kdump.config
centos-ca-secureboot.der                                    kernel-3.10.0-x86_64.config
centos-kpatch.x509                                          kernel-3.10.0-x86_64.config.tis_extra
centos-ldup.x509                                            kernel-3.10.0-x86_64-debug.config
centossecureboot001.crt                                     kernel-abi-whitelists-957.tar.bz2
CGTS-3744-route-do-not-cache-fib-route-info-on-local.patch  kernel-kabi-dw-957.tar.bz2
check-kabi                                                  kernel.spec
cma-add-placement-specifier-for-cma-kernel-parameter.patch  linux-3.10.0-957.1.3.el7.tar.xz
cpuidle-menu-add-per-CPU-PM-QoS-resume-latency-consi.patch  linux-kernel-test.patch
cpuidle-menu-Avoid-taking-spinlock-for-accessing-QoS.patch  Makefile.common
cpuidle-menu-stop-seeking-deeper-idle-if-current-sta.patch  Make-kernel-start-eth-devices-at-offset.patch
CPU-PM-expose-pm_qos_resume_latency-for-CPUs.patch          memblock-introduce-memblock_alloc_range.patch
cpupower.config                                             modprobe-dccp-blacklist.conf
cpupower.service                                            Module.kabi_dup_ppc64
debrand-rh-i686-cpu.patch                                   Module.kabi_dup_ppc64le
debrand-rh_taint.patch                                      Module.kabi_dup_s390x
debrand-single-cpu.patch                                    Module.kabi_dup_x86_64
dpt_i2o-fix-build-warning.patch                             Module.kabi_ppc64
Enable-building-kernel-with-CONFIG_BLK_DEV_NBD.patch        Module.kabi_ppc64le
Enable-building-mpt2sas-and-mpt3sas-as-builtin-for-C.patch  Module.kabi_s390x
extra_certificates                                          Module.kabi_x86_64
Fix-cacheinfo-compilation-issues-for-3.10.patch             Notification-of-death-of-arbitrary-processes.patch
fix-CentOS-7.6-upgrade-compile-error.patch                  PCI-Add-ACS-quirk-for-Intel-Fortville-NICs.patch
fix-compilation-issues.patch                                Porting-Cacheinfo-from-Kernel-4.10.17.patch
Fix-compile-issue-when-transparent-hugepages-are-off.patch  rcu-Don-t-wake-rcuc-X-kthreads-on-NOCB-CPUs.patch
ima_signing_key.pub                                         sign-modules
intel-iommu-allow-ignoring-Ethernet-device-RMRR-with.patch  turn-off-write-same-in-smartqpi-driver.patch
kernel                                                      US101216-IMA-support-in-Titanium-kernel.patch
kernel-3.10.0-957.1.3.el7.1.tis.src.rpm                     US103091-IMA-System-Configuration.patch
kernel-3.10.0-ppc64.config                                  x509.genkey
kernel-3.10.0-ppc64-debug.config                            x86-enable-DMA-CMA-with-swiotlb.patch
kernel-3.10.0-ppc64le.config                                x86-make-dma_alloc_coherent-return-zeroed-memory-if-.patch
kernel-3.10.0-ppc64le-debug.config
```

- kernel-3.10.0-x86_64.config.tis_extra
- linux-3.10.0-957.1.3.el7.tar.xz

```sh
$ cp /home/user/starlingx/workspace/localdisk/designer/builder/starlingx/sandbox/kernel-3.10.0-957.1.3.el7.1.tis.src/kernel-3.10.0-x86_64.config.tis_extra ~/starlingx/kernel/linux.github/
```

## Customizations

```
user@workstation:~/starlingx/kernel/linux.github$ scripts/kconfig/merge_config.sh -m -n .config customizations
```

```sh
$ wget https://git.openstack.org/cgit/openstack/stx-integ/plain/kernel/kernel-std/centos/patches/kernel-3.10.0-x86_64.config.tis_extra
```

```sh
$ scripts/kconfig/merge_config.sh -m -n .config kernel-3.10.0-x86_64.config.tis_extra 
...
...
Value of CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND is redefined by fragment kernel-3.10.0-x86_64.config.tis_extra:
Previous value: # CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND is not set
New value: CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND=n

#
# merged configuration written to .config (needs make)
#
```

```sh
Makefile:940: "Cannot use CONFIG_STACK_VALIDATION=y, please install libelf-dev, libelf-devel or elfutils-libelf-devel"
scripts/kconfig/conf  --silentoldconfig Kconfig
.config:8789:warning: override: MCORE2 changes choice state
.config:9296:warning: override: reassigning to symbol DRM_BOCHS
.config:9341:warning: override: reassigning to symbol HID_RMI
.config:9470:warning: override: reassigning to symbol HP_WIRELESS
.config:9537:warning: override: reassigning to symbol CRYPTO_DRBG_MENU
.config:9546:warning: override: PREEMPT changes choice state
*
* Restart config...
*
*
* Generic Driver Options
*
Support for uevent helper (UEVENT_HELPER) [Y/n/?] y
  path to uevent helper (UEVENT_HELPER_PATH) [] 
Maintain a devtmpfs filesystem to mount at /dev (DEVTMPFS) [Y/n/?] y
  Automount devtmpfs at /dev, after the kernel mounted the rootfs (DEVTMPFS_MOUNT) [Y/n/?] y
Select only drivers that don't need compile-time external firmware (STANDALONE) [N/y/?] n
Prevent firmware from being built (PREVENT_FIRMWARE_BUILD) [Y/n/?] y
Userspace firmware loading support (FW_LOADER) [Y/?] y
  Include in-kernel firmware blobs in kernel binary (FIRMWARE_IN_KERNEL) [Y/n/?] y
  External firmware blobs to build into the kernel binary (EXTRA_FIRMWARE) [] 
Fallback user-helper invocation for firmware loading (FW_LOADER_USER_HELPER_FALLBACK) [N/y/?] n
Allow device coredump (ALLOW_DEV_COREDUMP) [Y/n/?] y
Driver Core verbose debug messages (DEBUG_DRIVER) [N/y/?] n
Managed device resources verbose debug messages (DEBUG_DEVRES) [N/y/?] n
Test driver remove calls during probe (UNSTABLE) (DEBUG_TEST_DRIVER_REMOVE) [N/y/?] n
Build kernel module to test asynchronous driver probing (TEST_ASYNC_DRIVER_PROBE) [N/m/?] n
Enable verbose DMA_FENCE_TRACE messages (DMA_FENCE_TRACE) [N/y/?] n
DMA Contiguous Memory Allocator (DMA_CMA) [Y/n/?] y
  *
  * Default contiguous memory area size:
  *
  Size in Mega Bytes (CMA_SIZE_MBYTES) [16] 16
  Selected region size
  > 1. Use mega bytes value only (CMA_SIZE_SEL_MBYTES)
    2. Use percentage value only (CMA_SIZE_SEL_PERCENTAGE) (NEW)
    3. Use lower value (minimum) (CMA_SIZE_SEL_MIN) (NEW)
    4. Use higher value (maximum) (CMA_SIZE_SEL_MAX) (NEW)
  choice[1-4]: 

```


```sh
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_SIGEXIT
CONFIG_SIGEXIT=y
```

```sh
user@workstation:~$ cat /proc/kallsyms | grep do_notify_task_state
0000000000000000 T do_notify_task_state
user@workstation:~$ cat /proc/kallsyms | grep do_notify_others
0000000000000000 T do_notify_others
```

```sh
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_INTEL_IOMMU_DEFAULT_ON
CONFIG_INTEL_IOMMU_DEFAULT_ON=y
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_E1000E
# CONFIG_E1000E is not set
```

```sh
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_PREEMPT
CONFIG_PREEMPT_RCU=y
CONFIG_PREEMPT_NOTIFIERS=y
# CONFIG_PREEMPT_NONE is not set
# CONFIG_PREEMPT_VOLUNTARY is not set
CONFIG_PREEMPT=y
CONFIG_PREEMPT_COUNT=y
# CONFIG_PREEMPT_TRACER is not set
```

Issues? Computer hang! Will test removing __CONFIG_PREEMPT__.

# Mainline v4.14 RT Compilation Default defconfig

## Prerequisites

> Transition: From [kernel: 4.15.0-43-generic ](https://github.com/xe1gyq/starlingx/commit/3a9482ac6a4f7e3c038d3e1a35edb143ea382663#diff-ef9338168208a30d9a07034844f14a1b) to [kernel: config-4.14.0 ](https://github.com/xe1gyq/starlingx/commit/b80d2b896f18033a838edf6226bb6cdba33965d5#diff-ef9338168208a30d9a07034844f14a1b)

## RT Patch

```
CONFIG_PREEMPT_RT_FULL:

All and everything

Symbol: PREEMPT_RT_FULL [=y]
Type  : boolean
Prompt: Fully Preemptible Kernel (RT)
  Location:
    -> Processor type and features
      -> Preemption Model (<choice> [=y])
  Defined at kernel/Kconfig.preempt:76
Depends on: <choice> && IRQ_FORCED_THREADING [=y]
Selects: PREEMPT_RT_BASE [=y] && PREEMPT_RCU [=y]   
```

- [RT 4.14](http://cdn.kernel.org/pub/linux/kernel/projects/rt/4.14/)
- Using [kernel: config-4.14.0 CONFIG_PREEMPT_RT_FULL ](https://github.com/xe1gyq/starlingx/commit/05f528fdbb7e5de075bba0e42d18306ae9e06388#diff-ef9338168208a30d9a07034844f14a1b)

```sh
user@workstation:~/linux$ wget http://cdn.kernel.org/pub/linux/kernel/projects/rt/4.14/older/patch-4.14-rt1.patch.gz
```

```sh
user@workstation:~/linux$ gunzip patch-4.14-rt1.patch.gz
```

```sh
user@workstation:~/linux$ ls
arch   certs    CREDITS  Documentation  firmware  include  ipc     Kconfig  lib              MAINTAINERS  mm   patch-4.14-rt1.patch  samples  security  starlingx  usr
block  COPYING  crypto   drivers        fs        init     Kbuild  kernel   localversion-rt  Makefile     net  README                scripts  sound     tools      virt
```

```sh
user@workstation:~/linux$ git clean -f -d && git clean -f -X && git clean -fx && git reset --hard
```

```sh
user@workstation:~/linux$ patch -p1 < patch-4.14-rt1.patch
```

```sh
user@workstation:~/linux$ make
```

```sh
user@workstation:~/linux$ sudo make modules_install install
```

From 

```sh
user@workstation:~$ uname -a
Linux workstation 4.15.0-43-generic #46~16.04.1-Ubuntu SMP Fri Dec 7 13:31:08 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

To

```
user@workstation:~$ uname -a
Linux workstation 4.14.0-rt1+ #2 SMP PREEMPT RT Thu Feb 14 01:15:08 CST 2019 x86_64 x86_64 x86_64 GNU/Linux
```

```sh
user@workstation:~$ cat /boot/config-$(uname -r) | grep CONFIG_PREEMPT_RT_FULL
CONFIG_PREEMPT_RT_FULL=y
```

# Mainline v4.14 RT Tests Default defconfig

> rt-tests is a test suite, that contains programs to test various real time Linux features. It is maintained by Clark Williams and John Kacur.

- [rt-tests homepage](https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/rt-tests)

```sh
user@workstation:~$ sudo apt-get install build-essential libnuma-dev
```

```sh
user@workstation:~/starlingx/kernel$ git clone git://git.kernel.org/pub/scm/utils/rt-tests/rt-tests.git
Cloning into 'rt-tests'...
```

```sh
user@workstation:~/starlingx/kernel$ cd rt-tests
user@workstation:~/starlingx/kernel/rt-tests$ git checkout stable/v1.0
Branch stable/v1.0 set up to track remote branch stable/v1.0 from origin.
Switched to a new branch 'stable/v1.0'
```

```sh
user@workstation:~/starlingx/kernel/rt-tests$ make all
```

```sh
user@workstation:~/starlingx/kernel/rt-tests$ ls
bld      cyclictest  hwlatdetect  Makefile    pi_stress  ptsematest       rt-migrate-test  sendme      sigwaittest  svsematest
COPYING  hackbench   MAINTAINERS  pip_stress  pmqtest    README.markdown  scripts          signaltest  src
```

```On a non-realtime system, you may see something like

   T: 0 ( 3431) P:99 I:1000 C: 100000 Min:      5 Act:   10 Avg:   14 Max:   39242
   T: 1 ( 3432) P:98 I:1500 C:  66934 Min:      4 Act:   10 Avg:   17 Max:   39661

The rightmost column contains the most important result, i.e. the worst-case
latency of 39.242 milliseconds. On a realtime-enabled system, my workstation:

user@workstation:~/starlingx/kernel/rt-tests$ sudo ./cyclictest -a -t -n -p99
# /dev/cpu_dma_latency set to 0us
policy: fifo: loadavg: 1.11 0.88 1.15 4/1092 10482          

T: 0 (10468) P:99 I:1000 C:   7027 Min:      1 Act:    1 Avg:    1 Max:      11
T: 1 (10469) P:99 I:1500 C:   4685 Min:      1 Act:    1 Avg:    1 Max:      17
T: 2 (10470) P:99 I:2000 C:   3513 Min:      1 Act:    1 Avg:    1 Max:      22
T: 3 (10471) P:99 I:2500 C:   2810 Min:      1 Act:    1 Avg:    1 Max:      19
T: 4 (10472) P:99 I:3000 C:   2342 Min:      1 Act:    1 Avg:    1 Max:      16
T: 5 (10473) P:99 I:3500 C:   2007 Min:      1 Act:    2 Avg:    1 Max:      16
T: 6 (10474) P:99 I:4000 C:   1756 Min:      1 Act:    1 Avg:    1 Max:      19
T: 7 (10475) P:99 I:4500 C:   1561 Min:      1 Act:    1 Avg:    1 Max:      19
```
