## Live Host Discovery
Scan Type | Example Command
------------| -----------
ARP Scan | `sudo nmap -PR -sn MACHINE_IP/24`
ICMP Echo Scan | `sudo nmap -PE -sn MACHINE_IP/24`
ICMP Timestamp Scan | `sudo nmap -PP -sn MACHINE_IP/24`
ICMP Address Mask Scan | `sudo nmap -PM -sn MACHINE_IP/24`
TCP SYN Ping Scan | `sudo nmap -PS22,80,443 -sn MACHINE_IP/30`
TCP ACK Ping Scan | `sudo nmap -PA22,80,443 -sn MACHINE_IP/30`
UDP Ping Scan | `sudo nmap -PU53,161,162 -sn MACHINE_IP/30`
`-sn` is for live host scanning without port-scan
`-n` for no DNS lookup
`-R` for reverse DNS lookup

## Port scan
Port Scan Type | Example Command
---- | -----
TCP Connect Scan | `nmap -sT 10.10.151.230`
TCP SYN Scan | `sudo nmap -sS 10.10.151.230`
UDP Scan | `sudo nmap -sU 10.10.151.230`
TCP Null Scan | `sudo nmap -sN 10.10.168.147`
TCP FIN Scan | `sudo nmap -sF 10.10.168.147`
TCP Xmas Scan | `sudo nmap -sX 10.10.168.147`
TCP Maimon Scan | `sudo nmap -sM 10.10.168.147`
TCP ACK Scan | `sudo nmap -sA 10.10.168.147`
TCP Window Scan | `sudo nmap -sW 10.10.168.147`
Custom TCP Scan | `sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.168.147`
Spoofed Source IP |`sudo nmap -S SPOOFED_IP 10.10.168.147`
Spoofed MAC Address |`--spoof-mac SPOOFED_MAC`
Decoy Scan | `nmap -D DECOY_IP,ME 10.10.168.147`
Idle (Zombie) Scan | `sudo nmap -sI ZOMBIE_IP 10.10.168.147`
Fragment IP data into 8 bytes | `-f`
Fragment IP data into 16 bytes |`-ff`

Options | Purpose
---| ---
`-p-` | all ports
`-p1-1023` | scan ports 1 to 1023
`-F` |100 most common ports
`-r` |scan ports in consecutive order
`-T<0-5>` | -T0 being the slowest and T5 the fastest
`--max-rate 50` | rate <= 50 packets/sec
`--min-rate 15` |rate >= 15 packets/sec
`--min-parallelism 100` | at least 100 probes in parallel
