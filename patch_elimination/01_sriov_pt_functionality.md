# Glossary

- Virtual Functions (VF)
- Physical Functions (PF)
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


# Architecture

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

Tests

- Tbd

Issues

- Tbd

### sriov_agent

- neutron
- nova
- stx-config
- stx-upstream

Tests

- Tbd

Issues

- https://bugzilla.redhat.com/show_bug.cgi?id=1344315
