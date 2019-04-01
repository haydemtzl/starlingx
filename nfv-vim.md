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

```sh
cgcs-root/stx/git/nova/stx-patches
cgcs-root/stx/stx-config/controllerconfig
cgcs-root/stx/stx-config/puppet-manifests
cgcs-root/stx/stx-config/puppet-modules-wrs
cgcs-root/stx/stx-ha/service-mgmt
cgcs-root/stx/stx-integ/config-files
cgcs-root/stx/stx-integ/tools/collector/
cgcs-root/stx/stx-integ/tools/engtools/
cgcs-root/stx/stx-integ/utilities/
cgcs-root/stx/stx-metal/bsp-files/
cgcs-root/stx/stx-metal/mtce/src/maintenance/
cgcs-root/stx/stx-nfv/
cgcs-root/stx/stx-update/patch-scripts/
```

## mtcAgent

```sh
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/mtce.pp:    command => 'pkill -HUP mtcAgent',
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> mtcAgent_alarm.log ${HOST}", value("HOST") condition(filter(f_mtcag
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> mtcAgent_api.log ${HOST}", value("HOST") condition(filter(f_mtcagen
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> mtcAgent_event.log ${HOST}", value("HOST") condition(filter(f_mtcag
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb:  set("<%= @system_name %> mtcAgent.log ${HOST}", value("HOST") condition(filter(f_mtcagent)))
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/cluster/cluster_xml.py:                    <resource id="mtcAgent"/>
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICES" VALUES(20,'yes','mtc-agent','initial','initial','none','none',2,1,90000,4,16,'/var/run/mtcAgent
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICE_ACTIONS" VALUES('mtc-agent','enable','ocf-script','platform','mtcAgent','start','',2,2,2,30,'');
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICE_ACTIONS" VALUES('mtc-agent','disable','ocf-script','platform','mtcAgent','stop','',1,1,1,15,'');
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICE_ACTIONS" VALUES('mtc-agent','audit-enabled','ocf-script','platform','mtcAgent','monitor','',2,2,2
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql:INSERT INTO "SERVICE_ACTIONS" VALUES('mtc-agent','audit-disabled','ocf-script','platform','mtcAgent','monitor','',0,0,
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_mtcagent    { file("/var/log/mtcAgent.log" template(t_mtc)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_mtcagentalarm { file("/var/log/mtcAgent_alarm.log" template(t_mtc)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_mtcagentapi   { file("/var/log/mtcAgent_api.log" template(t_mtc)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:destination d_mtcagentevent { file("/var/log/mtcAgent_event.log" template(t_mtc)); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:filter f_mtcagent    { facility(local5) and program(mtcAgent); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:filter f_mtcagentalarm { facility(local5) and program(/var/log/mtcAgent_alarm.log); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:filter f_mtcagentapi   { facility(local5) and program(/var/log/mtcAgent_api.log); };
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf:filter f_mtcagentevent { facility(local5) and program(/var/log/mtcAgent_event.log); };
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py:#  4. send mtcAgent the degrade state message.
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py:    # for mtcAgent to optionally log it.
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py:    # Just reduce load on mtcAgent
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py:    # Send the degrade state ; assert or clear message to mtcAgent.
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf:CONTROLLER_SERVICE_LIST=aodh-api aodh-listener aodh-notifier aodh-evaluator barbican-api barbican-keyston
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-mtce:       "mtcAgent")
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-mtce:           sm_managed_processes=(  ${sm_managed_processes[@]} "mtcAgent:0:mtc-agent")
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    sudo install -m 755 -p -D scripts/mtcAgent ${OCF_ROOT}/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:        sudo sed -i "s|#!/bin/sh|#!/bin/bash|g" ${OCF_ROOT}/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    # sed -i "s|\${OCF_ROOT}|\${OCF_ROOT}|g" ${OCF_ROOT}/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    sudo install -m 755 -p -D maintenance/mtcAgent ${bin_dir}/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:function start_mtcAgent {
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    # init mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    iniset -sudo /etc/systemd/system/devstack@mtcAgent.service "Service" "Type" "forking"
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    iniset -sudo /etc/systemd/system/devstack@mtcAgent.service "Service" "PIDFile" "/var/run/mtcAgent.pid"
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    iniset -sudo /etc/systemd/system/devstack@mtcAgent.service "Service" "Environment" "OCF_ROOT=${OCF_ROOT}"
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:        ${OCF_ROOT}/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    run_process mtcAgent "${OCF_ROOT}/resource.d/platform/mtcAgent start" root root
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    if is_service_enabled mtcAgent; then
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:        start_mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:function stop_mtcAgent {
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    stop_process mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    if is_service_enabled mtcAgent; then
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:        stop_mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    sudo rm -rf ${OCF_ROOT}/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/devstack/lib/stx-metal:    sudo rm -rf ${bin_dir}/mtcAgent
cgcs-root/stx/stx-metal/devstack/settings:# unless they are coupled in functionality(e.g. mtcAgent/mtcClient).
cgcs-root/stx/stx-metal/devstack/settings:    # enable mtcAgent/mtcClient service as part of mtce service
cgcs-root/stx/stx-metal/devstack/settings:    enable_service mtcAgent
cgcs-root/stx/stx-metal/devstack/settings:if is_service_enabled mtcAgent; then
cgcs-root/stx/stx-metal/devstack/settings:        # mtcAgent requires that hostname must be "controller-0" or "controller-1"
cgcs-root/stx/stx-metal/devstack/settings:        echo_summary "hostname doesn't meet requirements, so disable mtcAgent"
cgcs-root/stx/stx-metal/devstack/settings:        disable_service mtcAgent
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__POWER_CMD    ("/var/run/fit/power_cmd_result") /* mtcAgent */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__MC_INFO      ("/var/run/fit/mc_info")          /* mtcAgent */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__POWER_STATUS ("/var/run/fit/power_status")     /* mtcAgent */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__RESTART_CAUSE ("/var/run/fit/restart_cause")   /* mtcAgent */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__UPTIME        ("/var/run/fit/uptime")          /* mtcAgent */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__LOUD_BM_PW    ("/var/run/fit/loud_bm_pw")      /* mtcAgent & hwmond */
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h:#define MTC_CMD_FIT__GOENABLE_AUDIT ("/var/run/fit/goenable_audit") /* mtcAgent  */
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h:    int   mtc_agent_port        ; /**< mtcAgent receive port (from Client)    */
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h:    int   mtc_to_hbs_cmd_port   ; /**< mtcAgent to hbsAgent command port      */
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h:    int   mtc_to_guest_cmd_port ; /**< mtcAgent to guestAgent command port    */
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h:    int   hwmon_cmd_port        ; /**< mtcAgent to hwmon command port         */
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h:    int   hbs_to_mtc_event_port ; /**< hbsAgent tm mtcAgent event port        */
cgcs-root/stx/stx-metal/mtce-common/src/daemon/daemon_config.cpp: * Assumptions : Both mtcAgent and hbsAgent need this conversion.
cgcs-root/stx/stx-metal/mtce-common/src/daemon/daemon_config.cpp:    /* mtcAgent & hwmond */
cgcs-root/stx/stx-metal/mtce-common/src/daemon/daemon_config.cpp:    /* mtcAgent */
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:notification and recovery.The Maintenance Service (mtcAgent/mtcClient)
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:notification and recovery.The Maintenance Service (mtcAgent/mtcClient)
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:install -m 755 -p -D %{_buildsubdir}/scripts/mtcAgent %{buildroot}/usr/lib/ocf/resource.d/platform/mtcAgent
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:install -m 755 -p -D %{_buildsubdir}/maintenance/mtcAgent %{buildroot}/%{local_bindir}/mtcAgent
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:%{ocf_resourced}/platform/mtcAgent
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec:%{local_bindir}/mtcAgent
cgcs-root/stx/stx-metal/mtce/src/common/nodeClass.h:          * on reset recovery until it is acknowledged by active mtcAgent
cgcs-root/stx/stx-metal/mtce/src/common/nodeClass.h:        /** current state of heartbeat failure per interface for mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsAgent.cpp:static string mtcAgent_ip = "" ;
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsAgent.cpp:        mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsAgent.cpp:        new msgClassTx ( mtcAgent_ip.data(),
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsAgent.cpp:                   hbs_config.mgmnt_iface, mtcAgent_ip.c_str(), port );
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsBase.h:    /** Mtce to Heartbeat Service Cmd Interface - mtcAgent -> hbsAgent      */
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsBase.h:    /** Heartbeat Service Event Transmit Interface - hbsAgent -> mtcAgent   */
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsBase.h:    /** Heartbeat Service Event Transmit Interface - hbsClient -> mtcAgent  */
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsClient.cpp:string mtcAgent_ip = "" ;
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsClient.cpp:    mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsClient.cpp:    hbs_sock.hbs_ready_tx_sock = new msgClassTx(mtcAgent_ip.c_str(), hbs_config.mtc_rx_mgmnt_port, IPPROTO_UDP, hbs_config.mgmnt_ifac
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonClass.cpp:        /* handle sending a degrade clear request to mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonClass.cpp:        /* handle sending a degrade request to mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonHdlr.cpp:                                        /* Send reset request to mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonHdlr.cpp:                                        /* Send reset request to mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonHdlr.cpp:         * BMC access error in mtcAgent where we only raise a minor alarm */
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:string mtcAgent_ip = "" ;
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:    mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:    hwmon_sock.cmd_sock = new msgClassRx(mtcAgent_ip.c_str(), hwmon_sock.cmd_port, IPPROTO_UDP, NULL, true);
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:    mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:    ilog ("ControllerIP: %s\n", mtcAgent_ip.c_str());
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:    hwmon_sock.event_sock = new msgClassTx(mtcAgent_ip.c_str(),hwmon_sock.event_port , IPPROTO_UDP, iface);
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp:        mlog ("%s sending '%s' event to mtcAgent for '%s'\n", 
cgcs-root/stx/stx-metal/mtce/src/hwmon/scripts/ocf/hwmon:    check_binary "/usr/local/bin/mtcAgent"
cgcs-root/stx/stx-metal/mtce/src/maintenance/Makefile:BINS = mtcAgent mtcClient
cgcs-root/stx/stx-metal/mtce/src/maintenance/Makefile:mtcAgent: $(OBJS)
cgcs-root/stx/stx-metal/mtce/src/maintenance/Makefile:  $(CXX) $(CONTROL_OBJS) -L../public -L../alarm $(LDLIBS) $(EXTRALDFLAGS) -o mtcAgent
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCompMsg.cpp:            /* inform mtcAgent of enhanced ost services support */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCompMsg.cpp:/** Send an event to the mtcAgent **/
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCtrlMsg.cpp:                        /* Notify mtcAgent that we got a pmond ready event */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCtrlMsg.cpp:                        /* Notify mtcAgent that we got a hbsClient ready event */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCtrlMsg.cpp:            slog ("request to send mtcAlive message from mtcAgent ; invalid\n");
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:    ctrl.mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:    ilog ("Controller  : %s\n", ctrl.mtcAgent_ip.c_str());
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:    /* Setup the Mgmnt Interface Transmit messaging to mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:            /* Setup the Infra Interface Transmit Messaging to mtcAgent  */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp: *              to mtcAgent.
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp: *              to mtcAgent.
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:    ctrl.mtcAgent_ip = "";
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp:     * This saves the mtcAgent from having to issue and manage 2 commands,
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp: * child process will send a GOENABLED message to the mtcAgent
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp: * This success message is logged in the mtcAgent and if this
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp: * returned and sends that message to the mtcAgent which will cause
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.h:    string mtcAgent_ip ;
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeCtrl.cpp:     * locally or on peer controller can get events into mtcAgent */
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeHdlrs.cpp:            /** Manage the subfunction goenabled alarm over a mtcAgent restart
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeMsg.h:         * mtcAgent *                      * mtcClient *
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeMsg.h:    /** UDP sockets used by the mtcAgent to transmit and receive
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeMsg.h:      * commands from and transmit replies to the mtcAgent              */
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonHdlr.cpp:        /* Unlike the above call to pmonAlarm_minor_log, this call only creates a log entry in mtcAgent.log */
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonMsg.cpp:string mtcAgent_ip = "" ;
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonMsg.cpp:    mtcAgent_ip = getipbyname ( CONTROLLER );
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonMsg.cpp:    ilog ("ControllerIP: %s\n", mtcAgent_ip.c_str());
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonMsg.cpp:    pmon_sock.event_sock = new msgClassTx(mtcAgent_ip.c_str(), port, IPPROTO_UDP, iface);
cgcs-root/stx/stx-metal/mtce/src/scripts/dmemchk.sh:    echo "sudo memchk -t 60 --C mtcClient mtcAgent            ... Check PSS and RSS values of the processes belonging to mtcClient"
cgcs-root/stx/stx-metal/mtce/src/scripts/dmemchk.sh:    echo "                                                        and mtcAgent every 60 seconds (1 minute)"
cgcs-root/stx/stx-metal/mtce/src/scripts/hbsAgent:    check_binary "/usr/local/bin/mtcAgent"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtc.conf:; configuration, goenabled etc. , then the mtcAgent will stop retrying after
cgcs-root/stx/stx-metal/mtce/src/scripts/mtc.conf:; the mtcAgent will use the service specific interval value specified
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:OCF_RESKEY_binary_default="mtcAgent"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:OCF_RESKEY_pid_default="/var/run/mtcAgent.pid"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:$0 manages the Platform's Controller Maintenance (mtcAgent) process as an HA resource
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:   The 'meta-data' .  operation reports the mtcAgent's meta-data information.
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        ocf_log info "mtcAgent:meta_data"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:<resource-agent name="mtcAgent">
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:This 'mtcAgent' is an OCF Compliant Resource Agent that manages start, stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:Manages the Titanium Cloud's Maintenance (mtcAgent) Daemon.
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:This option is used to direct the mtcAgent dameon log stream.
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:logging = true  ... /var/log/mtcAgent.log  (default)
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_validate() {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        ocf_log info "mtcAgent:validate"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_status () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    proc="mtcAgent:status"  
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        ocf_log info "mtcAgent:status"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_monitor () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    proc="mtcAgent:monitor"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    mtcAgent_status
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_start () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    start_proc="mtcAgent:start"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        mtcAgent_status
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:            mtcAgent_stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:               ocf_log info "mtcAgent: Startup Health Test Failed - missing info"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_confirm_stop () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    proc="mtcAgent:confirm_stop"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_stop () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    proc="mtcAgent:stop"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        mtcAgent_confirm_stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    mtcAgent_confirm_stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_reload () {
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    proc="mtcAgent:reload"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    mtcAgent_stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:        mtcAgent_start
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:ocf_log info "mtcAgent:${__OCF_ACTION} action"
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:mtcAgent_validate || exit $?
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    start)        mtcAgent_start
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    stop)         mtcAgent_stop
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    status)       mtcAgent_status
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    reload)       mtcAgent_reload
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    monitor)      mtcAgent_monitor
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent:    validate-all) mtcAgent_validate
cgcs-root/stx/stx-metal/mtce/src/scripts/mtce.logrotate:/var/log/mtcAgent.log
cgcs-root/stx/stx-metal/mtce/src/scripts/mtce.logrotate:/var/log/mtcAgent_api.log
cgcs-root/stx/stx-metal/mtce/src/scripts/mtce.logrotate:/var/log/mtcAgent_event.log
cgcs-root/stx/stx-metal/mtce/src/scripts/mtce.logrotate:/var/log/mtcAgent_alarm.log
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh:# This test script soaks the mtcAgent and hbsAgent start and stop operations.
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh:    for service in "sysinv-api" "sysinv-conductor" "sysinv-agent" "mtcAgent" "hbsAgent"
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh:    for service in "sysinv-api" "sysinv-conductor" "sysinv-agent" "mtcAgent" "hbsAgent"
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh:    mtc=`cat /var/run/mtcAgent.pid`
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh:    echo "$mtc:`pidof mtcAgent`  <:> $hbs:`pidof hbsAgent`"
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_swact.sh:# This test script soaks the mtcAgent and hbsAgent start and stop operations.
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:    ilog("Command Port: %d (rx) from mtcAgent\n",   guest_config.mtc_to_guest_cmd_port );
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:    ilog("Event   Port: %d (tx)   to mtcAgent\n",   guest_config.hbs_to_mtc_event_port );
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:    /* UDP Tx Message Socket Towards mtcAgent                         */
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:        elog ("Failed to setup 'mtcAgent' 'lo' transmitter on port (%d)\n",
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp: * Name:        send_event_to_mtcAgent
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp: *              messages and sending them to the mtcAgent daemon locally.
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:int send_event_to_mtcAgent ( string       hostname, 
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:            ilog ("%s requesting inventory from mtcAgent\n", hostname.c_str());
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp: * Name:        service_mtcAgent_command
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp: * Description: Message handling interface capable of servicing mtcAgent
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:int service_mtcAgent_command ( unsigned int cmd , char * buf_ptr )
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:        rc = send_event_to_mtcAgent ( hostInv.hostBase.my_hostname, 
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:            /* Service mtcAgent commands */
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:                mlog1 ("Received %d bytes from %s:%d:mtcAgent\n", bytes,
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp:                    service_mtcAgent_command ( msg.cmd , &msg.buf[0] );
cgcs-root/stx/stx-nfv/mtce-guest/src/guestBase.h:    /** Guest Services Messaging Socket mtcAgent commands are received on */
cgcs-root/stx/stx-nfv/mtce-guest/src/guestClass.h:    int  del_host ( string hostname ); /* delete the host from the daemon - mtcAgent */
cgcs-root/stx/stx-nfv/mtce-guest/src/guestHttpSvr.cpp:extern int send_event_to_mtcAgent ( string hostname, unsigned int event);
cgcs-root/stx/stx-nfv/mtce-guest/src/guestHttpSvr.cpp:                    send_event_to_mtcAgent ( obj_ptr->hostBase.my_hostname, MTC_EVENT_MONITOR_READY ) ;
cgcs-root/stx/stx-nfv/mtce-guest/src/guestHttpSvr.cpp:                    send_event_to_mtcAgent ( obj_ptr->hostBase.my_hostname, MTC_EVENT_MONITOR_READY ) ;
cgcs-root/stx/stx-nfv/mtce-guest/src/guestHttpSvr.cpp:                    send_event_to_mtcAgent ( obj_ptr->hostBase.my_hostname, MTC_EVENT_MONITOR_READY ) ;
cgcs-root/stx/stx-update/patch-scripts/EXAMPLE_MTCE/scripts/mtce-restart-example:    mtcAgent mtcClient \
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_A/scripts/restart-script:    mtcAgent mtcClient \
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_C/scripts/restart-script:    mtcAgent mtcClient \
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_E/scripts/restart-script:    mtcAgent mtcClient \
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_F/scripts/restart-script:    mtcAgent mtcClient \
```

```sh
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/mtce.pp
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb
cgcs-root/stx/stx-config/sysinv/sysinv/sysinv/sysinv/cluster/cluster_xml.py
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-mtce
cgcs-root/stx/stx-metal/devstack/lib/stx-metal
cgcs-root/stx/stx-metal/devstack/settings
cgcs-root/stx/stx-metal/mtce-common/src/common/fitCodes.h
cgcs-root/stx/stx-metal/mtce-common/src/common/logMacros.h
cgcs-root/stx/stx-metal/mtce-common/src/daemon/daemon_config.cpp
cgcs-root/stx/stx-metal/mtce/centos/mtce.spec
cgcs-root/stx/stx-metal/mtce/src/common/nodeClass.h
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsAgent.cpp
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsBase.h
cgcs-root/stx/stx-metal/mtce/src/heartbeat/hbsClient.cpp
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonClass.cpp
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonHdlr.cpp
cgcs-root/stx/stx-metal/mtce/src/hwmon/hwmonMsg.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/Makefile
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCompMsg.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcCtrlMsg.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeComp.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeCtrl.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeHdlrs.cpp
cgcs-root/stx/stx-metal/mtce/src/maintenance/mtcNodeMsg.h
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonHdlr.cpp
cgcs-root/stx/stx-metal/mtce/src/pmon/pmonMsg.cpp
cgcs-root/stx/stx-metal/mtce/src/scripts/dmemchk.sh
cgcs-root/stx/stx-metal/mtce/src/scripts/hbsAgent
cgcs-root/stx/stx-metal/mtce/src/scripts/mtc.conf
cgcs-root/stx/stx-metal/mtce/src/scripts/mtcAgent
cgcs-root/stx/stx-metal/mtce/src/scripts/mtce.logrotate
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_ras.sh
cgcs-root/stx/stx-metal/mtce/src/scripts/stress_swact.sh
cgcs-root/stx/stx-nfv/mtce-guest/src/guestAgent.cpp
cgcs-root/stx/stx-nfv/mtce-guest/src/guestBase.h
cgcs-root/stx/stx-nfv/mtce-guest/src/guestClass.h
cgcs-root/stx/stx-nfv/mtce-guest/src/guestHttpSvr.cpp
cgcs-root/stx/stx-update/patch-scripts/EXAMPLE_MTCE/scripts/mtce-restart-example
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_A/scripts/restart-script
```

```sh
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/manifests/mtce.pp
cgcs-root/stx/stx-config/puppet-manifests/src/modules/platform/templates/remotelogging.conf.erb
cgcs-root/stx/stx-config/sysinv/
cgcs-root/stx/stx-ha/service-mgmt/sm-db-1.0.0/database/create_sm_db.sql
cgcs-root/stx/stx-integ/config-files/syslog-ng-config/files/syslog-ng.conf
cgcs-root/stx/stx-integ/monitoring/collectd-extensions/src/mtce_notifier.py
cgcs-root/stx/stx-integ/tools/engtools/hostdata-collectors/scripts/cfg/engtools.conf
cgcs-root/stx/stx-integ/utilities/platform-util/scripts/patch-restart-mtce
cgcs-root/stx/stx-metal/devstack
cgcs-root/stx/stx-metal/mtce-common
cgcs-root/stx/stx-metal/mtce
cgcs-root/stx/stx-nfv/mtce-guest
cgcs-root/stx/stx-update/patch-scripts/EXAMPLE_MTCE/scripts/mtce-restart-example
cgcs-root/stx/stx-update/patch-scripts/test-patches-suite-b/SUITE_B_PATCH_A/scripts/restart-script
```
