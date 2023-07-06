 http://networking-ctfd-1.server.vta:8000
https://net.cybbh.io/public/networking/latest/index.html

Programming mode in the calculator
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Basic Fundamentals 
<p> Bit- ON/Off 1/0 (Flag of a certain aspect)
Nibble = 4 bits 
Bytes = 8 bits  (octet)
Half word= 16 Bits
Word = 32 Bits 
Double word = 64 Bits (Very long word)

Header - Information what the packet is gonna be based on the layer and PDU
Data - The physical bits of the packet 
RFC - The source of how things are layed out and how each protocol is formatted so everybody can communicate
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
OSI Layer 	        PDU 	              Common Protocols
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
7 - Application    Data                DNS 53 UDP/TCP, HTTP 443 TCP, TELNET 23 TCP ,FTP 20(command)/21(Reply), TACACS (TCP 49) Server user config, DHCP (UDP 67/68) Auto IP,
  What is actually using that layer
   Server - A socket(listening port) that is ready to recive communication
   
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
6 - Presentation    Data                SSL, TLS, JPEG, GIF
Translation/ Formating
Encoding (ASCII, EBCDIC, HEX, BASE64)
Encryption (Symmetric or Asymmetric)/ Compression

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
5 - Session         Data              NetBIOS, PPTP, RPC, NFS
SOCKS 4/5 (TCP 1080)
L2TP (TCP 1701)- Tunneling
PPTP (TCP 1723)
SMB/CIFS (TCP 139/445---SMB Rides over Netbios,   Netbios Dgram Service - UDP 138 Netbios Session Service - TCP 139

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
4 - Transport    Segment/Datagram            TCP, UDP
    TCP FLAGS - URG, ACK, PSH, RST, SYN, FIN
    Active/ Passive - Who is doing the communication, Active has the reciver send messages back but passive only has the reciver take the info and sends nothing back
    UDP - Sending information as fast as possible no matter the flag
    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
3 - Network        Packet                IP, ICMP, IGMP
  Fragmentation- Telling  the other side that there is more packets to come indecated by the MF bit
    How to calcualte the Offset  (MTU- (IHL x4 )) /8 = offset   1500- (5x4) /8 = 
    IHL (Internet Header Length) - Defalt of 5 and determins the protocol
    ECN Flag is the last nibble of the DSCP  
    Fingerprinting- Vendors have chosen different values for TTL which can provide insight to which OS family a generated packet is from.
    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
ICMP -Ping
  Echo request and reply
Zero Configuration
  APIPA - The network for the default ip (RFC 3927)
  IPv6 SLAAC (StateLess Address Auto-configuration) (RFC 4862)
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
2 - Data Link      Frames          PPP, ATM, 802.2/3 Ethernet, Frame Relay
  Data Link Sub-Layers
        MAC (Media Access Control) (PHYSICAL LAYER)
            Ether Type
              0800 = IPV4 ----
              0806 = ARP
              86DD = IPV6
        LLC (Logical Link Control) (Point to Point connection magager(switch)
              8100 = VLAN TAG ADDs an extra 2 Bytes for VLAN id 
Trunking - Encoding on both sides of a switch so they can pass inforamtion on the same wire

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
1 - Physical        Bits          Bluetooth, USB, 802.11 (Wi-Fi), DSL, 1000Base-T
  Physical Layer Responsibilities - What is the Device? Is it on? Where is it at?
    Hardware Specifications
    Encoding and Signaling
    Data Transmission and Reception
    Physical Network Design
    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Network Traffic Sniffing
Capture Library
    What makes traffic capture possible?
        Libpcap --Linux lib
        WinPcap ---Windows lib
        NPCAP ---Current lib for both
Wireshark, TShark, TCPDump and BPFs
pref -Protocols -TCP - check boxes to what might work
CTRL + F --search string --Packet Bytes
Cloud Shark
TCPDUMP - Check if its installed with which tcpdump 
  sudo tcpdump -i ens3 not port 22 -nvvXX
  -n ---does not name resolute
  -vv --very verbose
  -XX --shows the actual Bytes
  tcpdump -D  --- Shows interfaces
  tcpdump -w something.pcap ----writes it to the file specified
  tcpdump -r something.pcap ----reads the file specified, can be combined with native linux commands
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Berkeley Packet Filters (BPF) 
  -Looks at each Bytes
  -Using BPFs with operators, bitmasking, and TCPDump creates a powerful tool for traffic filtering and parsing.
  tcpdump {A} [B:C] {D} {E} {F} {G}

A = Protocol (ether | arp | ip | ip6 | icmp | tcp | udp)
B = Header Byte offset
C = optional: Byte Length. Can be 1, 2 or 4 (default 1)
D = optional: Bitwise mask (&)
E = Operator (= | == | > | < | <= | >= | != | () | << | >>)
F = Result of Expresion
G = optional: Logical Operator (&& ||) to bridge expressions

Example:
tcpdump 'ether[12:2] = 0x0800 && (tcp[2:2] != 22 && tcp[2:2] != 23)'
Bitwise Masking

To filter down to the bit(s) and not just the byte.
ip[0] & 0x0F > 0x05
ver ihl bpf

Filter Logic - Most exclusive
-All designated bit values must be set; no others can be set
tcp[13] = 0x11
--or--
tcp[13] & 0xFF = 0x11
most bpf

Filter Logic - Less exclusive
All designated bits must be set; all others may be set
tcp[13] & 0x11 = 0x11

Filter Logic - Least exclusive
At least one of the designated bits must be set to not equal 0; all others may be set
tcp[13] & 0x11 !=0

tcpdump -r something.pcap 'tcp[13] & 0x10 = = 0x10' ----Look for any ack
tcpdump -r soemthing.pcap 'ip[1] & 0xFC = 4' ----Look for the DSCP 

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
LAYER 2 SWITCHING TECHNOLOGIES
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
SWITCH OPERATION
Fast Forward - Only Destination MAC
Fragment Free - First 64 bytes
Store and Forward - Entire Frame and FCS
  CAM TABLE ---The table that lists the prots with the corasponding mac
    Static -- Person manually set
    Dynamic --Protocol set
SPANNING TREE PROTOCOL (STP)
  VLAN Trunking Protocol (VTP) 
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
LAYER 3 ROUTING TECHNOLOGIES
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Routing tables
Network address connections on interfaces, telling what port to send things out of




















