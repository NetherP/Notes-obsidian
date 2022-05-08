Manage logging from various programs and write them to the appropriate log files in `/var/log`.

### config files
`/etc/rsyslog.conf`
to redirect log files to another computer, change the location in the rules section into @(`@loghost` for example).

## logwatch
`yum install logwatch`
logwatch gather the logs and send to email what looks like a problem.

### config
`/etc/logwatch/conf/logwatch.conf`