```sh
cgcs-root/stx/git/nova/stx-patches/0045-nova-service-enabled-flag-for-nova-goenabled.patch:nfv-vim subsequently issues nova API os-services/force-down and
cgcs-root/stx/git/nova/stx-patches/0045-nova-service-enabled-flag-for-nova-goenabled.patch:   hard-rebooted during the init or later by nfv-vim recovery.
cgcs-root/stx/git/nova/stx-patches/0045-nova-service-enabled-flag-for-nova-goenabled.patch:   by nfv-vim.
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/upgrades/controller.py:            ("nfv-vim",
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/upgrades/controller.py:             "nfv-vim-manage db-load-data -d %s -f %s" %
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/upgrades/management.py:        vim_cmd = ("nfv-vim-manage db-dump-data -d %s -f %s" %
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/nfv.pp:  platform::firewall::rule { 'nfv-vim-api':
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/nfv.pp:    service_name => 'nfv-vim',
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> nfv-vim-api.log ${HOST}", value("HOST") condition(filter(f_vim_api)
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> nfv-vim.log ${HOST}", value("HOST") condition(filter(f_vim)));
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> nfv-vim-webserver.log ${HOST}", value("HOST") condition(filter(f_vi
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/alarm.pp:  $storage_file        = '/var/log/nfv-vim-alarms.log',
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/event_log.pp:  $storage_file        = '/var/log/nfv-vim-events.log',
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/params.pp:    $package_name            = 'nfv-vim'
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/params.pp:    $package_name            = 'nfv-vim'
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/params.pp:    $package_name            = 'nfv-vim'
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICES" VALUES(59,'yes','vim','initial','initial','none','none',2,1,90000,4,16,'/var/run/nfv-vim.pid');
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICES" VALUES(60,'yes','vim-api','initial','initial','none','none',2,1,90000,4,16,'/var/run/nfv-vim-ap
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICES" VALUES(61,'yes','vim-webserver','initial','initial','none','none',2,1,90000,4,16,'/var/run/nfv-
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_vim           { file("/var/log/nfv-vim.log" template(t_nfv)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_vim_api       { file("/var/log/nfv-vim-api.log" template(t_nfv)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_vim_webserver { file("/var/log/nfv-vim-webserver.log" template(t_nfv)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.logrotate:/var/log/nfv-vim-api.log
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.logrotate:/var/log/nfv-vim-webserver.log
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.logrotate:/var/log/nfv-vim.log
cgcs-root/stx/stx-integ/tools/collector/scripts/collect_date:filedatelist+=("/var/log/nfv-vim-events.log")
cgcs-root/stx/stx-integ/tools/collector/scripts/collect_date:filedatelist+=("/var/log/nfv-vim-alarms.log")
cgcs-root/stx/stx-integ/tools/collector/scripts/collect_nfv_vim.sh:LOGFILE="${extradir}/nfv-vim.info"
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf:CONTROLLER_SERVICE_LIST=aodh-api aodh-listener aodh-notifier aodh-evaluator barbican-api barbican-keyston
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf:API_STATS_STRUCTURE=ironic-conductor;ironic-co;|ironic-api;ironic-ap;6485|radosgw-swift;radosgw;8|magnum-
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:      "nfv-vim")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:         process_list=(${process_list[@]} "sm:nfv-vim:vim:controller:/var/run/nfv-vim.pid:0")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:      "nfv-vim-api")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:         process_list=(${process_list[@]} "sm:nfv-vim-api:vim-api:controller:/var/run/nfv-vim-api.pid:0")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:      "nfv-vim-webserver")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes:         process_list=(${process_list[@]} "sm:nfv-vim-webserver:vim-webserver:controller:/var/run/nfv-vim-webs
cgcs-root/stx/stx-metal/bsp-files/filter_out_from_storage:nfv-vim
cgcs-root/stx/stx-metal/bsp-files/filter_out_from_worker:nfv-vim
cgcs-root/stx/stx-metal/bsp-files/filter_out_from_worker_lowlatency:nfv-vim
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeCtrl.cpp:    ilog("nfv-vim-api : %d (port)\n", mtc_config.vim_cmd_port );
cgcs-root/stx/stx-nfv/.zuul.yaml:        nfv-vim: true
cgcs-root/stx/stx-nfv/api-ref/source/api-ref-nfv-vim-v1.rst:     "name": "nfv-vim",
cgcs-root/stx/stx-nfv/api-ref/source/index.rst:   api-ref-nfv-vim-v1
cgcs-root/stx/stx-nfv/centos_iso_image.inc:nfv-vim
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:NFV_VIM_DIR=$NFV_DIR/nfv-vim
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim || is_service_enabled nfv-vim-api || is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:        sudo rm -rf $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim.service
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-api; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:        sudo rm -rf $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-api.service
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:        sudo rm -rf $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-webserver.service
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    pip_uninstall nfv-vim
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim || is_service_enabled nfv-vim-api || is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-api; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim.service "Service" "Type" "forking"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim.service "Service" "PIDFile" "/var/run/nfv-vim.pid"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-api.service "Service" "Type" "forking"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-api.service "Service" "PIDFile" "/var/run/nfv-vim-api.pid"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-webserver.service "Service" "Type" "forking"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    iniset -sudo $STXNFV_SYSCONFDIR/systemd/system/devstack@nfv-vim-webserver.service "Service" "PIDFile" "/var/run/nfv-vim-webserver.pid"
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim || is_service_enabled nfv-vim-api || is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-api; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    run_process nfv-vim "$NFV_OCF_DIR/vim start" root root
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    run_process nfv-vim-api "$NFV_OCF_DIR/vim-api start" root root
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    run_process nfv-vim-webserver "$NFV_OCF_DIR/vim-webserver start" root root
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-api; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    if is_service_enabled nfv-vim-webserver; then
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    stop_process nfv-vim
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    stop_process nfv-vim-api
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv:    stop_process nfv-vim-webserver
cgcs-root/stx/stx-nfv/nfv/centos/build_srpm:SRC_DIR=("./nfv-common" "./nfv-plugins" "./nfv-tools" "./nfv-vim" "./nfv-client")
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%package -n nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%description -n nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:sed -i -e 's|@SYSCONFDIR@|%{_sysconfdir}|g' nfv-vim/scripts/vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:sed -i -e 's|@SYSCONFDIR@|%{_sysconfdir}|g' nfv-vim/scripts/vim-api
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:sed -i -e 's|@SYSCONFDIR@|%{_sysconfdir}|g' nfv-vim/scripts/vim-webserver
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:sed -i -e 's|@SYSCONFDIR@|%{_sysconfdir}|g' nfv-vim/nfv_vim/config.ini
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:sed -i -e 's|@PYTHONROOT@|%{pythonroot}|g' nfv-vim/nfv_vim/config.ini
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%build_python nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%install_python nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:# nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:install -p -D -m 755 nfv-vim/scripts/vim %{buildroot}/usr/lib/ocf/resource.d/nfv/vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:install -p -D -m 755 nfv-vim/scripts/vim-api %{buildroot}/usr/lib/ocf/resource.d/nfv/vim-api
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:install -p -D -m 755 nfv-vim/scripts/vim-webserver %{buildroot}/usr/lib/ocf/resource.d/nfv/vim-webserver
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:install -p -D -m 600 nfv-vim/nfv_vim/config.ini %{buildroot}/%{_sysconfdir}/nfv/vim/config.ini
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:install -p -D -m 600 nfv-vim/nfv_vim/debug.ini %{buildroot}/%{_sysconfdir}/nfv/vim/debug.ini
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%files -n nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%doc nfv-vim/LICENSE
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%{local_bindir}/nfv-vim
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%{local_bindir}/nfv-vim-api
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%{local_bindir}/nfv-vim-manage
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec:%{local_bindir}/nfv-vim-webserver
cgcs-root/stx/stx-nfv/nfv/nfv-common/nfv_common/forensic/_nfv_vim_parser.py:    config_file = os.path.dirname(path) + "/config/nfv-vim.yaml"
cgcs-root/stx/stx-nfv/nfv/nfv-common/nfv_common/forensic/_parsers.py:    _parsers['nfv-vim'] = _nfv_vim_parser.parser_initialize()
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:# Description: Un-gzips nfv-vim log files within the same directory and generates
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:# Behaviour   : The script runs without any input arguments, copies all nfv-vim
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:#               original gzipped files). After this it begins parsing each nfv-vim
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:#               log file found, starting from the highest numbered e.g. nfv-vim.log.20
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:#               down to the lowest nfv-vim.log.1 or nfv-vim.log. This script expects
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:call("cp nfv-vim.log nfv-vim.log.[0-9] nfv-vim.log.[0-9][0-9] nfv-vim.log.[0-9].gz nfv-vim.log.[0-9][0-9].gz logs/",
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:call("gunzip logs/nfv-vim.log.[0-9].gz logs/nfv-vim.log.[0-9][0-9].gz", shell=True)
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py:        sorted_files = glob.glob(logDir + "nfv-vim.log*")
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/plotter.py:# Description: Graphs nfv-vim histogram data from CSV files to html page and saves
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/plotter.py:    print("This script is meant to graph average execution times and the delta of hits between sample periods for proc
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/alarm_handlers/config.ini:file=/var/log/nfv-vim-alarms.log
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/event_log_handlers/config.ini:file=/var/log/nfv-vim-events.log
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/scripts/nfvi-plugins.logrotate:/var/log/nfv-vim-alarms.log
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/scripts/nfvi-plugins.logrotate:/var/log/nfv-vim-events.log
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/README:#   kill nfv-vim processes
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/README:#   coverage run --rcfile ./coverage.ini /usr/bin/nfv-vim -c /etc/nfv/vim/config.ini &
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/README:#   kill nfv-vim processes
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/config.ini:dir=/var/log/nfv-vim-tests
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/config.ini:result_file=/var/log/nfv-vim-tests/results.txt
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/config.ini:tar_file=/var/log/nfv-vim-test.tar.gz
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/tests/_test_hosts.py:    LOG_FILES = {'nfv-vim': '/var/log/nfv-vim.log'}
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/tests/_test_instances.py:    LOG_FILES = {'nfv-vim': '/var/log/nfv-vim.log'}
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_database_upgrades.py:            vim_cmd = ("nfv-vim-manage db-load-data -d %s "
cgcs-root/stx/stx-nfv/nfv/nfv-tools/nfv_tools/forensic.ini:nfv-vim: /var/log/nfv-vim.log
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/api/controllers/root.py:        root.name = "nfv-vim"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/directors/_instance_director.py:NFV_VIM_UNLOCK_COMPLETE_FILE = '/var/run/.nfv-vim.unlock_complete'
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/dor/_dor_module.py:NFV_VIM_DOR_COMPLETE_FILE = '/var/run/.nfv-vim.dor_complete'
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/dor/_dor_module.py:    # controller.  Also need to remove /var/run/.nfv-vim.dor_complete
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim.py:PROCESS_NOT_RUNNING_FILE = '/var/run/.nfv-vim.not_running'
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_api.py:PROCESS_NOT_RUNNING_FILE = '/var/run/.nfv-vim-api.not_running'
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_manage.py:            usage=('nfv-vim-manage <command> [<args>] \n' +
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_webserver.py:PROCESS_NOT_RUNNING_FILE = '/var/run/.nfv-vim-webserver.not_running'
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim:process="nfv-vim"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim:PROCESS_NOT_RUNNING_FILE="/var/run/.nfv-vim.not_running"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-api:process="nfv-vim-api"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-api:PROCESS_NOT_RUNNING_FILE="/var/run/.nfv-vim-api.not_running"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-webserver:process="nfv-vim-webserver"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-webserver:PROCESS_NOT_RUNNING_FILE="/var/run/.nfv-vim-webserver.not_running"
cgcs-root/stx/stx-nfv/nfv/nfv-vim/setup.py:            'nfv-vim = nfv_vim.vim:process_main',
cgcs-root/stx/stx-nfv/nfv/nfv-vim/setup.py:            'nfv-vim-api = nfv_vim.vim_api:process_main',
cgcs-root/stx/stx-nfv/nfv/nfv-vim/setup.py:            'nfv-vim-manage = nfv_vim.vim_manage:process_main',
cgcs-root/stx/stx-nfv/nfv/nfv-vim/setup.py:            'nfv-vim-webserver = nfv_vim.vim_webserver:process_main',
cgcs-root/stx/stx-nfv/nfv/tox.ini:nfv_vim_dir = ./nfv-vim
cgcs-root/stx/stx-update/patch-scripts/EXAMPLE_VIM/scripts/vim-restart-example:    processes_to_restart="nfv-vim nfv-vim-api nfv-vim-webserver"
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_B/scripts/restart-script:    processes_to_restart="nfv-vim nfv-vim-api nfv-vim-webserver"
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_F/scripts/restart-script:    processes_to_restart="nfv-vim nfv-vim-api nfv-vim-webserver"
cgcs-root/stx/stx-update/patch-scripts/test-patches/INSVC_CONTROLLER/scripts/controller-restart:    processes_to_restart="nfv-vim n
```

```sh
cgcs-root/stx/git/nova/stx-patches/0045-nova-service-enabled-flag-for-nova-goenabled.patch
cgcs-root/stx/stx-config/controllerconfig/controllerconfig/controllerconfig/upgrades/controller.py
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/nfv.pp
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/alarm.pp
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/event_log.pp
cgcs-root/stx/stx-config/puppet-modules-wrs/puppet-nfv/src/nfv/manifests/params.pp
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.logrotate
cgcs-root/stx/stx-integ/tools/collector/scripts/collect_date
cgcs-root/stx/stx-integ/tools/collector/scripts/collect_nfv_vim.sh
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-processes
cgcs-root/stx/stx-metal/bsp-files/filter_out_from_storage
cgcs-root/stx/stx-metal/bsp-files/filter_out_from_worker
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeCtrl.cpp
cgcs-root/stx/stx-nfv/.zuul.yaml
cgcs-root/stx/stx-nfv/api-ref/source/api-ref-nfv-vim-v1.rst
cgcs-root/stx/stx-nfv/api-ref/source/index.rst
cgcs-root/stx/stx-nfv/centos_iso_image.inc
cgcs-root/stx/stx-nfv/devstack/lib/stx-nfv
cgcs-root/stx/stx-nfv/nfv/centos/build_srpm
cgcs-root/stx/stx-nfv/nfv/centos/nfv.spec
cgcs-root/stx/stx-nfv/nfv/nfv-common/nfv_common/forensic/_nfv_vim_parser.py
cgcs-root/stx/stx-nfv/nfv/nfv-common/nfv_common/forensic/_parsers.py
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/Histogram.py
cgcs-root/stx/stx-nfv/nfv/nfv-debug-tools/histogram_analysis/plotter.py
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/alarm_handlers/config.ini
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/nfv_plugins/event_log_handlers/config.ini
cgcs-root/stx/stx-nfv/nfv/nfv-plugins/scripts/nfvi-plugins.logrotate
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/README
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/config.ini
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/tests/_test_hosts.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_scenario_tests/tests/_test_instances.py
cgcs-root/stx/stx-nfv/nfv/nfv-tests/nfv_unit_tests/tests/test_database_upgrades.py
cgcs-root/stx/stx-nfv/nfv/nfv-tools/nfv_tools/forensic.ini
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/api/controllers/root.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/directors/_instance_director.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/dor/_dor_module.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_api.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_manage.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/nfv_vim/vim_webserver.py
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-api
cgcs-root/stx/stx-nfv/nfv/nfv-vim/scripts/vim-webserver
cgcs-root/stx/stx-nfv/nfv/nfv-vim/setup.py:
cgcs-root/stx/stx-nfv/nfv/tox.ini
cgcs-root/stx/stx-update/patch-scripts/EXAMPLE_VIM/scripts/vim-restart-example
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_B/scripts/restart-script
cgcs-root/stx/stx-update/patch-scripts/test-patches/INSVC_CONTROLLER/scripts/controller-restart
```
