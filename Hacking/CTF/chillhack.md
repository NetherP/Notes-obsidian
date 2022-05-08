A Very hard easy machine
First off, there is a RCE console in the webserver, though it is filtered, so you can't create a reverse shell easily.

Inside as www, you can sudo as apaar for a shell script that is vulnerable. Exploiting it gives you apaar user. Create a ssh key for easier access

This step confuse me, because this user actually don't have any access to privilege escalation.

The privilege escalation is available in a hidden file in the picture that you can get from the RCE console webpage. The hidden file is a php file, containing a base64 password for anurodh.

As anurodh, you have docker privilege. As docker is run by root `ps -aux | grep docker` , you can maybe mount the root filesystem inside docker to get the root flag. Run the container with:
`docker run -it --rm -v /root:/mnt/root alpine /bin/sh`