primary command for viewing message from systemd journal

```bash
journalctl -k
journalctl -u NetworkManager.service

journalctl --list-boots
journalctl -b c848e7442932488d91a3a467e8d92fcf
```

While some log message is stored in `/var/log/` directory, a lot can only be viewed in journal ctl

### options
- `-k`: see kernel message
- `-u name.service`: see log for this service
- `-f`: look at message as they come in(real time)
- `--list-boots`: list a boot entry, as in for a particular time the machine is booted
- `-b bootID`: used to look at that particular boot entry. Usually used after `--list-boots` to see the boot id(blob of random alphanumeric).
- 