# Glossary

- Physical Functions (PF)
  - Virtual Functions (VF)
- Network Function Virtualisation (NFV)
  - Single Root I/O Virtualisation (SR-IOV)
  - PCI-Passthrough
  - DPDK
  - CPU Pinning
  - NUMA Nodes

# Repo Grep

## sriov_numfs

```sh
$ repo grep sriov_numvfs
```

- neutron
- stx-config
- stx-gui
- stx-integ
- stx-metal

```sh
cgcs-root/stx/git/neutron/doc/source/admin/config-sriov.rst
cgcs-root/stx/stx-config/api-ref/source/api-ref-sysinv-v1-config.rst
cgcs-root/stx/stx-config/puppet-manifests/src/bin/apply_network_config.sh
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/iinterface.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/iinterface_shell.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/port_shell.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/manager.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/pci.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/ethernet_port.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/host.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/interface.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/pci_device.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/port.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/profile.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/conductor/manager.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/db/sqlalchemy/migrate_repo/versions/002_consolidated_rel15ga.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/db/sqlalchemy/models.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/objects/interface.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/objects/interface_base.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/objects/pci_device.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/objects/port.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/interface.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/nova.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/api/test_interface.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/db/sqlalchemy/test_migrations.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/db/utils.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/puppet/test_interface.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/api/sysinv.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/interfaces/forms.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/devices/_detail_overview.html
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/interfaces/_detail_overview.html
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/ports/_detail_overview.html
cgcs-root/stx/stx-integ/kernel/kernel-modules/qat17/files/qat_service
cgcs-root/stx/stx-integ/utilities/nova-utils/nova-utils/nova-sriov
cgcs-root/stx/stx-metal/api-ref/source/api-ref-sysinv-v1-metal.rst
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/manager.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/pci.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/ethernet_port.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/pci_device.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/port.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/conductor/manager.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/db/sqlalchemy/migrate_repo/versions/001_init.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/db/sqlalchemy/models.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/objects/pci_device.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/objects/port.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/systemconfig/plugin.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/pci_device_shell.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/port_shell.py
```

# Documentation

## Neutron 

- https://docs.openstack.org/neutron/latest/admin/config-sriov.html
- https://docs.openstack.org/mitaka/networking-guide/config-sriov.html

Steps

1. Create Virtual Functions (Compute)
2. Whitelist PCI devices in nova-compute (Compute)
3. Configure neutron-server (Controller)
4. Configure nova-scheduler (Controller)
5. Enable neutron sriov-agent (Compute)


## NUMA

- https://specs.openstack.org/openstack/nova-specs/specs/queens/implemented/share-pci-between-numa-nodes.html

What part of the source code it is found?

- stx-config
- stx-metal

What are key functions?

- get_minimum_platform_reserved_memory
- get_required_platform_reserved_memory

# Architecture

- stx-config/sysinv
- stx-metal/inventory

## Network Types

- PLATFORM_NETWORK_TYPES
  - NETWORK_TYPE_INFRA
  - NETWORK_TYPE_MGMT
  - NETWORK_TYPE_OAM
  - NETWORK_TYPE_CLUSTER_HOST
  - NETWORK_TYPE_PXEBOOT
- PCI_NETWORK_TYPES
- NETWORK_TYPE_NONE
- NETWORK_TYPE_INFRA
- NETWORK_TYPE_MGMT
- NETWORK_TYPE_OAM
- NETWORK_TYPE_BM
- NETWORK_TYPE_MULTICAST
- NETWORK_TYPE_DATA
- NETWORK_TYPE_SYSTEM_CONTROLLER
- NETWORK_TYPE_CLUSTER_HOST
- NETWORK_TYPE_CLUSTER_POD
- NETWORK_TYPE_CLUSTER_SERVICE
- NETWORK_TYPE_PCI_PASSTHROUGH
- NETWORK_TYPE_PCI_SRIOV
- NETWORK_TYPE_PXEBOOT

## Interface Types

- INTERFACE_TYPE_ETHERNET
- INTERFACE_TYPE_VLAN
- INTERFACE_TYPE_AE
- INTERFACE_TYPE_VIRTUAL

## Interface Class

- INTERFACE_CLASS_NONE
- INTERFACE_CLASS_PLATFORM
- INTERFACE_CLASS_DATA
- INTERFACE_CLASS_PCI_PASSTHROUGH
- INTERFACE_CLASS_PCI_SRIOV

## Mechanism Drivers

- openvswitch
- linuxbridge
- macvtap
- sriovnicswitch
- openvswitch

## Agents

- https://docs.openstack.org/neutron/latest/admin/config-sriov.html

1. openvswitch_agent
2. sriov_agent


### openvswitch_agent

What part of the source code it is found?

- neutron
- nova
- stx-config
- stx-upstream

What are key code definitions?

- OPENVSWITCH_LABEL

Tests

- Tbd

API

- Tbd

Issues

- Tbd

### sriov_agent

- neutron
- nova
- stx-config
- stx-upstream

What are key code definitions?

- SRIOV_LABEL

Tests

- Kernel Parameters
  - intel_iommu=on
  - ixgbe.max_vfs=5
  - nomdmonddf
  - nomdmonisw
  - default_hugepagesz=1G hugepagesz=1G hugepages=8
  - iommu=pt
- ip link show p5p1
  - ip link show p5p1_1
- GRUB_CMDLINE_LINUX /etc/default/grub
  - grub2-mkconfig -o /boot/grub2/grub.cfg
- echo ${num_of_ports} > /sys/class/net/${interface}/devices/sriov_numvfs
  - sriov_numvfs
    - neutron
    - stx-config
    - stx-gui
    - stx-integ
    - stx-metal

API

- Tbd

Issues

- https://bugzilla.redhat.com/show_bug.cgi?id=1344315
