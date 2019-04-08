```sh
[builder@a51007fb8bff cgcs-root]$ repo grep anaconda
cgcs-root/build-tools/make-installer-images.sh:    if [ -d ./usr/lib64/python2.7/site-packages/pyanaconda/ ];then
cgcs-root/build-tools/make-installer-images.sh:            rm -rf usr/lib64/python2.7/site-packages/pyanaconda/
cgcs-root/build-tools/make-installer-images.sh:        all_pyo="`find ./usr/lib64/python2.7/site-packages/pyanaconda/ usr/lib64/python2.7/site-packages/rpm/ -name *.pyo`"
cgcs-root/build-tools/update-pxe-network-installer:echo "--> find anaconda rpm"
cgcs-root/build-tools/update-pxe-network-installer:anaconda=$(find $MY_BUILD_DIR/installer/rpmbuild/RPMS -type f -name "anaconda-[0-9]*.x86_64.rpm")
cgcs-root/build-tools/update-pxe-network-installer:if [ -n $anaconda ] && [ -f $anaconda ];then
cgcs-root/build-tools/update-pxe-network-installer:    cp -f $anaconda $rootfs_rpms/.
cgcs-root/build-tools/update-pxe-network-installer:    echo "ERROR: failed to find anaconda RPM!"
cgcs-root/build-tools/update-pxe-network-installer:echo "--> find anaconda-core rpm"
cgcs-root/build-tools/update-pxe-network-installer:anaconda_core=$(find $MY_BUILD_DIR/installer/rpmbuild/RPMS -type f -name "anaconda-core-[0-9]*.x86_64.rpm")
cgcs-root/build-tools/update-pxe-network-installer:if [ -n $anaconda_core ] && [ -f $anaconda_core ];then
cgcs-root/build-tools/update-pxe-network-installer:    cp -f $anaconda_core $rootfs_rpms/.
cgcs-root/build-tools/update-pxe-network-installer:    echo "ERROR: failed to find anaconda-core RPM!"
cgcs-root/build-tools/update-pxe-network-installer:echo "--> find anaconda-tui rpm"
cgcs-root/build-tools/update-pxe-network-installer:anaconda_tui=$(find $MY_BUILD_DIR/installer/rpmbuild/RPMS -type f -name "anaconda-tui-[0-9]*.x86_64.rpm")
cgcs-root/build-tools/update-pxe-network-installer:if [ -n $anaconda_tui ] && [ -f $anaconda_tui ];then
cgcs-root/build-tools/update-pxe-network-installer:    cp -f $anaconda_tui $rootfs_rpms/.
cgcs-root/build-tools/update-pxe-network-installer:    echo "ERROR: failed to find anaconda-tui RPM!"
cgcs-root/build-tools/update-pxe-network-installer:echo "--> find anaconda-widgets rpm"
cgcs-root/build-tools/update-pxe-network-installer:anaconda_widgets=$(find $MY_BUILD_DIR/installer/rpmbuild/RPMS -type f -name "anaconda-widgets-[0-9]*.x86_64.rpm")
cgcs-root/build-tools/update-pxe-network-installer:if [ -n $anaconda_widgets ] && [ -f $anaconda_widgets ];then
cgcs-root/build-tools/update-pxe-network-installer:    cp -f $anaconda_widgets $rootfs_rpms/.
cgcs-root/build-tools/update-pxe-network-installer:    echo "ERROR: failed to find anaconda-widgets RPM!"
cgcs-root/stx/git/ceph/src/test/centos-7/Dockerfile.in:RUN yum -y swap -- remove fakesystemd systemd-libs systemd-container -- install systemd systemd-libs && (cd /lib/systemd/system/sysinit
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/configassistant.py:    logfile = '/var/log/anaconda/storage.log'
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/configassistant.py:    # will be to check the anaconda install log for the parameters passed
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/configassistant.py:    logfile = '/var/log/anaconda/anaconda.log'
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0001-Update-package-versioning-for-TIS-format.patch: SPECS/anaconda.spec | 2 +-
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0001-Update-package-versioning-for-TIS-format.patch:diff --git a/SPECS/anaconda.spec b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0001-Update-package-versioning-for-TIS-format.patch:--- a/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0001-Update-package-versioning-for-TIS-format.patch:+++ b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0001-Update-package-versioning-for-TIS-format.patch: Name:    anaconda
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0002-Add-TIS-patches.patch: SPECS/anaconda.spec | 6 ++++++
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0002-Add-TIS-patches.patch:diff --git a/SPECS/anaconda.spec b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0002-Add-TIS-patches.patch:--- a/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0002-Add-TIS-patches.patch:+++ b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0002-Add-TIS-patches.patch: Patch10: anaconda-centos-armhfp-extloader.patch
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0003-revert-7.4-grub2-efi-handling.patch: SPECS/anaconda.spec | 2 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0003-revert-7.4-grub2-efi-handling.patch:diff --git a/SPECS/anaconda.spec b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0003-revert-7.4-grub2-efi-handling.patch:--- a/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0003-revert-7.4-grub2-efi-handling.patch:+++ b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0003-revert-7.4-grub2-efi-handling.patch:@@ -26,6 +26,7 @@ Patch10: anaconda-centos-armhfp-extloader.patch
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0004-Upversion-rpm-devel-dependency.patch: SPECS/anaconda.spec | 2 +-
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0004-Upversion-rpm-devel-dependency.patch:diff --git a/SPECS/anaconda.spec b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0004-Upversion-rpm-devel-dependency.patch:--- a/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0004-Upversion-rpm-devel-dependency.patch:+++ b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch: SPECS/anaconda.spec | 9 +++++++++
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:diff --git a/SPECS/anaconda.spec b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:--- a/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:+++ b/SPECS/anaconda.spec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:@@ -27,6 +27,8 @@ Patch10: anaconda-centos-armhfp-extloader.patch
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch: mkdir -p %{buildroot}%{_datadir}/anaconda/site-python
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch: install -m 0644 pyanaconda/sitecustomize.py %{buildroot}%{_datadir}/anaconda/site-python
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:+# Add anaconda-preexec script
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:+install -m 0755 scripts/anaconda-preexec %{buildroot}%{_sbindir}/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch:+%{_sbindir}/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/meta_patches/0005-Add-TIS-patches-for-host-lookup.patch: %{_libdir}/python*/site-packages/pyanaconda/ui/gui/*
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/errors.py                     | 24 +++++++--
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/flags.py                      |  1 +
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/install.py                    |  4 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/kickstart.py                  |  3 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/packaging/rpmostreepayload.py |  5 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/packaging/yumpayload.py       | 15 +++++-
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/tisnotify.py                  | 91 ++++++++++++++++++++++++++++++++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/ui/gui/hubs/progress.py       |  4 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: pyanaconda/ui/tui/spokes/progress.py     |  4 ++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: create mode 100644 pyanaconda/tisnotify.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: # tmux.conf for the anaconda environment
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: new-session -s anaconda -n main "anaconda"
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: new-window -d -n log            "tail -F /tmp/anaconda.log"
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/errors.py b/pyanaconda/errors.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/errors.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/errors.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.i18n import _
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+# WRS: If a fatal error occurs in a %pre, anaconda hasn't setup the UI yet,
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/flags.py b/pyanaconda/flags.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/flags.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/flags.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/install.py b/pyanaconda/install.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/install.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/install.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:@@ -35,6 +35,9 @@ from pyanaconda.ui.lib.entropy import wait_for_entropy
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.kexec import setup_kexec
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.kickstart import runPostScripts, runPreInstallScripts
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: log = logging.getLogger("anaconda")
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/kickstart.py b/pyanaconda/kickstart.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/kickstart.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/kickstart.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: log = logging.getLogger("anaconda")
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: stderrLog = logging.getLogger("anaconda.stderr")
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/packaging/rpmostreepayload.py b/pyanaconda/packaging/rpmostreepayload.
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/packaging/rpmostreepayload.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/packaging/rpmostreepayload.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: log = logging.getLogger("anaconda")
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/packaging/yumpayload.py b/pyanaconda/packaging/yumpayload.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/packaging/yumpayload.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/packaging/yumpayload.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:@@ -46,6 +46,8 @@ from pyanaconda.simpleconfig import simple_replace
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:             log.error("Error running anaconda-yum: %s", e)
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/tisnotify.py b/pyanaconda/tisnotify.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/tisnotify.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.flags import flags
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/ui/gui/hubs/progress.py b/pyanaconda/ui/gui/hubs/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/ui/gui/hubs/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/ui/gui/hubs/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.ui.gui.hubs import Hub
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.ui.gui.utils import gtk_action_nowait, gtk_call_once
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:diff --git a/pyanaconda/ui/tui/spokes/progress.py b/pyanaconda/ui/tui/spokes/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:--- a/pyanaconda/ui/tui/spokes/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+++ b/pyanaconda/ui/tui/spokes/progress.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:@@ -31,6 +31,8 @@ from pyanaconda.ui.tui.spokes import StandaloneTUISpoke
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.ui.tui.hubs.summary import SummaryHub
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch: from pyanaconda.ui.tui.simpleline.base import ExitAllMainLoops
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0001-TIS-Progress-and-error-handling.patch:+from pyanaconda.tisnotify import tisnotify
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0002-revert-7.4-grub2-efi-handling.patch: pyanaconda/bootloader.py | 36 +++---------------------------------
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0002-revert-7.4-grub2-efi-handling.patch:diff --git a/pyanaconda/bootloader.py b/pyanaconda/bootloader.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0002-revert-7.4-grub2-efi-handling.patch:--- a/pyanaconda/bootloader.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0002-revert-7.4-grub2-efi-handling.patch:+++ b/pyanaconda/bootloader.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0003-Set-default-hostname-to-localhost.patch: pyanaconda/network.py | 2 +-
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0003-Set-default-hostname-to-localhost.patch:diff --git a/pyanaconda/network.py b/pyanaconda/network.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0003-Set-default-hostname-to-localhost.patch:--- a/pyanaconda/network.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0003-Set-default-hostname-to-localhost.patch:+++ b/pyanaconda/network.py
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0003-Set-default-hostname-to-localhost.patch: ipv6ConfFile = "/etc/sysctl.d/anaconda.conf"
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch: data/systemd/anaconda.service |  1 +
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch: scripts/anaconda-preexec      | 50 +++++++++++++++++++++++++++++++++++++++++++
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch: create mode 100644 scripts/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:diff --git a/data/systemd/anaconda.service b/data/systemd/anaconda.service
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:--- a/data/systemd/anaconda.service
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:+++ b/data/systemd/anaconda.service
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:@@ -7,4 +7,5 @@ Wants=anaconda-noshell.service
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:+ExecStartPre=/usr/sbin/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch: ExecStart=/usr/bin/tmux -u -f /usr/share/anaconda/tmux.conf start
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:diff --git a/scripts/anaconda-preexec b/scripts/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:+++ b/scripts/anaconda-preexec
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:+exec >>/tmp/anaconda-preexec.log
cgcs-root/stx/stx-integ/base/anaconda/centos/patches/0004-Cache-server-ip-in-etc-hosts.patch:+exec 2>>/tmp/anaconda-preexec.log
cgcs-root/stx/stx-integ/base/anaconda/centos/srpm_path:mirror:Source/anaconda-21.48.22.147-1.el7.centos.src.rpm
cgcs-root/stx/stx-integ/base/rpm/centos/rpm.spec:- use header from rpmdb in posttrans to make anaconda happy
cgcs-root/stx/stx-integ/centos_pkg_dirs_installer:base/anaconda
cgcs-root/stx/stx-integ/database/mariadb/centos/mariadb.spec:- Do not use pretrans scriptlet, which doesn't work in anaconda
cgcs-root/stx/stx-integ/database/mariadb/centos/mariadb.spec.unmodified:- Do not use pretrans scriptlet, which doesn't work in anaconda
cgcs-root/stx/stx-integ/virt/qemu/centos/qemu-kvm.spec:- Prevent locked cdrom eject - fixes hang at end of anaconda installs (#501412)
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_common.cfg:echo "# Persisted network interfaces from anaconda installer" > /etc/udev/rules.d/70-persistent-net.rules
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:anaconda_logdir=/var/log/anaconda
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:mkdir -p $anaconda_logdir
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:wget --recursive --no-parent --no-host-directories --no-clobber --reject 'index.html*' --reject '*.log' $feed_url/ -o $an
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/wget-feed-mirror.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:wget --mirror --no-parent --no-host-directories --reject 'index.html*' --reject '*.log' $updates_url/ -o $anaconda_logdir
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/wget-updates-mirror.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:anaconda_logdir=/var/log/anaconda
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:mkdir -p $anaconda_logdir
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$cut_dirs $feed_url/Packages/ -o $a
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/rpmget.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$cut_dirs $feed_url/repodata/ -o $a
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/rpmget_repo.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:wget $feed_url/isolinux.cfg --append $anaconda_logdir/wget_kickstarts.log \
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/wget_kickstarts.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget_repo.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget_metadata.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:anaconda_logdir=/var/log/anaconda
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:mkdir -p $anaconda_logdir
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$cut_dirs $feed_url/Packages/ -o $anaco
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/rpmget.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$cut_dirs $feed_url/repodata/ -o $anaco
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/rpmget_repo.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:wget $feed_url/isolinux.cfg --append $anaconda_logdir/wget_kickstarts.log \
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    || report_post_failure_with_logfile $anaconda_logdir/wget_kickstarts.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/Pack
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/repo
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget_repo.log
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:    wget --mirror --no-parent --no-host-directories --reject 'index.html*' --cut-dirs=$patches_cut_dirs $patches_url/meta
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg:        || report_post_failure_with_logfile $anaconda_logdir/patches_rpmget_metadata.log
cgcs-root/stx/stx-metal/installer/initrd/README:  components like anaconda
cgcs-root/stx/stx-metal/installer/initrd/README:# anaconda-*.tis.*.rpm rpm-*.tis*.rpm will be generated by this step
cgcs-root/stx/stx-metal/installer/initrd/README:If we want to make changes to the rootfs of the installer (ie. update anaconda),
cgcs-root/stx/stx-metal/installer/initrd/README:# Build the TIS-modified installer RPMs first (see anaconda jiggery-pokery at end of this file):
cgcs-root/stx/stx-metal/installer/initrd/README:# We also now need bind-utils in the squashfs, due to the anaconda-preexec we've added.
cgcs-root/stx/stx-metal/installer/initrd/README:# For anaconda, ignore these RPMs that are built:
cgcs-root/stx/stx-metal/installer/initrd/README:# anaconda-debuginfo
cgcs-root/stx/stx-metal/installer/initrd/README:# anaconda-dracut
cgcs-root/stx/stx-metal/installer/initrd/README:# anaconda-widgets-devel
cgcs-root/stx/stx-metal/installer/initrd/README:# anaconda-gui
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm                rpm-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-core-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm           rpm-build-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-debuginfo-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm      rpm-build-libs-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-dracut-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm         rpm-libs-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-gui-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm            rpm-plugin-systemd-inhibit-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-tui-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm            rpm-python-4.14.0-1.tis.1.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-widgets-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm        systemd-219-42.el7_4.1.tis.10.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:anaconda-widgets-devel-21.48.22.121-1.el7.centos.tis.5.x86_64.rpm  systemd-libs-219-42.el7_4.1.tis.10.x86_64.rpm
cgcs-root/stx/stx-metal/installer/initrd/README:rm -rf usr/lib64/python2.7/site-packages/pyanaconda/
cgcs-root/stx/stx-metal/installer/initrd/README:find usr/lib64/python2.7/site-packages/pyanaconda/ usr/lib64/python2.7/site-packages/rpm/ -name *.pyo | xargs rm
cgcs-root/stx/stx-metal/installer/initrd/README:Jiggery-pokery required to build anaconda after rebase to 7.3:
cgcs-root/stx/stx-metal/installer/initrd/README:The anaconda build reports a dependency error:
cgcs-root/stx/stx-metal/installer/initrd/README:massaging to get the anaconda RPM to build.
stx-tools/Dockerfile:        yum install -y anaconda \
stx-tools/Dockerfile:        anaconda-help \
stx-tools/Dockerfile:        anaconda-runtime \
stx-tools/Dockerfile:    rm -f /lib/systemd/system/anaconda.target.wants/*
stx-tools/centos-mirror-tools/rpms_centos.lst:anaconda-21.48.22.147-1.el7.centos.src.rpm
stx-tools/centos-mirror-tools/utils_tests.sh:res=$(get_yum_command "anaconda-21.48.22.147-1.el7.centos.src.rpm" "L1")
stx-tools/centos-mirror-tools/utils_tests.sh:expect="yumdownloader -q -C  --releasever=7 --source anaconda-21.48.22.147-1.el7.centos"
stx-tools/centos-mirror-tools/utils_tests.sh:res=$(get_rpm_level_name "anaconda-21.48.22.147-1.el7.centos.src.rpm" "L2")
stx-tools/centos-mirror-tools/utils_tests.sh:expect="anaconda-21.48.22.147"
```

```shy
[builder@a51007fb8bff cgcs-root]$ repo grep anaconda
cgcs-root/build-tools/make-installer-images.sh
cgcs-root/build-tools/update-pxe-network-installer
cgcs-root/stx/git/ceph/src/test/centos-7/Dockerfile.in
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/configassistant.py
cgcs-root/stx/stx-integ/base/anaconda
cgcs-root/stx/stx-integ/base/rpm/centos/rpm.spec
cgcs-root/stx/stx-integ/centos_pkg_dirs_installer
cgcs-root/stx/stx-integ/database/mariadb/centos/mariadb.spec
cgcs-root/stx/stx-integ/virt/qemu/centos/qemu-kvm.spec
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_common.cfg
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_net_controller.cfg
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_pxeboot_controller.cfg
cgcs-root/stx/stx-metal/bsp-files/kickstarts/post_yow_controller.cfg
cgcs-root/stx/stx-metal/installer/initrd/README
stx-tools/Dockerfile
stx-tools/centos-mirror-tools/rpms_centos.lst
stx-tools/centos-mirror-tools/utils_tests.sh
```

```sh
cgcs-root/build-tools/update-pxe-network-installer:mk_images_tool="$same_folder/make-installer-images.sh"
cgcs-root/build-tools/make-installer-images.sh:## this script is called by "update-pxe-network-installer" and run in "sudo"
stx-tools/centos-mirror-tools/download_mirror.sh:echo "running \"update-pxe-network-installer\" command after \"build-iso\""
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/helm/common.py:# Matches configassistant.py value => Should change to STARLINGX
cgcs-root/build-tools/build-pkgs-parallel:   TARGETS_INSTALLER="$(find_targets centos_pkg_dirs_installer)"
cgcs-root/build-tools/build-pkgs-serial:   TARGETS_INSTALLER="$(find_targets centos_pkg_dirs_installer)"
cgcs-root/stx/stx-metal/bsp-files/centos-ks-gen.pl:                  "post_common.cfg",
cgcs-root/stx/stx-metal/bsp-files/centos-ks-gen.pl:                  "post_net_controller.cfg",
cgcs-root/stx/stx-metal/bsp-files/centos-ks-gen.pl:                  "post_pxeboot_controller.cfg");
cgcs-root/stx/stx-metal/bsp-files/centos-ks-gen.pl:                      "post_yow_controller.cfg");
```

```sh
[builder@a51007fb8bff starlingx]$ repo grep Dockerfile.in
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/debian-jessie/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/ubuntu-12.04/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/ubuntu-14.04/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/fedora-21/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/centos-6/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/centos-7/Dockerfile.in \
cgcs-root/stx/git/ceph/src/Makefile.in: $(srcdir)/test/opensuse-13.2/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/debian-jessie/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/ubuntu-12.04/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/ubuntu-14.04/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/fedora-21/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/centos-6/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/centos-7/Dockerfile.in \
cgcs-root/stx/git/ceph/src/test/Makefile.am:    $(srcdir)/test/opensuse-13.2/Dockerfile.in \
cgcs-root/stx/git/docker-distribution/BUILDING.md:People looking for advanced operational use cases might consider rolling their own image with a custom Dockerfile inheriting `FROM registry:
```

```sh
[builder@a51007fb8bff starlingx]$ repo grep bsp-files
cgcs-root/build-tools/build-iso:   export BSP_FILES_PATH="$STX_DIR/stx-metal/bsp-files"
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/build_srpm.data:           $GIT_BASE/bsp-files/grub.cfg \
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/build_srpm.data:           $GIT_BASE/bsp-files/kickstarts/post_clone_iso_ks.cfg \
cgcs-root/stx/stx-metal/kickstart/centos/build_srpm.data:SRC_DIR="${GIT_BASE}/bsp-files"
```

```sh
[builder@a51007fb8bff starlingx]$ repo grep post_clone_iso_ks.cfg
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/clone.py:        subprocess.check_output("cat /pxeboot/post_clone_iso_ks.cfg >> " +
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/build_srpm.data:           $GIT_BASE/bsp-files/kickstarts/post_clone_iso_ks.cfg \
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/pxe-network-installer.spec:Source013: post_clone_iso_ks.cfg
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/pxe-network-installer.spec:install -v -m 644 %{_sourcedir}/post_clone_iso_ks.cfg \
cgcs-root/stx/stx-metal/installer/pxe-network-installer/centos/pxe-network-installer.spec:    %{buildroot}/pxeboot/post_clone_iso_ks.cfg
```

## [2]

```sh
[builder@a51007fb8bff stx-metal]$ ls -al bsp-files/
total 116
drwxr-xr-x  4 builder cgts  4096 Apr  8 12:15 .
drwxr-xr-x 17 builder cgts  4096 Apr  8 09:13 ..
-rwxr-xr-x  1 builder cgts 11940 Feb 11 10:24 centos-ks-gen.pl
-rw-r--r--  1 builder cgts  7542 Feb 11 10:24 centos.syslinux.cfg
-rw-r--r--  1 builder cgts   823 Mar 29 10:55 filter_out_from_controller
-rw-r--r--  1 builder cgts   367 Mar 29 10:55 filter_out_from_smallsystem
-rw-r--r--  1 builder cgts   321 Mar 29 10:55 filter_out_from_smallsystem_lowlatency
-rw-r--r--  1 builder cgts  5977 Mar 29 10:55 filter_out_from_storage
-rw-r--r--  1 builder cgts  5171 Mar 29 10:55 filter_out_from_worker
-rw-r--r--  1 builder cgts  5182 Mar 29 10:55 filter_out_from_worker_lowlatency
-rw-r--r--  1 builder cgts  9135 Feb 11 10:24 grub.cfg
drwxr-xr-x  2 builder cgts  4096 Apr  8 09:13 kickstarts
-rwxr-xr-x  1 builder cgts   825 Feb 11 10:24 pkg-list.pl
-rw-r--r--  1 builder cgts  4314 Feb 11 10:24 platform_comps.py
-rw-r--r--  1 builder cgts  7108 Feb 11 10:24 pxeboot.cfg
-rw-r--r--  1 builder cgts  5637 Feb 11 10:24 pxeboot_grub.cfg
-rwxr-xr-x  1 builder cgts  3168 Feb 11 10:24 pxeboot_setup.sh
drwxr-xr-x  2 builder cgts  4096 Feb 11 10:24 upgrades
```

- bsp-files/centos.syslinux.cfg

```sh
menu begin
  menu title Standard Controller Configuration
```

```sh
[builder@a51007fb8bff stx-metal]$ ls -al bsp-files/filter_out_from_*
-rw-r--r-- 1 builder cgts  823 Mar 29 10:55 bsp-files/filter_out_from_controller
-rw-r--r-- 1 builder cgts  367 Mar 29 10:55 bsp-files/filter_out_from_smallsystem
-rw-r--r-- 1 builder cgts  321 Mar 29 10:55 bsp-files/filter_out_from_smallsystem_lowlatency
-rw-r--r-- 1 builder cgts 5977 Mar 29 10:55 bsp-files/filter_out_from_storage
-rw-r--r-- 1 builder cgts 5171 Mar 29 10:55 bsp-files/filter_out_from_worker
-rw-r--r-- 1 builder cgts 5182 Mar 29 10:55 bsp-files/filter_out_from_worker_lowlatency
```

- bsp-files/grub.cfg

```sh
# ---------------------- NOTE ----------------------
# If you are updating menus, make sure that controllerconfig/clone.py
# is in sync with your changes (only serial console ids).
#     STANDARD_STANDARD = 'standard>serial>' + 
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_STANDARD
#     STANDARD_EXTENDED = 'standard>serial>' +
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_EXTENDED
#     AIO_STANDARD = 'standard>aio>' +
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_STANDARD
#     AIO_EXTENDED = 'standard>aio>'  +
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_EXTENDED
#     AIO_LL_STANDARD = 'standard>aio-lowlat>' +
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_STANDARD
#     AIO_LL_EXTENDED = 'standard>aio-lowlat>'  +
#                         sysinv_constants.SYSTEM_SECURITY_PROFILE_EXTENDED
#     SUBMENUITEM_TBOOT = 'tboot'
#     SUBMENUITEM_SECUREBOOT = 'secureboot'
# --------------------------------------------------
```

- bsp-files/platform_comps.py


```sh
    add_group(comps, 'controller', rpmlist,
              filter_dir, 'filter_out_from_controller')
    add_group(comps, 'controller-worker', rpmlist,
              filter_dir, 'filter_out_from_smallsystem')
    add_group(comps, 'controller-worker-lowlatency', rpmlist,
              filter_dir, 'filter_out_from_smallsystem_lowlatency')
    add_group(comps, 'worker', rpmlist, filter_dir, 'filter_out_from_worker')
    add_group(comps, 'worker-lowlatency', rpmlist,
              filter_dir, 'filter_out_from_worker_lowlatency')
    add_group(comps, 'storage', rpmlist, filter_dir, 'filter_out_from_storage')

    add_group(comps, 'controller')
    add_group(comps, 'controller-worker')
    add_group(comps, 'controller-worker-lowlatency')
    add_group(comps, 'worker')
    add_group(comps, 'worker-lowlatency')
    add_group(comps, 'storage')
```

```sh
[builder@a51007fb8bff stx-metal]$ ls -al bsp-files/kickstarts/
total 140
drwxr-xr-x 2 builder cgts 4096 Apr  8 09:13 .
drwxr-xr-x 4 builder cgts 4096 Apr  8 12:21 ..
-rw-r--r-- 1 builder cgts 1383 Feb 11 10:24 functions.sh
-rw-r--r-- 1 builder cgts  926 Feb 11 10:24 post_clone_iso_ks.cfg
-rw-r--r-- 1 builder cgts 3636 Feb 11 10:24 post_common.cfg
-rw-r--r-- 1 builder cgts 3445 Mar 29 10:55 post_kernel_aio_and_worker.cfg
-rw-r--r-- 1 builder cgts 1512 Feb 11 10:24 post_kernel_controller.cfg
-rw-r--r-- 1 builder cgts 1335 Feb 11 10:24 post_kernel_storage.cfg
-rw-r--r-- 1 builder cgts  449 Feb 11 10:24 post_lvm_no_pv_on_rootfs.cfg
-rw-r--r-- 1 builder cgts  723 Feb 11 10:24 post_lvm_pv_on_rootfs.cfg
-rwxr-xr-x 1 builder cgts 4040 Feb 11 10:24 post_net_common.cfg
-rw-r--r-- 1 builder cgts 3113 Feb 11 10:24 post_net_controller.cfg
-rw-r--r-- 1 builder cgts  459 Feb 11 10:24 post_platform_conf_aio.cfg
-rw-r--r-- 1 builder cgts  470 Feb 11 10:24 post_platform_conf_aio_lowlatency.cfg
-rw-r--r-- 1 builder cgts  450 Feb 11 10:24 post_platform_conf_controller.cfg
-rw-r--r-- 1 builder cgts  722 Feb 11 10:24 post_platform_conf_storage.cfg
-rw-r--r-- 1 builder cgts  903 Feb 11 10:24 post_platform_conf_worker.cfg
-rw-r--r-- 1 builder cgts  914 Feb 11 10:24 post_platform_conf_worker_lowlatency.cfg
-rw-r--r-- 1 builder cgts 4397 Feb 11 10:24 post_pxeboot_controller.cfg
-rw-r--r-- 1 builder cgts 1200 Feb 11 10:24 post_system_aio.cfg
-rw-r--r-- 1 builder cgts 2605 Feb 11 10:24 post_usb_controller.cfg
-rw-r--r-- 1 builder cgts 4479 Feb 11 10:24 post_yow_controller.cfg
-rw-r--r-- 1 builder cgts 1818 Feb 11 10:24 pre_common_head.cfg
-rwxr-xr-x 1 builder cgts 4456 Feb 11 10:24 pre_disk_aio.cfg
-rwxr-xr-x 1 builder cgts 1036 Feb 11 10:24 pre_disk_controller.cfg
-rw-r--r-- 1 builder cgts 5286 Feb 11 10:24 pre_disk_setup_common.cfg
-rwxr-xr-x 1 builder cgts 1069 Feb 11 10:24 pre_disk_storage.cfg
-rwxr-xr-x 1 builder cgts 1471 Feb 11 10:24 pre_disk_worker.cfg
-rw-r--r-- 1 builder cgts  215 Feb 11 10:24 pre_net_common.cfg
-rw-r--r-- 1 builder cgts  311 Feb 11 10:24 pre_pkglist.cfg
-rw-r--r-- 1 builder cgts  260 Feb 11 10:24 pre_pkglist_lowlatency.cfg
```

## [3]

- [builder@a51007fb8bff stx-metal]$ git log bsp-files/

```sh
[builder@a51007fb8bff stx-metal]$ git show 8267e3ce994e224657b172775b68d1762ab14711

    Add ntpd to installer, sync time from active controller during install
    
    To avoid potential issues due to large time jumps when NTP first syncs
    the system time at runtime, this update adds ntpd to the installer
    rootfs and adds a pre-script to the kickstarts to sync the time from
    the active controller before starting to install the software. This
    also ensures that any filesystem timestamps will be accurate right
    from the node installation.

```

## [4]

- [builder@a51007fb8bff starlingx]$ repo grep pxecontroller


## [5] How are the different directories called?

- installer/pxe-network-installer/
- kickstart/

## [6] Others

- What is this file for?

```
bsp-files/upgrades/metadata.xml
```
