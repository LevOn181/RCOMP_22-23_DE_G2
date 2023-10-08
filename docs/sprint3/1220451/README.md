# B - Monika Zemankiewicz 1220451

## 1. OSPF Dynamic Routing

OSPF area for all building B networks: 2

Backbone: 0

```
router ospf 1

log-adjacency-changes

network 10.80.209.0 0.0.0.127 area 0 <- backbone

network 10.80.208.0 0.0.0.127 area 2 <- Wifi

network 10.80.211.128 0.0.0.63 area 2 <- FF

network 10.80.213.192 0.0.0.31 area 2 <- GF

network 10.80.214.128 0.0.0.15 area 2 <- VoIP

network 10.80.214.160 0.0.0.15 area 2 <- DMZ

```

## 2. HTTP Servers

HTTP Server IPv4: 10.80.214.164/28

![webpage](Screenshots/http_server_page.png)


## 3. DHCPv4 Service

DMZ network and the backbone use static and manually set node addresses.

DHCP is enabled for all other networks in the building.

For VoIP VLAN the DHCP server configuration includes option 150.

## 4. VoIP Service

Building B pattern: 2...

max-ephones number was set to 2.

Phones model: 7960

We use call forwarding based on buildings prefixes specified in the general planning document. 

### Phone numbers used:

ephone-dn 1 --> number 2111

ephone-dn 2 --> number 1221

#### Configuration:

-Voice VLAN enabled

-Access VLAN disabled

-Acces mode enabled

-Call forwarding enabled based on destination patterns:

```
dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:10.80.209.2
!
dial-peer voice 2 voip
destination-pattern 3...
session target ipv4:10.80.209.4
!
dial-peer voice 3 voip
destination-pattern 4...
session target ipv4:10.80.209.5
!
dial-peer voice 4 voip
destination-pattern 5...
session target ipv4:10.80.209.6

```

## 5. DNS

All end-nodes within a building are using the local DNS name server.

For servers that information was configured manually. 

![dns.png](Screenshots/dns.png)




## 6. NAT

The following static NAT configuration was implemented:

### HTTP and HTTPS requests:

```

ip nat inside source static tcp 10.80.214.164 80 10.80.209.3 80 

ip nat inside source static tcp 10.80.214.164 443 10.80.209.3 443 

```

### DNS requests:

```

ip nat inside source static tcp 10.80.214.163 53 10.80.209.3 53 

ip nat inside source static udp 10.80.214.163 53 10.80.209.3 53 

```

## 7. Static Firewall (ACLs)

#### First floor:

```
ip access-list extended FF
 permit ip 10.80.211.128 0.0.0.63 any
 permit icmp any any echo
 permit icmp any any echo-reply
 permit ip host 0.0.0.0 host 0.0.0.0
 permit udp host 0.0.0.0 eq bootpc host 25.255.255.255 eq bootps
 permit ospf any any
 deny ip any host 10.80.208.2
 deny ip any host 10.80.213.194
 deny ip any host 10.80.214.130
 deny ip any host 10.80.214.162
```

#### Ground floor:

```
ip access-list extended GF
 permit ip 10.80.213.192 0.0.0.31 any
 permit icmp any any echo
 permit icmp any any echo-reply
 permit ip host 0.0.0.0 host 0.0.0.0
 permit udp host 0.0.0.0 eq bootpc host 255.255.255.255 eq bootps
 permit ospf any any
 deny ip any host 10.80.208.2
 deny ip any host 10.80.211.130
 deny ip any host 10.80.214.130
 deny ip any host 10.80.214.162

```
 
#### WiFi:

```
ip access-list extended WiFi
 permit ip 10.80.208.0 0.0.0.127 any
 permit icmp any any echo
 permit icmp any any echo-reply
 permit ip host 0.0.0.0 host 0.0.0.0
 permit udp host 0.0.0.0 eq bootpc host 255.255.255.255 eq bootps
 permit ospf any any
 deny ip any host 10.80.211.130
 deny ip any host 10.80.213.194
 deny ip any host 10.80.214.130
 deny ip any host 10.80.214.162
```

#### VoIP:

```
ip access-list extended VoIP
 permit icmp any any echo
 permit icmp any any echo-reply
 permit ip host 0.0.0.0 host 0.0.0.0
 permit udp host 0.0.0.0 eq bootpc host 255.255.255.255 eq bootps
 permit ospf any any
 deny ip any host 10.80.208.2
 deny ip any host 10.80.211.130
 deny ip any host 10.80.213.194
 deny ip any host 10.80.214.162
```

#### DMZ: 

```
ip access-list extended DMZ
 permit icmp any any echo
 permit icmp any any echo-reply
 permit tcp any host 10.80.214.164 eq www
 permit tcp any host 10.80.214.164 eq 443
 permit udp any host 10.80.214.163 eq domain
 permit udp any eq domain host 10.80.214.163
 permit tcp any host 10.80.214.163 eq domain
 permit tcp any eq domain host 10.80.214.163
ip access-list extended BB
 deny ip 10.80.208.0 0.0.0.127 any
 deny ip 10.80.211.128 0.0.0.63 any
 deny ip 10.80.213.192 0.0.0.31 any
 deny ip 10.80.214.128 0.0.0.15 any
 deny ip 10.80.214.160 0.0.0.15 any
 permit udp host 0.0.0.0 eq bootpc host 255.255.255.255 eq bootps
 permit ospf any any
 permit tcp any host 10.80.209.3 eq www
 permit tcp any host 10.80.209.3 eq 433
 permit tcp any host 10.80.209.3 eq domain
 permit udp any host 10.80.209.3 eq domain
 deny ip any host 10.80.208.2
 deny ip any host 10.80.211.130
 deny ip any host 10.80.213.194
 deny ip any host 10.80.214.130
```


