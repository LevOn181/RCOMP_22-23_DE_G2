
# Building D - SPRINT 2 #

### Sérgio Cardoso - 1210891 ###

<hr>

## 2.1 - VLAN Distribution ##

These are the VLANs for the end nodes of building D

| VLAN ID | VLAN NAME |
|---------|-----------|
| 461	    | BD_GF     |
| 462	    | BD_FF     |
| 463	    | BD_WiFi   |
| 464     | BD_DMZ    |
| 465     | BD_VoIP   |

## 2.2 - Spanning Tree Protocol ##

STP is enabled in every switch by default

## 2.3 - VLAN Trunking Protocol ##

The IC Switch of building D is set to be the server (vtp mode server)

The HC switches of the ground floor and first floor are in client mode (vtp mode client)

The interconnection ports are in trunk mode

## 2.4 - VLAN Database ##

The switches have in their VLAN database all the VLANs of all buildings.

Please refer to the planning.md file to consult the VLAN database.

## 2.5 - VTP Domain ##

VTP Domain name : rc23deg2

This vtp domain name is set up in every switch

## 3.1 IPv4 Networks


| Subnet Name | Needed Size | Allocated Size | Address       | Mask | Dec Mask        | Assignable Range              | Broadcast     |
|-------------|-------------|----------------|---------------|------|-----------------|-------------------------------|---------------|
| BD_WiFi     | 80          | 126            | 10.80.210.0   | /25  | 255.255.255.128 | 10.80.210.1 - 10.80.210.126   | 10.80.210.127 |
| BD_FF       | 50          | 62             | 10.80.211.192 | /26  | 255.255.255.192 | 10.80.211.193 - 10.80.211.254 | 10.80.211.255 |
| BD_GF       | 40          | 62             | 10.80.214.0   | /27  | 255.255.255.224 | 10.80.214.1 - 10.80.214.30    | 10.80.214.31  |
| BD_VoIP     | 25          | 30             | 10.80.214.144 | /28  | 255.255.255.240 | 10.80.214.145 - 10.80.214.158 | 10.80.214.159 |
| BD_DMZ      | 20          | 30             | 10.80.214.176 | /28  | 255.255.255.240 | 10.80.214.177 - 10.80.214.190 | 10.80.214.191 |

## 3.2. End devices in the Packet Tracer simulation ##

Ground Floor : 

- 1 workstation (PC) connected to the VLAN for end-user outlets on the ground floor. 

- 2 Laptops connected to the wireless access-point and the VLAN of the Wi-Fi network of the building.

- 1 server connected to the DMZ VLAN of the building.

- 1 VoIP phone connected to the VoIP VLAN of the building.

First Floor:

- 1 workstation (PC) connected to the VLAN for end-user outlets on the first floor.

- 2 Laptops connected to the wireless access-point and the VLAN of the Wi-Fi network of the building.

- 1 server connected to the DMZ VLAN of the building.

- 1 VoIP phone connected to the VoIP VLAN of the building.

## 3.3 - Routers and Static routing

Router backbone IP: 10.80.96.4

### Router sub-interfaces: ###

interface FastEthernet1/0.1 - ip address 10.80.214.1 255.255.255.224

interface FastEthernet1/0.2 - ip address 10.80.211.193 255.255.255.192

interface FastEthernet1/0.3 - ip address 10.80.210.1 255.255.255.128

interface FastEthernet1/0.4 - ip address 10.80.214.177 255.255.255.240

interface FastEthernet1/0.5 - ip address 10.80.214.145 255.255.255.240


### Static routing: ###

Building D I uses the default routing: ip route 0.0.0.0 0.0.0.0 10.80.209.1

All the packages from the building are sent to the router of Building A. 

That router knows all the building networks and sends the package do the chosen location.


