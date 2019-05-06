# General

- https://etherpad.openstack.org/p/stx-networking

# Terminology

- Tenant: A group of users is referred to as a project or tenant. There terms are interchangeable. [Openstack Operations Guide](https://docs.openstack.org/operations-guide/ops-projects-users.html).
- StarlingX Nova scheduler: A modified version of the OpenStack Nova scheduler.
- NUMA: Non-uniform memory access (NUMA) is a computer memory design used in multiprocessing, where the memory access time depends on the memory location relative to the processor. [Wikipedia](https://en.wikipedia.org/wiki/Non-uniform_memory_access)
- Live Migration: Live-migrating an instance means moving its virtual machine to a different OpenStack Compute server while the instance continues running. [Openstack Compute Nova](https://docs.openstack.org/nova/pike/admin/live-migration-usage.html)
- PCI pass through allows compute nodes to pass a physical PCI device to a hosted VM. This can be used for direct access to a PCI device inside the VM. [OpenStack Charms Deployment Guide](https://docs.openstack.org/project-deploy-guide/charm-deployment-guide/latest/app-pci-passthrough-gpu.html)

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

# Trunk

"Trunk" has some relationship with the following terminology:

- Network Management
- Tenant
  - It
- User Guide

# NUMA

- https://specs.openstack.org/openstack/nova-specs/specs/rocky/approved/numa-aware-live-migration.html

Details

- Relationship with:
  - CPU Pinning
  - Hugepages
  - Best Effort

# Live Migration

- [OpenStack Nova](https://docs.openstack.org/nova/latest/admin/live-migration-usage.html)

Details

- Types
  - Block
    - Ephemeral Disk
  - Volume-backed
    - Attached Cinder Volumes
    - Ephemeral Disks backed by Ceph via RBD
  - Shared storage
- Policies
  - Affinity: 
  - Non-Affinity
- Limitations
  - iso9660 is not migratable
  - PCI passthrough?
  - PCI Alias?
  - Live migration is not supported for instances with SR-IOV ports. [Here](https://docs.openstack.org/newton/networking-guide/config-sriov.html)
