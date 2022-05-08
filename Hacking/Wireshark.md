# Wireshark
![[Pasted image 20220501113553.png]]
Above is color coding that is available on wireshark
## Collection method
There is various ways to get the PCAP file(network traffic capture). 
### 1. Network Taps
Physically tap the network cable. Two methods for tapping network:
 - Intercept traffic as it comes accross: Vampire tap
 - Inline network tap, replicate the packet as it passes the tap: Throwing star Lan Tap
### 2. Mac Flooding
### 3. ARP Poisoning
Pose as another NIC, by spoofing its MAC address. Thus the traffic will be directed to your device, acting as a middleman.

## Filtering Captures
Display filter is easier than capture filter. The syntax:
### Operator:
-   and - operator: and / &&
-   or - operator: or / ||
-   equals - operator: eq / ==
-   not equal - operator: ne / !=
-   greater than - operator: gt /  >
-   less than - operator: lt / <
[Wireshark Filtering Documentation](https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html)

## Basic Filtering
By IP (source/destination): `ip.addr == <IP Address>`
By source/destination: `ip.src == <SRC IP Address> and ip.dst == <DST IP Address>`
By TCP Protocols: `tcp.port eq <Port #> or <Protocol Name>`
By UDP Protocols: `udp.port eq <Port #> or <Protocol Name>`

## Packet Dissection
Wireshark break down packet based on OSI model. For example:
![[Pasted image 20220501111318.png]]
The image above show that wireshark separate the packet into 7 distinct layer:
- Frame/packet
- MAC source/destination
- IP source/Destination
- Protocol, containing src/dest port
- Protocol Error
- Application Protocol
- Application data

## https packet analysis
As the https is encrypted, you can decrypt the data by adding the key to ***edit > preferences > Protocols > TLS > +*** then add the fields with:
IP Address: DEST_IP
Port: start_tls
Protocol: http
Keyfile: RSA key location