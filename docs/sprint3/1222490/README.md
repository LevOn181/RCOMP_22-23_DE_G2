1222490 - RCOMP SPRINT 3
+========================+

In this sprint the following configurations were established:
• OSPF dynamic routing
• DHCPv4 service
• VoIP service
• Adding a second server to each DMZ network to run the HTTP service.
• Configuring DNS servers to establish a DNS domains tree.
• Enforcing NAT (Network Address Translation).
• Establishing traffic access policies on routers (static firewall).

1. OSPF dynamic routing
 network 10.80.212.64 0.0.0.63 area 3
 network 10.80.212.128 0.0.0.63 area 3
 network 10.80.213.64 0.0.0.63 area 3
 network 10.80.213.224 0.0.0.31 area 3
 network 10.80.214.64 0.0.0.31 area 3
 network 10.80.209.0 0.0.0.127 area 0

2. HTTP server
-HTTP server added with IP 10.80.214.68.
-Simple HTML page created as index.html

3. DHCPv4 service
-Each subnetwork has it's own DHCP pool:
    Ground Floor: 
         network 10.80.213.64 255.255.255.192
         default-router 10.80.213.66
         dns-server 10.80.214.67
         domain-name server1
    First Floor: 
         10.80.212.128 255.255.255.192
         network 10.80.212.128 255.255.255.192
         default-router 10.80.212.130
         dns-server 10.80.214.67
         domain-name server1
    WiFi: 
         network 10.80.212.64 255.255.255.192
         network 10.80.212.64 255.255.255.192
         default-router 10.80.212.66
         dns-server 10.80.214.67
         domain-name server1
    VoIP: 
        network 10.80.213.224 255.255.255.224
        default-router 10.80.213.226
        option 150 ip 10.80.213.226
        
-All device connected with successful IP distribution.

4. VoIP service
-For the Voice over IP I used Cisco's 7960 IP phone with the following configuration:

 Voice VLAN enabled on connected switch
 Switchport in access mode
 Switchport access vlan disabled
 option 150 used on DHCP
 
 telephony-service
 max-ephones 2
 max-dn 2
 ip source-address 10.80.213.226 port 2000
 auto assign 1 to 2

ephone-dn 1
 number 3001

ephone-dn 2
 number 3002
 
-Reciving and initating calls with other buildings successfully.

5. DNS
-DNS configurations:
     Local DNS domain name: building-C.rcomp-22-23-de-g2
     Configuration visible on image [dnsconfig.png]
     
 NS and other DNS records configured.
 End users connectod to DNS server.
 

6. NAT
-Static NAT traffic redirection applied. Configuration:
    ip nat inside source static tcp 10.80.214.68 80 10.80.209.4 80 
    ip nat inside source static tcp 10.80.214.67 53 10.80.209.4 53 
    ip nat inside source static udp 10.80.214.67 53 10.80.209.4 53 
    ip nat inside source static tcp 10.80.214.68 443 10.80.209.4 443 
    ip classless
    ip route 0.0.0.0 0.0.0.0 10.80.209.1 
    
 TCP requests sent to HTTP server on port 80, 443
 TCP, UDP requests sent to DNS server on port 53

7. Static Firewall
-Firewall is capable of filtering out the following threats:
    Internal, external spoofing
    All traffic is blocked to the DMZ server (except HTTP, HTTPS requests). 
    All incoming traffic from the DMZ is enabled.
    All traffic directed to the router is blocked except for the traffic required for the following features:
     DHCPv4, TFTP, ITS, OSPF, NAT
    All ICMP echo requests and replies are allowed.
    All remaining traffic is enabled.
    
The devices specification is to be found in the Configuration Dump.