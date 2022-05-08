## kill
```bash
kill pid
kill -signumber pid
kill -9 10432
```
Kill send a signal to a process. By default, it send SIGTERM signal. Various signal is:
![[Linux Signal.png||650]]
Or use `man 7 signal` to read about others available signal.
Use the middle for SIGCONT and SIGSTOP(x86 & Power architecture).

## killall
```bash
killall process-name
killall -HUP gnome-shell
```
Similiar to kill, but use a process name instead.