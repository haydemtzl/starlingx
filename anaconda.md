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
