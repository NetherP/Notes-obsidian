Its like VM, but use the host kernel instead of having its own kernel. Basically more weightlight VM. Usually used for a single service/application and then discarded.
Tools to use it is `docker` and `podman`. 
You can get the base image(image to build from) from docker.com or search for UBI(Universal Base Image) for each OS.

### Install
```bash
yum install docker -y
systemctl start docker
systemctl enable docker

yum install podman -y
```
Podman doesn't need a daemon to be used.
Most docker command can be substituted with podman.

### Pulling and running contaner
Pull with:
```bash
podman pull PULL_LINK
podman pull registry.access.redhat.com/ubi8/ubi

podman images #to see available image
```

Run it with:
```bash
podman run -it IMAGE_ID/NAME bash
podman run -it 096cae65a207 bash
```
`-i` for interactive session
`-t` for terminal. Run both to get interactive session with the container from the bash shell.

For an example of running an FTP server from a container this is the command:
```bash
podman run -d -p 20:20 -p 21:21 -p 21100-21110:21100-21110 -v /etc/vsftpd/:/etc/vsftpd/ -v /var/ftp/pub:var/ftp/pub --name vsftpd vsftpd
```
`-d` runs the container in detached mode(background)
`-p` Publish the port inside the container to the host port(i.e tcp 20, 21, and 21100-21110) with format host:container
`--rm` remove the container when it exit
`-v` bind-mount the folder in the host-container format.
`--name` set the name of the container.

### starting and stopping containers
check the container with:
```bash
podman ps
podman ps -a
```
`-a` show all container, including the already stopped one
Start container with:
```bash
podman start -a 7da88bd62667
podman stop 4d6be3e63fe3
podman start 4d6be3e63fe3
```
`-a` basically restart the previous stopped session. Withouth it, the container would run in detached mode.

### Building container image
You only need a Dockerfile(describing how to build the image) and all content that you want to be included in the image.
#### simple container image
1. Create a directory to hold your container project and enter that directory
2. copy the content you want in your image, in the example you build a script called `cworks.sh`.
3. Create a file named `Dockerfile` that contain an instruction to build the image. Here is an example:
```Dockerfile
FROM registry.access.redhat.com/ubi7/ubi-minimal
COPY ./cworks.sh /usr/local/bin/
CMD ["/usr/local/bin/cworks.sh"]
```
4. Build the container image and cal it `myproject` with `podman build -t myproject .`
5. Run it to make sure it works `podman run 6837ec3a37a241`.

#### FTP container from Github
1. clone the project from github: `git clone https://github.com/container-images/vsftpd.git` and enter that directory
2. Modify the Dockerfile as needed, for example change the base image to fedora.
3. build it with `podman build -t vsftpd .`