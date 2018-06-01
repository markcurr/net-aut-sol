# EVPN service deployment

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

This file describes the VNIDs present in the network, mapped VLANs and which switch and interface they are present on. The playbooks can then use this to determine which node to apply a VNI/VLAN/Subnet conf
iguration to dependent on end port.
                                                                          
                  
## Deployment via nxos-modules

The nxos_roles.yml file generates per-node models, then deploys the configuration using roles, and finally validates the fabric:

**nxos/tasks/main.yml** - this sets common configuration like Loopback, OSPF and Ethernet Layer 3 configuration

**spine/tasks/main.yml** - configures BGP peerings to Leaves

**leaf/tasks/main.yml** - configures BGP peerings to spine, VTEP, SVIs, and VNI and VLANs (where applicable)

(Note - there is a bug in VTEP configuration which requires the loopback interface to be shut/no shut after VTEP binding. I have included a play to check for and fix this issue.)

**validate_fabric.yml** - this collects desired and actual state of LLDP, OSPF and BGP adjacencies and confirms they match using 'assert' modules
  
(Note - I found an issue where trying to run nxos_facts on more than 4 N9Ks at once causes a failure; therefore I run this module with serial: 3)
  
## Deployment via configuration replace - in development

**replace-config.yml** - this playbook generates a per-node configuration using **leaf-config.j2** and **spine-config.j2**


Config generation works fairly well however actually getting the files onto the Nexus is slightly more complicated, and the devices are quite particular about the format of the config replace. Therefore this is still in development.

