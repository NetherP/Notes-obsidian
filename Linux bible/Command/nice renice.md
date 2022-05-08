## nice
Start process with a particular nice value.
```bash
nice -n +5 updatedb &
nice -n nice_number command
```
## renice
Change running program nice value.
```bash
renice -n -5 20284
renice -n nice_number pid
```
Change by pid.