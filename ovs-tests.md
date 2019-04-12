# OVS Tests

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

