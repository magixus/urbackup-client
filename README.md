# UrBackup Docker Client
Source files for the Docker image magixus/urbackup-client at https://hub.docker.com/r/magixus/urbackup-client/.

## Introduction
The UrBackup solution has a comprehensive list of featuers and supported systems. The official website states that

> UrBackup is an easy to setup Open Source client/server backup system, that through a combination of image and file backups accomplishes both data safety and a fast restoration time.

This Docker image runs a UrBackup client `v2.4.10`. Note that it does not run the UrBackup server. Please see https://www.urbackup.org for more information on the complete UrBackup solution.

## Starting the Docker container
if the server is installed on local machine and you want to backup a local directory, you may start the container by running

```
docker run \
  -d --name urbackup-client \
  -p 35621:35621 -p 35622:35622 -p 35623:35623 \
  -v /path/on/host:/backup \
  -h <your-client-computer-name>
  magixus/urbackup-client:2.4.10
```

This will start the UrBackup client and automatically connect to any UrBackup servers on the local network. The necessary ports are forwarded from the Docker container. By default, the container will backup files found in ```/backup``` and all its sub directories. Mounting a host directory to that path enables the client to backup the files. Note the `-h <your-client-computer-name>`  is used to recognize the host on urbackup server web interface

In order to connect the client to a server outside of the local network, a number of additional parameters as environment variables are needed:

```
docker run \
  -p 35621:35621 -p 35622:35622 -p 35623:35623 \
  -v /path/on/host:/backup \
  -e COMPUTERNAME=<your-client-computer-name> \
  -e INTERNET_MODE_ENABLED=true \
  -e INTERNET_SERVER=<your-server-name-or-ip> \
  -e INTERNET_AUTHKEY=<your-client-authkey> \
  magixus/urbackup-client:2.4.10
```

There is also an optional parameter ```INTERNET_SERVER_PORT``` that defaults to `55415`.
