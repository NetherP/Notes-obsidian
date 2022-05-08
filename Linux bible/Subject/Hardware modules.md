Modules is what linux called a driver in windows(i think).

`lsmod` list all loaded modules.

`modinfo` can be used to see information about the modules
```bash
/sbin/modinfo -d namesofmodule
/sbin/modinfo -d e1000
```
- `-d` for the module information/description
- `-a` see author of the module
- `-n` object file representing the module

## Loading modules
You can load module that has been compiled and installed(located at /lib/modules subdirectory) using `modprobe`.

```bash
modprobe parport
modprobe parport_pc io=0x3bc irq=auto
```

modprobe loades module temporarily. They disappear after reboot. Add modprobe as a startup script to permanently load a module.

## Removing modules
You can use `rmmod` to remove a modules from a running kernel. You can also use `modprobe -r` to remove a module, as rmmod can't remove a module that depend on other module.
```bash
rmmod parport
modprobe -r usbcore
```


