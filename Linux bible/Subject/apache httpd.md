The most popular web server(apache).
`httpd` is the daemon that run on the web server, waiting for request from clients.


### config files
`/etc/httpd/conf/httpd.conf` is the main conf file.
`/etc/httpd/conf.d/welcome.conf`  defines the default homepage for our website, until you add some content.
`/etc/httpd/conf/magic` defines rules the server can use to figure out a file's type when the server tries to open it.
Most module packages have their conf files in the `/etc/httpd/conf.d` directory.

### modules
apache is a modular server and many features are implemented by external modules that the main program loads during its initialization. You can work with module with this command:
```bash
a2enmod _module_ #enable _module_
a2dismod module  #disable module
```

### Web Server packages
It's preferred to install some other packages that are associated with the httpd package to begin setting up web server.
In fedora, there is the "Web Server" package group that can be installed by:
`yum groupinstall "Web Server"`

Some notable package in this group:
- httpd-manual: Detailed apache documentation. Can be accessed in http://localhost/manual after the httpd service is started.
- mod_ssl: Module and config files for SSL(https). The config files is `/etc/httpd/conf.d/ssl.conf`
- crypto-utils: generating keys and certificate.
- mod_perl: perl module.
- php: php module.
- squid: proxy service for specific protocol.
- webalizer: analyzing web server data.

### Starting
Starting the httpd service:
```bash
service httpd start #init

systemctl start httpd.service #systemd
```

### Securing Apache
apache user and group have access to `/var/www/html` directory, which is the directory where HTML content is stored by default.(Set in the `DocumentRoot` value in httpd.conf).

Don't forget to set firewall to open port 80 and 443(if https). The firewall in old linux is in iptables, while newer linux use nftables. google it.

Turn off SELinux or make it run on permissive mode to ensure everythin works(unsafely). Change SELinux in: `/etc/sysconfig/selinux`.

### Virtual host
A way to serve multiple domain name in a single server. For example, example.com and example.org will have different content depending on their Virtual Host container, even though they connect to the same IP or server.
The setting can be set up by creating a conf files int `/etc/httpd/conf.d/` with a VirtualHost container in it.


### Troubleshooting
You can check your config by running:
```bash
apachectl configtest
apachectl graceful #This restart the server, re-reading the new config.
```

