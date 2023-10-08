#RCOMP - SPRINT 2 - PLANNING #



### Sprint Master: Sérgio Cardoso - 1210891 ###



<hr>

## Sprint Backlog ##


T.2.1  - Development of a layer two and layer three Packet Tracer simulation for building A, encompassing the campus backbone. Integration of every member’s Packet Tracer simulation into a single simulation.

T.2.2  - Development of a layer two and layer three Packet Tracer simulation for building B, encompassing the campus backbone.

T.2.3  - Development of a layer two and layer three Packet Tracer simulation for building C, encompassing the campus backbone.

T.2.4  - Development of a layer two and layer three Packet Tracer  simulation for building D, encompassing the campus backbone.

T.2.5  - Development of a layer two and layer three Packet Tracer simulation for building E, encompassing the campus backbone.

<hr>

## Technical Decisions
 
### Cisco Packet Tracer ###

All the simulations of the layer two and layer three configurations must be made using Cisco
Packet Tracer.

Version of the Cisco Packet Tracer software: Version 8.2.0.0162

### VLANs Configuration ###

Our team can use VLAN IDs in this range: 445 - 475

The VLAN are assigned in the following manner:

Default vlan: VLANID 1

BACKBONE VLAN: VLANID 445

Building A: VLANID: 446-450

Building B: VLANID: 451-455

Building C: VLANID: 456-460

Building D: VLANID: 461-465

Building E: VLANID: 466-470

## VLAN Database: ##

445 - BB (BackBone)

###Building A ###

446 - BA_GF

447 - BA_FF

448 - BA_WiFi

449 - BA_DMZ

450 - BA_VoIP

### Building B ###

451 - BB_GF

452 - BB_FF

453 - BB_WiFi

454 - BB_DMZ

455 - BB_VoIP

### Building C ###

456 - BC_GF

457 - BC_FF

458 - BC_WiFi

459 - BC_DMZ

460 - BC_VoIP

### Building D ###

461 - BD_GF

462 - BD_FF

463 - BD_WiFi

464 - BD_DMZ

465 - BD_VoIP

### Building E ###

466 - BE_GF

467 - BE_FF

468 - BE_WiFi

469 - BE_DMZ

470 - BE_VoIP



### VTP Domain Name ###

The VTP domain name to be used in every switch is : <b> rc23deg2 </b>

### IPv4 Configuration ###

The IPv4 addresses used by our team must be in the following range:

10.80.208.0/21 - 10.80.216.0/21 (not including)

<b>IPv4 adress assignement by building/network:</b>

The IPv4 addresses were distributed taking into account the number of nodes of each network.

Bigger blocks (smaller prefixes) are assigned first.
Please refer to the IP_Assignement excel file for details about the IP distribution

<b> Backbone IPs: </b>

Building A - 10.80.209.2

Building B - 10.80.209.3

Building C - 10.80.209.4

Building D - 10.80.209.5

Building E - 10.80.209.6

<b> ISP router IPv4 address : 121.60.202.98/30 </b>












