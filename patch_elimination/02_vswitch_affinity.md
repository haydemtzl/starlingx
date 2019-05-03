# Vswitch Affinity

- Virtual NUMA to Physical NUMA
- NUMA Affinity PCI Devices
- NUMA Affinity (strict/best-effort)

# Terminology

# Source Code

## vswitch_numa_affinity

```sh
$ repo grep vswitch_numa_affinity
```

```sh
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json:        "hw:wrs:vswitch_numa_affinity": {
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch:+        "hw:wrs:vswitch_numa_affinity": {
```

## numa_nodes

```sh
$ repo grep numa_nodes
```

```sh
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch
cgcs-root/stx/git/libvirt/
cgcs-root/stx/git/nova/
cgcs-root/stx/git/qemu/
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/lib/facter/number_of_numa_nodes.rb
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/compute.pp
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/manager.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/conductor/manager.py
cgcs-root/stx/stx-config/worker-utils/worker-utils/topology.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/memories/views.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/memorys/_createprofile.html
cgcs-root/stx/stx-integ/tools/monitor-tools/scripts/memtop
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/manager.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/conductor/manager.py
```
