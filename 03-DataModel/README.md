# Data Model for an EVPN service

These data files describe the EVPN fabric, and the VNIDs which exist in the network.

## Setup

The scripts expect:
* managed hosts in _hosts_ Ansible inventory;
* fabric settings in _fabric/evpn-fabric.yml_
* VNIDs in _fabric/vni-segments.yml_
 
The SSH username and password are contained in the inventory file.

## Data model

### evpn-fabric.yml

This file contains

* **fabric**: a list of infrastructure links to build the leaf-spine underlay
* **loopbacks**: each switches loopback. Loopback0 is used for BGP, Loopback1 (where applied) is used for VTEP
* **features**: a list of the required NXOS features - shared, and leaf specific

### vni-segments.yml

This file describes the VNIDs present in the network, mapped VLANs and which switch and interface they are present on. The playbooks can then use this to determine which node to apply a VNI/VLAN/Subnet configuration to dependent on end port.

The list is tructured with the VNID name as the key for each dictionary, in this structure:

* _vni_name_
** **vlan:** _vlan number_
** **vni:** _vni ID_
** **interfaces:** 
*** _switch name:_
**** _ - Interface _

## Playbooks

The data validation playbook assembles the required per-node information, to ensure that later this can be used to push configuration to each N9K as required. 

