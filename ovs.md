```sh
commit 9fafe08b65bb0b6832441ef3ab726b4d28e3601d
Author: Matt Peters <matt.peters@windriver.com>                                                                                                                                                                                              │···
    Open vSwitch integration with host and configuration framework
```

Terminology

- OVS-DPDK (old)
- ovs-dpdk
- VSWITCH_TYPE
- neutron::agents::dhcp::interface_driver: 'openvswitch'
- neutron::agents::ml2::ovs::manage_vswitch: false
- neutron.plugins.ml2.plugin.Ml2Plugin
- flowclassifier_drivers = 'ovs'
- neutron::agents::ml2::ovs
- platform::params::vswitch_type
- platform::vswitch::ovs::device
- puppet-manifests/src/modules/platform/manifests/vswitch.pp
- puppet-manifests/src/modules/platform/templates/ovs.add-bridge.erb
- sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory.py
- sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
- vswitch_hugepages_size_mib
- vswitch_hugepages_nr
- vswitch_hugepages_avail
- vswitch_type
- /sysinv/cgts-client/cgts-client/cgtsclient/v1/isystem_shell.py
- sysinv/sysinv/sysinv/sysinv/agent/node.py
- sysinv/sysinv/sysinv/sysinv/cmd/query_pci_id
- VSWITCH_TYPE_OVS_DPDK = "ovs-dpdk"
- update_vswitch_type
- _vswitch_type
- sysinv/sysinv/sysinv/sysinv/puppet/ovs.py

```sh
commit f61f96fa116357c28ab10e661d2c38f9590f7341
Author: Joseph Richard <Joseph.Richard@windriver.com>
    Allow vswitch_class to be configured
```

- puppet-manifests/src/modules/platform/manifests/vswitch.pp
- vswitch_class

```sh
commit b03820df16ab91ca56ec2061bad6a6efc58d712c
Author: Matt Peters <matt.peters@windriver.com>
    Update puppet vswitch service dependency on hugepage mount
```

- puppet-manifests/src/modules/platform/manifests/vswitch.pp
- openvswitch

```sh
commit da1110a3d845933f6497a7a393bba3654a12d95b
Author: Steven Webster <steven.webster@windriver.com>
    LLDP OVS enablement: puppet configuration
    
    This commit introduces puppet configuration enabling LLDP to operate over
    OVS.  Specifically, separate ports flows are configured to handle LLDP
    traffic.

    In addition, we restrict the lldpd daemon from
    operating over bridge, tap, and ovs-netdev devices.
```

- puppet-manifests/src/modules/platform/manifests/vswitch.pp
  - platform/ovs.add-flow.erb
  - platform::vswitch::ovs
  - Platform::Vswitch::Ovs
- puppet-manifests/src/modules/platform/templates/lldp.conf.erb
  - configure system interface pattern *,!br*,!ovs*,!tap*
- puppet-manifests/src/modules/platform/templates/ovs.add-bridge.erb
- puppet-manifests/src/modules/platform/templates/ovs.add-flow.erb
- puppet-manifests/src/modules/platform/templates/ovs.add-port.erb
- puppet-modules-wrs/puppet-sysinv/src/sysinv/manifests/agent.pp
  - lldp/drivers
- sysinv/sysinv/sysinv/sysinv/common/constants.py
  - LLDP_OVS_PORT_PREFIX
  - LLDP_OVS_PORT_NAME_LEN
- sysinv/sysinv/sysinv/sysinv/conductor/manager.py 
  - lldp_id_to_port
- sysinv/sysinv/sysinv/sysinv/puppet/ovs.py
  - _get_lldp_config
  - OVSPuppet
  - _get_lldp_interface
  - _get_lldp_port
  - _get_lldp_flow
  - _get_lldp_config
- sysinv/sysinv/sysinv/sysinv/puppet/platform.py
  - _get_host_lldp_config

```sh
commit 99ef34d919a03c5b80f7ee1070a863c8acd13810                                                                                                                                               Author: Steven Webster <steven.webster@windriver.com>                                                                                                                                         Date:   Mon Oct 15 13:54:02 2018 -0400

    Puppet: enforce OVS init/config order
    
    During debugging of a separate, unrelated issue, it was found that
    there were instances of ports being added to OVS bridges after
    the dpdk-init config option was passed to OVS.

    Although there doesn't seem to be an actual execution error here,
    it seems proper to ensure that the dpdk-init happens before DPDK
    enabled ports are added.  
```

- puppet-manifests/src/modules/platform/manifests/vswitch.pp
  - Vs_config<||> -> Platform::Vswitch::Ovs::Bridge<||>


```sh
commit 74baed87deef6c5565581b71875aa44d07271663                                                                                                                                               Author: Steven Webster <steven.webster@windriver.com>                                                                                                                                         Date:   Mon Dec 17 15:41:54 2018 -0500                                                                                                                                                        

    Enable configurable vswitch memory
    Currently, a DPDK enabled vswitch makes use of a fixed 1G hugepage to
    enable an optimized datapath.
    
    In the case of OVS-DPDK, this can cause an issue when changing the
    MTU of one or more interfaces, as a separate mempool is allocated
    for each size.  If the minimal mempool size(s) cannot fit into the
    1G page, DPDK memory initialization will fail.
    
    This commit allows an operator to configure the amount of hugepage
    memory allocated to each socket on a host, which can enable
    jumboframe support for OVS-DPDK.
    
        The system memory command has been modified to accept vswitch
    hugepage configuration via the function flag. ie:
    
    system host-memory-modify -f vswitch -1G 4 <worker_name> <node>
```

- puppet-manifests/src/modules/platform/manifests/compute.pp
  - g_hugepages
  - number_of_numa_nodes
- puppet-manifests/src/modules/platform/manifests/vswitch.pp
  - ovsdb-server
  - Service['openvswitch']
  - platform/ovs.disable-dpdk-init.erb
  - Service['ovsdb-server']

```
+    # Since OVS socket memory is configurable, it is required to start the
+    # ovsdb server and disable DPDK initialization before the openvswitch
+    # service runs to prevent any previously stored OVSDB configuration from
+    # being used before the new Vs_config gets applied.
```

- puppet-manifests/src/modules/platform/templates/ovs.disable-dpdk-init.erb

```sh
+# Disable DPDK initialization in ovsdb
+# ovs-vsctl is not used here as it can fail after the initial start of ovsdb
+# (even though the dpdk-init parameter actually gets applied).
+ovsdb-client -v transact '["Open_vSwitch", {"op" : "mutate", "table": "Open_vSwitch", "where": [], "mutations" : [["other_config","delete", ["map",[["dpdk-init", "true"]]]]]}]'
+ovsdb-client -v transact '["Open_vSwitch", {"op" : "mutate", "table": "Open_vSwitch", "where": [], "mutations" : [["other_config","insert", ["map",[["dpdk-init", "false"]]]]]}]'
```

- sysinv/cgts-client/cgts-client/cgtsclient/v1/iHost_shell.py
  - do_host_apply_memprofile
    - field_labels
    - fields
  - vm_hugepages_2M_pending
  - vm_hugepages_1G_pending
  - vswitch_hugepages_nr
  - vswitch_hugepages_size_reqd
  - vswitch_hugepages_size_mib
- sysinv/cgts-client/cgts-client/cgtsclient/v1/imemory_shell.py
  - _print_imemory_show
    - vswitch_hugepages_reqd
  - do_host_memory_list
    - vswitch_hugepages_reqd
  - do_host_memory_list
    - vs_hp_reqd
  - The number of 2M vm huge pages for the numa node
  - The number of 1G vm huge pages for the numa node
  - hugepages_nr_2M_pending
  - hugepages_nr_1G_pending
- sysinv/sysinv/sysinv/etc/sysinv/profileSchema.xsd
  - vsHugePagesNr
  - vsHugePagesSz
- sysinv/sysinv/sysinv/sysinv/agent/node.py
  - _get_vswitch_reserved_memory
  - vswitch_hugepages_size_mib
  - vswitch_hugepages_nr
  - vswitch_hugepages_avail
- sysinv/sysinv/sysinv/sysinv/api/controllers/v1/host.py
  - vswitch_hugepages_reqd
  - vswitch_hugepages_nr
  - vswitch_hugepages_size_mib
  - vswitch_hp_size
  - pecan.request.dbapi.imemory_get_by_inode
- sysinv/sysinv/sysinv/sysinv/api/controllers/v1/memory.py
  - vswitch_hugepages_reqd
  - vswitch_hugepages_size_mib
  - _check_huge_values
  
```sh

```
