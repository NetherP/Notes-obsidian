# Service
The classic linux use sysVini(or `init` in the process name), while the more modern linux use systemd for its initialization daemon.

## References

-   [Difference between Systemctl and service command - Stack Overflow](https://stackoverflow.com/questions/43537851/difference-between-systemctl-and-service-command#:~:text=service%20operates%20on%20the%20files,file%20in%20%2Fetc%2Finit.)
-   [Systemctl Cheatsheet (github.com)](https://gist.github.com/adriacidre/307d2f9f5179fc748f22edac5af3d218) (A small cheatsheet you wanna read before working with systemctl).


To get service in systemd you can use
```bash
systemctl list-units
systemctl list-units-file --type=service
systemctl list-units-file --type=target
```
The first one list all units, the second  and third one list unit configuration file available for services and targets respectively.
The configuration file can also be found in `/lib/systemd/system` or `/etc/systemd/system` directory.

To see various unit a target unit will activate use this command:
```bash
systemctl show --property "Wants" multi-user.target
systemctl show --property "Wants" multi-user.target | fmt -10 | sed 's/Wants=//g' | sort
```
The second command is formatted to be easier to follow.

## Checking service

Checking all offered service with this command:
```bash
### init
chkconfig --list 
### systemd
systemctl list-unit-files --type=service | grep -v disabled 
```

Check if the service is running:
```bash
### init
service --status-all | grep running... | sort
### systemd
systemctl status cups.service # for cups service
```

## Start and Stop service
For SysVinit(`init`):
```bash
service cups status #check the status
service cups stop #stop(exit) service
service cups start #start service
service cups restart #restart service
service cups reload # reload the config 
```

For systemd:
```bash
systemctl status cups.service #check status
systemctl stop cups.service #stop service
systemctl start cups.service #start service
systemctl restart cups.service #restart service
systemctl condrestart cups.service #restart only if active before
systemctl reload sshd.service # reload config
```

## Enabling Persistent
Enabling means adding the service to be started automatically at startup at boot time or for particular runlevel.

For sysVinit:
```bash
chkconfig --list cups #check if service is enabled at all runlevel
chkconfig --level 3 cups on #enable at runlevel 3
chkconfig --level 2345 cups on #enable at runlevel 2,3,4,5
chkconfig --level 5 cups off #disable at runlevel 5
```

For systemd:
```bash
systemctl enable cups.service #enable to start at boot
systemctl disable cups.service #disable to stop start at boot
```
Enable make some symlink in `/etc/systemd/system`, so you can check there to see what service start. But using the `systemctl` command is recommended.
Disable also doesn't stop the service immediatle, so use `systemctl stop daemon.service` to stop the service immediately.

To disable static service, use this command:
```bash
systemctl mask NetworkManager.service
systemctl unmask NetworkManager.service # re-enable static service
```


### Edit
`systemctl edit httpd` can be used to create an overriding conf files, either replacing or appending a configuration.