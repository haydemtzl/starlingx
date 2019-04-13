! Important to study

```
[builder@a51007fb8bff stx-config]$ git log sysinv/sysinv/sysinv/sysinv/helm/
```

```
[builder@a51007fb8bff stx-config]$ repo grep openvswitch | grep rpm
cgcs-root/stx/git/ironic/devstack/files/rpms/ironic:openvswitch
cgcs-root/stx/stx-integ/networking/openvswitch/centos/meta_patches/0004-rpm-check-with-condition.patch: SPECS/openvswitch.spec | 2 +-
cgcs-root/stx/stx-integ/networking/openvswitch/centos/meta_patches/0004-rpm-check-with-condition.patch:diff --git a/SPECS/openvswitch.spec b/SPECS/openvswitch.spec
cgcs-root/stx/stx-integ/networking/openvswitch/centos/meta_patches/0004-rpm-check-with-condition.patch:--- a/SPECS/openvswitch.spec
cgcs-root/stx/stx-integ/networking/openvswitch/centos/meta_patches/0004-rpm-check-with-condition.patch:+++ b/SPECS/openvswitch.spec
cgcs-root/stx/stx-integ/networking/openvswitch/centos/srpm_path:mirror:/Source/openvswitch-2.9.0-3.el7.src.rpm
stx-tools/centos-mirror-tools/rpms_centos.lst:openvswitch-2.9.0-3.el7.src.rpm
```

```sh
[builder@a51007fb8bff starlingx]$ repo grep ovsdb-server
cgcs-root/stx/git/networking-odl/devstack/override-defaults:# neutron agent with native uses also 6640 to connect to ovsdb-server
cgcs-root/stx/git/neutron/neutron/agent/common/ovs_lib.py:        """Have ovsdb-server listen for manager connections
cgcs-root/stx/git/neutron/releasenotes/notes/ovsdb-native-by-default-38835d6963592396.yaml:    - The native interface configures ovsdb-server to listen for
cgcs-root/stx/git/neutron/releasenotes/notes/ovsdb-native-by-default-38835d6963592396.yaml:      ovsdb-server has permissions to listen on the configured address.
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/vswitch.pp:    service { 'ovsdb-server':
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/vswitch.pp:      require  => Service['ovsdb-server']
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovsdb.clean.erb:ovs-vsctl -t ovsdb-server --no-wait del-manager
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovsdb.clean.erb:for bridge in $(ovs-vsctl -t ovsdb-server --timeout 10 list-br); do
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovsdb.clean.erb:    ovs-vsctl -t ovsdb-server --timeout 10 --no-wait del-br $bridge
cgcs-root/stx/stx-integ/networking/openvswitch-config/centos/openvswitch-config.spec:install -m 0644 ovsdb-server.pmon.conf %{buildroot}%{_sysconfdir}/openvswitch/ovsdb-server.pmon.conf
cgcs-root/stx/stx-integ/networking/openvswitch-config/centos/openvswitch-config.spec:%config(noreplace) %{_sysconfdir}/openvswitch/ovsdb-server.pmon.conf
cgcs-root/stx/stx-integ/networking/openvswitch-config/files/ovsdb-server.pmon.conf:process  = ovsdb-server
cgcs-root/stx/stx-integ/networking/openvswitch-config/files/ovsdb-server.pmon.conf:service  = ovsdb-server   ; The name of the process's systemd service file without the extension
cgcs-root/stx/stx-integ/networking/openvswitch-config/files/ovsdb-server.pmon.conf:pidfile  = /var/run/openvswitch/ovsdb-server.pid
cgcs-root/stx/stx-integ/networking/openvswitch/centos/patches/run-services-as-root-user.patch:           --no-ovsdb-server --no-monitor --system-id=random \
cgcs-root/stx/stx-integ/networking/openvswitch/files/ovsdb-server.pmon.conf:process  = ovsdb-server
cgcs-root/stx/stx-integ/networking/openvswitch/files/ovsdb-server.pmon.conf:service  = ovsdb-server   ; The name of the process's systemd service file without the extension
cgcs-root/stx/stx-integ/networking/openvswitch/files/ovsdb-server.pmon.conf:pidfile  = /var/run/openvswitch/ovsdb-server.pid
```

```sh
[builder@a51007fb8bff starlingx]$ ls cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs*
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs.add-bridge.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs.add-flow.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs.add-port.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs.clean.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovsdb.clean.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/ovs.disable-dpdk-init.erb
```

```sh
[builder@a51007fb8bff starlingx]$ repo grep ovs.add-bridge.erb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/vswitch.pp:    command => template('platform/ovs.add-bridge.erb')
```

```
[builder@a51007fb8bff starlingx]$ repo grep  platform::vswitch::ovs
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/vswitch.pp:  $vswitch_class = ::platform::vswitch::ovs,
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/puppet/ovs.py:            'platform::vswitch::ovs::devices': ovs_devices,
```

## openvswitch

> [builder@a51007fb8bff stx-integ]$ 

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
  - _check_vswitch_huge_values
  - _check_huge_values
  - cannot provision 1G huge pages if the processor does not support
  - max possible 2M VM pages
  - No available space for 2M VM huge page allocation
  - No available space for 1G VM huge page allocation
  - No available space for new VM hugepage settings
  - No available space for 1G VM huge page allocation
  - No available space for 2M VM huge page allocation
  - _check_vswitch_huge_values
- sysinv/sysinv/sysinv/sysinv/api/controllers/v1/profile.py
  - get_mem_assignment
  - vsHugePagesNr
  - vsHugePagesSz
  - get_mem_size
  - vswitch_hugepages_reqd
  - vswitch_hugepages_size_mib
  - vswitch_hugepages_nr
  - vswitch_hugepages_size_mib
- sysinv/sysinv/sysinv/sysinv/conductor/manager.py
  - vswitch_hugepages_reqd
- sysinv/sysinv/sysinv/sysinv/puppet/ovs.py
  - vswitch_size
  - vswitch_pages
  - dpdk_socket_mem
  - platform::vswitch::params::hugepage_dir
- sysinv/sysinv/sysinv/sysinv/puppet/platform.py
  - vs_pages_updated
  - vswitch_pages
  - total_hugepages_1G
  - grub_hugepages_1G
  - platform::compute::grub::params::g_hugepages
- sysinv/sysinv/sysinv/sysinv/tests/api/test_profile.py
  - vm_hugepages_nr_2M_pending
  - vm_hugepages_nr_1G_pending
  - vswitch_hugepages_reqd
  
```sh
commit 822b99c016c1f2bd0cb5236e46bfb6a55456bb3d
Author: chengli3 <cheng1.li@intel.com>
Date:   Wed Jan 30 19:44:57 2019 +0800

    Support ovs in container                                                                                                                                                                  
    
    As stx cutovers to containerization, most openstack components run in
    containers, but ovs-dpdk running on the host.
    
    This patch is to support ovs running in container, and make it the
    default setting. We still support running ovs-dpdk on the host.
    
    For option ovs-dpdk on the host, run follow command before unlock.
    ```
    system modify --vswitch_type ovs-dpdk
    ```
```

- controllerconfig/controllerconfig/controllerconfig/configassistant.py
  - self.vswitch_type = "none"
- kubernetes/applications/stx-openstack/stx-openstack-helm/stx-openstack-helm/manifests/manifest.yaml

```sh
 schema: armada/Chart/v1
 metadata:
   schema: metadata/Document/v1
+  name: openstack-openvswitch
+data:
+  chart_name: openvswitch
+  release: openstack-openvswitch
+  namespace: openstack
+  # If we deploy ovs-dpdk on the host, ovs pod will not be created.
+  # We can use "native wait" instead. But it's not supported in current armada version.
+  # Before we upgrade armada to new version, we comment the wait
+  # https://github.com/openstack/airship-armada/blob/master/armada/schemas/armada-chart-schema.yaml#L81-L111
+  #wait:
+  #  timeout: 1800
+  install:
+    no_hooks: false
+  upgrade:
+    no_hooks: false
+    pre:
+      delete:
+        - type: job
+          labels:
+            release_group: osh-openstack-openvswitch
+  values:
+    labels:
+      ovs:
+        node_selector_key: openvswitch
+        node_selector_value: enabled
+  source:
+    type: tar
+    location: http://172.17.0.1/helm_charts/openvswitch-0.1.0.tgz
+    subpath: openvswitch
+    reference: master
+  dependencies:
+    - helm-toolkit
+---
+schema: armada/Chart/v1
+metadata:
+  schema: metadata/Document/v1
   name: openstack-nova
 data:
   chart_name: nova


   chart_group:
     - openstack-libvirt
+    - openstack-openvswitch
```

- puppet-manifests/src/modules/platform/manifests/vswitch.pp
  - enable_unsafe_noiommu_mode /sys/module/vfio/parameters/enable_unsafe_noiommu_mode
  - vfio-iommu-mode
- sysinv/sysinv/sysinv/sysinv/common/constants.py
  - VSWITCH_TYPE_NONE
- sysinv/sysinv/sysinv/sysinv/common/utils.py
  - get_vswitch_type
- sysinv/sysinv/sysinv/sysinv/helm/neutron.py
  - # if ovs runs on host, auto bridge add is covered by sysinv
  - constants.VSWITCH_TYPE_NONE
  - auto_bridge_add
  - _get_datapath_type
  - _get_host_bridges
  - constants.DATANETWORK_TYPE_FLAT
  - constants.DATANETWORK_TYPE_VLAN
  - constants.DATANETWORK_TYPE_VXLAN
  - brname = 'br-phy%d' % index
  - 'datapath_type': self._get_datapath_type()
  - physical_device_mappings
  - ifdatanets

- sysinv/sysinv/sysinv/sysinv/helm/openvswitch.py

```sh
--- a/sysinv/sysinv/sysinv/sysinv/helm/openvswitch.py
+++ b/sysinv/sysinv/sysinv/sysinv/helm/openvswitch.py
@@ -6,6 +6,7 @@
 from sysinv.common import constants
 from sysinv.common import exception
+from sysinv.common import utils
 from sysinv.openstack.common import log as logging
 from sysinv.helm import common
 from sysinv.helm import openstack
@@ -19,8 +20,18 @@ class OpenvswitchHelm(openstack.OpenstackBaseHelm):
     CHART = constants.HELM_CHART_OPENVSWITCH
 
     def get_overrides(self, namespace=None):
+        # helm has an issue with installing release of no pod
+        # https://github.com/helm/helm/issues/4295
+        # once this is fixed, we can use 'manifests' instead of 'label' to
+        # control ovs enable or not
         overrides = {
             common.HELM_NS_OPENSTACK: {
+                'labels': {
+                    'ovs': {
+                        'node_selector_key': 'openvswitch',
+                        'node_selector_value': self._ovs_label_value(),
+                    }
+                }
             }
         }
 
@@ -31,3 +42,9 @@ class OpenvswitchHelm(openstack.OpenstackBaseHelm):
                                                  namespace=namespace)
         else:
             return overrides
+
+    def _ovs_label_value(self):
+        if utils.get_vswitch_type(self.dbapi) == constants.VSWITCH_TYPE_NONE:
+            return "enabled"
+        else:
+            return "none"
```


## openvswitch

> [builder@a51007fb8bff stx-integ]$ git log networking/openvswitch/

- ovs_agent_service
- neutron-openvswitch-agent
- ovsdb-server.pmon.conf
- openvswitch.service
- ovsdb-server.service
- ovs-vswitchd.service


## openvswitch-config

- filter_out_from_controller
  - openstack-neutron-openvswitch
  - openvswitch-config
- filter_out_from_storage
  - openvswitch-config
