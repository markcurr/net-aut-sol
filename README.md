# net-aut-sol

My lab is composed of an ESXi server hosting the following VMs:

| VM		| Management IP     |
|:---------:|:-----------------:| 
| ansible	|	192.168.0.60    |
| R1		|	192.168.0.201   |
| R2		|	192.168.0.202   |
| R3		|	192.168.0.203   |
| R4		|	192.168.0.204   |
| N9K1		|	192.168.0.211   |
| N9K2		|	192.168.0.212   |
| N9K3		|	192.168.0.213   |
| N9K4		|	192.168.0.214   |
  
Nexus 9000v VMs are connected in a leaf-spine topology

| Device | Link | Link | Device |
| ---- | ---- | ---- | ---- |
| N9K1 | E1/1 | E1/1 | N9K3 |
| N9K1 | E1/2 | E1/1 | N9K4 |
| N9K2 | E1/1 | E1/2 | N9K3 |
| N9k2 | E1/2 | E1/2 | N9K4 |

These links are running in OSPF, loopbacks (1.1.1.1,2.2.2.2 etc) are advertised into OSPF