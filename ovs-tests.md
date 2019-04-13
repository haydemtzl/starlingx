# OVS Tests

## VSwitch

- Add Bridge
- Add Flow
- Add Port

## VSwitch Paams

cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/vswitch.pp

```sh
class platform::vswitch::params(
  $iommu_enabled = true,
  $hugepage_dir = '/mnt/huge-1048576kB',
  $driver_type = 'vfio-pci',
  $vswitch_class = ::platform::vswitch::ovs,
) { }
```

- platform::vswitch::ovs::device
  - exec ovs-bind-device
    - path /usr/share/openvswitch/scripts
    - command dpdk-devbind.py
- platform::vswitch::ovs::bridge
  - netdev
  - exec ovs-add-br
    - command platform/ovs.add-bridge.erb
  - exec ovs-link-up
    - ip link set ${name} up
- platform::vswitch::ovs::port
  - exec ovs-add-port
    - command platform/ovs.add-port.erb
- platform::vswitch::ovs::address
  - exec ovs-add-address
    - command ip addr replace ${address}/${prefixlen} dev ${ifname}
- platform::vswitch::ovs::flow
  - exec ovs-add-flow
    - command platform/ovs.add-flow.erb
- platform::vswitch::ovs

```sh
  if $::platform::params::vswitch_type == 'ovs' {
    include ::vswitch::ovs
  } elsif $::platform::params::vswitch_type == 'ovs-dpdk' {
    include ::vswitch::dpdk
```

cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/ovs.py

- brname
- 'platform::vswitch::ovs::devices': ovs_devices,
- 'platform::vswitch::ovs::bridges': ovs_bridges,
- 'platform::vswitch::ovs::ports': ovs_ports,
- 'platform::vswitch::ovs::addresses': ovs_addresses,
- 'platform::vswitch::ovs::flows': ovs_flows,

- _get_ethernet_device
- _get_ethernet_interface
- _get_ethernet_port
- _get_lldp_interface
- _get_lldp_port
- _get_lldp_flow
- _get_bond_port
- _get_vlan_port
- _get_cpu_config
- _get_memory_config
- _get_virtual_config
- _is_vxlan_datanet
- _get_neutron_config
- _get_providernet_type
- _is_vxlan_providernet
- _get_lldp_config

## ovs-vsctl

- delete manager
  - ovs-vsctl -t ovsdb-server --no-wait del-manager
- delete all bridges
  - bridge ovs-vsctl -t ovsdb-server --timeout 10 list-br
  - ovs-vsctl -t ovsdb-server --timeout 10 --no-wait del-br $bridge

## Node Selector

> What is node selector?

- cgcs-root/stx/stx-config/kubernetes/applications/stx-openstack/stx-openstack-helm/stx-openstack-helm/manifests/manifest.yaml
  - node_selector_key
    - openstack-compute-node
    - openstack-control-plane
    - openvswitch
    - linuxbridge
- cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/helm/openvswitch.py

## LLDPD

> LLDP allows you to know exactly on which port is a server (and reciprocally). LLDP is an industry standard protocol designed to supplant proprietary Link-Layer protocols such as EDP or CDP. The goal of LLDP is to provide an inter-vendor compatible mechanism to deliver Link-Layer notifications to adjacent network devices.

##

> Armada is a complete solution for development, deployment, configuration and discovery of microservices.
> Armada is more than just a tool, it defines conventions and good practices designed towards making your platform more service oriented.
