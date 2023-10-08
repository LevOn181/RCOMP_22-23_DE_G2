2.1. Arrangements of VLANs: (456-460)
 _________________
|VLAN ID|VLAN NAME|
|-------|---------|
| 456	| BC_GF   |
| 457	| BC_FF   |
| 458	| BC_WiFi |
| 459   | BC_DMZ  |
| 460   | BC_VoIP |
|_______|_________|

2.2. Spanning tree protocol
-STP is enabled on each switch.

2.3. VLAN Trucking Protocol
-Each switch is in the domain of: rc23deg2.
-The switch insed the IC is in SERVER mode.
-All the other switches are in CLIENT mode.


3.1. Arrangements of IPs:
				
+-----------+-----------+--------------+-------------+----+---------------+---------------------------- +-------------+
|Subnet Name|Needed Size|Allocated Size|Address      |Mask|Dec Mask       |Assignable Range             | Broadcast   |
+===========+===========+==============+=============+====+===============+=============================+=============+
|BC_WiFi    |55         |62            |10.80.212.64 |/26 |255.255.255.192|10.80.212.65 - 10.80.212.126 |10.80.212.127|
+-----------+-----------+--------------+-------------+----+---------------+-----------------------------+-------------+
|BC_FF      |50         |62            |10.80.212.128|/26 |255.255.255.192|10.80.212.129 - 10.80.212.190|10.80.212.191|
+-----------+-----------+--------------+-------------+----+---------------+-----------------------------+-------------+
|BC_GF      |40         |62            |10.80.213.64 |/26 |255.255.255.192|10.80.213.65 - 10.80.213.126 |10.80.213.127|
+-----------+-----------+--------------+-------------+----+---------------+-----------------------------+-------------+
|BC_VoIP    |25         |30            |10.80.213.224|/27 |255.255.255.224|10.80.213.225 - 10.80.213.254|10.80.213.255|
+-----------+-----------+--------------+-------------+----+---------------+-----------------------------+-------------+
|BC_DMZ     |20         |30            |10.80.214.64 |/27 |255.255.255.224|10.80.214.65 - 10.80.214.94  |10.80.214.95 |
+-----------+-----------+--------------+-------------+----+---------------+-----------------------------+-------------+

Router backbone IP: 10.80.209.3/25

Router Sub-Intefaces:

interface FastEthernet0/0.1
 10.80.213.65 255.255.255.192

interface FastEthernet0/0.2
 10.80.212.129 255.255.255.192

interface FastEthernet0/0.3
 10.80.212.65 255.255.255.192

interface FastEthernet0/0.4
 10.80.214.65 255.255.255.224

interface FastEthernet0/0.5
 10.80.213.225 255.255.255.224

Static routing:
 In Building C I used default routing: ip route 0.0.0.0 0.0.0.0 10.80.209.1
 The idea is that all the packages from the building is sent to the router of
 Building A. The router there knows all Building every sub-interfaces' network
 and sends the packate do the designated location.
 

