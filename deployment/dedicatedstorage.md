# Installing StarlingX with containers: Standard Storage configuration

```sh
controller-0:~$ cat /etc/build.info 
###
### StarlingX
###     Built from master
###

OS="centos"
SW_VERSION="19.01"
BUILD_TARGET="Host Installer"
BUILD_TYPE="Formal"
BUILD_ID="20190523T013000Z"

JOB="STX_build_master_master"
BUILD_BY="starlingx.build@cengn.ca"
BUILD_NUMBER="113"
BUILD_HOST="starlingx_mirror"
BUILD_DATE="2019-05-23 01:30:00 +0000"
```

## Setup Controller-0

### Install StarlingX

1. bootimage.iso
2. Use password _St4rlingX*_

### Bootstrap the controller

```sh
localhost:~$ ip a
```

```sh
localhost:~$ sudo ip address add 10.10.10.10/24 dev enp2s1
localhost:~$ sudo ip link set up dev enp2s1
localhost:~$ sudo ip route add default via 10.10.10.1 dev enp2s1
localhost:~$ ping 8.8.8.8
localhost:~$ ping 10.248.2.1
localhost:~$ ping 10.22.224.196
```

```sh
user@workstation:~/stx-tools/deployment/libvirt$ ssh wrsroot@10.10.10.10
The authenticity of host '10.10.10.10 (10.10.10.10)' can't be established.
ECDSA key fingerprint is SHA256:zmpmzT51KHw5VgFnH4J5fwAi5Q1hsYv2QSu03pKudyY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.10.10' (ECDSA) to the list of known hosts.
Release 19.01
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

wrsroot@10.10.10.10's password: 
Last login: Wed May 29 08:27:52 2019

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

localhost:~$ 
````

```sh
localhost:~$ vi /home/wrsroot/localhost.yml
```

```yaml
system_mode: duplex

dns_servers:
  - 10.248.2.1
  - 10.22.224.196

docker_http_proxy: http://proxy-chain.intel.com:911
docker_https_proxy: http://proxy-chain.intel.com:912
docker_no_proxy:
  - localhost
  - 127.0.0.1
  - 192.168.204.2
  - 192.168.204.3
  - 192.168.204.4
  - 10.10.10.3
  - 10.10.10.4
  - 10.10.10.5

external_oam_subnet: 10.10.10.0/24
external_oam_gateway_address: 10.10.10.1
external_oam_floating_address: 10.10.10.3

ansible_become_pass: St4rlingX*
admin_password: St4rlingX*
```

```sh
localhost:~$ ansible-playbook /usr/share/ansible/stx-ansible/playbooks/bootstrap/bootstrap.yml -e "ansible_become_pass=St4rlingX*"
```

```sh
localhost:~$ ansible-playbook /usr/share/ansible/stx-ansible/playbooks/bootstrap/bootstrap.yml -e "ansible_become_pass=St4rlingX*"

PLAY [bootstrap] ***************************************************************************************************************************************************************

TASK [include_vars] ************************************************************************************************************************************************************
ok: [localhost]

TASK [include_vars] ************************************************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Update SSH known hosts] ************************************************************************************************************************************

TASK [prepare-env : Check connectivity] ****************************************************************************************************************************************

TASK [prepare-env : Fail if host is unreachable] *******************************************************************************************************************************

TASK [prepare-env : debug] *****************************************************************************************************************************************************

TASK [prepare-env : Change initial password] ***********************************************************************************************************************************

TASK [prepare-env : Look for unmistakenly StarlingX package] *******************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Fail if host is not running the right image] ***************************************************************************************************************

TASK [prepare-env : Check initial config flag] *********************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Set skip_play flag for host] *******************************************************************************************************************************

TASK [prepare-env : Skip remaining tasks if host is already unlocked] **********************************************************************************************************

TASK [prepare-env : Fail if any of the mandatory configurations are not defined] ***********************************************************************************************

TASK [prepare-env : Set initial address facts if not defined. They will be updated later] **************************************************************************************
ok: [localhost]

TASK [prepare-env : Set docker registries to default values if not specified] **************************************************************************************************
ok: [localhost]

TASK [prepare-env : Initialize some flags to be used in subsequent roles/tasks] ************************************************************************************************
ok: [localhost]

TASK [prepare-env : Set initial facts] *****************************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Turn on use_docker_proxy flag] *****************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Set default values for docker proxies if not defined] ******************************************************************************************************
ok: [localhost]

TASK [prepare-env : Retrieve software version number] **************************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Fail if software version is not defined] *******************************************************************************************************************

TASK [prepare-env : Retrieve system type] **************************************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Fail if system type is not defined] ************************************************************************************************************************

TASK [prepare-env : Set software version, system type config path facts] *******************************************************************************************************
ok: [localhost]

TASK [prepare-env : Set config path facts] *************************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Check Docker status] ***************************************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Look for openrc file] **************************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Turn on replayed flag] *************************************************************************************************************************************

TASK [prepare-env : Check if the controller-0 host has been successfully provisioned] ******************************************************************************************

TASK [prepare-env : Set flag to indicate that this host has been previously configured] ****************************************************************************************

TASK [prepare-env : Find previous config file for this host] *******************************************************************************************************************

                                                                                                                                                                      [964/1922]
TASK [prepare-env : Fetch previous config file from this host] *****************************************************************************************************************

TASK [prepare-env : Read in last config values] ********************************************************************************************************************************

TASK [prepare-env : Turn on system attributes reconfiguration flag] ************************************************************************************************************

TASK [prepare-env : Turn on docker reconfiguration flag if docker config is changed] *******************************************************************************************

TASK [prepare-env : Turn on service endpoints reconfiguration flag if management and/or oam network config is changed] *********************************************************

TASK [prepare-env : Turn on network reconfiguration flag if any of the network related config is changed] **********************************************************************

TASK [prepare-env : Turn on restart services flag if management/oam/cluster network or docker config is changed] ***************************************************************

TASK [prepare-env : Turn off save_password flag if admin password has not changed] *********************************************************************************************

TASK [prepare-env : Turn off save_config flag if system, network, and docker configurations have not changed] ******************************************************************

TASK [prepare-env : debug] *****************************************************************************************************************************************************

TASK [prepare-env : Turn on skip_play flag] ************************************************************************************************************************************

TASK [prepare-env : Check volume groups] ***************************************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Fail if volume groups are not configured] ******************************************************************************************************************

TASK [prepare-env : Check size of root disk] ***********************************************************************************************************************************
changed: [localhost]

TASK [prepare-env : Update root disk index for remote play] ********************************************************************************************************************

TASK [prepare-env : Set root disk and root disk size facts] ********************************************************************************************************************
ok: [localhost]

TASK [prepare-env : debug] *****************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "[WARNING]: Root disk /dev/sda size is 250GB which is less than the standard size of 500GB. Please consult the Software Installation Guide for details."
}

TASK [prepare-env : Look for branding tar file] ********************************************************************************************************************************
ok: [localhost]

TASK [prepare-env : Fail if there are more than one branding tar files] ********************************************************************************************************

TASK [prepare-env : Look for other branding files] *****************************************************************************************************************************
ok: [localhost]

                                                                                                                                                                      [916/1922]
TASK [prepare-env : Fail if the branding filename is not valid] ****************************************************************************************************************

TASK [prepare-env : Mark environment as Ansible bootstrap] *********************************************************************************************************************
changed: [localhost]

TASK [prepare-env : debug] *****************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "system_config_update flag: False, network_config_update flag: False, docker_config_update flag: False, restart_services flag:  False, endpoints_reconfiguration_flag
: False, save_password flag: True, save_config flag: True, skip_play flag: False"
}

TASK [validate-config : debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "msg": [
        "System mode is simplex", 
        "Timezone is UTC", 
        "DNS servers is [u'10.248.2.1', u'10.22.224.196']", 
        "PXE boot subnet is 169.254.202.0/24", 
        "Management subnet is 192.168.204.0/28", 
        "Cluster host subnet is 192.168.206.0/24", 
        "Cluster pod subnet is 172.16.0.0/16", 
        "Cluster service subnet is 10.96.0.0/12", 
        "OAM subnet is 10.10.10.0/24", 
        "OAM gateway is 10.10.10.1", 
        "OAM floating ip is 10.10.10.3", 
        "Dynamic address allocation is True", 
        "Docker registries is [u'k8s.gcr.io', u'gcr.io', u'quay.io', u'docker.io']", 
        "Docker HTTP proxy is http://proxy-chain.intel.com:911", 
        "Docker HTTPS proxy is http://proxy-chain.intel.com:912", 
        "Docker no proxy list is [u'localhost', u'127.0.0.1', u'192.168.204.2', u'192.168.204.3', u'192.168.204.4', u'10.10.10.3', u'10.10.10.4', u'10.10.10.5']"
    ]
}

TASK [validate-config : Set system mode fact] **********************************************************************************************************************************
ok: [localhost]

TASK [validate-config : debug] *************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "System type is Standard, system mode will be set to duplex."
}

TASK [validate-config : Set system mode to duplex for Standard system] *********************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate system mode if system type is All-in-one] *****************************************************************************************************

TASK [validate-config : Checking registered timezones] *************************************************************************************************************************

TASK [validate-config : Fail if provided timezone is unknown] ******************************************************************************************************************

TASK [validate-config : Fail if the number of dns servers provided is not at least 1 and no more than 3] ***********************************************************************

TASK [validate-config : include] ***********************************************************************************************************************************************
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_dns.yml for localhost => (item=10.248.2.1)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_dns.yml for localhost => (item=10.22.224.196)

TASK [validate-config : Check format of DNS Server IP] *************************************************************************************************************************
ok: [localhost] => {
    "msg": "DNS Server: 10.248.2.1"
}

TASK [validate-config : Perform ping test] *************************************************************************************************************************************
changed: [localhost]

TASK [validate-config : Fail if DNS Server is unreachable] *********************************************************************************************************************

TASK [validate-config : Check format of DNS Server IP] *************************************************************************************************************************
ok: [localhost] => {
    "msg": "DNS Server: 10.22.224.196"
}

TASK [validate-config : Perform ping test] *************************************************************************************************************************************
changed: [localhost]

TASK [validate-config : Fail if DNS Server is unreachable] *********************************************************************************************************************

TASK [validate-config : Validate provided subnets (both IPv4 & IPv6 notations)] ************************************************************************************************
ok: [localhost] => (item={'value': u'10.10.10.0/24', 'key': u'external_oam_subnet'}) => {
    "msg": "external_oam_subnet: 10.10.10.0/24"
}
ok: [localhost] => (item={'value': u'192.168.206.0/24', 'key': u'cluster_host_subnet'}) => {
    "msg": "cluster_host_subnet: 192.168.206.0/24"
}
ok: [localhost] => (item={'value': u'10.10.10.1', 'key': u'external_oam_gateway_address'}) => {
    "msg": "external_oam_gateway_address: 10.10.10.1"
}
ok: [localhost] => (item={'value': u'10.96.0.0/12', 'key': u'cluster_service_subnet'}) => {
    "msg": "cluster_service_subnet: 10.96.0.0/12"
}
ok: [localhost] => (item={'value': u'169.254.202.0/24', 'key': u'pxeboot_subnet'}) => {
    "msg": "pxeboot_subnet: 169.254.202.0/24"
}
ok: [localhost] => (item={'value': u'239.1.1.0/28', 'key': u'management_multicast_subnet'}) => {
    "msg": "management_multicast_subnet: 239.1.1.0/28"

}
ok: [localhost] => (item={'value': u'192.168.204.0/28', 'key': u'management_subnet'}) => {
    "msg": "management_subnet: 192.168.204.0/28"
}
ok: [localhost] => (item={'value': u'172.16.0.0/16', 'key': u'cluster_pod_subnet'}) => {
    "msg": "cluster_pod_subnet: 172.16.0.0/16"
}
ok: [localhost] => (item={'value': u'10.10.10.3', 'key': u'external_oam_floating_address'}) => {
    "msg": "external_oam_floating_address: 10.10.10.3"
}

TASK [validate-config : Fail if cluster pod/service subnet size is too small (minimum size = 65536)] ***************************************************************************

TASK [validate-config : Fail if pxeboot/management/multicast subnet size is too small (minimum size = 16)] *********************************************************************

TASK [validate-config : Fail if the size of the remaining subnets is too small (minimum size = 8)] *****************************************************************************

TASK [validate-config : Generate warning if subnet prefix is not typical for Standard systems] *********************************************************************************
ok: [localhost] => (item=192.168.204.0/28) => {
    "msg": "WARNING: Subnet prefix of less than /24 is not typical. This will affect scaling of the system!"
}
ok: [localhost] => (item=239.1.1.0/28) => {
    "msg": "WARNING: Subnet prefix of less than /24 is not typical. This will affect scaling of the system!"
}

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Fail if IPv6 management on simplex] ********************************************************************************************************************

TASK [validate-config : Fail if IPv6 prefix length is too short] ***************************************************************************************************************

TASK [validate-config : Update localhost name ip mapping for IPv6] *************************************************************************************************************

TASK [validate-config : Fail if address allocation is misconfigured] ***********************************************************************************************************

TASK [validate-config : Set default start and end addresses based on provided subnets] *****************************************************************************************
ok: [localhost]

TASK [validate-config : Build address pairs for validation, merging default and user provided values] **************************************************************************
ok: [localhost]

TASK [validate-config : include] ***********************************************************************************************************************************************
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'oam_node', 'value': {u'use_
default': True, u'start': u'10.10.10.4', u'end': u'10.10.10.5', u'subnet': u'10.10.10.0/24'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'cluster_pod', 'value': {u'u
se_default': True, u'start': u'172.16.0.1', u'end': u'172.16.255.254', u'subnet': u'172.16.0.0/16'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'management', 'value': {u'us
e_default': True, u'start': u'192.168.204.2', u'end': u'192.168.204.14', u'subnet': u'192.168.204.0/28'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'multicast', 'value': {u'use
_default': True, u'start': u'239.1.1.1', u'end': u'239.1.1.14', u'subnet': u'239.1.1.0/28'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'cluster_service', 'value': 
{u'use_default': True, u'start': u'10.96.0.1', u'end': u'10.111.255.254', u'subnet': u'10.96.0.0/12'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'oam', 'value': {u'use_defau
lt': True, u'start': u'10.10.10.1', u'end': u'10.10.10.254', u'subnet': u'10.10.10.0/24'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'cluster_host', 'value': {u'
use_default': True, u'start': u'192.168.206.2', u'end': u'192.168.206.254', u'subnet': u'192.168.206.0/24'}})
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address_range.yml for localhost => (item={'key': u'pxeboot', 'value': {u'use_d
efault': True, u'start': u'169.254.202.2', u'end': u'169.254.202.254', u'subnet': u'169.254.202.0/24'}})

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate oam_node start and end address format] ********************************************************************************************************

TASK [validate-config : Validate oam_node start and end range] *****************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate cluster_pod start and end address format] *****************************************************************************************************

TASK [validate-config : Validate cluster_pod start and end range] **************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate management start and end address format] ******************************************************************************************************

TASK [validate-config : Validate management start and end range] ***************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate multicast start and end address format] *******************************************************************************************************


TASK [validate-config : Validate multicast start and end range] ****************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate cluster_service start and end address format] *************************************************************************************************

TASK [validate-config : Validate cluster_service start and end range] **********************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate oam start and end address format] *************************************************************************************************************

TASK [validate-config : Validate oam start and end range] **********************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate cluster_host start and end address format] ****************************************************************************************************

TASK [validate-config : Validate cluster_host start and end range] *************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate pxeboot start and end address format] *********************************************************************************************************

TASK [validate-config : Validate pxeboot start and end range] ******************************************************************************************************************

TASK [validate-config : Fail if address range did not meet required criteria] **************************************************************************************************

TASK [validate-config : Set floating addresses based on subnets or start addresses] ********************************************************************************************
ok: [localhost]

TASK [validate-config : Set derived facts for subsequent tasks/roles] **********************************************************************************************************
ok: [localhost]

                                                                                                                                                                      [683/1922]
TASK [validate-config : Set facts for IP address provisioning against loopback interface] **************************************************************************************
ok: [localhost]

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Set default no-proxy address list (non simplex)] *******************************************************************************************************
ok: [localhost]

TASK [validate-config : Validate http proxy urls] ******************************************************************************************************************************
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_url.yml for localhost => (item=http://proxy-chain.intel.com:911)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_url.yml for localhost => (item=http://proxy-chain.intel.com:912)

TASK [validate-config : Check if the supplied proxy is a valid URL] ************************************************************************************************************
changed: [localhost]

TASK [validate-config : Fail if proxy has the wrong format] ********************************************************************************************************************

TASK [validate-config : Check if the supplied proxy is a valid URL] ************************************************************************************************************
changed: [localhost]

TASK [validate-config : Fail if proxy has the wrong format] ********************************************************************************************************************

TASK [validate-config : Validate no proxy addresses] ***************************************************************************************************************************
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=localhost)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=127.0.0.1)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=192.168.204.2)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=192.168.204.3)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=192.168.204.4)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=10.10.10.3)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=10.10.10.4)
included: /usr/share/ansible/stx-ansible/playbooks/bootstrap/roles/validate-config/tasks/validate_address.yml for localhost => (item=10.10.10.5)

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

                                                                                                                                                                      [636/1922]
TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************


TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Check if the supplied address is a valid domain name or ipv4 address] **********************************************************************************
changed: [localhost]

TASK [validate-config : Check if the supplied address is of ipv6 with port format] *********************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6] ******************************************************************************************************

TASK [validate-config : Fail if the supplied address is not a valid ipv6 with port] ********************************************************************************************

TASK [validate-config : Add user defined no-proxy address list to default] *****************************************************************************************************
ok: [localhost]

TASK [validate-config : Fail if secure registry flag is misconfigured] *********************************************************************************************************

TASK [validate-config : Default the unified registry to secure if not specified] ***********************************************************************************************

TASK [validate-config : Turn on use_unified_registry flag] *********************************************************************************************************************

TASK [validate-config : Update use_default_registries flag] ********************************************************************************************************************

TASK [validate-config : include] ***********************************************************************************************************************************************

TASK [validate-config : set_fact] **********************************************************************************************************************************************
ok: [localhost]

TASK [validate-config : Check if images archive location exists] ***************************************************************************************************************

TASK [validate-config : Get list of archived files] ****************************************************************************************************************************

TASK [validate-config : Turn on images archive flag] ***************************************************************************************************************************

TASK [validate-config : Create config workdir] *********************************************************************************************************************************
changed: [localhost]

TASK [validate-config : Generate config ini file for python sysinv db population script] ***************************************************************************************
changed: [localhost] => (item=[BOOTSTRAP_CONFIG])
changed: [localhost] => (item=CONTROLLER_HOSTNAME=controller-0)
changed: [localhost] => (item=SYSTEM_TYPE=Standard)
changed: [localhost] => (item=SYSTEM_MODE=duplex)
changed: [localhost] => (item=TIMEZONE=UTC)
changed: [localhost] => (item=SW_VERSION=19.01)
changed: [localhost] => (item=NAMESERVERS=10.248.2.1,10.22.224.196)
changed: [localhost] => (item=PXEBOOT_SUBNET=169.254.202.0/24)
changed: [localhost] => (item=PXEBOOT_START_ADDRESS=169.254.202.2)
changed: [localhost] => (item=PXEBOOT_END_ADDRESS=169.254.202.254)
changed: [localhost] => (item=MANAGEMENT_SUBNET=192.168.204.0/28)
changed: [localhost] => (item=MANAGEMENT_START_ADDRESS=192.168.204.2)
changed: [localhost] => (item=MANAGEMENT_END_ADDRESS=192.168.204.14)
changed: [localhost] => (item=DYNAMIC_ADDRESS_ALLOCATION=True)
changed: [localhost] => (item=MANAGEMENT_INTERFACE=lo)
changed: [localhost] => (item=CONTROLLER_0_ADDRESS=192.168.204.3)
changed: [localhost] => (item=CLUSTER_HOST_SUBNET=192.168.206.0/24)
changed: [localhost] => (item=CLUSTER_HOST_START_ADDRESS=192.168.206.2)
changed: [localhost] => (item=CLUSTER_HOST_END_ADDRESS=192.168.206.254)
changed: [localhost] => (item=CLUSTER_POD_SUBNET=172.16.0.0/16)
changed: [localhost] => (item=CLUSTER_POD_START_ADDRESS=172.16.0.1)
changed: [localhost] => (item=CLUSTER_POD_END_ADDRESS=172.16.255.254)
changed: [localhost] => (item=CLUSTER_SERVICE_SUBNET=10.96.0.0/12)
changed: [localhost] => (item=CLUSTER_SERVICE_START_ADDRESS=10.96.0.1)
changed: [localhost] => (item=CLUSTER_SERVICE_END_ADDRESS=10.96.0.1)
changed: [localhost] => (item=EXTERNAL_OAM_SUBNET=10.10.10.0/24)
changed: [localhost] => (item=EXTERNAL_OAM_START_ADDRESS=10.10.10.1)
changed: [localhost] => (item=EXTERNAL_OAM_END_ADDRESS=10.10.10.254)
changed: [localhost] => (item=EXTERNAL_OAM_GATEWAY_ADDRESS=10.10.10.1)
changed: [localhost] => (item=EXTERNAL_OAM_FLOATING_ADDRESS=10.10.10.3)
changed: [localhost] => (item=EXTERNAL_OAM_0_ADDRESS=10.10.10.4)
changed: [localhost] => (item=EXTERNAL_OAM_1_ADDRESS=10.10.10.5)
changed: [localhost] => (item=MANAGEMENT_MULTICAST_SUBNET=239.1.1.0/28)
changed: [localhost] => (item=MANAGEMENT_MULTICAST_START_ADDRESS=239.1.1.1)
changed: [localhost] => (item=MANAGEMENT_MULTICAST_END_ADDRESS=239.1.1.14)
changed: [localhost] => (item=DOCKER_HTTP_PROXY=http://proxy-chain.intel.com:911)
changed: [localhost] => (item=DOCKER_HTTPS_PROXY=http://proxy-chain.intel.com:912)
changed: [localhost] => (item=DOCKER_NO_PROXY=localhost,127.0.0.1,192.168.204.2,192.168.204.3,10.10.10.3,10.10.10.4,192.168.204.4,10.10.10.5)
changed: [localhost] => (item=DOCKER_REGISTRIES=k8s.gcr.io,gcr.io,quay.io,docker.io)
changed: [localhost] => (item=USE_DEFAULT_REGISTRIES=True)
changed: [localhost] => (item=IS_SECURE_REGISTRY=True)
changed: [localhost] => (item=RECONFIGURE_ENDPOINTS=False)

TASK [validate-config : Write simplex flag] ************************************************************************************************************************************
changed: [localhost]

TASK [store-passwd : debug] ****************************************************************************************************************************************************

TASK [store-passwd : set_fact] *************************************************************************************************************************************************

TASK [store-passwd : Print warning if admin credentials are not stored in vault] ***********************************************************************************************
ok: [localhost] => {
    "msg": "[WARNING: Default admin username and password (unencrypted) are used. Consider storing both of these variables in Ansible vault.]"
}

TASK [store-passwd : Set admin username and password facts] ********************************************************************************************************************
ok: [localhost]

TASK [store-passwd : Look for password rules file] *****************************************************************************************************************************
ok: [localhost]

TASK [store-passwd : Fail if password rules file is missing] *******************************************************************************************************************

TASK [store-passwd : Get password rules] ***************************************************************************************************************************************
changed: [localhost]

TASK [store-passwd : Get password rules description] ***************************************************************************************************************************
changed: [localhost]

TASK [store-passwd : Set password regex facts] *********************************************************************************************************************************
ok: [localhost]

TASK [store-passwd : Fail if password regex cannot be found] *******************************************************************************************************************

TASK [store-passwd : Set password regex description fact] **********************************************************************************************************************

TASK [store-passwd : Validate admin password] **********************************************************************************************************************************
changed: [localhost]

TASK [store-passwd : Fail if provided admin password does not meet required complexity] ****************************************************************************************

TASK [store-passwd : Store admin password] *************************************************************************************************************************************
changed: [localhost]

TASK [apply-bootstrap-manifest : Create config workdir] ************************************************************************************************************************
changed: [localhost]

TASK [apply-bootstrap-manifest : Generating static config data] ****************************************************************************************************************
changed: [localhost]

TASK [apply-bootstrap-manifest : Fail if static hieradata cannot be generated] *************************************************************************************************

TASK [apply-bootstrap-manifest : Applying puppet bootstrap manifest] ***********************************************************************************************************
changed: [localhost]

TASK [apply-bootstrap-manifest : debug] ****************************************************************************************************************************************
ok: [localhost] => {
    "bootstrap_manifest": {
        "changed": true, 
        "cmd": [                                                                                                                                                      [452/1922]
            "/usr/local/bin/puppet-manifest-apply.sh", 
            "/tmp/hieradata", 
            "192.168.204.3", 
            "controller", 
            "ansible_bootstrap", 
            ">", 
            "/tmp/apply_manifest.log"
        ], 
        "delta": "0:02:13.051606", 
        "end": "2019-05-29 13:31:29.173395", 
        "failed": false, 
        "rc": 0, 
        "start": "2019-05-29 13:29:16.121789", 
        "stderr": "cp: cannot stat ‘/tmp/hieradata/192.168.204.3.yaml’: No such file or directory\ncp: cannot stat ‘/tmp/hieradata/system.yaml’: No such file or directory\ncp: 
cannot stat ‘/tmp/hieradata/secure_system.yaml’: No such file or directory\ncp: cannot stat ‘>’: No such file or directory", 
        "stderr_lines": [
            "cp: cannot stat ‘/tmp/hieradata/192.168.204.3.yaml’: No such file or directory", 
            "cp: cannot stat ‘/tmp/hieradata/system.yaml’: No such file or directory", 
            "cp: cannot stat ‘/tmp/hieradata/secure_system.yaml’: No such file or directory", 
            "cp: cannot stat ‘>’: No such file or directory"
        ], 
        "stdout": "Applying puppet ansible_bootstrap manifest...\n[DONE]", 
        "stdout_lines": [
            "Applying puppet ansible_bootstrap manifest...", 
            "[DONE]"
        ]
    }
}

TASK [apply-bootstrap-manifest : Fail if puppet manifest apply script returns an error] ****************************************************************************************

TASK [apply-bootstrap-manifest : Ensure Puppet directory exists] ***************************************************************************************************************
changed: [localhost]

TASK [apply-bootstrap-manifest : Persist puppet working files] *****************************************************************************************************************
changed: [localhost]

TASK [persist-config : Delete the previous python_keyring directory if exists] *************************************************************************************************
ok: [localhost]

TASK [persist-config : Persist keyring data] ***********************************************************************************************************************************
changed: [localhost]

TASK [persist-config : Ensure replicated config parent directory exists] *******************************************************************************************************
changed: [localhost]

TASK [persist-config : Get list of new config files] ***************************************************************************************************************************
ok: [localhost]

TASK [persist-config : Remove existing config files from permanent location] ***************************************************************************************************
ok: [localhost] => (item={u'rusr': True, u'uid': 0, u'rgrp': True, u'xoth': False, u'islnk': False, u'woth': False, u'nlink': 1, u'issock': False, u'mtime': 1559136552.668, u'g
r_name': u'root', u'path': u'/tmp/config/bootstrap_config', u'xusr': False, u'atime': 1559136552.668, u'inode': 80565, u'isgid': False, u'size': 1545, u'isdir': False, u'ctime'
: 1559136552.668, u'roth': True, u'isblk': False, u'xgrp': False, u'isuid': False, u'dev': 34, u'wgrp': False, u'isreg': True, u'isfifo': False, u'mode': u'0644', u'pw_name': u
'root', u'gid': 0, u'ischr': False, u'wusr': True})
ok: [localhost] => (item={u'rusr': True, u'uid': 0, u'rgrp': True, u'xoth': True, u'islnk': False, u'woth': False, u'nlink': 2, u'issock': False, u'mtime': 1559136555.338, u'gr
_name': u'root', u'path': u'/tmp/config/ssh_config', u'xusr': True, u'atime': 1559136555.272, u'inode': 83007, u'isgid': False, u'size': 120, u'isdir': True, u'ctime': 15591365
55.338, u'roth': True, u'isblk': False, u'xgrp': True, u'isuid': False, u'dev': 34, u'wgrp': False, u'isreg': False, u'isfifo': False, u'mode': u'0755', u'pw_name': u'root', u'
gid': 0, u'ischr': False, u'wusr': True})

TASK [persist-config : Move new config files to permanent location] ************************************************************************************************************
changed: [localhost]

TASK [persist-config : Delete working config directory] ************************************************************************************************************************
changed: [localhost]

TASK [persist-config : Set Postgres, PXE, branding config directory fact] ******************************************************************************************************
ok: [localhost]

TASK [persist-config : debug] **************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "postgres_config_dir: /opt/platform/config/19.01/postgresql pxe_config_dir: /opt/platform/config/19.01/pxelinux.cfg branding_config_dir: /opt/platform/config/19.01/p
xelinux.cfg"
}

TASK [persist-config : Ensure Postres, PXE config directories exist] ***********************************************************************************************************
changed: [localhost] => (item=/opt/platform/config/19.01/postgresql)
changed: [localhost] => (item=/opt/platform/config/19.01/pxelinux.cfg)

TASK [persist-config : Get list of Postgres conf files] ************************************************************************************************************************
ok: [localhost]

TASK [persist-config : Copy postgres conf files for mate] **********************************************************************************************************************
changed: [localhost] => (item={u'rusr': True, u'uid': 120, u'rgrp': True, u'xoth': False, u'islnk': False, u'woth': False, u'nlink': 1, u'issock': False, u'mtime': 1559136606.4
78, u'gr_name': u'postgres', u'path': u'/etc/postgresql/pg_hba.conf', u'xusr': False, u'atime': 1559136606.669, u'inode': 669951, u'isgid': False, u'size': 929, u'isdir': False
, u'ctime': 1559136606.481, u'roth': False, u'isblk': False, u'xgrp': False, u'isuid': False, u'dev': 2051, u'wgrp': False, u'isreg': True, u'isfifo': False, u'mode': u'0640', 
u'pw_name': u'postgres', u'gid': 120, u'ischr': False, u'wusr': True})
changed: [localhost] => (item={u'rusr': True, u'uid': 120, u'rgrp': True, u'xoth': False, u'islnk': False, u'woth': False, u'nlink': 1, u'issock': False, u'mtime': 1559136606.4
64, u'gr_name': u'postgres', u'path': u'/etc/postgresql/pg_ident.conf', u'xusr': False, u'atime': 1559136606.669, u'inode': 669952, u'isgid': False, u'size': 47, u'isdir': Fals
e, u'ctime': 1559136606.467, u'roth': False, u'isblk': False, u'xgrp': False, u'isuid': False, u'dev': 2051, u'wgrp': False, u'isreg': True, u'isfifo': False, u'mode': u'0640',
 u'pw_name': u'postgres', u'gid': 120, u'ischr': False, u'wusr': True})
changed: [localhost] => (item={u'rusr': True, u'uid': 120, u'rgrp': False, u'xoth': False, u'islnk': False, u'woth': False, u'nlink': 1, u'issock': False, u'mtime': 1559136606.
473, u'gr_name': u'postgres', u'path': u'/etc/postgresql/postgresql.conf', u'xusr': False, u'atime': 1559136606.657, u'inode': 669945, u'isgid': False, u'size': 20195, u'isdir'
: False, u'ctime': 1559136606.473, u'roth': False, u'isblk': False, u'xgrp': False, u'isuid': False, u'dev': 2051, u'wgrp': False, u'isreg': True, u'isfifo': False, u'mode': u'
0600', u'pw_name': u'postgres', u'gid': 120, u'ischr': False, u'wusr': True})

TASK [persist-config : Create a symlink to PXE config files] *******************************************************************************************************************
changed: [localhost]

TASK [persist-config : Check if copying of branding files for mate is required] ************************************************************************************************
ok: [localhost]

TASK [persist-config : Ensure branding config directory exists] ****************************************************************************************************************
changed: [localhost]

TASK [persist-config : Check if horizon-region-exclusion.csv file exists] ******************************************************************************************************
ok: [localhost]

TASK [persist-config : Copy horizon-region-exclusions.csv if exists] ***********************************************************************************************************
changed: [localhost]

TASK [persist-config : Check if branding tar files exist (there should be only one)] *******************************************************************************************
ok: [localhost]

TASK [persist-config : Copy branding tar files] ********************************************************************************************************************************

TASK [persist-config : Get grub default kernel] ********************************************************************************************************************************
changed: [localhost]

TASK [persist-config : Add default security feature to kernel parameters] ******************************************************************************************************
changed: [localhost] => (item=grubby --update-kernel=/boot/vmlinuz-3.10.0-957.1.3.el7.1.tis.x86_64 --args='nopti nospectre_v2')
changed: [localhost] => (item=grubby --efi --update-kernel=/boot/vmlinuz-3.10.0-957.1.3.el7.1.tis.x86_64 --args='nopti nospectre_v2')

TASK [persist-config : Resize filesystems (default)] ***************************************************************************************************************************
changed: [localhost] => (item=lvextend -L20G /dev/cgts-vg/pgsql-lv)
changed: [localhost] => (item=lvextend -L10G /dev/cgts-vg/cgcs-lv)
changed: [localhost] => (item=lvextend -L16G /dev/cgts-vg/dockerdistribution-lv)
changed: [localhost] => (item=lvextend -L40G /dev/cgts-vg/backup-lv)
changed: [localhost] => (item=drbdadm -- --assume-peer-has-space resize all)
changed: [localhost] => (item=resize2fs /dev/drbd0)
changed: [localhost] => (item=resize2fs /dev/drbd3)
changed: [localhost] => (item=resize2fs /dev/drbd8)

TASK [persist-config : Further resize if root disk size is larger than 240G] ***************************************************************************************************
changed: [localhost] => (item=lvextend -L40G /dev/cgts-vg/pgsql-lv)
changed: [localhost] => (item=lvextend -L20G /dev/cgts-vg/cgcs-lv)
changed: [localhost] => (item=lvextend -L50G /dev/cgts-vg/backup-lv)
changed: [localhost] => (item=drbdadm -- --assume-peer-has-space resize all)
changed: [localhost] => (item=resize2fs /dev/drbd0)
changed: [localhost] => (item=resize2fs /dev/drbd3)

TASK [persist-config : Set input parameters to populate config script] *********************************************************************************************************
ok: [localhost]

TASK [persist-config : Update input parameters with reconfigure system flag] ***************************************************************************************************

TASK [persist-config : Update input parameters with reconfigure network flag] **************************************************************************************************

TASK [persist-config : Update input parameters with reconfigure service flag] **************************************************************************************************

TASK [persist-config : Update input parameters if config from previous play is missing] ****************************************************************************************

TASK [persist-config : debug] **************************************************************************************************************************************************
ok: [localhost] => {
    "script_input": "/opt/platform/config/19.01/bootstrap_config"
}

TASK [persist-config : Shutdown Maintenance services] **************************************************************************************************************************

TASK [persist-config : Shutdown FM services] ***********************************************************************************************************************************

TASK [persist-config : Shut down and remove Kubernetes components] *************************************************************************************************************

TASK [persist-config : Clear etcd data cache] **********************************************************************************************************************************

TASK [persist-config : Restart etcd] *******************************************************************************************************************************************

TASK [persist-config : Set facts derived from previous network configurations] *************************************************************************************************

TASK [persist-config : Set facts derived from previous floating addresses] *****************************************************************************************************

TASK [persist-config : Set facts for the removal of addresses assigned to loopback interface] **********************************************************************************

TASK [persist-config : Remove loopback interface in sysinv db and associated addresses] ****************************************************************************************

TASK [persist-config : Remove the .config_applied flag from previous run before reconfiguring service endpoints] ***************************************************************

TASK [persist-config : Add the new management address for service endpoints reconfiguration] ***********************************************************************************

TASK [persist-config : Saving config in sysinv database] ***********************************************************************************************************************
changed: [localhost]

TASK [persist-config : debug] **************************************************************************************************************************************************
ok: [localhost] => {
    "populate_result": {
        "changed": true, 
        "failed": false, 
        "failed_when_result": false, 
        "rc": 0, 
        "stderr": "", 
        "stderr_lines": [], 
        "stdout": "Populating system config...\nPopulating load config...\nPopulating management network...\nPopulating pxeboot network...\nPopulating oam network...\nPopulatin
g multicast network...\nPopulating cluster host network...\nPopulating cluster pod network...\nPopulating cluster service network...\nNetwork config completed.\nPopulating DNS 
config...\nPopulating docker config...\nDocker proxy config completed.\nManagement mac = 00:00:00:00:00:00\nRoot fs device = /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0\nBoot de
vice = /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0\nConsole = tty0\nTboot = false\nInstall output = text\nHost values = {'tboot': 'false', 'install_output': 'text', 'rootfs_devi
ce': '/dev/disk/by-path/pci-0000:00:1f.2-ata-1.0', 'boot_device': '/dev/disk/by-path/pci-0000:00:1f.2-ata-1.0', 'availability': 'offline', 'mgmt_mac': '00:00:00:00:00:00', 'con
sole': 'tty0', 'mgmt_ip': '192.168.204.3', 'hostname': 'controller-0', 'operational': 'disabled', 'invprovision': 'provisioned', 'administrative': 'locked', 'personality': 'con
troller'}\nPopulating ceph-mon config for controller-0...\nPopulating ceph storage backend config...\nSuccessfully updated the initial system config.\n", 
        "stdout_lines": [
            "Populating system config...", 
            "Populating load config...", 
            "Populating management network...", 
            "Populating pxeboot network...", 
            "Populating oam network...", 
            "Populating multicast network...", 
            "Populating cluster host network...", 
            "Populating cluster pod network...", 
            "Populating cluster service network...", 
            "Network config completed.", 
            "Populating DNS config...", 
            "Populating docker config...", 
            "Docker proxy config completed.", 
            "Management mac = 00:00:00:00:00:00", 
            "Root fs device = /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0", 
            "Boot device = /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0", 
            "Console = tty0", 
            "Tboot = false", 
            "Install output = text", 
            "Host values = {'tboot': 'false', 'install_output': 'text', 'rootfs_device': '/dev/disk/by-path/pci-0000:00:1f.2-ata-1.0', 'boot_device': '/dev/disk/by-path/pci-000
0:00:1f.2-ata-1.0', 'availability': 'offline', 'mgmt_mac': '00:00:00:00:00:00', 'console': 'tty0', 'mgmt_ip': '192.168.204.3', 'hostname': 'controller-0', 'operational': 'disab
led', 'invprovision': 'provisioned', 'administrative': 'locked', 'personality': 'controller'}", 
            "Populating ceph-mon config for controller-0...", 
            "Populating ceph storage backend config...", 
            "Successfully updated the initial system config."
        ]
    }
}

TASK [persist-config : Fail if populate config script throws an exception] *****************************************************************************************************

TASK [persist-config : Wait for service endpoints reconfiguration to complete] *************************************************************************************************

TASK [persist-config : Update sysinv API bind host with new management floating IP] ********************************************************************************************

TASK [persist-config : Restart sysinv-agent and sysinv-api to pick up sysinv.conf update] **************************************************************************************

TASK [persist-config : Ensure docker config directory exists] ******************************************************************************************************************
ok: [localhost]

TASK [persist-config : Ensure docker proxy config exists] **********************************************************************************************************************
changed: [localhost]

TASK [persist-config : Write header to docker proxy conf file] *****************************************************************************************************************
changed: [localhost]

TASK [persist-config : Add http proxy URL to docker proxy conf file] ***********************************************************************************************************
changed: [localhost]

TASK [persist-config : Add https proxy URL to docker proxy conf file] **********************************************************************************************************
changed: [localhost]

TASK [persist-config : Add no proxy address list to docker proxy config file] **************************************************************************************************
changed: [localhost]

TASK [persist-config : Restart Docker] *****************************************************************************************************************************************
changed: [localhost]

TASK [persist-config : Set pxeboot files source if address allocation is dynamic] **********************************************************************************************
ok: [localhost]

TASK [persist-config : Set pxeboot files source if address allocation is static] ***********************************************************************************************

TASK [persist-config : Set pxeboot files symlinks] *****************************************************************************************************************************
changed: [localhost] => (item={u'dest': u'pxelinux.cfg/default', u'src': u'pxelinux.cfg.files/default'})
changed: [localhost] => (item={u'dest': u'pxelinux.cfg/grub.cfg', u'src': u'pxelinux.cfg.files/grub.cfg'})

TASK [persist-config : Update the management_interface in platform.conf] *******************************************************************************************************
changed: [localhost]

TASK [persist-config : Add new entries to platform.conf] ***********************************************************************************************************************
ok: [localhost] => (item=region_config=no)
changed: [localhost] => (item=system_mode=simplex)
ok: [localhost] => (item=sw_version=19.01)
ok: [localhost] => (item=vswitch_type=none)

TASK [persist-config : Update resolv.conf with list of dns servers] ************************************************************************************************************
ok: [localhost] => (item=10.248.2.1)
ok: [localhost] => (item=10.22.224.196)

TASK [persist-config : Remove localhost address from resolv.conf] **************************************************************************************************************
changed: [localhost]                                                                                                                                                  [172/1922]

TASK [bringup-essential-services : Add loopback interface] *********************************************************************************************************************
changed: [localhost] => (item=source /etc/platform/openrc; system host-if-add controller-0 lo virtual none lo -c platform --networks mgmt -m 1500)
changed: [localhost] => (item=source /etc/platform/openrc; system host-if-modify controller-0 -c platform --networks cluster-host lo)
changed: [localhost] => (item=ip addr add 192.168.206.3/24  brd 192.168.206.255 dev lo scope host label lo:5)
changed: [localhost] => (item=ip addr add 192.168.204.3/28 brd 192.168.204.15 dev lo scope host label lo:1)
changed: [localhost] => (item=ip addr add 169.254.202.2/24 dev lo scope host)
changed: [localhost] => (item=ip addr add 192.168.206.2/24 dev lo scope host)
changed: [localhost] => (item=ip addr add 192.168.204.5/28 dev lo scope host)
changed: [localhost] => (item=ip addr add 192.168.204.6/28 dev lo scope host)

TASK [bringup-essential-services : Add management floating adddress if this is the initial play] *******************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Remove previous management floating address if management network config has changed] *******************************************************

TASK [bringup-essential-services : Remove existing /etc/hosts] *****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Populate /etc/hosts] ************************************************************************************************************************
changed: [localhost] => (item=127.0.0.1 localhost       localhost.localdomain localhost4 localhost4.localdomain4)
changed: [localhost] => (item=192.168.204.2     controller)
changed: [localhost] => (item=192.168.206.3     controller-0-infra)
changed: [localhost] => (item=169.254.202.2     pxecontroller)
changed: [localhost] => (item=10.10.10.3        oamcontroller)
changed: [localhost] => (item=192.168.204.5     controller-platform-nfs)
changed: [localhost] => (item=192.168.204.4     controller-1)
changed: [localhost] => (item=192.168.204.3     controller-0)
changed: [localhost] => (item=192.168.206.4     controller-1-infra)
changed: [localhost] => (item=192.168.204.6     controller-nfs)

TASK [bringup-essential-services : Save hosts file to permanent location] ******************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update name service caching server] *********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set default directory for image files copy] *************************************************************************************************

TASK [bringup-essential-services : Copy Docker images to remote host] **********************************************************************************************************

TASK [bringup-essential-services : Adjust the images directory fact for local host] ********************************************************************************************

TASK [bringup-essential-services : Get list of archived files] *****************************************************************************************************************

TASK [bringup-essential-services : Load system images] *************************************************************************************************************************
TASK [bringup-essential-services : Setup iptables for Kubernetes] **************************************************************************************************************
changed: [localhost] => (item=net.bridge.bridge-nf-call-ip6tables = 1)
changed: [localhost] => (item=net.bridge.bridge-nf-call-iptables = 1)

TASK [bringup-essential-services : Create daemon.json file for insecure registry] **********************************************************************************************

TASK [bringup-essential-services : Update daemon.json with registry IP] ********************************************************************************************************

TASK [bringup-essential-services : Restart docker] *****************************************************************************************************************************

TASK [bringup-essential-services : Update kernel parameters for iptables] ******************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create manifests directory required by kubelet] *********************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create kubelet cgroup for minimal set] ******************************************************************************************************
changed: [localhost] => (item=cpuset)
changed: [localhost] => (item=cpu)
changed: [localhost] => (item=cpuacct)
changed: [localhost] => (item=memory)
changed: [localhost] => (item=systemd)

TASK [bringup-essential-services : Get default k8s cpuset] *********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Get default k8s nodeset] ********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set mems for cpuset controller] *************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set cpus for cpuset controller] *************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create a tasks file for cpuset controller] **************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Enable kubelet] *****************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create Kube admin yaml] *********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update Kube admin yaml with network info] ***************************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @apiserver_advertise_address %>|'$CLUSTER_IP'|g' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @etcd_endpoint %>|'http://"$CLUSTER_IP":$ETCD_PORT'|g' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @service_domain %>|'cluster.local'|g' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @pod_network_cidr %>|'$POD_NETWORK_CIDR'|g' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @service_network_cidr %>|'$SERVICE_NETWORK_CIDR'|g' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i '/<%- /d' /etc/kubernetes/kubeadm.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @k8s_registry %>|'$K8S_REGISTRY'|g' /etc/kubernetes/kubeadm.yaml)

TASK [bringup-essential-services : Update image repo in admin yaml if unified registry is used] ********************************************************************************

TASK [bringup-essential-services : Initializing Kubernetes master] *************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update kube admin.conf file mode and owner] *************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set up k8s environment variable] ************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create Multus config file] ******************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update Multus config file] ******************************************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @docker_registry %>|'$DOCKER_REGISTRY'|g' /etc/kubernetes/multus.yaml)

TASK [bringup-essential-services : Update Multus yaml file with new registry info if unified registry is used] *****************************************************************

TASK [bringup-essential-services : Activate Multus Networking] *****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create Calico config file] ******************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update Calico config files with networking info] ********************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @apiserver_advertise_address %>|'$CLUSTER_IP'|g' /etc/kubernetes/calico.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @pod_network_cidr %>|'$POD_NETWORK_CIDR'|g' /etc/kubernetes/calico.yaml)
changed: [localhost] => (item=sed -i -e 's|<%= @quay_registry %>|'$QUAY_REGISTRY'|g' /etc/kubernetes/calico.yaml)

TASK [bringup-essential-services : Update Calico yaml file with new registry info if unified registry is used] *****************************************************************

TASK [bringup-essential-services : Activate Calico Networking] *****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create SRIOV Networking config file] ********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update SRIOV Networking config file] ********************************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @docker_registry %>|'$DOCKER_REGISTRY'|g' /etc/kubernetes/sriov-cni.yaml)
                                                                                                                                                                       [31/1922]
TASK [bringup-essential-services : Update SRIOV Networking yaml file with new registry info if unified registry is used] *******************************************************

TASK [bringup-essential-services : Activate SRIOV Networking] ******************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create SRIOV device plugin config file] *****************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update SRIOV device plugin config file] *****************************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @docker_registry %>|'$DOCKER_REGISTRY'|g' /etc/kubernetes/sriovdp-daemonset.yaml)

TASK [bringup-essential-services : Update SRIOV device plugin yaml file with new registry info if unified registry is used] ****************************************************

TASK [bringup-essential-services : Activate SRIOV device plugin] ***************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restrict coredns to master node] ************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Use anti-affinity for coredns pods] *********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Remove taint from master node] **************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Add kubelet service override] ***************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Register kubelet with pmond] ****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Reload systemd] *****************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Mark Kubernetes config complete] ************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create www group] ***************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create www user in preparation for Helm bringup] ********************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Ensure /www/tmp exists] *********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Ensure /www/var exists] *********************************************************************************************************************
TASK [bringup-essential-services : Ensure /www/tmp exists] *********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Ensure /www/var exists] *********************************************************************************************************************
changed: [localhost] => (item=/www/var)
changed: [localhost] => (item=/www/var/log)

TASK [bringup-essential-services : Set up lighttpd.conf] ***********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update lighttpd.conf] ***********************************************************************************************************************
changed: [localhost] => (item=sed -i -e 's|<%= @http_port %>|'$PORT_NUM'|g' /etc/lighttpd/lighttpd.conf)
changed: [localhost] => (item=sed -i '/@enable_https/,/% else/d' /etc/lighttpd/lighttpd.conf)
changed: [localhost] => (item=sed -i '/@tmp_object/,/%- end/d' /etc/lighttpd/lighttpd.conf)
changed: [localhost] => (item=sed -i '/<% end/d' /etc/lighttpd/lighttpd.conf)
changed: [localhost] => (item=sed -i '/@tpm_object/,/%- end/d' /etc/lighttpd/lighttpd.conf)

TASK [bringup-essential-services : Set up lighttpd-inc.conf] *******************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update management subnet in lighttpd-inc.conf] **********************************************************************************************
 [WARNING]: Module remote_tmp /tmp/.ansible-root/tmp did not exist and was created with a mode of 0700, this may cause issues when running as another user. To avoid this,
create the remote_tmp dir with the correct permissions manually

changed: [localhost]

TASK [bringup-essential-services : Update pxe subnet in lighttp-inc.conf] ******************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update tiller image tag if using unified registry] ******************************************************************************************

TASK [bringup-essential-services : Pull Tiller and Armada images] **************************************************************************************************************
changed: [localhost] => (item=gcr.io/kubernetes-helm/tiller:v2.13.1)
changed: [localhost] => (item=quay.io/airshipit/armada:af8a9ffd0873c2fbc915794e235dbd357f2adab1)

TASK [bringup-essential-services : Create source and target helm bind directories] *********************************************************************************************
changed: [localhost] => (item=/opt/cgcs/helm_charts)
changed: [localhost] => (item=/www/pages/helm_charts)

TASK [bringup-essential-services : Create helm repository directories] *********************************************************************************************************
changed: [localhost] => (item=/opt/cgcs/helm_charts/starlingx)
changed: [localhost] => (item=/opt/cgcs/helm_charts/stx-platform)

TASK [bringup-essential-services : Create service account for Tiller] **********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Create cluster role binding for Tiller service account] *************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Initialize Helm (local host)] ***************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Initialize Helm (remote host)] **************************************************************************************************************

TASK [bringup-essential-services : Change helm directory ownership (remote host)] **********************************************************************************************

TASK [bringup-essential-services : Generate Helm repo indicies] ****************************************************************************************************************
changed: [localhost] => (item=starlingx)
changed: [localhost] => (item=stx-platform)

TASK [bringup-essential-services : Stop lighttpd] ******************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Disable lighttpd] ***************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Bind mount on /www/pages/helm_charts] *******************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Enable lighttpd] ****************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restart lighttpd for Helm] ******************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Add Helm repos (local host)] ****************************************************************************************************************
changed: [localhost] => (item=starlingx)
changed: [localhost] => (item=stx-platform)

TASK [bringup-essential-services : Add Helm repos (remote host)] ***************************************************************************************************************

TASK [bringup-essential-services : Change helm directory ownership to pick up newly generated files (remote host)] *************************************************************

TASK [bringup-essential-services : Update info of available charts locally from chart repos] ***********************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update info of available charts locally from chart repos (remote host)] *********************************************************************

TASK [bringup-essential-services : Change helm directory ownership (remote host)] **********************************************************************************************

TASK [bringup-essential-services : Generate cnf file from template] ************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update cnf file with network info] **********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Generate certificate and key files] *********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Generate pkcs1 key file] ********************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Remove extfile used in certificate generation] **********************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set certificate file and key permissions to root read-only] *********************************************************************************
changed: [localhost] => (item=/etc/ssl/private/registry-cert.key)
changed: [localhost] => (item=/etc/ssl/private/registry-cert.crt)
changed: [localhost] => (item=/etc/ssl/private/registry-cert-pkcs1.key)
                                                                                                                                                                       [46/1891]
TASK [bringup-essential-services : Copy certificate and keys to shared filesystem for mate] ************************************************************************************
changed: [localhost] => (item=/etc/ssl/private/registry-cert.key)
changed: [localhost] => (item=/etc/ssl/private/registry-cert.crt)
changed: [localhost] => (item=/etc/ssl/private/registry-cert-pkcs1.key)

TASK [bringup-essential-services : Create docker certificate directory] ********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Copy certificate file to docker certificate directory] **************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Wait for service endpoints reconfiguration to complete] *************************************************************************************
ok: [localhost]

TASK [bringup-essential-services : Update sysinv API bind host with management floating IP] ************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restart sysinv-agent and sysinv-api] ********************************************************************************************************
changed: [localhost] => (item=/etc/init.d/sysinv-agent restart)
changed: [localhost] => (item=/usr/lib/ocf/resource.d/platform/sysinv-api reload)

TASK [bringup-essential-services : Update barbican bind host with management floating IP] **************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restart barbican] ***************************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Apply workaround for fm-api] ****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Update bind_host config parameter in fm config file] ****************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restart FM API and bring up FM Manager] *****************************************************************************************************
changed: [localhost] => (item=/etc/init.d/fm-api restart)
changed: [localhost] => (item=/etc/init.d/fminit start)

TASK [bringup-essential-services : Bring up Maintenance Agent] *****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Restart Maintenance Client] *****************************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Set dnsmasq.leases flag for unlock] *********************************************************************************************************
changed: [localhost]

                                                                                                                                                                        [0/1891]
TASK [bringup-essential-services : Update resolv.conf file for unlock] *********************************************************************************************************
changed: [localhost]

TASK [bringup-essential-services : Remove config file from previous play] ******************************************************************************************************
ok: [localhost]

TASK [bringup-essential-services : Save the current system and network config for reference in subsequent replays] *************************************************************
changed: [localhost] => (item=prev_system_mode: duplex)
changed: [localhost] => (item=prev_timezone: UTC)
changed: [localhost] => (item=prev_dynamic_address_allocation: True)
changed: [localhost] => (item=prev_pxeboot_subnet: 169.254.202.0/24)
changed: [localhost] => (item=prev_management_subnet: 192.168.204.0/28)
changed: [localhost] => (item=prev_cluster_host_subnet: 192.168.206.0/24)
changed: [localhost] => (item=prev_cluster_pod_subnet: 172.16.0.0/16)
changed: [localhost] => (item=prev_cluster_service_subnet: 10.96.0.0/12)
changed: [localhost] => (item=prev_external_oam_subnet: 10.10.10.0/24)
changed: [localhost] => (item=prev_external_oam_gateway_address: 10.10.10.1)
changed: [localhost] => (item=prev_external_oam_floating_address: 10.10.10.3)
changed: [localhost] => (item=prev_management_multicast_subnet: 239.1.1.0/28)
changed: [localhost] => (item=prev_dns_servers: 10.248.2.1,10.22.224.196)
changed: [localhost] => (item=prev_docker_registries: k8s.gcr.io,gcr.io,quay.io,docker.io)
changed: [localhost] => (item=prev_docker_http_proxy: http://proxy-chain.intel.com:911)
changed: [localhost] => (item=prev_docker_https_proxy: http://proxy-chain.intel.com:912)
changed: [localhost] => (item=prev_docker_no_proxy: localhost,127.0.0.1,192.168.204.2,192.168.204.3,192.168.204.4,10.10.10.3,10.10.10.4,10.10.10.5)
changed: [localhost] => (item=prev_admin_username: d033e22ae348aeb5660fc2140aec35850c4da997)
changed: [localhost] => (item=prev_admin_password: 619e17c6392297cf2a08dcf5ebd62114e376ea12)
changed: [localhost] => (item=prev_pxeboot_start_address: derived)
changed: [localhost] => (item=prev_pxeboot_end_address: derived)
changed: [localhost] => (item=prev_management_start_address: derived)
changed: [localhost] => (item=prev_management_end_address: derived)
changed: [localhost] => (item=prev_cluster_host_start_address: derived)
changed: [localhost] => (item=prev_cluster_host_end_address: derived)
changed: [localhost] => (item=prev_cluster_pod_start_address: derived)
changed: [localhost] => (item=prev_cluster_pod_end_address: derived)
changed: [localhost] => (item=prev_cluster_service_start_address: derived)
changed: [localhost] => (item=prev_cluster_service_end_address:  derived)
changed: [localhost] => (item=prev_external_oam_start_address: derived)
changed: [localhost] => (item=prev_external_oam_end_address: derived)
changed: [localhost] => (item=prev_management_multicast_start_address: derived)
changed: [localhost] => (item=prev_management_multicast_end_address: derived)
changed: [localhost] => (item=prev_external_oam_node_0_address: derived)
changed: [localhost] => (item=prev_external_oam_node_1_address: derived)

PLAY RECAP *********************************************************************************************************************************************************************
localhost                  : ok=225  changed=139  unreachable=0    failed=0   
```

### Provisioning Controller-0

#### Configure OAM, Management and Cluster interfaces

```sh
controller-0:~$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ OAM_IF=enp2s1
[wrsroot@controller-0 ~(keystone_admin)]$ MGMT_IF=enp2s2
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 lo -c none
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | lo                                   |
| iftype       | virtual                              |
| ports        | []                                   |
| datanetworks | []                                   |
| imac         | 00:00:00:00:00:00                    |
| imtu         | 1500                                 |
| ifclass      | None                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 5b06a590-f731-4307-8cc1-1ce20084763f |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:37:15.412532+00:00     |
| updated_at   | 2019-05-29T08:47:33.759353+00:00     |
| sriov_numvfs | 0                                    |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $OAM_IF --networks oam -c platform

+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s1                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s1']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:f6:f5:3a                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | oam                                  |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 231c2c43-833f-4125-bc11-1e18a89d1ea1 |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.591815+00:00     |
| updated_at   | 2019-05-29T08:48:13.362198+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $MGMT_IF -c platform --networks mgmt
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s2                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:36:bc:90                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | mgmt                                 |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | cef6994a-5033-42ad-a4f4-09645347c73e |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.714315+00:00     |
| updated_at   | 2019-05-29T08:48:15.564040+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $MGMT_IF -c platform --networks cluster-host
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s2                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:36:bc:90                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | cef6994a-5033-42ad-a4f4-09645347c73e |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.714315+00:00     |
| updated_at   | 2019-05-29T08:48:18.168298+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

#### Set the ntp server

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ntp-modify ntpservers=0.pool.ntp.org,1.pool.ntp.org
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| uuid         | 257f46a1-c315-43de-8c09-b08155dcb69b |
| enabled      | True                                 |
| ntpservers   | 0.pool.ntp.org,1.pool.ntp.org        |
| isystem_uuid | d046949c-e16e-4475-bdee-f420a430fb00 |
| created_at   | 2019-05-29T08:34:05.340438+00:00     |
| updated_at   | None                                 |
+--------------+--------------------------------------+
```

#### Configure the vswitch type (optional)

> Not done under a virtual installation.


#### Prepare the host for running the containerized services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | d43c36fa-8f5a-4e50-ba8c-d54b894d8c01 |
| host_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

#### Unlock Controller-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-0
+---------------------+--------------------------------------------+
| Property            | Value                                      |
+---------------------+--------------------------------------------+
| action              | none                                       |
| administrative      | locked                                     |
| availability        | online                                     |
| bm_ip               | None                                       |
| bm_type             | None                                       |
| bm_username         | None                                       |
| boot_device         | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| capabilities        | {u'stor_function': u'monitor'}             |
| config_applied      | 41d00c71-4a6d-4ae3-8db8-0f153060251d       |
| config_status       | None                                       |
| config_target       | 8ababfce-3309-4745-96ad-feb7167c99f8       |
| console             | tty0                                       |
| created_at          | 2019-05-29T08:35:18.384310+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:36:bc:90                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-05-29T08:50:08.683148+00:00           |
| uptime              | 1966                                       |
| uuid                | b0f5e498-3e83-4c3e-9404-0dac97913b8a       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

```sh
[wrsroot@localhost ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

## Install remaining hosts

```sh
user@workstation:~/stx-tools/deployment/libvirt$ ssh wrsroot@10.10.10.3                                                                                                         
The authenticity of host '10.10.10.3 (10.10.10.3)' can't be established.
ECDSA key fingerprint is SHA256:zmpmzT51KHw5VgFnH4J5fwAi5Q1hsYv2QSu03pKudyY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.10.3' (ECDSA) to the list of known hosts.
Release 19.01
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

wrsroot@10.10.10.3's password: 
Last login: Wed May 29 08:47:20 2019 from 10.10.10.1
/etc/motd.d/00-header:

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ system host-list
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

### PXE boot hosts

Power on the nodes... see procedure below.

### Configure host personalities

Power on controller-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 2 personality=controller
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:05:53.758084+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:af:f3:e9                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | ba798c74-978c-40bf-8530-ba638e5a3021 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Power on storage-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 3 personality=storage 
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:07:10.797882+00:00     |
| hostname            | storage-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.9                        |
| mgmt_mac            | 52:54:00:d2:e5:5b                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 62acd311-72fa-4535-b542-c5419c0df36a |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Power on storage-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | storage-0    | storage     | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 4 personality=storage  
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:08:09.929773+00:00     |
| hostname            | storage-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.14                       |
| mgmt_mac            | 52:54:00:7b:80:cd                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | d472b007-f9d0-4947-bff0-6484ab7fa40e |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Power on compute-0

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | storage-0    | storage     | locked         | disabled    | offline      |
| 4  | storage-1    | storage     | locked         | disabled    | offline      |
| 5  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 5 personality=worker hostname=compute-0
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:09:15.968293+00:00     |
| hostname            | compute-0                            |
| id                  | 5                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.10                       |
| mgmt_mac            | 52:54:00:19:e2:ea                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

Power on compute-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | storage-0    | storage     | locked         | disabled    | offline      |
| 4  | storage-1    | storage     | locked         | disabled    | offline      |
| 5  | compute-0    | worker      | locked         | disabled    | offline      |
| 6  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 6 personality=worker hostname=compute-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:10:23.228399+00:00     |
| hostname            | compute-1                            |
| id                  | 6                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.8                        |
| mgmt_mac            | 52:54:00:bb:9b:06                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

### Wait for hosts to become online

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | online       |
| 3  | storage-0    | storage     | locked         | disabled    | online       |
| 4  | storage-1    | storage     | locked         | disabled    | online       |
| 5  | compute-0    | worker      | locked         | disabled    | online       |
| 6  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

## Prepare the remaining hosts for running the containerized services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-1 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 6a95e300-eb7d-4eb0-b5e7-acda4acd9d70 |
| host_uuid   | ba798c74-978c-40bf-8530-ba638e5a3021 |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for NODE in compute-0 compute-1; do
>   system host-label-assign $NODE  openstack-compute-node=enabled
>   system host-label-assign $NODE  openvswitch=enabled
>   system host-label-assign $NODE  sriov=enabled
> done
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | d6f1b995-83f5-447f-b2c9-bd9ece9d1160 |
| host_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 683e15bf-6359-43b5-aff0-1c2f05b58ba6 |
| host_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 34e96388-c7a3-46bc-b4fe-0128d099ab60 |
| host_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 940d4b2e-043a-41b1-bb1f-45b9bc5226ee |
| host_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | d341a0bd-2db1-4543-af38-ce24e52c1918 |
| host_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | e2587a11-28c5-4502-bad3-ab05a0742536 |
| host_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

## (Optional) Setup Remote Storage

> None in virtual installation.

## Provisioning controller-1

### Add interfaces on Controller-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -n oam0 -c platform --networks oam controller-1 $(system host-if-list -a controller-1 | awk '/enp2s1/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | oam0                                 |
| iftype       | ethernet                             |
| ports        | [u'enp2s1']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:30:78:84                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | oam                                  |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 4eeda01e-751c-4ace-b17b-b7b5343ffc6f |
| ihost_uuid   | ba798c74-978c-40bf-8530-ba638e5a3021 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:17:38.563207+00:00     |
| updated_at   | 2019-05-29T14:24:51.033954+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-1 mgmt0 --networks cluster-host
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:af:f3:e9                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 5a39bb5c-8b1a-4dba-b46b-04c0af5a5c67 |
| ihost_uuid   | ba798c74-978c-40bf-8530-ba638e5a3021 |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:17:39.137755+00:00     |
| updated_at   | 2019-05-29T14:25:13.775820+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

### Unlock Controller-1

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {u'stor_function': u'monitor'}       |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:05:53.758084+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:af:f3:e9                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T14:25:38.123525+00:00     |
| uptime              | 499                                  |
| uuid                | ba798c74-978c-40bf-8530-ba638e5a3021 |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | disabled    | offline      |
| 3  | storage-0    | storage     | locked         | disabled    | online       |
| 4  | storage-1    | storage     | locked         | disabled    | online       |
| 5  | compute-0    | worker      | locked         | disabled    | online       |
| 6  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

After ~ 15 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | locked         | disabled    | online       |
| 4  | storage-1    | storage     | locked         | disabled    | online       |
| 5  | compute-0    | worker      | locked         | disabled    | online       |
| 6  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_WARN
            Reduced data availability: 64 pgs inactive
 
  services:
    mon: 2 daemons, quorum controller-0,controller-1
    mgr: controller-0(active), standbys: controller-1
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   1 pools, 64 pgs
    objects: 0  objects, 0 B
    usage:   0 B used, 0 B / 0 B avail
    pgs:     100.000% pgs unknown
             64 unknown

```

## Provision storage

### Add the cluster-host interface on storage hosts

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-0 $(system host-if-list -a storage-0 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:d2:e5:5b                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2bca8281-dcb4-41fd-9e99-8eb51a156331 |
| ihost_uuid   | 62acd311-72fa-4535-b542-c5419c0df36a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:18:22.649481+00:00     |
| updated_at   | 2019-05-29T14:43:19.779850+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-1 $(system host-if-list -a storage-1 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:7b:80:cd                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 9b33ac4f-580d-43be-bd65-c7cb9f6fa82f |
| ihost_uuid   | d472b007-f9d0-4947-bff0-6484ab7fa40e |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:19:28.044123+00:00     |
| updated_at   | 2019-05-29T14:43:46.498227+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

### Add an OSD to the storage hosts

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-0 $(system host-disk-list storage-0 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | 620ae832-acf9-4029-8040-ed78ff585da2             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | 620ae832-acf9-4029-8040-ed78ff585da2             |
| ihost_uuid       | 62acd311-72fa-4535-b542-c5419c0df36a             |
| idisk_uuid       | aed89456-43c1-4b77-a6c1-af05660b83d4             |
| tier_uuid        | a9d1d16f-68d6-4aa6-9274-d417da4a4068             |
| tier_name        | storage                                          |
| created_at       | 2019-05-29T14:44:26.039558+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-1 $(system host-disk-list storage-1 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 1                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | fea553fe-dd9a-4962-baa5-2710ec3c4669             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | fea553fe-dd9a-4962-baa5-2710ec3c4669             |
| ihost_uuid       | d472b007-f9d0-4947-bff0-6484ab7fa40e             |
| idisk_uuid       | ed276e6c-7a01-4d44-951c-c1174f0a64b0             |
| tier_uuid        | a9d1d16f-68d6-4aa6-9274-d417da4a4068             |
| tier_name        | storage                                          |
| created_at       | 2019-05-29T14:44:47.905042+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
```

### Unlock the storage hosts

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {u'stor_function': u'monitor'}                                |
| config_applied      | None                                                          |
| config_status       | Config out-of-date                                            |
| config_target       | ce0f0c4a-3974-4e26-9c99-dcb0d1c41c9d                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2019-05-29T14:07:10.797882+00:00                              |
| hostname            | storage-0                                                     |
| id                  | 3                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.9                                                 |
| mgmt_mac            | 52:54:00:d2:e5:5b                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-1', u'storage-0'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 19.01                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2019-05-29T14:45:15.797490+00:00                              |
| uptime              | 1642                                                          |
| uuid                | 62acd311-72fa-4535-b542-c5419c0df36a                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-1
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {}                                                            |
| config_applied      | None                                                          |
| config_status       | Config out-of-date                                            |
| config_target       | d17518f0-68c7-4e86-961e-18f210726c12                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2019-05-29T14:08:09.929773+00:00                              |
| hostname            | storage-1                                                     |
| id                  | 4                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.14                                                |
| mgmt_mac            | 52:54:00:7b:80:cd                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-0', u'storage-1'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 19.01                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2019-05-29T14:45:15.811526+00:00                              |
| uptime              | 1567                                                          |
| uuid                | d472b007-f9d0-4947-bff0-6484ab7fa40e                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | unlocked       | disabled    | offline      |
| 4  | storage-1    | storage     | unlocked       | disabled    | offline      |
| 5  | compute-0    | worker      | locked         | disabled    | online       |
| 6  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

After ~ 10 minutes...


```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | storage-0    | storage     | unlocked       | enabled     | available    |
| 4  | storage-1    | storage     | unlocked       | enabled     | available    |
| 5  | compute-0    | worker      | locked         | disabled    | online       |
| 6  | compute-1    | worker      | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   225 MiB used, 398 GiB / 398 GiB avail
    pgs:     256 active+clean
```

## Provision Computes

### Setup the cluster-host interfaces on the computes

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host compute-0 $(system host-if-list -a compute-0 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:19:e2:ea                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2317233a-2243-4c5d-b4ea-3a26397ac37f |
| ihost_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:20:36.097240+00:00     |
| updated_at   | 2019-05-29T15:04:37.163220+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host compute-1 $(system host-if-list -a compute-1 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:bb:9b:06                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 966a07aa-d4e9-4569-8630-b4c59744a6ca |
| ihost_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:21:23.695143+00:00     |
| updated_at   | 2019-05-29T15:04:41.514166+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

### Configure data interfaces for computes

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0IF=eth1000
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1IF=eth1001
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET0='physnet0'
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET1='physnet1'
[wrsroot@controller-0 ~(keystone_admin)]$ SPL=/tmp/tmp-system-port-list
[wrsroot@controller-0 ~(keystone_admin)]$ SPIL=/tmp/tmp-system-host-if-list
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ # configure the datanetworks in sysinv, prior to referencing it in the 'system host-if-modify command'.
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET0} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 1                                    |
| uuid         | 2027954a-4f1d-47eb-85d1-fdde0ae9b314 |
| name         | physnet0                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET1} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 2                                    |
| uuid         | aad8fcd6-a304-4fd1-9f5c-66dcae174704 |
| name         | physnet1                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   echo "Configuring interface for: $COMPUTE"
>   set -ex
>   system host-port-list ${COMPUTE} --nowrap > ${SPL}
>   system host-if-list -a ${COMPUTE} --nowrap > ${SPIL}
>   DATA0PCIADDR=$(cat $SPL | grep $DATA0IF |awk '{print $8}')
>   DATA1PCIADDR=$(cat $SPL | grep $DATA1IF |awk '{print $8}')
>   DATA0PORTUUID=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $2}')
>   DATA1PORTUUID=$(cat $SPL | grep ${DATA1PCIADDR} | awk '{print $2}')
>   DATA0PORTNAME=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $4}')
>   DATA1PORTNAME=$(cat  $SPL | grep ${DATA1PCIADDR} | awk '{print $4}')
>   DATA0IFUUID=$(cat $SPIL | awk -v DATA0PORTNAME=$DATA0PORTNAME '($12 ~ DATA0PORTNAME) {print $2}')
>   DATA1IFUUID=$(cat $SPIL | awk -v DATA1PORTNAME=$DATA1PORTNAME '($12 ~ DATA1PORTNAME) {print $2}')
>   system host-if-modify -m 1500 -n data0 -d ${PHYSNET0} -c data ${COMPUTE} ${DATA0IFUUID}
>   system host-if-modify -m 1500 -n data1 -d ${PHYSNET1} -c data ${COMPUTE} ${DATA1IFUUID}
>   set +ex
> done
Configuring interface for: compute-0
+ system host-port-list compute-0 --nowrap
+ system host-if-list -a compute-0 --nowrap
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1000
++ awk '{print $8}'
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ grep --color=auto 0000:02:03.0
++ awk '{print $2}'
++ cat /tmp/tmp-system-port-list
+ DATA0PORTUUID=d94e891a-5048-464e-886b-9c254c8dc60e
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'
+ DATA1PORTUUID=aa5a1206-9b34-40ac-a2a2-813bce4e7d8b
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'mkdir -p /etc/openstack
tee /etc/openstack/clouds.yaml << EOF
clouds:
  openstack_helm:
    region_name: RegionOne
    identity_api_version: 3
    endpoint_type: internalURL
    auth:
      username: 'admin'
      password: 'Li69nux*'
      project_name: 'admin'
      project_domain_name: 'default'
      user_domain_name: 'default'
      auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
EOF

export OS_CLOUD=openstack_helm
openstack endpoint list

+ DATA0IFUUID=a69f68e6-8e5e-4791-9669-4f76cba8d9ef
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
+ DATA1IFUUID=e14a7551-a7e5-4774-b001-c788ec0ea2a1
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-0 a69f68e6-8e5e-4791-9669-4f76cba8d9ef
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |                                                                                                              [90/1829]
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:37:9f:75                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | a69f68e6-8e5e-4791-9669-4f76cba8d9ef |
| ihost_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:20:36.380837+00:00     |
| updated_at   | 2019-05-29T15:07:28.596229+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-0 e14a7551-a7e5-4774-b001-c788ec0ea2a1
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:0d:03:2f                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | e14a7551-a7e5-4774-b001-c788ec0ea2a1 |
| ihost_uuid   | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:20:36.632473+00:00     |
| updated_at   | 2019-05-29T15:07:31.456005+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
Configuring interface for: compute-1
+ system host-port-list compute-1 --nowrap
+ system host-if-list -a compute-1 --nowrap
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1000
++ awk '{print $8}'
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $2}'
+ DATA0PORTUUID=3ea9b452-3895-46ba-bd44-652685f2a3c0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'
+ DATA1PORTUUID=0d25e26e-ff4b-4f98-8c32-0176ae239bd6
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'
+ DATA0IFUUID=2e6c98ce-7d96-4824-a7fa-319378b8591d
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
+ DATA1IFUUID=04d35fd3-7309-4d56-ac96-0f21851539d7
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-1 2e6c98ce-7d96-4824-a7fa-319378b8591d
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:87:5f:36                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 2e6c98ce-7d96-4824-a7fa-319378b8591d |
| ihost_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:21:23.988766+00:00     |
| updated_at   | 2019-05-29T15:07:37.029628+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-1 04d35fd3-7309-4d56-ac96-0f21851539d7
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:f6:ee:79                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 04d35fd3-7309-4d56-ac96-0f21851539d7 |
| ihost_uuid   | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T14:21:24.126199+00:00     |
| updated_at   | 2019-05-29T15:07:39.554004+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
```

### Create volume groups for computes

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   ROOT_DISK=$(system host-show ${COMPUTE} | grep rootfs | awk '{print $4}')
>   ROOT_DISK_UUID=$(system host-disk-list ${COMPUTE} --nowrap | awk /${ROOT_DISK}/'{print $2}')
>   AVAIL_SIZE=$(system host-disk-list ${COMPUTE} | awk /${ROOT_DISK}/'{printf("%d",$12)}')
>   CGTS_PARTITION_SIZE=4
>   NOVA_PARTITION_SIZE=$(($AVAIL_SIZE - $CGTS_PARTITION_SIZE))
>   CGTS_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${CGTS_PARTITION_SIZE})
>   CGTS_PARTITION_UUID=$(echo ${CGTS_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
>   NOVA_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${NOVA_PARTITION_SIZE})
>   NOVA_PARTITION_UUID=$(echo ${NOVA_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
>   system host-lvg-add ${COMPUTE} cgts-vg
>   system host-pv-add ${COMPUTE} cgts-vg  ${CGTS_PARTITION_UUID}
>   system host-lvg-add ${COMPUTE} nova-local
>   system host-pv-add ${COMPUTE} nova-local ${NOVA_PARTITION_UUID}
>   system host-lvg-modify -b image ${COMPUTE} nova-local
> done
cgts-vg volume group already exists
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | bb5f08cb-d130-4155-a93f-1e51116de676             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | a2561c83-9248-4b09-bd22-3c920a1b1d35             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | cgts-vg                                          |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | e2efc721-727f-4166-a2dd-9623dc8fa23b             |
| created_at               | 2019-05-29T15:12:59.553520+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 96effd94-b892-4fe9-803e-92a48044a80d                              |
| ihost_uuid            | e2efc721-727f-4166-a2dd-9623dc8fa23b                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T15:13:04.727690+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | a5149515-3c01-4ab0-a63f-61c29e6578fe             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 82b5e3bf-b39a-4c4d-a6ba-0d7fb35b02a4             |
| disk_or_part_device_node | /dev/sda6                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 |
| lvm_pv_name              | /dev/sda6                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | e2efc721-727f-4166-a2dd-9623dc8fa23b             |
| created_at               | 2019-05-29T15:13:06.596774+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 96effd94-b892-4fe9-803e-92a48044a80d                              |
| ihost_uuid            | e2efc721-727f-4166-a2dd-9623dc8fa23b                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T15:13:04.727690+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
cgts-vg volume group already exists
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 9d3d66c7-4da3-4244-8fec-d076a81e9896             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | b06c2717-b6df-4d78-9c24-bbee47c6a7eb             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | cgts-vg                                          |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba             |
| created_at               | 2019-05-29T15:13:19.449844+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+                                                                        [12/1804]
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 729e32d6-defd-4124-8c31-5667da180a27                              |
| ihost_uuid            | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T15:13:24.392637+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 2bbe459d-f7c0-4fe9-a6cc-f47d9734385a             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | cba9aaac-9f98-4934-9fd4-1bfc643f95ba             |
| disk_or_part_device_node | /dev/sda6                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 |
| lvm_pv_name              | /dev/sda6                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba             |
| created_at               | 2019-05-29T15:13:26.276962+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 729e32d6-defd-4124-8c31-5667da180a27                              |
| ihost_uuid            | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T15:13:24.392637+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>     system host-unlock $COMPUTE
> done
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | 22487d47-7be0-43f4-8b58-8f405246274c |
| config_status       | None                                 |
| config_target       | 22487d47-7be0-43f4-8b58-8f405246274c |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:09:15.968293+00:00     |
| hostname            | compute-0                            |
| id                  | 5                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.10                       |
| mgmt_mac            | 52:54:00:19:e2:ea                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T15:14:16.043084+00:00     |
| uptime              | 3269                                 |
| uuid                | e2efc721-727f-4166-a2dd-9623dc8fa23b |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

## Using sysinv to bring up/down the containerized services

### Generate the stx-openstack application tarball

```sh
user@workstation:~/stx-tools/deployment/libvirt$ wget http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190523T013000Z/outputs/helm-charts/stx-openstack-1.0-13-centos-stable-latest.tgz
```

### Stage application for deployment

```sh
user@workstation:~/stx-tools/deployment/libvirt$ scp stx-openstack-1.0-13-centos-stable-latest.tgz wrsroot@10.10.10.3:~/
...
wrsroot@10.10.10.3's password: 
stx-openstack-1.0-13-centos-stable-latest.tgz                                                                                               100% 1083KB   1.1MB/s   00:00
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ls
ansible.log  localhost.yml  stx-openstack-1.0-13-centos-stable-latest.tgz
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------+-------------------------------+---------------+---------+-----------+
| application         | version | manifest name                 | manifest file | status  | progress  |
+---------------------+---------+-------------------------------+---------------+---------+-----------+
| platform-integ-apps | 1.0-5   | platform-integration-manifest | manifest.yaml | applied | completed |
+---------------------+---------+-------------------------------+---------------+---------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-upload stx-openstack-1.0-13-centos-stable-latest.tgz
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| active        | False                            |
| app_version   | 1.0-13-centos-stable-latest      |
| created_at    | 2019-05-29T15:41:38.332856+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | uploading                        |
| updated_at    | None                             |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show stx-openstack' to view the current progress.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
| application         | version                   | manifest name                 | manifest file | status    | progress                        |
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
| platform-integ-apps | 1.0-5                     | platform-integration-manifest | manifest.yaml | applied   | completed                       |
| stx-openstack       | 1.0-13-centos-stable-     | armada-manifest               | manifest.yaml | uploading | validating and uploading charts |
|                     | latest                    |                               |               |           |                                 |
|                     |                           |                               |               |           |                                 |
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
```

After ~ 15 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+-----------------------------+-------------------------------+---------------+----------+-----------+
| application         | version                     | manifest name                 | manifest file | status   | progress  |
+---------------------+-----------------------------+-------------------------------+---------------+----------+-----------+
| platform-integ-apps | 1.0-5                       | platform-integration-manifest | manifest.yaml | applied  | completed |
| stx-openstack       | 1.0-13-centos-stable-latest | armada-manifest               | manifest.yaml | uploaded | completed |
+---------------------+-----------------------------+-------------------------------+---------------+----------+-----------+
```

### Bring Up Services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-apply stx-openstack
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| active        | False                            |
| app_version   | 1.0-13-centos-stable-latest      |
| created_at    | 2019-05-29T15:41:38.332856+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | applying                         |
| updated_at    | 2019-05-29T15:44:03.135360+00:00 |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show stx-openstack' to view the current progress.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+-----------------------------+-------------------------------+---------------+----------+--------------------------+
| application         | version                     | manifest name                 | manifest file | status   | progress                 |
+---------------------+-----------------------------+-------------------------------+---------------+----------+--------------------------+
| platform-integ-apps | 1.0-5                       | platform-integration-manifest | manifest.yaml | applied  | completed                |
| stx-openstack       | 1.0-13-centos-stable-latest | armada-manifest               | manifest.yaml | applying | retrieving docker images |
+---------------------+-----------------------------+-------------------------------+---------------+----------+--------------------------+
```

After ~ 15 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------------------------+-------------------------------+---------------+----------+-----------------------------------------------------+
| application         | version                   | manifest name                 | manifest file | status   | progress                                            |
+---------------------+---------------------------+-------------------------------+---------------+----------+-----------------------------------------------------+
| platform-integ-apps | 1.0-5                     | platform-integration-manifest | manifest.yaml | applied  | completed                                           |
| stx-openstack       | 1.0-13-centos-stable-     | armada-manifest               | manifest.yaml | applying | processing chart: osh-kube-system-ingress, overall  |
|                     | latest                    |                               |               |          | completion: 5.0%                                    |
|                     |                           |                               |               |          |                                                     |
+---------------------+---------------------------+-------------------------------+---------------+----------+-----------------------------------------------------+
```

After ~ 30 minutes...

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+-----------------------------+-------------------------------+---------------+---------+-----------+
| application         | version                     | manifest name                 | manifest file | status  | progress  |
+---------------------+-----------------------------+-------------------------------+---------------+---------+-----------+
| platform-integ-apps | 1.0-5                       | platform-integration-manifest | manifest.yaml | applied | completed |
| stx-openstack       | 1.0-13-centos-stable-latest | armada-manifest               | manifest.yaml | applied | completed |
+---------------------+-----------------------------+-------------------------------+---------------+---------+-----------+
```

## Verify the cluster endpoints


```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ sudo mkdir -p /etc/openstack
Password: 
controller-0:~$ 
```

```sh
controller-0:~$ sudo tee /etc/openstack/clouds.yaml << EOF
> clouds:
>   openstack_helm:
>     region_name: RegionOne
>     identity_api_version: 3
>     endpoint_type: internalURL
>     auth:
>       username: 'admin'
>       password: 'Li69nux*'
>       project_name: 'admin'
>       project_domain_name: 'default'
>       user_domain_name: 'default'
>       auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
> EOF
clouds:
  openstack_helm:
    region_name: RegionOne
    identity_api_version: 3
    endpoint_type: internalURL
    auth:
      username: 'admin'
      password: 'Li69nux*'
      project_name: 'admin'
      project_domain_name: 'default'
      user_domain_name: 'default'
      auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
controller-0:~$ 
```

```sh
controller-0:~$ sudo vi /etc/openstack/clouds.yaml
```

```yaml
clouds:
  openstack_helm:
    region_name: RegionOne
    identity_api_version: 3
    endpoint_type: internalURL
    auth:
      username: 'admin'
      password: 'Sta4rlingX*'
      project_name: 'admin'
      project_domain_name: 'default'
      user_domain_name: 'default'
      auth_url: 'http://keystone.openstack.svc.cluster.local/v3'
```

```sh
controller-0:~$ export OS_CLOUD=openstack_helm
```

```sh
controller-0:~$ openstack endpoint list
controller-0:~$ openstack endpoint list                                                                                                                                       
+----------------------------------+-----------+--------------+----------------+---------+-----------+------------------------------------------------------------------------
---+
| ID                               | Region    | Service Name | Service Type   | Enabled | Interface | URL                                                                    
   |
+----------------------------------+-----------+--------------+----------------+---------+-----------+------------------------------------------------------------------------
---+
| 01260daf8fec4aeab5135eebbf4bb361 | RegionOne | aodh         | alarming       | True    | public    | http://aodh.openstack.svc.cluster.local:80/                            
   |
| 01321839956d450b96f09c327559d0d2 | RegionOne | cinder       | volume         | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v1/%(tenant_id)s    
   |
| 078fd4da88e14eaeb771368892211380 | RegionOne | gnocchi      | metric         | True    | public    | http://gnocchi.openstack.svc.cluster.local:80/                         
   |
| 0c69000f50204b228c8ebedbd6ed2bef | RegionOne | panko        | event          | True    | internal  | http://panko-api.openstack.svc.cluster.local:8977/                     
   |
| 1804d100862f4af2a05c33d36b4ee796 | RegionOne | gnocchi      | metric         | True    | admin     | http://gnocchi-api.openstack.svc.cluster.local:8041/                   
   |
| 18cb05e909064152946c68a2f6c8a3ae | RegionOne | nova         | compute        | True    | admin     | http://nova-api-proxy.openstack.svc.cluster.local:8774/v2.1/%(tenant_id
)s |
| 238cb5dd9ea94d34a143013cdb524cce | RegionOne | panko        | event          | True    | admin     | http://panko-api.openstack.svc.cluster.local:8977/                     
   |
| 2e10b3dd6d204885b840c58c90860ce9 | RegionOne | nova         | compute        | True    | public    | http://nova.openstack.svc.cluster.local:80/v2.1/%(tenant_id)s          
   |
| 39c0b58e98104c3990daaea0eac43dbf | RegionOne | aodh         | alarming       | True    | admin     | http://aodh-api.openstack.svc.cluster.local:8042/                      
   |
| 4062815bc5bc4f08a64f96d763c53e43 | RegionOne | keystone     | identity       | True    | admin     | http://keystone.openstack.svc.cluster.local:80/v3                      
   |
| 40aa55cd3e464f22875ea24595446eb6 | RegionOne | aodh         | alarming       | True    | internal  | http://aodh-api.openstack.svc.cluster.local:8042/                      
   |
| 44f4341014b14b26807b4d392053cd20 | RegionOne | cinderv3     | volumev3       | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v3/%(tenant_id)s    
   |
| 4bd3bcfa3124449fbaf4577556499f96 | RegionOne | heat-cfn     | cloudformation | True    | admin     | http://heat-cfn.openstack.svc.cluster.local:8000/v1                    
   |
| 4f2cc437dde7403e9b484ae312cd09f7 | RegionOne | neutron      | network        | True    | public    | http://neutron.openstack.svc.cluster.local:80/                         
   |
| 5b482036884d43aa8088b7d115f01da5 | RegionOne | heat         | orchestration  | True    | internal  | http://heat-api.openstack.svc.cluster.local:8004/v1/%(project_id)s     
   |
| 5fe5c525abf545058b150f32e4cf9a83 | RegionOne | cinderv3     | volumev3       | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v3/%(tenant_id)s    
   |
| 68851056271d4d17bb61e602fc58ecd4 | RegionOne | cinderv3     | volumev3       | True    | public    | http://cinder.openstack.svc.cluster.local:80/v3/%(tenant_id)s          
   |
| 697b27e1f1184fe2bbc0bf7f05624aaa | RegionOne | cinderv2     | volumev2       | True    | public    | http://cinder.openstack.svc.cluster.local:80/v2/%(tenant_id)s          
   |
| 6e3d10af7b144cfda1c35a89a29df67f | RegionOne | cinderv2     | volumev2       | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v2/%(tenant_id)s    
   |
| 72ad226512f24421a20a034565859292 | RegionOne | neutron      | network        | True    | admin     | http://neutron-server.openstack.svc.cluster.local:9696/                
   |
| 755579127f374faabbd2866ed3fa2674 | RegionOne | heat-cfn     | cloudformation | True    | internal  | http://heat-cfn.openstack.svc.cluster.local:8000/v1                       |
| 7ce8efd04862499e9ec24d57b04afd05 | RegionOne | barbican     | key-manager    | True    | internal  | http://barbican-api.openstack.svc.cluster.local:9311/                     |
| 81d12673f41b4a61b2bd598a33bf37e0 | RegionOne | barbican     | key-manager    | True    | admin     | http://barbican-api.openstack.svc.cluster.local:9311/                     |
| 8276cd38691741df91f7174638b206be | RegionOne | heat-cfn     | cloudformation | True    | public    | http://cloudformation.openstack.svc.cluster.local:80/v1                   |
| 85f345ebe9a643dda778b7a67248b7cc | RegionOne | heat         | orchestration  | True    | admin     | http://heat-api.openstack.svc.cluster.local:8004/v1/%(project_id)s        |
| 8bb66c19ebc74337a084e4b262fd663b | RegionOne | heat         | orchestration  | True    | public    | http://heat.openstack.svc.cluster.local:80/v1/%(project_id)s              |
| 9062c0e6763b4b33829e1db0f24aa75b | RegionOne | panko        | event          | True    | public    | http://panko.openstack.svc.cluster.local:80/                              |
| 9791a4d1c9494f63908afc5d0a2c476d | RegionOne | neutron      | network        | True    | internal  | http://neutron-server.openstack.svc.cluster.local:9696/                   |
| 9803f619bd2141dc97f490bb6cf4433c | RegionOne | placement    | placement      | True    | internal  | http://placement-api.openstack.svc.cluster.local:8778/                    |
| 9a1f8cc214004ff8ae9da79491bcf7a1 | RegionOne | glance       | image          | True    | admin     | http://glance-api.openstack.svc.cluster.local:9292/                       |
| 9a5967dee3244fb58c5fa214588e5029 | RegionOne | cinder       | volume         | True    | public    | http://cinder.openstack.svc.cluster.local:80/v1/%(tenant_id)s             |
| a085ec422d8d4274973220c05204abbc | RegionOne | cinderv2     | volumev2       | True    | internal  | http://cinder-api.openstack.svc.cluster.local:8776/v2/%(tenant_id)s       |
| a6a45707b55743cfae8fccfd37ad1772 | RegionOne | barbican     | key-manager    | True    | public    | http://barbican.openstack.svc.cluster.local:80/                           |
| a7692ce12f8a40528399794c5d04107c | RegionOne | placement    | placement      | True    | public    | http://placement.openstack.svc.cluster.local:80/                          |
| a89646fa8e6c470daaf09b8e9364a93e | RegionOne | glance       | image          | True    | internal  | http://glance-api.openstack.svc.cluster.local:9292/                       |
| af4a4af815934a02959868e478cab4b7 | RegionOne | nova         | compute        | True    | internal  | http://nova-api-proxy.openstack.svc.cluster.local:8774/v2.1/%(tenant_id)s |
| b668d9607e334b268c0fad667566c64a | RegionOne | placement    | placement      | True    | admin     | http://placement-api.openstack.svc.cluster.local:8778/                    |
| cccdb9d2d0ad4921894228ea7fdb38c3 | RegionOne | glance       | image          | True    | public    | http://glance.openstack.svc.cluster.local:80/                             |
| e489e534de66444cbff26e2b0ccf6a79 | RegionOne | cinder       | volume         | True    | admin     | http://cinder-api.openstack.svc.cluster.local:8776/v1/%(tenant_id)s       |
| e956ddf74bd144c283ee54e3cd74d38d | RegionOne | gnocchi      | metric         | True    | internal  | http://gnocchi-api.openstack.svc.cluster.local:8041/                      |
| f383bce5b8bd482e83452b8a2f96374c | RegionOne | keystone     | identity       | True    | internal  | http://keystone-api.openstack.svc.cluster.local:5000/v3                   |
| ffedebc9af4348af9a42107e98886cf4 | RegionOne | keystone     | identity       | True    | public    | http://keystone.openstack.svc.cluster.local:80/v3                         |
+----------------------------------+-----------+--------------+----------------+---------+-----------+---------------------------------------------------------------------------+
```

## Provider/tenant networking setup

```sh
controller-0:~$ ADMINID=`openstack project list | grep admin | awk '{print $2}'`
controller-0:~$ PHYSNET0='physnet0'
controller-0:~$ PHYSNET1='physnet1'
```

```sh
controller-0:~$ openstack network segment range create ${PHYSNET0}-a --network-type vlan --physical-network ${PHYSNET0}  --minimum 400 --maximum 499 --private --project ${ADMINID}
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['400-499']                                                                                                                                                                                               |
| default          | False                                                                                                                                                                                                     |
| id               | 816e675d-1fff-40d0-a1d1-785fcc4b88d3                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'e74952133e364290bf5da9fc694687c2'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 499                                                                                                                                                                                                       |
| minimum          | 400                                                                                                                                                                                                       |
| name             | physnet0-a                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet0                                                                                                                                                                                                  |
| project_id       | e74952133e364290bf5da9fc694687c2                                                                                                                                                                          |
| shared           | False                                                                                                                                                                                                     |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:~$ openstack network segment range create  ${PHYSNET0}-b --network-type vlan  --physical-network ${PHYSNET0}  --minimum 10 --maximum 10 --shared
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['10']                                                                                                                                                                                                    |
| default          | False                                                                                                                                                                                                     |
| id               | 76353ed4-a46e-4e18-9ab8-f28d553959a1                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'e74952133e364290bf5da9fc694687c2'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 10                                                                                                                                                                                                        |
| minimum          | 10                                                                                                                                                                                                        |
| name             | physnet0-b                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet0                                                                                                                                                                                                  |
| project_id       | None                                                                                                                                                                                                      |
| shared           | True                                                                                                                                                                                                      |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```sh
controller-0:~$ openstack network segment range create ${PHYSNET1}-a --network-type vlan  --physical-network  ${PHYSNET1} --minimum 500 --maximum 599  --private --project ${ADMINID}
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                                                     |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| available        | ['500-599']                                                                                                                                                                                               |
| default          | False                                                                                                                                                                                                     |
| id               | c6f2ee53-c111-4e55-829e-8815efdaa4fb                                                                                                                                                                      |
| location         | Munch({'project': Munch({'domain_name': 'default', 'domain_id': None, 'name': 'admin', 'id': u'e74952133e364290bf5da9fc694687c2'}), 'cloud': 'openstack_helm', 'region_name': 'RegionOne', 'zone': None}) |
| maximum          | 599                                                                                                                                                                                                       |
| minimum          | 500                                                                                                                                                                                                       |
| name             | physnet1-a                                                                                                                                                                                                |
| network_type     | vlan                                                                                                                                                                                                      |
| physical_network | physnet1                                                                                                                                                                                                  |
| project_id       | e74952133e364290bf5da9fc694687c2                                                                                                                                                                          |
| shared           | False                                                                                                                                                                                                     |
| used             | {}                                                                                                                                                                                                        |
+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

### Tenant Networking setup

> All followed

- https://wiki.openstack.org/wiki/StarlingX/Containers/Installation#Provider.2Ftenant_networking_setup

### Additional Setup Instructions

> None followed, just for reference

## Health

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.41 k objects, 747 MiB
    usage:   1.6 GiB used, 396 GiB / 398 GiB avail
    pgs:     856 active+clean
 
  io:
    client:   339 KiB/s wr, 0 op/s rd, 63 op/s wr

```

## Failures

```sh
Insufficient memory reserved for platform on compute-1. Platform memory must be at least 1100 MiB summed across all numa nodes.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-reboot compute-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | 74c228ad-9232-450e-aecb-aae1992a4aed |
| config_status       | None                                 |
| config_target       | 74c228ad-9232-450e-aecb-aae1992a4aed |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T14:10:23.228399+00:00     |
| hostname            | compute-1                            |
| id                  | 6                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.8                        |
| mgmt_mac            | 52:54:00:bb:9b:06                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Rebooting                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T15:19:16.393857+00:00     |
| uptime              | 3518                                 |
| uuid                | 81bcd63d-32f5-47e4-a1b3-db49bcf47fba |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

## Others

- https://www.google.com/search?client=ubuntu&channel=fs&q=%22operation+aborted%2C+check+logs+for+detail%22&ie=utf-8&oe=utf-8

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods --all-namespaces | grep tiller
kube-system   tiller-deploy-9c6df7f79-7x8fp              1/1     Running   1          3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services --all-namespaces | grep tiller
kube-system   tiller-deploy   ClusterIP   10.109.71.107   <none>        44134/TCP       3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get deployments.apps --all-namespaces | grep tiller
kube-system   tiller-deploy             1/1     1            1           3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ get pods --all-namespaces | grep tiller
-sh: get: command not found
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods --all-namespaces | grep tiller
kube-system   tiller-deploy-9c6df7f79-7x8fp              1/1     Running   1          3h40m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services --all-namespaces | grep tiller
kube-system   tiller-deploy   ClusterIP   10.109.71.107   <none>        44134/TCP       3h40m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get deployments.apps --all-namespaces | grep tiller
kube-system   tiller-deploy             1/1     1            1           3h40m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ cat /etc/build.info
###
### StarlingX
###     Built from master
###

OS="centos"
SW_VERSION="19.01"
BUILD_TARGET="Host Installer"
BUILD_TYPE="Formal"
BUILD_ID="20190528T202529Z"

JOB="STX_build_master_master"
BUILD_BY="starlingx.build@cengn.ca"
BUILD_NUMBER="119"
BUILD_HOST="starlingx_mirror"
BUILD_DATE="2019-05-28 20:25:29 +0000"
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ cat /var/log/sysinv.log
...
...
2019-05-29 12:15:47.743 95438 INFO sysinv.conductor.kube_app [-] Application overrides generated.
2019-05-29 12:15:47.781 95438 INFO sysinv.conductor.kube_app [-] Armada manifest file has no img tags for chart helm-toolkit
2019-05-29 12:15:47.804 95438 INFO sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/quay.io/external_storage/rbd-provisioner:v2.1.1-k8s1.11 download started from local registry
2019-05-29 12:15:47.858 95438 INFO sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/docker.io/port/ceph-config-helper:v1.10.3 download started from local registry
2019-05-29 12:15:58.047 95438 ERROR sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/quay.io/external_storage/rbd-provisioner:v2.1.1-k8s1.11 download failed from local registry: 500 Server Error: Internal Server Error ("Get https://192.168.204.2:9001/v2/: net/http: TLS handshake timeout")
2019-05-29 12:15:58.089 95438 ERROR sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/docker.io/port/ceph-config-helper:v1.10.3 download failed from local registry: 500 Server Error: Internal Server Error ("Get https://192.168.204.2:9001/v2/: net/http: TLS handshake timeout")
2019-05-29 12:15:58.090 95438 ERROR sysinv.conductor.kube_app [-] Deployment of application platform-integ-apps (1.0-5) failed: failed to download one or more image(s).
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app Traceback (most recent call last):
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app   File "/usr/lib64/python2.7/site-packages/sysinv/conductor/kube_app.py", line 1212, in perform_app_apply
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app     self._download_images(app)
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app   File "/usr/lib64/python2.7/site-packages/sysinv/conductor/kube_app.py", line 546, in _download_images
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app     reason="failed to download one or more image(s).")
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app KubeAppApplyFailure: Deployment of application platform-integ-apps (1.0-5) failed: failed to download one or more image(s).
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app 
2019-05-29 12:15:58.100 95438 ERROR sysinv.conductor.kube_app [-] Application apply aborted!.
2019-05-29 12:16:25.598 96284 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_start_2019-05-29-12-16-25 patch
2019-05-29 12:16:25.599 96284 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:16:27.045 96284 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_start_2019-05-29-12-16-27 patch
2019-05-29 12:16:27.046 96284 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:16:53.927 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_start_2019-05-29-12-16-53 patch
2019-05-29 12:16:53.927 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:18:41.853 96283 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_start_2019-05-29-12-18-41 patch
2019-05-29 12:18:41.853 96283 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:19:41.285 96283 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_start_2019-05-29-12-19-41 patch
2019-05-29 12:19:41.285 96283 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:20:07.508 96284 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_start_2019-05-29-12-20-07 patch
2019-05-29 12:20:07.508 96284 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:25.620 96283 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_start_2019-05-29-12-21-25 patch
2019-05-29 12:21:25.620 96283 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:27.079 96283 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_start_2019-05-29-12-21-27 patch
2019-05-29 12:21:27.079 96283 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:54.012 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_start_2019-05-29-12-21-53 patch
2019-05-29 12:21:54.013 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:23:41.921 96284 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_start_2019-05-29-12-23-41 patch
2019-05-29 12:23:41.922 96284 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:24:41.586 96284 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_start_2019-05-29-12-24-41 patch
2019-05-29 12:24:41.586 96284 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:25:07.515 96283 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_start_2019-05-29-12-25-07 patch
2019-05-29 12:25:07.515 96283 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_end.  No changes from mtce/1.0.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker logs armada_service
Password: 
+ CMD=armada
+ ARMADA_UWSGI_PORT=8000
+ ARMADA_UWSGI_TIMEOUT=3600
+ ARMADA_UWSGI_WORKERS=4
+ ARMADA_UWSGI_THREADS=1
+ '[' server = server ']'
+ exec uwsgi -b 32768 --die-on-term --http :8000 --http-timeout 3600 --enable-threads -L --lazy-apps --master --paste config:/etc/armada/api-paste.ini --pyargv '--config-file $
etc/armada/armada.conf' --threads 1 --workers 4
*** Starting uWSGI 2.0.18 (64bit) on [Wed May 29 09:06:44 2019] ***
compiled with version: 7.3.0 on 09 April 2019 13:34:27
os: Linux-3.10.0-957.1.3.el7.1.tis.x86_64 #1 SMP PREEMPT Tue May 28 21:28:13 UTC 2019
nodename: d2887dc5d97e
machine: x86_64
clock source: unix
detected number of CPU cores: 4
current working directory: /armada
detected binary path: /usr/local/bin/uwsgi
!!! no internal routing support, rebuild with pcre support !!!
your memory page size is 4096 bytes
detected max file descriptor number: 65536
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8000 fd 4
uwsgi socket 0 bound to TCP address 127.0.0.1:46633 (port auto-assigned) fd 3
Python version: 3.6.7 (default, Oct 22 2018, 11:32:17)  [GCC 8.2.0]
Python main interpreter initialized at 0x5571f22eda70
python threads support enabled
your server socket listen backlog is limited to 100 connections                                                                                                         [0/1999]
your mercy for graceful operations on workers is 60 seconds
mapped 507960 bytes (496 KB) for 4 cores
*** Operational MODE: preforking ***
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI master process (pid: 1)
spawned uWSGI worker 1 (pid: 15, cores: 1)
spawned uWSGI worker 2 (pid: 16, cores: 1)
spawned uWSGI worker 3 (pid: 17, cores: 1)
spawned uWSGI worker 4 (pid: 18, cores: 1)
spawned uWSGI http 1 (pid: 19)
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
2019-05-29 09:06:45.997 15 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:45.998 15 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:45.998 15 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x5571f22eda70 pid: 15 (default app)
2019-05-29 09:06:45.998 16 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:45.999 16 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:45.999 16 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x5571f22eda70 pid: 16 (default app)
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 2 seconds on interpreter 0x5571f22eda70 pid: 17 (default app)
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 2 seconds on interpreter 0x5571f22eda70 pid: 18 (default app)
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'ls *.log'
platform-integ-apps-delete.log
stx-openstack-delete.log
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'cat stx-openstack-delete.log'
2019-05-29 10:27:23.891 129 DEBUG armada.handlers.tiller [-] Using Tiller namespace: kube-system _get_tiller_namespace /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:174
2019-05-29 10:27:23.906 129 DEBUG armada.handlers.tiller [-] Found at least one Running Tiller pod. _get_tiller_pod /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:150
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Using Tiller pod IP: 192.168.204.3 _get_tiller_ip /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:165
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Using Tiller host port: 44134 _get_tiller_port /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:170
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Tiller getting gRPC insecure channel at 192.168.204.3:44134 with options: [grpc.max_send_message_length=429496729, grpc.max_receive_message_length=429496729] get_channel /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:124
2019-05-29 10:27:23.910 129 DEBUG armada.handlers.tiller [-] Armada is using Tiller at: None:44134, namespace=kube-system, timeout=300 __init__ /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:104
2019-05-29 10:27:23.916 129 INFO armada.handlers.lock [-] Acquiring lock
2019-05-29 10:27:23.929 129 DEBUG armada.handlers.tiller [-] Getting known releases from Tiller... list_charts /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:349
2019-05-29 10:27:23.929 129 DEBUG armada.handlers.tiller [-] Tiller ListReleases() with timeout=300, request=limit: 32
status_codes: UNKNOWN
status_codes: DEPLOYED
status_codes: DELETED
status_codes: DELETING
status_codes: FAILED
status_codes: PENDING_INSTALL
status_codes: PENDING_UPGRADE
status_codes: PENDING_ROLLBACK
 get_results /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:210
2019-05-29 10:27:24.225 129 INFO armada.cli [-] There's no release to delete.
2019-05-29 10:27:24.930 129 INFO armada.handlers.lock [-] Releasing lock

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'cat platform-integ-apps-delete.log'
2019-05-29 12:12:12.332 245 DEBUG armada.handlers.tiller [-] Using Tiller namespace: kube-system _get_tiller_namespace /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:174
2019-05-29 12:12:12.345 245 DEBUG armada.handlers.tiller [-] Found at least one Running Tiller pod. _get_tiller_pod /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:150
2019-05-29 12:12:12.345 245 DEBUG armada.handlers.tiller [-] Using Tiller pod IP: 192.168.204.3 _get_tiller_ip /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:165
2019-05-29 12:12:12.346 245 DEBUG armada.handlers.tiller [-] Using Tiller host port: 44134 _get_tiller_port /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:170
2019-05-29 12:12:12.346 245 DEBUG armada.handlers.tiller [-] Tiller getting gRPC insecure channel at 192.168.204.3:44134 with options: [grpc.max_send_message_length=429496729, grpc.max_receive_message_length=429496729] get_channel /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:124
2019-05-29 12:12:12.348 245 DEBUG armada.handlers.tiller [-] Armada is using Tiller at: None:44134, namespace=kube-system, timeout=300 __init__ /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:104
2019-05-29 12:12:12.353 245 INFO armada.handlers.lock [-] Acquiring lock
2019-05-29 12:12:12.362 245 DEBUG armada.handlers.tiller [-] Getting known releases from Tiller... list_charts /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:349
2019-05-29 12:12:12.362 245 DEBUG armada.handlers.tiller [-] Tiller ListReleases() with timeout=300, request=limit: 32
status_codes: UNKNOWN
status_codes: DEPLOYED
status_codes: DELETED
status_codes: DELETING
status_codes: FAILED
status_codes: PENDING_INSTALL
status_codes: PENDING_UPGRADE
status_codes: PENDING_ROLLBACK
 get_results /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:210
2019-05-29 12:12:12.376 245 INFO armada.cli [-] There's no release to delete.
2019-05-29 12:12:13.364 245 INFO armada.handlers.lock [-] Releasing lock
```


# Bare Metal

```sh
Release 19.01 localhost ttyS0
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

localhost login: wrsroot
Password: 
You are required to change your password immediately (root enforced)
Changing password for wrsroot.
(current) UNIX password: 
New password: 
Retype new password: 

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

localhost:~$ 
```

```
localhost:~$ sudo config_controller --force --config-file config.ini                                                                                                            
Parsing system configuration file...  DONE
Validating system configuration file...  DONE
Creating config apply file...  DONE

The following configuration will be applied:

System Configuration
--------------------
Time Zone: UTC
System mode: duplex
Distributed Cloud System Controller: no

PXEBoot Network Configuration
-----------------------------
Separate PXEBoot network not configured
PXEBoot Controller floating hostname: pxecontroller

Management Network Configuration
--------------------------------
Management interface name: eno2
Management interface: eno2
Management interface MTU: 1500
Management subnet: 10.10.58.0/24
Controller floating address: 10.10.58.2
Controller 0 address: 10.10.58.3
Controller 1 address: 10.10.58.4
NFS Management Address 1: 10.10.58.5
NFS Management Address 2: 10.10.58.6
Controller floating hostname: controller
Controller hostname prefix: controller-
OAM Controller floating hostname: oamcontroller
Dynamic IP address allocation is selected
Management multicast subnet: 239.1.1.0/28

Kubernetes Cluster Network Configuration
----------------------------------------
Cluster pod network subnet: 172.16.0.0/16
Cluster service network subnet: 10.96.0.0/12
Cluster host interface name: eno2
Cluster host interface: eno2
Cluster host interface MTU: 1500
Cluster host subnet: 192.168.206.0/24

External OAM Network Configuration
----------------------------------
External OAM interface name: eno1
External OAM interface: eno1
External OAM interface MTU: 1500
External OAM subnet: 192.168.200.0/24
External OAM gateway address: 192.168.200.1
External OAM floating address: 192.168.200.201
External OAM 0 address: 192.168.200.73
External OAM 1 address: 192.168.200.86

DNS Configuration
-----------------
Nameserver 1: 192.168.100.60

Docker Registry Configuration
-----------------------------
Alternative registry to k8s.gcr.io: 192.168.100.60
Alternative registry to gcr.io: 192.168.100.60
Alternative registry to quay.io: 192.168.100.60
Alternative registry to docker.io: 192.168.100.60
Is registries secure: False

Applying configuration (this will take several minutes):

01/08: Creating bootstrap configuration ... DONE[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     e8ab94c1-8878-40bd-ac05-381366f91e97
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   9 pools, 856 pgs
    objects: 1.41 k objects, 747 MiB
    usage:   1.6 GiB used, 396 GiB / 398 GiB avail
    pgs:     856 active+clean
 
  io:
    client:   339 KiB/s wr, 0 op/s rd, 63 op/s wr
 

02/08: Applying bootstrap manifest ... DONE
03/08: Persisting local configuration ... DONE
04/08: Populating initial system inventory ... DONE
05/08: Creating system configuration ... 2019-05-29 11:51:41,730 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:41.730 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,065 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.065 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,095 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.095 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,096 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.096 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,413 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.413 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,734 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.734 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,764 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.764 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,766 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.766 76522 WARNING ceph_client [-] skip checking server certificate
DONE
06/08: Applying controller manifest ... 
Failed to execute controller manifest

Configuration failed: Failed to apply controller manifest. See /var/log/puppet/latest/puppet.log for details.
localhost:~$ 
```

Issues

```sh
06/08: Applying controller manifest ... 
[ 1115.133420] block drbd2: Unrelated data, aborting!
[ 1116.194033] block drbd1: Unrelated data, aborting!
[ 1116.274835] block drbd5: Unrelated data, aborting!
[ 1118.807825] block drbd3: Unrelated data, aborting!
[ 1119.807887] block drbd0: Unrelated data, aborting!
[ 1134.299383] block drbd5: Unrelated data, aborting!
[ 1136.749724] block drbd2: Unrelated data, aborting!
[ 1139.486247] block drbd1: Unrelated data, aborting!
[ 1141.610208] block drbd8: Unrelated data, aborting!
[ 1141.615197] block drbd3: Unrelated data, aborting!
```

