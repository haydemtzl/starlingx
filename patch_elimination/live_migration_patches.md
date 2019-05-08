# Others

- Still having source code references for "compute" nodes, instead of "worker" nodes.
- Same for Documentation

```
[user@ecfb67fa2760 starlingx]$ repo grep "compute nodes"
```

# Agenda

- Hardware Architecture
  - 

# General

- https://etherpad.openstack.org/p/stx-networking

# Background

## Terminology

- __Tenant__: A group of users is referred to as a project or tenant. There terms are interchangeable. [Openstack Operations Guide](https://docs.openstack.org/operations-guide/ops-projects-users.html).
- StarlingX Nova scheduler: A modified version of the OpenStack Nova scheduler.
- __NUMA__: Non-uniform memory access (NUMA) is a computer memory design used in multiprocessing, where the memory access time depends on the memory location relative to the processor. [Wikipedia](https://en.wikipedia.org/wiki/Non-uniform_memory_access)
- __PCI pass through__: Allows compute nodes to pass a physical PCI device to a hosted VM. This can be used for direct access to a PCI device inside the VM. [OpenStack Charms Deployment Guide](https://docs.openstack.org/project-deploy-guide/charm-deployment-guide/latest/app-pci-passthrough-gpu.html)
- __Live Migration__: Live-migrating an instance means moving its virtual machine to a different OpenStack Compute server while the instance continues running. [Openstack Compute Nova](https://docs.openstack.org/nova/pike/admin/live-migration-usage.html)
- __Glance__: The Image service (glance) project provides a service where users can upload and discover data assets that are meant to be used with other services. This currently includes images and metadata definitions. [Homepage](https://docs.openstack.org/glance/latest/)
- __Nova__: Nova is the OpenStack project that provides a way to provision compute instances (aka virtual servers). Nova supports creating virtual machines, baremetal servers (through the use of ironic), and has limited support for system containers. Nova runs as a set of daemons on top of existing Linux servers to provide that service. [Homepage](https://docs.openstack.org/nova/latest/)
- __NFV__: Manage patch orchestration with the StarlingX NFV VIM API. This includes creation, application and querying of patch strategies. Manage upgrade orchestration with the StarlingX NFV VIM API. This includes creation, application and querying of upgrade strategies. [Homepage](https://docs.starlingx.io/stx-nfv/index.html)

## General

- Storage Configuration
  - Migration
  - Resize
  - Evacuation
- Resource Monitoring
  - NUMA node resources on a host

## Trunk

"Trunk" has some relationship with the following terminology:

- Network Management
- Tenant
  - It
- User Guide

# CPU Topologies

See [OpenStack Compute \(nova\) CPU topologies](https://docs.openstack.org/nova/latest/admin/cpu-topologies.html)

- SMP: In SMP, all memory access was tied to a singular shared physical bus.
- NUMA: NUMA was created to limit the number of CPUs tied to a single memory bus and as a result defines what is construed as a NUMA node.
- Simultaneous Multi-Threading (SMT)
- Non-Uniform I/O Access (NUMA I/O)

## NUMA

> NUMA is a derivative of the SMP design that is found in many multi-socket systems. In a NUMA system, system memory is divided into cells or nodes that are associated with particular CPUs. Requests for memory on other nodes are possible through an interconnect bus. However, bandwidth across this shared bus is limited. As a result, competition for this resource can incur performance penalties.

> In OpenStack, SMP CPUs are known as cores, NUMA cells or nodes are known as sockets, and SMT CPUs are known as threads.

### Links

- https://www.exitthefastlane.com/2016/04/vsphere-design-for-numa-architecture.html

### Key Things

- The maximum vCPU value assignable to a VM is dictated by the total number of logical cores available in all NUMA nodes. 
- To design your virtual environment to be NUMA aligned means ensuring that your VMs receive vCPUs and RAM tied to a single physical CPU (pCPU), thus ensuring the memory and pCPU cores they access are directly connected and not accessed via traversal of a CPU interconnect (QPI).

### Documentation

What those documentations says about NUMA?

### StarlingX

- numa_node

### Details

- Relation with:
  - CPU Pinning
  - Hugepages
- Affinity
  - Strict
  - Best Effort

### Extra Specs

#### hw:numa_nodes

```sh
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch
cgcs-root/stx/git/nova
```

```sh
$ glance/etc/metadefs/
```

```sh
commit 1c242032fbb26fed3a82691abb030583b4f8940b
Author: Wayne Okuma <wayne.okuma@hp.com>
Date:   Thu Aug 28 04:33:53 2014 -0400

    Glance Metadata Definitions Catalog - Seed
    
    Implements: blueprint metadata-schema-catalog
    
    A common API hosted by the Glance service for vendors, admins,
    services, and users to meaningfully define available key / value
    pair and tag metadata. The intent is to enable better metadata
    collaboration across artifacts, services, and projects for
    OpenStack users.
    
    This is about the definition of the available metadata that can
    be used on different types of resources (images, artifacts,
    volumes, flavors, aggregates, etc). A definition includes the
    properties type, its key, it's description, and it's constraints.
    This catalogue will not store the values for specific instance
    properties.
```

```sh
commit 01ace04438282b5b5602bcdedc02d55b9b120c5f
Author: Travis Tripp <travis.tripp@hp.com>
Date:   Fri Jan 16 14:48:15 2015 -0700

    Software Metadata Definitions
    
    The will provide a base library of metadata definitions for
    various common software products, components, and libraries that
    may exist on particular image (or volume or instance). These
    metadata definitions will make it easier for end users and
    admins to easily describe the software and its properties.
    This information will enable an improved and faster user
    experience for applying software metadata, searching based
    on software metadata, and viewing the software information
    about an image.
    
    Various improvements in horizon are underway to take
    advantage of this metadata. For example, a user launching
    an instance will be able to expand the image row to see
    additional information about the image.  This will include
    providing the metadata definition for properties on the image.
    This same information will also be visible from the image and
    instance details page.
```

```sh
commit 85de1149b61eb43ac085afe7b0e26d7e71456b93
Author: Waldemar Znoinski <waldemar.znoinski@intel.com>
Date:   Fri Jul 3 09:41:42 2015 +0100

    Add CPU Pinning in metadata definitions
    
    This metadef adds CPU pinning namespace and property to Nova::Flavor,
    Glance::Image, Cinder::Volume(image) resource types metadata.
```

```sh
commit 788e8ad69b09d1005dc916aec90bfbffb486af9a
Author: NAO NISHIJIMA <nao.nishijima.xt@hitachi.com>
Date:   Mon Dec 7 11:50:42 2015 +0900

    Add missing CPU features to Glance Metadata Catalog
    
    This patch adds missing CPU features to Glance Metadata Catalog.
    CPU features based on linux v4.4-rc4 kernel source code in
    arch/x86/include/asm/cpufeature.h and picked up Intel&AMD vender.
```

```sh
commit 411418b04449f53afcbd103efcc0231fc825b0a7
Author: Stephen Finucane <sfinucan@redhat.com>
Date:   Thu Aug 11 17:32:58 2016 +0100

    Add CPU thread pinning to metadata defs
    
    CPU pinning policy is defined, but CPU thread pinning policy is not.
    Resolve this.
```

StarlingX Related

```sh
commit 1ec64167057e3368f27a1a81aca294b771e79c5e
Author: Dean Troyer <dtroyer@gmail.com>
Date:   Sat May 19 21:28:10 2018 -0700

    StarlingX open source release updates
    
    Signed-off-by: Dean Troyer <dtroyer@gmail.com>
```

- stx-patches/0001-Pike-Rebase-Validate-image-properties.patch
  - glance/api/v1/images.py
  - glance/api/v2/images.py
  - Extra Properties
    - hw_wrs_live_migration_max_downtime
    - hw_wrs_live_migration_timeout
    - sw_wrs_auto_recovery
- glance/api/v1/images.py
- glance/api/v2/images.py
- stx-patches/0002-Pike-Rebase-Enhanced-error-messages.patch
  - glance/api/v1/upload_utils.py
  - glance/tests/unit/fake_rados.py
  - glance/tests/unit/v1/test_api.py
  - glance/tests/unit/v1/test_upload_utils.py
- glance/api/v1/upload_utils.py
- glance/tests/unit/fake_rados.py
- glance/tests/unit/v1/test_api.py
- glance/tests/unit/v1/test_upload_utils.py
- stx-patches/0003-Pike-Rebase-RAW-image-caching-support.patch
  - Add RAW caching image feature
- etc/glance-api.conf
- glance/api/v1/images.py
- glance/api/v2/image_data.py
- glance/api/v2/images.py
- glance/cache_raw.py
- glance/cmd/api.py
- glance/common/exception.py
- glance/common/imageutils.py
- glance/common/wsgi.py
- glance/tests/integration/legacy_functional/test_v1_api.py
- glance/tests/unit/fake_cache_raw.py
- glance/tests/unit/v1/test_api.py

```sh
commit ab1c3ba413a3785aebd1c088124ba4e51b9762b3
Author: Daniel Chavolla <daniel.chavolla@windriver.com>
Date:   Wed Sep 19 17:05:26 2018 -0400

    add the ability to request HPET support
    
    Updated HPET timer extra spec name to “hw:hpet” to match
    changes in nova. Also, some minor formatting fixes.
    
    Closes bug: 1790961
    https://bugs.launchpad.net/starlingx/+bug/1790961
    
    To be upstreamed along with stx-nova commit e0e80c8024a
    and with internal glance commit 6aa614c7
```

- High Precision Event Timer (HPET)
- etc/metadefs/compute-tis-flavor.json
- __Question?__ Do we have an issue? Nope! 
  - Under Glance, it is called:
    - hw:hpet
  - Under Nova, it is found as:
    - hw_time_hpet (image property)
    - hpet (flag)

```sh
commit 47c1d756a547122e29b43b008d94f264389cc7f4
Author: Sun Austin <austin.sun@intel.com>
Date:   Wed Nov 14 13:17:24 2018 +0800

    update 'hw:cpu_model' meta data to support *-IBRS vcpu type
    
    after libvirt upgraded to 4.7.0, there are some new vcpu types for
    instance vcpu_model, if compute node is using *-IBRS cpu,
    Passthrough favor can not be used for creating instance.
    it will cause some like below error
    "No valid host was found. There are not enough hosts available.
     computer-0: (VCpuModelFilter) Host VCPU model Skylake-Server-IBRS
     required Passthrough
     Code 501"
    
    Partial-Bug: 1803280
```

- etc/metadefs/compute-tis-flavor.json
  - Nehalem-IBRS
  - ...
  - Skylake-Server-IBRS

```sh
commit d8ab19da86a28a6369656d3b9284e30bf267ec8d
Author: Jim Gauld <james.gauld@windriver.com>
Date:   Tue Nov 27 17:44:23 2018 -0400

    Remove support for nova-local lvm backend for compute hosts
    
    This story tracks the removal of the nova-local lvm backend for compute
    hosts. The lvm backend is no longer required; nova-local storage will
    continue to support settings of "image" or "remote" backends.
    
    This story will remove custom code related to lvm nova-local storage:
    - this removes local_lvm from /etc/metadefs/compute-tis-flavor.json
    
    DocImpact
    Story: 2004427
    Task: 28083
```

- etc/metadefs/compute-tis-flavor.json
  - remote
  - local_lvm
  - local_image

Interesting Links

- [Power off commands should give guests a chance to shutdown](https://review.opendev.org/#/c/68942/)

```sh
commit 5fcb3aa2e35e9af17cb8be9e24c6613626036f2b
Author: Travis Tripp <travis.tripp@hp.com>
Date:   Fri Sep 12 11:55:52 2014 -0600

    Add missing metadefs for shutdown behavior
    
    The following Nova patch adds support for graceful shutdown
    of a guest VM and allows setting timeout properties on images.
    The properties should be updated in the Metadata Definitions catalog.
    
    https://review.openstack.org/#/c/68942/
```

#### hw:numa_cpus

```sh
cgcs-root/stx/git/nova
```

#### hw:numa_mem

```sh
cgcs-root/stx/git/nova
```

### Source Code

- vcpu_pin_set

### Links

- https://specs.openstack.org/openstack/nova-specs/specs/rocky/approved/numa-aware-live-migration.html
- http://lists.openstack.org/pipermail/openstack-dev/2016-March/090367.html
- https://docs.openstack.org/nova/latest/user/flavors.html#extra-specs-numa-topology

### Tasks

An instance’s vCPUs to spread across one host NUMA node:

```sh
$ openstack flavor set m1.large --property hw:numa_nodes=1
```

An instance’s vCPUs across two host NUMA nodes:

```
$ openstack flavor set m1.large --property hw:numa_nodes=2

```

```
$ openstack flavor set m1.large \  # configure guest node 0
  --property hw:numa_cpus.0=0,1 \
  --property hw:numa_mem.0=2048
$ openstack flavor set m1.large \  # configure guest node 1
  --property hw:numa_cpus.1=2,3,4,5 \
  --property hw:numa_mem.1=4096
```

## CPU Pinning

> Related to NUMA

From Glance source code:

```sh
commit 85de1149b61eb43ac085afe7b0e26d7e71456b93
Author: Waldemar Znoinski <waldemar.znoinski@intel.com>
Date:   Fri Jul 3 09:41:42 2015 +0100

    Add CPU Pinning in metadata definitions
    
    This metadef adds CPU pinning namespace and property to Nova::Flavor,
    Glance::Image, Cinder::Volume(image) resource types metadata.
```



## CPU

> Shared physical CPU on Worker hosts

CPU Core Assignments:

- platform: platform to use N cores for each NUME node
- vswitch: avs cores for each processor
- shared: used by multiple VMs, low load workloads
  
CLI

```sh
$ system host-cpu-modify -f shared -pprocessor use_shared hostname
$ system host-cpu-modify -f shared -p0 1 compute-0
```

UI

1. Lock
2. Inventory Detail
3. Host Name
4. Processor
5. Edit CPU Assignments
6. Shared Function: 1 Shared / 0 Not Shared
7. Unlock
https://docs.openstack.org/nova/latest/admin/huge-pages.html
### do_host_cpu_modify

```
[user@ecfb67fa2760 starlingx]$ repo grep do_host_cpu_modify
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/common/utils.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py
cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/common/utils.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/common/utils.py
```

- do_host_cpu_modify under _does_command_need_no_wrap is called from:
  - cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/common/utils.py
  - cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/common/utils.py
  - cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/common/utils.py
- and used at the function:
  - define_commands_from_module found at:

```
[user@ecfb67fa2760 starlingx]$ repo grep define_commands_from_module
cgcs-root/stx/git/python-ironicclient/ironicclient/common/utils.py
cgcs-root/stx/git/python-ironicclient/ironicclient/shell.py
cgcs-root/stx/git/python-ironicclient/ironicclient/v1/shell.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/common/utils.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/shell.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/shell.py
cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/common/utils.py
cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/shell.py
cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/v1/shell.py
cgcs-root/stx/stx-ha/service-mgmt-client/sm-client/sm_client/common/utils.py
cgcs-root/stx/stx-ha/service-mgmt-client/sm-client/sm_client/shell.py
cgcs-root/stx/stx-ha/service-mgmt-client/sm-client/sm_client/v1/shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/common/utils.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/shell.py
```

Question? Why do_host_cpu_modify only appears under stx-config and not under stx-metal?

```sh
[user@ecfb67fa2760 starlingx]$ repo grep do_host_cpu
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/common/utils.py:             'do_host_cpu_modify',
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py:def do_host_cpu_show(cc, args):
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py:def do_host_cpu_list(cc, args):
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py:def do_host_cpu_modify(cc, args):
cgcs-root/stx/stx-fault/python-fmclient/fmclient/fmclient/common/utils.py:             'do_host_cpu_modify',
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/common/utils.py:             'do_host_cpu_modify',
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/cpu_shell.py:def do_host_cpu_show(cc, args):
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/cpu_shell.py:def do_host_cpu_list(cc, args):
```

## Memory

Inventory Memory Tab

- Memory: all host memory
- vSwitch Huge Pages: Workers: Size, Total, Available Huge Pages
- VM Pages: Workers: Size, Total, Available Pages

Allocation

- Platform Memory
- # of VM 2M Hugepages
- # of VM 1G Hugepages


```sh
$ system host-memory-modify hostname processor [-m reserved] [-2M 2Mpages] [-1G 1Gpages]
$ system host-memory-modify compute-0 1 -2M 4
```

## Huge Pages (Large Pages)

- [OpenStack Compute \(nova\) Huge Pages](https://docs.openstack.org/nova/latest/admin/huge-pages.html)
- http://lists.starlingx.io/pipermail/starlingx-discuss/2019-January/002884.html

### Terminology

- Pages
- Translation Lookaside Buffer (TLB)
- Huge Pages
- Persistent Huge Pages
- Transparent Huge Pages (THP)

### Documentation

What those documentations says about Huge PAges?

### Processes

- Enabling
  - hugepagesz
  - hugepages
  - transparent_hugepages=never
- Customizing

#### Processes > Enabling > hugepagesz

```sh
$ repo grep hugepagesz
cgcs-root/stx/git/libvirt/src/util/virfile.c
cgcs-root/stx/git/nova/doc/source/admin/huge-pages.rst
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/compute.pp
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/platform.py
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_kernel_aio_and_worker.cfg
```

From cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/platform.py

```sh
class platform::compute::grub::params

# Mounts virtual hugetlbfs filesystems for each supported page size
class platform::compute::hugetlbf {

# lint:ignore:variable_is_lowercase
class platform::compute::hugepage::params (

# Allocates HugeTLB memory according to the attributes specified in the
# nr_hugepages_2M and nr_hugepages_1G
class platform::compute::allocate
```

Keywords

- m_hugepages
- g_hugepages
- gb_hugepages
- hugepagesz
- hugepages
- default_hugepagesz
- nr_hugepages_2M
- nr_hugepages_1G

#### Processes > Enabling > hugepages

```sh

```

#### Processes > Enabling > transparent_hugepages

```sh
$ repo grep transparent_hugepages
cgcs-root/stx/git/nova/doc/source/admin/huge-pages.rst:``hugepages``, and ``transparent_hugepages=never`` arguments to
```

#### Processes > Enabling > Sysfs Hugepages

```sh
$ repo grep /sys/devices/system/node/ | grep hugepages
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/node.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/node.py
```

### StarlingX Terminology

- Memory
  - Host Memory Provisioning
    - Command Line Interface
      - system host-memory-list
        - do_host_memory_list
      - system host-memory-show
        - do_host_memory_show
      - system host-memory-modify
        - do_host_memory_modify
    - User Interface
      - Host Inventory > Memory
- vSwitch Huge Pages
- VM Pages

### Process

1. Set Huge Pages
   - Host Name
     - Memory
     - Update Memory (do_host_memory_modify)
       - Page Size for a VM
         - VM 2M Hugeages
         - VM 1G Hugepages
2. Save Memory Profile to apply ton other hosts

### Source Code

Keywords

- hw:mem_page_size

Files

```sh
[user@ecfb67fa2760 starlingx]$ repo grep hw:mem_page_size
cgcs-root/stx/git/neutron
cgcs-root/stx/git/nova
stx-tools/deployment/
```

```sh
[user@ecfb67fa2760 starlingx]$ repo grep hw:mem_page_size
cgcs-root/stx/git/neutron/doc/source/admin/config-ovs-dpdk.rst
cgcs-root/stx/git/nova/api-guide/source/down_cells.rst
cgcs-root/stx/git/nova/doc/api_samples/flavors/v2.61/flavors-detail-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.47/server-get-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.47/servers-details-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.63/server-action-rebuild-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.63/server-get-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.63/server-update-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.63/servers-details-resp.json
cgcs-root/stx/git/nova/doc/api_samples/servers/v2.66/servers-details-with-changes-before.json
cgcs-root/stx/git/nova/doc/source/admin/huge-pages.rst
cgcs-root/stx/git/nova/doc/source/user/flavors.rst
cgcs-root/stx/git/nova/nova/tests/fixtures.py
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/flavors/v2.61/flavors-detail-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.47/server-get-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.47/servers-details-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.63/server-action-rebuild-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.63/server-get-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.63/server-update-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.63/servers-details-resp.json.tpl
cgcs-root/stx/git/nova/nova/tests/functional/api_sample_tests/api_samples/servers/v2.66/servers-details-with-changes-before.json.tpl
cgcs-root/stx/git/nova/nova/tests/unit/virt/test_hardware.py
stx-tools/deployment/provision/simplex_stage_2.sh
```

#### do_host_memory

- Reserved
  - platform_reserved_mib
- 2M
  - hugepages_nr_2M_pending
    - vswitch_hp_size_mib = 2
  - vm_hugepages_nr_2M_pending
- 1G
  - hugepages_nr_1G_pending
    - vswitch_hp_size_mib = 1024
  - vm_hugepages_nr_1G_pending


##### stx-config

###### vswitch

```
$ system host-cpu-modify compute-0 -f vswitch -p0 1
```

From

- https://wiki.openstack.org/wiki/StarlingX/Containers/Installation

```sh
Configure the vswitch type (optional)

    As of March 29th 2019, OVS running in a container is deployed by default.

To deploy OVS-DPDK (supported only on baremetal hardware), please run the following commands:

system modify --vswitch_type ovs-dpdk
system host-cpu-modify -f vswitch -p0 1 controller-0
# To set the vswitch type back to the default (i.e. OVS running in a container), run:
# system modify --vswitch_type none

    NOTE: For virtual environments, only OVS running in a container is supported.
    NOTE: The vswitch type cannot be modified after controller-0 is unlocked.
    IMPORTANT: When deploying OVS-DPDK, VMs must be configured to use a flavor with property: hw:mem_page_size=large
```

From

- https://wiki.openstack.org/wiki/StarlingX/Installation_Guide_Virtual_Environment/Dedicated_Storage

```sh
VSwitch Virtual Environment

Only in Virtual Environment. If the compute has more than 4 cpus, the system will auto-configure the vswitch to use 2 cores. However some virtual environments do not properly support multi-queue required in a multi-cpu environment. Therefore run the following command to reduce the vswitch cores to 1:

[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify compute-0 -f vswitch -p0 1
```

Where is affinity happening?:

- cgcs-root/stx/stx-config/worker-utils/worker-utils/cpumap_functions.sh
- cgcs-root/stx/stx-config/worker-utils/worker-utils/task_affinity_functions.sh

How number of cores is related?

- N_CORES_IN_PKG
- /etc/platform/platform.conf
  - ./cgcs-root/stx/stx-metal/devstack/files/platform.conf
  - PLATFORM_CORES
  - PLATFORM_CPULIST
- /etc/vswitch/vswitch.conf
  - AVP_CORES
  - AVP_CPULIST
- /etc/platform/worker_reserved.conf
  - ./cgcs-root/stx/stx-config/worker-utils/worker-utils/worker_reserved.conf

Question? How this type of functions are called?

```sh
[user@ecfb67fa2760 starlingx]$ repo grep affine_tasks_to_idle_cores
cgcs-root/stx/stx-config/worker-utils/worker-utils/task_affinity_functions.sh:function affine_tasks_to_idle_cores {
cgcs-root/stx/stx-ha/service-mgmt/sm-1.0.0/src/sm_task_affining_thread.c:           system("source /etc/init.d/task_affinity_functions.sh; affine_tasks_to_idle_cores 2>/dev/null");
```

```sh
[user@ecfb67fa2760 starlingx]$ repo grep affine_tasks_to_platform_cores
cgcs-root/stx/stx-config/worker-utils/worker-utils/affine-tasks.sh:    ##affine_tasks_to_platform_cores
cgcs-root/stx/stx-config/worker-utils/worker-utils/task_affinity_functions.sh:# calling function affine_tasks_to_platform_cores to re-affine platform tasks
cgcs-root/stx/stx-config/worker-utils/worker-utils/task_affinity_functions.sh:function affine_tasks_to_platform_cores {
cgcs-root/stx/stx-ha/service-mgmt/sm-1.0.0/src/sm_task_affining_thread.c:            system("source /etc/init.d/task_affinity_functions.sh; affine_tasks_to_platform_cores 2>/dev/null");
```

```sh
./cgcs-root/stx/stx-metal/devstack/files/platform.conf

nodetype=controller
subfunction=@SUBFUNCTION@
system_type=@SYS_TYPE@
security_profile=standard
INSTALL_UUID=b4c47f98-1fe6-4959-b031-e4a71a261526
management_interface=@MGMT_ETH@
UUID=1be42dc6-a072-4364-9b51-c8535ff16644
oam_interface=@OAM_ETH@
sdn_enabled=no
region_config=no
system_mode=@SYS_MODE@
sw_version=@SW_VERSION@
security_feature="nopti nospectre_v2"
vswitch_type=ovs-dpdk
```

```sh
WORKER_CPU_LIST="0-1"

cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/worker_reserved.conf.erb
cgcs-root/stx/stx-config/worker-utils/worker-utils/worker_reserved.conf
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_system_aio.cfg

WORKER_BASE_RESERVED=("node0:8000MB:1" "node1:2000MB:0" "node2:2000MB:0" "node3:2000MB:0")

cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/worker_reserved.conf.erb
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/node.py
cgcs-root/stx/stx-config/worker-utils/worker-utils/worker_reserved.conf
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_system_aio.cfg

Question? Why the following two variables are not used in any other place?

WORKER_VSWITCH_MEMORY=("node0:1048576kB:1" "node1:1048576kB:1" "node2:1048576kB:1" "node3:1048576kB:1")

cgcs-root/stx/stx-config/worker-utils/worker-utils/worker_reserved.conf

WORKER_VSWITCH_CORES=("node0:2" "node1:0" "node2:0" "node3:0")

cgcs-root/stx/stx-config/worker-utils/worker-utils/worker_reserved.conf

```

```sh
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py

def do_host_memory_modify(cc, args):
    """Modify platform reserved and/or application huge page memory attributes for worker nodes.""" 

...

        if function == 'vswitch':
            if k == 'hugepages_nr_2M_pending':
                vswitch_hp_size_mib = 2
                k = 'vswitch_hugepages_reqd'
            elif k == 'hugepages_nr_1G_pending':
                vswitch_hp_size_mib = 1024
                k = 'vswitch_hugepages_reqd'
        else:
            if k == 'hugepages_nr_2M_pending':
                k = 'vm_hugepages_nr_2M_pending'
            elif k == 'hugepages_nr_1G_pending':
                k = 'vm_hugepages_nr_1G_pending'

        patch.append({'op': 'replace', 'path': '/' + k, 'value': v})

    if patch:
        if vswitch_hp_size_mib:
            patch.append({'op': 'replace', 'path': '/' + 'vswitch_hugepages_size_mib', 'value': vswitch_hp_size_mib})
```

###### Vswitch

Where it is founder in source code?

```sh
cgcs-root/stx/git/libvirt
cgcs-root/stx/git/neutron
cgcs-root/stx/stx-config
cgcs-root/stx/stx-gui
cgcs-root/stx/stx-integ
cgcs-root/stx/stx-metal
cgcs-root/stx/stx-nfv
```

```sh
cgcs-root/stx/git/libvirt/docs/news-2012.html.in
cgcs-root/stx/git/neutron/devstack/lib/ovs
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/l2/base.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/linux/openvswitch_firewall/test_firewall.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/linux/test_dhcp.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/test_firewall.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/test_l2_ovs_agent.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/test_ovs_flows.py:from neutron.plugins.ml2.drivers.openvswitch.agent.common import constants
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/test_ovs_flows.py
cgcs-root/stx/git/neutron/neutron/tests/functional/scheduler/test_dhcp_agent_scheduler.py
cgcs-root/stx/git/neutron/neutron/tests/functional/services/trunk/drivers/openvswitch/agent/test_ovsdb_handler.py
cgcs-root/stx/git/neutron/neutron/tests/functional/services/trunk/test_plugin.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/etc/sysinv/sampleProfile.xml
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/node.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/cpu_utils.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/host.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/profile.py
cgcs-root/stx/stx-config/worker-utils/worker-utils/cpumap_functions.sh
cgcs-root/stx/stx-config/worker-utils/worker-utils/task_affinity_functions.sh
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/cpu_functions/forms.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/cpu_functions/utils.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/cpu_functions/views.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/memories/tables.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/memorys/_vswitchfunction_hugepages.html
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/vswitch.sh
cgcs-root/stx/stx-metal/api-ref/source/api-ref-sysinv-v1-metal.rst
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/node.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/cpu_utils.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/test_data/nfv_vim_db_18.03_GA
```

##### stx-metal

```sh
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/memory_shell.py

def do_host_memory_modify(cc, args):
    """Modify platform reserved and/or libvirt vm huge page memory
       attributes for compute nodes.
    """
```

```sh
    host = host_utils._find_host(cc, args.hostnameorid)
    nodes = cc.node.list(host.uuid)
    memorys = cc.memory.list(host.uuid)
    mem = None
    for m in memorys:
        for n in nodes:
            if m.node_uuid == n.uuid:
                if int(n.numa_node) == int(args.numa_node):
                    mem = m
                    break
```

- do_host_memory_list
  - imemorys Vs memorys

```sh
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/memory_shell.py
```

- do_host_memory_show

```sh
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/memory_shell.py
```

- do_host_memory_modify

```sh
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/memory_shell.py
```

From Glance source code:

```sh
commit 3e35dd0033acd8249e4b2fd9b37c5914b755cba6
Author: Waldemar Znoinski <waldemar.znoinski@intel.com>
Date:   Tue Sep 29 22:32:10 2015 +0000

    Add Large pages meta definition
    
    This metadef enables Guest Memory backing namespace
    and large pagesize property in Nova::Flavor,
    Glance::Image, Cinder::Volume(image)
    resource types metadata
```

### Tests

- Modification
  - A existing instance, Huge pages configuration & Memory reserved for platform on compute node.
  - A new instance

```sh
$ openstack flavor set m1.large --property hw:mem_page_size=large
```

Links

- https://docs.openstack.org/nova/pike/admin/huge-pages.html

### Workshop

```sh

usage: system host-memory-modify [-m <Platform Reserved MiB>]
                                 [-2M <2M hugepages number>]
                                 [-1G <1G hugepages number>]
                                 <hostname or id> <processor>
```

# Patches

- 01 Enable delete bound trunk for linux bridge agent
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-August/000698.html
- 02 Prevent DHCP from OpenStack Charms Deployment Guideprocessing stale RPC messages
  - https://bugs.launchpad.net/neutron/+bug/1795212
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-December/002364.html
- 0039-dhcp-handle-concurrent-port-creation-error.patch
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-February/003222.html
  - https://bugs.launchpad.net/neutron/+bug/1760047
- 0040-rpc-removing-timeout-backoff-multiplier.patch (2e4ca00)
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-January/002544.html
- 0047-wsgi-prevent-accepting-socket-without-a-gr.patch (8e72491)
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-November/002051.html
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-February/003222.html
- 0062-dvr-force-admin-state-update-before-distri.patch (edf9731)
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-November/002051.html
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-February/003222.html
- 0065-dvr-do-not-create-agent-gateway-ports-unle.patch (2857911)
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-February/003222.html
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-November/002051.html
- 0077-api-reject-routes-with-invalid-network-val.patch (6ed8fe6)
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2019-January/002544.html
  - http://lists.starlingx.io/pipermail/starlingx-discuss/2018-November/002051.html
- 0014-add-support-for-querying-quotas-with-usage.patch (71c07d7)
  - Tbd

# Migration

- https://docs.openstack.org/nova/latest/admin/configuring-migrations.html
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/virtualization/chap-virtualization-kvm_live_migration

Types

- Live Migration, Live
- Cold Migration, Offline
- Evacuation

## Migration Requirements

- Storage
  - Settings
    - Flavor extra specifications
    - Image metadata
    - Instance metadata
  - Migration
    - Local Ephemeral
      - Not all supported
    - Remote Ephemeral
      - VM ephemeral
      -  Swap Disks
      - Boot-from-image Root Disks
  - Resize
  - Evacuation
- Network

## Cold Migration

> Non-live migration, also known as cold migration or simply migration. The instance is shut down, then moved to another hypervisor and restarted. The instance recognizes that it was rebooted, and the application running on the instance is disrupted.

## Live Migration

> The instance keeps running throughout the migration. This is useful when it is not possible or desirable to stop the application running on the instance.

Why

- Cloud maintenance
  - Upgrade Host
  - Patching
  - Upgrade n-Cpu
- Re-balance Workload
- Hardware Failover
- Energy Saving
- Georaphic Migration

Virtual Machine

- CPU / Memory
- Network
- Disk
  - Remote
  - Local

Links

- [OpenStack Nova](https://docs.openstack.org/nova/latest/admin/live-migration-usage.html)

Details

- Phases
  - Runs at source node
    - Setup, sync disk
    - Transfer memory
    - Flush
    - Dirtying memory
  - Pause
    - Downtime
    - Copy memory: dirty memory
  - Runs at destination node
    - Activate network
    - Arrange source
- Tradeoffs
  - Minimize downtime
  - Complete quickly
- Features
  - Auto-Converged (VM Throttling)
  - Post-Copy
  - Maximum Downtime per VM
- Storage Types
  - Shared storage is quicker, but local storage is possible
  - Block live migration: or simply block migration. The instance has ephemeral disks that are not shared between the source and destination hosts. Block migration is incompatible with read-only devices such as CD-ROMs and Configuration Drive (config_drive).
    - Ephemeral Disk
    - Live migration is not always supported for VM disks using local ephemeral storage.
  - Volume-backed live migration: Instances use volumes rather than ephemeral disks.
    - Attached Cinder Volumes
    - Ephemeral Disks backed by Ceph via RBD
  - Shared storage-based live migration: The instance has ephemeral disks that are located on storage shared between the source and destination hosts.
  - Mix of local qcow2 and cinder volumes
- Hosts
  - KVM-libvirt
- Policies
  - Affinity: 
  - Non-Affinity
- Limitations
  - iso9660 is not migratable
  - PCI Alias?
  - Direct Guest Access to NICs
    - PCI passthrough
    - SR-IOV
    - Enhanced NUMA scheduling options
  - Live migration is not supported for instances with SR-IOV ports. [Here](https://docs.openstack.org/newton/networking-guide/config-sriov.html)
  - Flavor extra specifications, image metadata, or instance metadata.
    - Extra Properties, From Glance
      - hw_wrs_live_migration_max_downtime
      - hw_wrs_live_migration_timeout
      - sw_wrs_auto_recovery
  - Instance Boot Type and Ephemeral and Swap Disks from flavor
  - Search under StarlingX for "does not support live migration"
    - cgcs-root/stx/git/qemu/block/parallels.c
    - cgcs-root/stx/git/qemu/block/qcow.c
    - cgcs-root/stx/git/qemu/block/vdi.c
    - cgcs-root/stx/git/qemu/block/vhdx.c
    - cgcs-root/stx/git/qemu/block/vmdk.c
    - cgcs-root/stx/git/qemu/block/vpc.c
    - cgcs-root/stx/git/qemu/block/vvfat.ccgcs-root/stx/git/glance/
    - cgcs-root/stx/git/qemu/contrib/vhost-user-blk/vhost-user-blk.c
- Output
  - Timeout
  - Tradeoffs
- Procedures
  - From
    - Manual
    - Tempest
    - Single
    - Parallel
      - 2 Compute hosts
  - Flavors
    - Small
    - Medium
    - Large
  - Images
    - Nova Ephemeral Block Storage
      - CoW-image backed storage ???
    - Remote Ephemeral, Storage Hosts
      - Ephemeral, Swap, Boot from Image Root Disk ???
    - Linux
    - Windows
      - --property os_type=windows
  - Parameters
    - Affinity
    - Anti-Affinity
    - Best Effort
    - Boot Configuration
  - Network
    - DPDK
    - SR-IOV No support
    - PCI Passthrough No support
  - Virtual Machine
    - Single
    - Server Groups
  - UI
    - Current Host
    - New Host
    - Block Migration
  - Workload Activity
    - Indefinite Ping
  - Admin > Panel > Instances
    - Live Migrate Instance
  - Fault Management
    - Alarm ID 700.008
    - Timeout
      - 800 seconds
      - 180 seconds
    - Maximum Downtime
      - 500 msec
  - Performancecgcs-root/stx/git/glance/
    - How much downtime
    - How long to pause
    - Metrics
      - Average non-block storage duration
      - Min/Max non-block storage duration
      - Average block storage duration
      - Min/Max block storage duration
   - Live Migration Optimization
     - 10 Gb Management / Infrastructure Network
     - StarlingX API to 
     - Tune
       - Flavor Extra Specs
       - Metadata
         - Image
         - Instance
       - Settings
         - Timeout
         - Maximum Downtime
         - Auto-Converge

#### Maximum Downtime

- live_migration_max_downtime
- hw_wrs_live_migration_max_downtime
- migrate_configure_max_downtime
- _max_live_migration_downtime_in_ms

```sh
$ repo grep max | grep downtime
```

```sh
cgcs-root/stx/git/ceph/qa/qa_scripts/openstack/files/nova.template.conf
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json
cgcs-root/stx/git/glance/etc/metadefs/tis-live-migration-image.json
cgcs-root/stx/git/glance/etc/metadefs/tis-live-migration-instance.json
cgcs-root/stx/git/glance/glance/api/v1/images.py
cgcs-root/stx/git/glance/stx-patches/0001-Pike-Rebase-Validate-image-properties.patch
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch
cgcs-root/stx/git/libvirt/docs/news-2010.html.in
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_sw_upgrade_strategy.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/utils.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/api/controllers/v1/virtualised_resources/_computes_api.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/audits/_vim_nfvi_audits.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/database/_database_compute_module.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/database/model/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/directors/_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/events/_vim_instance_api_events.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_image.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_image.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/rpc/_rpc_message_instance.py
cgcs-root/stx/git/nova/doc/source/admin/configuring-migrations.rst
cgcs-root/stx/git/nova/doc/source/contributor/testing/zero-downtime-upgrade.rst
cgcs-root/stx/git/nova/doc/source/contributor/testing/zero-downtime-upgrade.rst
cgcs-root/stx/git/nova/nova/conf/libvirt.py
cgcs-root/stx/git/nova/nova/tests/unit/virt/libvirt/test_guest.py
cgcs-root/stx/git/nova/nova/tests/unit/virt/libvirt/test_migration.py
cgcs-root/stx/git/nova/nova/virt/libvirt/driver.py
cgcs-root/stx/git/nova/nova/virt/libvirt/guest.py
cgcs-root/stx/git/nova/nova/virt/libvirt/migration.py
cgcs-root/stx/git/qemu/hmp-commands.hx
cgcs-root/stx/git/qemu/hw/i386/kvm/clock.c
cgcs-root/stx/git/qemu/hw/ppc/ppc.c
cgcs-root/stx/git/qemu/migration/block.c
cgcs-root/stx/git/qemu/migration/migration.h
cgcs-root/stx/git/qemu/qapi/migration.json
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/nfvi_plugins/nfvi_compute_api.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/test_data/nfv_vim_db_15.12_patch002
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_sw_patch_strategy.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/utils.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/api/controllers/v1/virtualised_resources/_computes_api.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/audits/_vim_nfvi_audits.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/database/_database_compute_module.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/database/model/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/directors/_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/events/_vim_instance_api_events.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_image.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_image.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_instance_type.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/rpc/_rpc_message_instance.py
```

##### live_migration_max_downtime

Found at:

```sh
cgcs-root/stx/git/glance
cgcs-root/stx/stx-nfv
```

##### hw_wrs_live_migration_max_downtime

Found at:

```sh
cgcs-root/stx/git/glance
cgcs-root/stx/stx-nfv
```

##### migrate_configure_max_downtime

```sh
cgcs-root/stx/git/nova
```

#####  _max_live_migration_downtime_in_ms

```sh
cgcs-root/stx/stx-nfv
```

### Training

- Definition
- Limitations
- Virtual Machines
- Fault Management

### Source Code

```sh
cgcs-root/stx/git/ceph
cgcs-root/stx/git/cinder
cgcs-root/stx/git/glance
cgcs-root/stx/git/horizon
cgcs-root/stx/git/ironic
cgcs-root/stx/git/kubernetes
cgcs-root/stx/git/libvirt
cgcs-root/stx/git/neutron
cgcs-root/stx/git/nova
cgcs-root/stx/git/python-novaclient
cgcs-root/stx/git/python-openstackclient
cgcs-root/stx/git/python-openstacksdk
cgcs-root/stx/git/qemu
cgcs-root/stx/stx-config
cgcs-root/stx/stx-fault
cgcs-root/stx/stx-integ
cgcs-root/stx/stx-nfv
```

```sh
cgcs-root/stx/git/ceph/doc/changelog/v0.67.10.txt
cgcs-root/stx/git/ceph/doc/changelog/v0.80.6.txt
cgcs-root/stx/git/ceph/doc/man/8/rbd.rst
cgcs-root/stx/git/ceph/doc/rbd/qemu-rbd.rst
cgcs-root/stx/git/ceph/doc/rbd/rbd-openstack.rst
cgcs-root/stx/git/ceph/qa/qa_scripts/openstack/files/cinder.template.conf
cgcs-root/stx/git/ceph/qa/qa_scripts/openstack/files/nova.template.conf
cgcs-root/stx/git/ceph/src/include/rbd/librbd.h
cgcs-root/stx/git/cinder/cinder/locale/ja/LC_MESSAGES/cinder.po
cgcs-root/stx/git/cinder/cinder/volume/drivers/dell_emc/ps.py
cgcs-root/stx/git/cinder/cinder/volume/drivers/dell_emc/vmax/common.py
cgcs-root/stx/git/cinder/cinder/volume/drivers/dell_emc/vmax/fc.py
cgcs-root/stx/git/cinder/cinder/volume/drivers/dell_emc/vmax/iscsi.py
cgcs-root/stx/git/cinder/cinder/volume/drivers/dell_emc/vmax/masking.py
cgcs-root/stx/git/cinder/cinder/volume/drivers/netapp/options.py
cgcs-root/stx/git/cinder/doc/source/configuration/block-storage/drivers/dell-emc-unity-driver.rst
cgcs-root/stx/git/cinder/doc/source/configuration/block-storage/drivers/emc-vmax-driver.rst
cgcs-root/stx/git/cinder/doc/source/configuration/block-storage/drivers/emc-vnx-driver.rst
cgcs-root/stx/git/cinder/doc/source/configuration/tables/cinder-netapp_eseries_iscsi.inc
cgcs-root/stx/git/cinder/releasenotes/notes/live_migration_v3-ae98c0d00e64c954.yaml
cgcs-root/stx/git/cinder/releasenotes/notes/ps-duplicate-ACL-5aa447c50f2474e7.yaml
cgcs-root/stx/git/glance/doc/source/glossary.rst
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json
cgcs-root/stx/git/glance/etc/metadefs/tis-live-migration-image.json
cgcs-root/stx/git/glance/etc/metadefs/tis-live-migration-instance.json
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch
cgcs-root/stx/git/horizon/openstack_dashboard/dashboards/admin/instances/forms.py
cgcs-root/stx/git/horizon/openstack_dashboard/locale/cs/LC_MESSAGES/django.po
...
cgcs-root/stx/git/horizon/openstack_dashboard/locale/zh_TW/LC_MESSAGES/django.po
cgcs-root/stx/git/ironic/doc/source/user/index.rst
cgcs-root/stx/git/kubernetes/vendor/google.golang.org/api/compute/v0.alpha/compute-api.json
cgcs-root/stx/git/kubernetes/vendor/google.golang.org/api/compute/v0.alpha/compute-gen.go
cgcs-root/stx/git/libvirt/docs/apps.html.in
cgcs-root/stx/git/libvirt/docs/formatcaps.html.in
cgcs-root/stx/git/libvirt/docs/internals/locking.html.in
cgcs-root/stx/git/libvirt/docs/news-2010.html.in
cgcs-root/stx/git/libvirt/include/libvirt/libvirt-domain.h
cgcs-root/stx/git/libvirt/po/as.mini.po
...
cgcs-root/stx/git/libvirt/po/zh_CN.mini.po
cgcs-root/stx/git/libvirt/src/conf/capabilities.c
cgcs-root/stx/git/libvirt/src/libvirt-domain.c
cgcs-root/stx/git/libvirt/tools/virsh.pod
cgcs-root/stx/git/neutron/doc/source/contributor/internals/images/live-mig-ovs-hybrid.txt
cgcs-root/stx/git/neutron/doc/source/contributor/internals/images/live-mig.txt
cgcs-root/stx/git/neutron/neutron/db/l3_dvr_db.py
cgcs-root/stx/git/neutron/neutron/notifiers/nova.py
cgcs-root/stx/git/neutron/neutron/plugins/ml2/drivers/macvtap/mech_driver/mech_macvtap.py
cgcs-root/stx/git/neutron/neutron/plugins/ml2/rpc.py
cgcs-root/stx/git/neutron/neutron/services/trunk/drivers/openvswitch/agent/ovsdb_handler.py
cgcs-root/stx/git/neutron/neutron/tests/functional/agent/l3/test_dvr_router.py
cgcs-root/stx/git/neutron/neutron/tests/functional/services/l3_router/test_l3_dvr_router_plugin.py
cgcs-root/stx/git/neutron/releasenotes/notes/dvr-support-live-migration-b818b12bd9cbb518.yaml
cgcs-root/stx/git/neutron/releasenotes/notes/macvtap-l2-agent-2b551d8ec341196d.yaml
cgcs-root/stx/git/nova/.zuul.yaml
cgcs-root/stx/git/nova/api-guide/source/server_concepts.rst
cgcs-root/stx/git/nova/api-ref/source/parameters.yaml
cgcs-root/stx/git/nova/api-ref/source/server-migrations.inc
cgcs-root/stx/git/nova/doc/source/admin/configuration/hypervisor-hyper-v.rst
cgcs-root/stx/git/nova/doc/source/admin/configuration/hypervisor-kvm.rst
cgcs-root/stx/git/nova/doc/source/admin/configuration/hypervisor-xen-api.rst
cgcs-root/stx/git/nova/doc/source/admin/configuration/schedulers.rst
cgcs-root/stx/git/nova/doc/source/admin/configuring-migrations.rst
cgcs-root/stx/git/nova/doc/source/admin/live-migration-usage.rst
cgcs-root/stx/git/nova/doc/source/admin/node-down.rst
cgcs-root/stx/git/nova/doc/source/admin/secure-live-migration-with-qemu-native-tls.rst
cgcs-root/stx/git/nova/doc/source/admin/security.rst
cgcs-root/stx/git/nova/doc/source/admin/support-compute.rst
cgcs-root/stx/git/nova/doc/source/admin/virtual-gpu.rst
cgcs-root/stx/git/nova/doc/source/common/numa-live-migration-warning.txt
cgcs-root/stx/git/nova/doc/source/reference/index.rst
cgcs-root/stx/git/nova/doc/source/user/cellsv2-layout.rst
cgcs-root/stx/git/nova/doc/source/user/support-matrix.ini
cgcs-root/stx/git/nova/nova/api/openstack/api_version_request.py
cgcs-root/stx/git/nova/nova/api/openstack/compute/rest_api_version_history.rst
cgcs-root/stx/git/nova/nova/api/openstack/compute/server_migrations.py
cgcs-root/stx/git/nova/nova/compute/api.py
cgcs-root/stx/git/nova/nova/compute/manager.py
cgcs-root/stx/git/nova/nova/compute/resource_tracker.py
cgcs-root/stx/git/nova/nova/conductor/tasks/live_migrate.py
cgcs-root/stx/git/nova/nova/conf/compute.py
cgcs-root/stx/git/nova/nova/conf/hyperv.py
cgcs-root/stx/git/nova/nova/conf/libvirt.py
cgcs-root/stx/git/nova/nova/conf/rpc.py
cgcs-root/stx/git/nova/nova/conf/workarounds.py
cgcs-root/stx/git/nova/nova/db/sqlalchemy/models.py
cgcs-root/stx/git/nova/nova/exception.py
cgcs-root/stx/git/nova/nova/locale/cs/LC_MESSAGES/nova.po
...
cgcs-root/stx/git/nova/nova/locale/zh_TW/LC_MESSAGES/nova.po
cgcs-root/stx/git/nova/nova/network/base_api.py
cgcs-root/stx/git/nova/nova/network/neutronv2/api.py
cgcs-root/stx/git/nova/nova/objects/migrate_data.py
cgcs-root/stx/git/nova/nova/objects/service.py
cgcs-root/stx/git/nova/nova/policies/servers_migrations.py
cgcs-root/stx/git/nova/nova/scheduler/utils.py
cgcs-root/stx/git/nova/nova/tests/functional/regressions/test_bug_1797580.py
cgcs-root/stx/git/nova/nova/tests/functional/test_availability_zones.py
cgcs-root/stx/git/nova/nova/tests/functional/test_server_group.py
cgcs-root/stx/git/nova/nova/tests/functional/test_servers.py
cgcs-root/stx/git/nova/nova/tests/unit/api/openstack/compute/test_migrations.py
cgcs-root/stx/git/nova/nova/tests/unit/api/openstack/compute/test_server_migrations.py
cgcs-root/stx/git/nova/nova/tests/unit/compute/test_compute.py
cgcs-root/stx/git/nova/nova/tests/unit/compute/test_compute_mgr.py
cgcs-root/stx/git/nova/nova/tests/unit/conductor/test_conductor.py
cgcs-root/stx/git/nova/nova/tests/unit/virt/libvirt/test_driver.py
cgcs-root/stx/git/nova/nova/tests/unit/virt/test_virt_drivers.py
cgcs-root/stx/git/nova/nova/virt/driver.py
cgcs-root/stx/git/nova/nova/virt/fake.py
cgcs-root/stx/git/nova/nova/virt/hyperv/livemigrationops.py
cgcs-root/stx/git/nova/nova/virt/hyperv/volumeops.py
cgcs-root/stx/git/nova/nova/virt/libvirt/driver.py
cgcs-root/stx/git/nova/nova/virt/libvirt/guest.py
cgcs-root/stx/git/nova/nova/virt/libvirt/host.py
cgcs-root/stx/git/nova/nova/virt/libvirt/migration.py
cgcs-root/stx/git/nova/nova/virt/libvirt/utils.py
cgcs-root/stx/git/nova/nova/virt/powervm/host.py
cgcs-root/stx/git/nova/nova/virt/vmwareapi/driver.py
cgcs-root/stx/git/nova/nova/virt/vmwareapi/vmops.py
cgcs-root/stx/git/nova/nova/virt/xenapi/driver.py
cgcs-root/stx/git/nova/nova/virt/libvirt/guest.py
cgcs-root/stx/git/nova/nova/virt/libvirt/host.py
cgcs-root/stx/git/nova/nova/virt/libvirt/migration.py
cgcs-root/stx/git/nova/nova/virt/libvirt/utils.py
cgcs-root/stx/git/nova/nova/virt/powervm/host.py
cgcs-root/stx/git/nova/nova/virt/vmwareapi/driver.py
cgcs-root/stx/git/nova/nova/virt/vmwareapi/vmops.py
cgcs-root/stx/git/nova/nova/virt/xenapi/driver.py
cgcs-root/stx/git/nova/nova/virt/xenapi/vmops.py
cgcs-root/stx/git/nova/playbooks/legacy/nova-grenade-live-migration/run.yaml
cgcs-root/stx/git/nova/releasenotes/notes/abort-live-migration-cb902bb0754b11b6.yaml
cgcs-root/stx/git/nova/releasenotes/notes/abort-live-migration-in-queue-0c917f415d6dac5a.yaml
cgcs-root/stx/git/nova/releasenotes/notes/add-live-migration-support-in-xapi-pool-5ac42c2468c3616e.yaml
cgcs-root/stx/git/nova/releasenotes/notes/automatic-live-migration-completion-auto-converge-3ddd3a40eaf3ef5b.yaml
cgcs-root/stx/git/nova/releasenotes/notes/automatic-live-migration-completion-post-copy-a7a3a986961c93d8.yaml
cgcs-root/stx/git/nova/releasenotes/notes/bp-split-network-plane-for-live-migration-40bc127734173759.yaml
cgcs-root/stx/git/nova/releasenotes/notes/bug-1712008-4ab2538211b8c3d9.yaml
cgcs-root/stx/git/nova/releasenotes/notes/disable-live-migration-with-numa-bc710a1bcde25957.yaml
cgcs-root/stx/git/nova/releasenotes/notes/force-live-migration-be5a10cd9c8eb981.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt-change-default-value-of-live-migration-tunnelled-4248cf76df605fdf.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt-live-migration-speed-limit-revert-81a9d29d60b0df4b.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt_fix_ipv6_live_migration-bbcde8f3b7d17921.yaml
cgcs-root/stx/git/nova/releasenotes/notes/live_migration_wait_for_vif_plug-c9dcb034067890d8.yaml
cgcs-root/stx/git/nova/releasenotes/notes/mutable-config-e7e82b3d7c38f3a5.yaml
cgcs-root/stx/git/nova/releasenotes/notes/qemu-native-luks-decryption-6e9ad8cc658be14d.yaml
cgcs-root/stx/git/nova/nova/virt/xenapi/driver.py
cgcs-root/stx/git/nova/nova/virt/xenapi/vmops.py
cgcs-root/stx/git/nova/playbooks/legacy/nova-grenade-live-migration/run.yaml
cgcs-root/stx/git/nova/releasenotes/notes/abort-live-migration-cb902bb0754b11b6.yaml
cgcs-root/stx/git/nova/releasenotes/notes/abort-live-migration-in-queue-0c917f415d6dac5a.yaml
cgcs-root/stx/git/nova/releasenotes/notes/add-live-migration-support-in-xapi-pool-5ac42c2468c3616e.yaml
cgcs-root/stx/git/nova/releasenotes/notes/automatic-live-migration-completion-auto-converge-3ddd3a40eaf3ef5b.yaml
cgcs-root/stx/git/nova/releasenotes/notes/automatic-live-migration-completion-post-copy-a7a3a986961c93d8.yaml
cgcs-root/stx/git/nova/releasenotes/notes/bp-split-network-plane-for-live-migration-40bc127734173759.yaml
cgcs-root/stx/git/nova/releasenotes/notes/bug-1712008-4ab2538211b8c3d9.yaml
cgcs-root/stx/git/nova/releasenotes/notes/disable-live-migration-with-numa-bc710a1bcde25957.yaml
cgcs-root/stx/git/nova/releasenotes/notes/force-live-migration-be5a10cd9c8eb981.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt-change-default-value-of-live-migration-tunnelled-4248cf76df605fdf.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt-live-migration-speed-limit-revert-81a9d29d60b0df4b.yaml
cgcs-root/stx/git/nova/releasenotes/notes/libvirt_fix_ipv6_live_migration-bbcde8f3b7d17921.yaml
cgcs-root/stx/git/nova/releasenotes/notes/live_migration_wait_for_vif_plug-c9dcb034067890d8.yaml
cgcs-root/stx/git/nova/releasenotes/notes/mutable-config-e7e82b3d7c38f3a5.yaml
cgcs-root/stx/git/nova/releasenotes/notes/qemu-native-luks-decryption-6e9ad8cc658be14d.yaml
cgcs-root/stx/git/nova/releasenotes/notes/rocky-prelude-b78b51b9026ed336.yaml
cgcs-root/stx/git/nova/releasenotes/notes/rpc_timeout_changes-6b7e365bb44f7f3a.yaml
cgcs-root/stx/git/nova/releasenotes/notes/server_migrations-30519b35d3ea6763.yaml
cgcs-root/stx/git/nova/releasenotes/notes/stein-prelude-b5fe92310e1e725e.yaml
cgcs-root/stx/git/nova/releasenotes/notes/vmware-live-migration-c09cce337301cab0.yaml
cgcs-root/stx/git/nova/releasenotes/source/locale/ja/LC_MESSAGES/releasenotes.po
cgcs-root/stx/git/nova/releasenotes/source/mitaka.rst
cgcs-root/stx/git/nova/releasenotes/source/newton.rst
cgcs-root/stx/git/python-novaclient/doc/source/cli/nova.rst
cgcs-root/stx/git/python-novaclient/novaclient/v2/server_migrations.py
cgcs-root/stx/git/python-novaclient/novaclient/v2/servers.py
cgcs-root/stx/git/python-novaclient/novaclient/v2/shell.py
cgcs-root/stx/git/python-novaclient/releasenotes/notes/microversion-v2_65-3c89c5932f4391cb.yaml
cgcs-root/stx/git/python-openstackclient/doc/source/cli/data/nova.csv
cgcs-root/stx/git/python-openstackclient/openstackclient/compute/v2/server.py
cgcs-root/stx/git/python-openstackclient/openstackclient/locale/tr_TR/LC_MESSAGES/openstackclient.po
cgcs-root/stx/git/python-openstacksdk/openstack/compute/v2/_proxy.py
cgcs-root/stx/git/qemu/Changelog
cgcs-root/stx/git/qemu/block/file-posix.c
cgcs-root/stx/git/qemu/block/parallels.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/qcow.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/vdi.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/vhdx.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/vmdk.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/vpc.c:               "does not support live migration",
cgcs-root/stx/git/qemu/block/vvfat.c:                   "does not support live migration",
cgcs-root/stx/git/qemu/contrib/vhost-user-blk/vhost-user-blk.c:    /* don't support live migration */
cgcs-root/stx/git/qemu/docs/devel/memory.txt
cgcs-root/stx/git/qemu/docs/devel/migration.rst
cgcs-root/stx/git/qemu/docs/devel/qapi-code-gen.txt
cgcs-root/stx/git/qemu/docs/interop/vhost-user.txt
cgcs-root/stx/git/qemu/docs/multi-thread-compression.txt
cgcs-root/stx/git/qemu/docs/rdma.txt
cgcs-root/stx/git/qemu/docs/xbzrle.txt
cgcs-root/stx/git/qemu/hw/block/virtio-blk.c
cgcs-root/stx/git/qemu/hw/char/sclpconsole-lm.c
cgcs-root/stx/git/qemu/hw/i386/pc_piix.c
cgcs-root/stx/git/qemu/hw/net/can/can_sja1000.c
cgcs-root/stx/git/qemu/hw/net/vmxnet3.c
cgcs-root/stx/git/qemu/hw/usb/hcd-xhci.h
cgcs-root/stx/git/qemu/include/hw/mem/pc-dimm.h
cgcs-root/stx/git/qemu/migration/channel.c
cgcs-root/stx/git/qemu/migration/channel.h
cgcs-root/stx/git/qemu/migration/exec.c
cgcs-root/stx/git/qemu/migration/exec.h
cgcs-root/stx/git/qemu/migration/fd.c
cgcs-root/stx/git/qemu/migration/fd.h
cgcs-root/stx/git/qemu/migration/migration.c
cgcs-root/stx/git/qemu/migration/migration.h
cgcs-root/stx/git/qemu/migration/savevm.c
cgcs-root/stx/git/qemu/migration/socket.c
cgcs-root/stx/git/qemu/migration/socket.h
cgcs-root/stx/git/qemu/migration/xbzrle.h
cgcs-root/stx/git/qemu/qapi/migration.json
cgcs-root/stx/git/qemu/qemu-deprecated.texi
cgcs-root/stx/git/qemu/qemu-options.hx
cgcs-root/stx/git/qemu/target/i386/machine.c
cgcs-root/stx/git/qemu/target/s390x/cpu.h
cgcs-root/stx/git/qemu/tests/qemu-iotests/091
cgcs-root/stx/git/qemu/tests/qemu-iotests/181
cgcs-root/stx/git/qemu/tests/qemu-iotests/201
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/events_for_testing.yaml
cgcs-root/stx/stx-fault/fm-doc/fm_doc/events.yaml
cgcs-root/stx/stx-integ/tools/vm-topology/vm-topology/vm_topology/exec/vm_topology.py
cgcs-root/stx/stx-integ/virt/libvirt/centos/libvirt.spec
cgcs-root/stx/stx-integ/virt/qemu/centos/qemu-kvm.spec
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/nfvi_plugins/nfvi_compute_api.py
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/nfvi_plugins/openstack/nova.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/alarm/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/directors/_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/instance_fsm/_instance_state_live_migrate.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/instance_fsm/_instance_task_work.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/nfvi/objects/v1/_instance.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_image.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/objects/_instance.py
```
