# samba-timemachine-docker

This is a docker container that contains SAMBA and is configured to provide Apple "Time Capsule" like backups.

There are three different builds:
stable - Debian Stable with latest stable SAMBA
latest - Debian SID with latest SAMBA
experimental - Debian Experimental with latest pinned version of SAMBA in Repo

To use the docker container do the following (it uses the mountpoint /backups to store your backups):

```
docker pull awlnx/samba-timemachine
docker run -d -t \
    -v /backups/timemachine:/backups:z \
    -p 10445:445 \
    --restart unless-stopped awlnx/samba-timemachine
    --name timemachine
```

Note that due to the use of port 10445 this container can be run along side a normal SAMBA service.

There is a single user called `timemachine` with a random password generated at startup (you see it with `docker logs timemachine`). By default USERID and GROUPID are set to 2000 which maybe conflicts with your running system. 

Set the environment variables USER, USERID, GROUPID AND/OR PASS to override.

```
docker run -d -t  \
    -e USER=test \
    -e PASS=test123 \
    -e USERID=1000 \
    -e GROUPID=1000 \
    -v /backups:/backups:z
    -p 10445:445 
    --name timemachine
    --restart unless-stopped awlnx/samba-timemachine
```

The container only runs smbd to find it on the network the best way is avahi (mDNS) there is an example service file included. This can be copied to /etc/avahi/services/timemachine.service or run in a container.

You can find the Repo on Github (https://github.com/awlx/samba-timemachine)

This thing was inspired by https://hub.docker.com/r/timjdfletcher/samba-timemachine/
