#RCOMP - SPRINT 3 - PLANNING #

### Sprint Master: Monika Zemankiewicz - 1220451

<hr>

## Sprint Backlog

T.3.1 - Update the buildingA.pkt layer three Packet Tracer simulation from the previous sprint, to include the described features in this sprint for building A.

T.3.2 - Update the buildingB.pkt layer three Packet Tracer simulation from the previous sprint, to include the described features in this sprint for building B. Final integration of each member’s Packet Tracer simulation into a single simulation (campus.pkt).

T.3.3 - Update the buildingC.pkt layer three Packet Tracer simulation from the previous sprint, to include the described features in this sprint for building C.

T.3.4 - Update the buildingD.pkt layer three Packet Tracer simulation from the previous sprint, to include the described features in this sprint for building D.

T.3.5 - Update the buildingE.pkt layer three Packet Tracer simulation from the previous sprint, to include the described features in this sprint for building E.

<hr>

## Technical Decisions

### OSPF Areas

0 - backbone area

1 - Building A

2 - Bulding B

3 - Bulding C

4 - Building D

5 - Building E

### VoIP phone numbers

We are using automatic registration and automatic directory numbers assignment.

<b>Prefix digits:</b>

1 - Building A

2 - Building B

3 - Building C

4 - Building D

5 - Building E

<b>Destination pattern:</b>

[Bulding prefix]...

### DNS domain names

highest level domain: <b> .rcomp-22-23-de-g2 </b>

Building A - rcomp-22-23-de-g2

Building B - building-B.rcomp-22-23-de-g2

Building C - building-C.rcomp-22-23-de-g2

Building D - building-D.rcomp-22-23-de-g2

Building E - building-E.rcomp-22-23-de-g2

### DNS servers IPv4 addresses

Building A:
10.80.209.131/25

Building B:
10.80.214.163/28

Building C:
10.80.214.67/27

Building D:
10.80.214.179/28

Building E:
10.80.214.99/27



<hr>

## Cisco Packet Tracer

All the simulations of the layer two and layer three configurations must be made using Cisco
Packet Tracer.

Version of the Cisco Packet Tracer software: Version 8.2.0.0162
