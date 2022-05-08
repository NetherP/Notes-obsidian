# Network
## GUI
NetworkManager is the typical desktop network settings. It run as a service in NetworkManager.service, can be run as a CLI and have an applet for the GUI. `man NetworkManager` for more info

## ifupdown
A cli network interface manager. It works with the `ifup` and `ifdown` command tools. It read definitions from `/etc/network/interfaces` and manage `/etc/init.d/networking` init script that configures network at boot time

## wpa-supplicant
A cli wireless network interface.

## sytemd-networkd
minimal network interface management for systemd. config: /etc/systemd/network/ or /lib/systemd/network/ or /run/systemd/network/ .network files.
Run alongside systemd-resolved for the dns settings.
```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl start systemd-networkd
systemctl start systemd-resolved
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
## ip
```bash
ip addr show
ip addr show eth0
```
Display information about your network interfaces.
The second command show only for that interface(eth0).

`ip route show` show routing


### options
- `-s`: show statistics of packet.


## ifconfig
```bash
ifconfig
ifconfig interface_name
```


## route
`route` show similiar output to `ip route show`.

## nmtui
Interactive non gui command to configure network.
The package is NetworkManager-tui(`yum install NetworkManager-tui`).

enter `nmtui` as root.

## config files
NetworkManager and nmtui use the `/etc/sysconfig/network-scripts` directory for their config (network interfaces and custom routes).
`/usr/share/doc/initscripts/sysconfig.txt` show the descriptions of network-scripts config files. (`initscripts` package).


### /etc/hosts
Translating(mapping) ip to hostname.

### /etc/resolv.conf
DNS server config with `dnsname ip_addr` as the format.

