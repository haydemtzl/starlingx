# cpu_model

```
$ repo grep cpu_model
```

- glance
- horizon
- libvirt
- nova
- qemu
- stx-config
- stx-gui
- stx-integ
- stx-metal
- stx-nfv
- stx-upstream

```
cgcs-root/stx/git/glance/etc/metadefs/compute-tis-flavor.json:        "hw:cpu_model": {
cgcs-root/stx/git/glance/stx-patches/0009-Pike-Rebase-Update-metadefs.patch:+        "hw:cpu_model": {
cgcs-root/stx/git/horizon/openstack_dashboard/dashboards/admin/hypervisors/tables.py
cgcs-root/stx/git/libvirt
cgcs-root/stx/git/nova/doc/source/admin/configuration/hypervisor-kvm.rst
cgcs-root/stx/git/nova/nova/conf/libvirt.py
cgcs-root/stx/git/nova/nova/db/sqlalchemy/api.py
cgcs-root/stx/git/nova/nova/db/sqlalchemy/migrate_repo/versions/276_vcpu_model.py
cgcs-root/stx/git/nova/nova/db/sqlalchemy/models.py
cgcs-root/stx/git/nova/nova/objects/__init__.py
cgcs-root/stx/git/nova/nova/objects/instance.py
cgcs-root/stx/git/nova/nova/objects/vcpu_model.py
cgcs-root/stx/git/nova/nova/tests/
cgcs-root/stx/git/nova/nova/virt/libvirt/driver.py
cgcs-root/stx/git/nova/nova/virt/libvirt/utils.py
cgcs-root/stx/git/nova/nova/virt/xenapi/driver.py
cgcs-root/stx/git/nova/nova/virt/xenapi/host.py
cgcs-root/stx/git/nova/releasenotes/notes/libvirt-cpu-model-extra-flags-a23085f58bd22d27.yaml
cgcs-root/stx/git/qemu
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/iHost_shell.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu.py
cgcs-root/stx/stx-config/sysinv/cgts-client/cgts-client/cgtsclient/v1/icpu_shell.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/agent/node.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/cpu.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/cpu_utils.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/api/controllers/v1/profile.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/db/sqlalchemy/migrate_repo/versions/001_init.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/db/sqlalchemy/models.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/objects/cpu.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/db/sqlalchemy/test_migrations.py
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/tests/db/utils.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/api/sysinv.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/cpu_functions/utils.py
cgcs-root/stx/stx-gui/starlingx-dashboard/starlingx-dashboard/starlingx_dashboard/dashboards/admin/inventory/templates/inventory/_detail_cpufunctions.html
cgcs-root/stx/stx-integ/virt/qemu/centos/qemu-kvm.spec
cgcs-root/stx/stx-metal/api-ref/source/api-ref-sysinv-v1-metal.rst
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/node.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/agent/node.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/cpu.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/api/controllers/v1/cpu_utils.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/db/sqlalchemy/migrate_repo/versions/001_init.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/db/sqlalchemy/models.py
cgcs-root/stx/stx-metal/inventory/inventory/inventory/objects/cpu.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/cpu.py
cgcs-root/stx/stx-metal/python-inventoryclient/inventoryclient/inventoryclient/v1/cpu_shell.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_api_tests/vim_mano_heat_test_cases.txt
cgcs-root/stx/stx-upstream/openstack/python-heat/python-heat/templates/hot/simple/OS_Nova_Flavor.yaml
cgcs-root/stx/stx-upstream/openstack/python-nova/centos/files/nova.conf.sample
```
