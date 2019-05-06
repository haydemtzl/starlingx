# General

- https://etherpad.openstack.org/p/stx-networking

# Terminology

- Tenant: A group of users is referred to as a project or tenant. There terms are interchangeable. [Openstack Operations Guide](https://docs.openstack.org/operations-guide/ops-projects-users.html).
- StarlingX Nova scheduler: A modified version of the OpenStack Nova scheduler.
- NUMA: Non-uniform memory access (NUMA) is a computer memory design used in multiprocessing, where the memory access time depends on the memory location relative to the processor. [Wikipedia](https://en.wikipedia.org/wiki/Non-uniform_memory_access)
- Live Migration: Live-migrating an instance means moving its virtual machine to a different OpenStack Compute server while the instance continues running. [Openstack Compute Nova](https://docs.openstack.org/nova/pike/admin/live-migration-usage.html)
- PCI pass through allows compute nodes to pass a physical PCI device to a hosted VM. This can be used for direct access to a PCI device inside the VM. [OpenStack Charms Deployment Guide](https://docs.openstack.org/project-deploy-guide/charm-deployment-guide/latest/app-pci-passthrough-gpu.html)

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

## NUMA

- https://specs.openstack.org/openstack/nova-specs/specs/rocky/approved/numa-aware-live-migration.html

Details

- Relation with:
  - CPU Pinning
  - Hugepages
- Affinity
  - Strict
  - Best Effort

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

Types

- Live Migration
- Cold Migration
- Evacuation

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
- Types
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
  - PCI passthrough?
  - PCI Alias?
  - Live migration is not supported for instances with SR-IOV ports. [Here](https://docs.openstack.org/newton/networking-guide/config-sriov.html)
  - Flavor extra specifications, image metadata, or instance metadata.
  - Instance Boot Type and Ephemeral and Swap Disks from flavor
  - Search under StarlingX for "does not support live migration"
    - cgcs-root/stx/git/qemu/block/parallels.c
    - cgcs-root/stx/git/qemu/block/qcow.c
    - cgcs-root/stx/git/qemu/block/vdi.c
    - cgcs-root/stx/git/qemu/block/vhdx.c
    - cgcs-root/stx/git/qemu/block/vmdk.c
    - cgcs-root/stx/git/qemu/block/vpc.c
    - cgcs-root/stx/git/qemu/block/vvfat.c
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
  - Performance
    - How much downtime
    - How long to pause
    - Metrics
      - Average non-block storage duration
      - Min/Max non-block storage duration
      - Average block storage duration
      - Min/Max block storage duration
   - Optimization
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
