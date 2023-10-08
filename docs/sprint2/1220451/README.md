-----BUILDING B------
The logical network layout mirrors the project made in the previous sprint. It includes one Intermediate crossconnect and two Horizontal crossconnects, one per floor.

Devices naming convention:
GF - ground floor
FF - first floor
AP - Access Point
HC - Horizontal Crossconnect
IC - Intermediate Crossconnect

-----VLANs:-----
451 - BB_GF
452 - BB_FF
453 - BB_WiFi
454 - BB_DMZ
455 - BB_VoIP

-----IPs----
BB_GF
Necessary size: 25
Allocated size: 32 - 2
Subnetwork Address: 10.80.213.192/27
Mask: 255.255.255.224
Assignable Addresses:10.80.213.193 - 10.80.213.222
Router interface: 10.80.213.193 255.255.255.224 (interface FastEthernet0/0.1)

BB_FF
Necessary size: 60
Allocated size: 64 - 2
Subnetwork Address: 10.80.211.128/26
Mask: 255.255.255.192
Assignable Addresses: 10.80.211.129-10.80.211.190
Router interface: 10.80.211.129 255.255.255.192 (interface FastEthernet0/0.2)

BB_Wifi
Necessary size: 110
Allocated size: 128 - 2
Subnetwork Address: 10.80.208.0/25
Mask: 255.255.255.128
Assignable Addresses: 10.80.208.1 - 10.80.208.126
Router interface: 10.80.208.1 255.255.255.128 (interface FastEthernet0/0.3)

BB_DMZ
Necessary size: 10
Allocated size: 16 - 2
Subnetwork Address: 10.80.214.160/28
Mask: 255.255.255.240
Assignable Addresses: 10.80.214.161 - 10.80.214.174
Router interface: 10.80.214.161 255.255.255.240 (interface FastEthernet0/0.4)

BB_VoIP
Necessary size: 13
Allocated size: 16 - 2
Subnetwork Address: 10.80.214.128/28
Mask: 255.255.255.240
Assignable Addresses: 10.80.214.129 - 10.80.214.142
Router interface: 10.80.214.129 255.255.255.240 (interface FastEthernet0/0.5)

------STP------
STP is enabled by default and wasn't disabled on any switch.

------VTP------
Domain rc23deg2 was assigned to every switch. The Intermediate Crossconnect switch (Switch_IC) is set up in server mode. The other switches are set up in client mode.

------Static Routing------
Static routing:
Default routing: ip route 0.0.0.0 0.0.0.0 10.80.209.1
(All packages go through the router in building A, which "knows" all the other sub interfaces)
