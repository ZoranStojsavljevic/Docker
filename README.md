### Docker Tutorial
* [Docker Tutorial for WIN10/macOS](https://www.youtube.com/watch?v=pTFZFxd4hOI)

### Docker Container Tutorial by Zoran Stojsavljevic
* [Docker Container Tutorial by Zoran IN PROGRESS](https://github.com/ZoranStojsavljevic/Docker_Container)

### Docker Compose Tutorial
* [Docker Compose Tutorial](https://www.youtube.com/watch?v=HG6yIjZapSA)

### Installing docker on F39
```
vuser@fedora39-ssd-2TB:~$ sudo dnf install docker
Fedora 39 - x86_64 - VirtualBox                                              13 kB/s | 819  B
Fedora 39 - x86_64 - VirtualBox                                             5.7 kB/s | 1.7 kB
Fedora 39 - x86_64 - VirtualBox                                              22 kB/s | 819  B
Error: Failed to download metadata for repo 'virtualbox': repomd.xml GPG signature verification error: Signing key not found
Ignoring repositories: virtualbox
Last metadata expiration check: 0:10:00 ago on Sat 16 Dec 2023 11:32:32 AM CET.
Modular dependency problem:

Dependencies resolved.
======================================================================
 Package	Architecture	Version		Repository	Size
======================================================================
Installing:
 moby-engine	x86_64		24.0.5-1.fc39	fedora		29 M
Installing
dependencies:
 containerd	x86_64		1.6.23-2.fc39	updates		39 M
 libnet		x86_64		1.3-1.fc39	updates		60 k
 runc		x86_64		2:1.1.8-1.fc39	fedora		3.1 M
Installing weak
dependencies:
 criu		x86_64		3.19-2.fc39	updates		560 k

Transaction Summary
======================================================================
Install  5 Packages

Total download size: 72 M
Installed size: 257 M
```
### Runnig system service docker
```
vuser@fedora39-ssd-2TB:/etc$ sudo find . -iname docker*
./sysconfig/docker
./systemd/system/sockets.target.wants/docker.socket

vuser@fedora39-ssd-2TB:/etc$ sudo find . -iname mysql*
./dnf/modules.d/mysql.module
./logrotate.d/mysqld
./systemd/system/multi-user.target.wants/mysqld.service

vuser@fedora39-ssd-2TB:/etc$ systemctl status docker.socket 
○ docker.socket - Docker Socket for the API
     Loaded: loaded (/usr/lib/systemd/system/docker.socket; enabled; preset: enabled)
     Active: inactive (dead)
   Triggers: ● docker.service
     Listen: /run/docker.sock (Stream)

vuser@fedora39-ssd-2TB:/etc$ sudo systemctl start docker.socket 

vuser@fedora39-ssd-2TB:/etc$ systemctl status docker.socket 
● docker.socket - Docker Socket for the API
     Loaded: loaded (/usr/lib/systemd/system/docker.socket; enabled; preset: enabled)
     Active: active (listening) since Sat 2023-12-16 12:24:38 CET; 3s ago
   Triggers: ● docker.service
     Listen: /run/docker.sock (Stream)
      Tasks: 0 (limit: 14144)
     Memory: 0B
        CPU: 1ms
     CGroup: /system.slice/docker.socket
```
### sudo docker version
```
vuser@fedora39-ssd-2TB:~$ sudo docker version
Client:
 Version:           24.0.5
 API version:       1.43
 Go version:        go1.21.0
 Git commit:        %{shortcommit_cli}
 Built:             Sun Aug 27 16:45:40 2023
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          24.0.5
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.21.0
  Git commit:       %{shortcommit_moby}
  Built:            Sun Aug 27 16:45:40 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.23
  GitCommit:
 runc:
  Version:          1.1.8
  GitCommit:
 docker-init:
  Version:          0.19.0
  GitCommit:
```
### Docker repositories with images
* [docker images](https://hub.docker.com/)

#### Registration https://hub.docker.com/

	user id: niemande
	Joined December 17, 2023
	Email Address: nobodyless@gmail.com (Verified)

#### How to pull docker image from docker repositories?

Explore option:

	https://hub.docker.com/search?q=
	sudo docker pull <image name>

Example:

	sudo docker pull dokken/fedora-31

#### Dockerfile

The explanation: dockerfile describes docker environment.

### Run Docker Image
* [Run Docker Image](https://www.stacksimplify.com/aws-eks/docker-basics/get-docker-image-from-docker-hub-and-run-/)

This documentation is based on the following lines:

List of running containers with image names:

	sudo docker ps
	<CONTAINER_ID>	<IMAGE_NAME>	COMMAND	CREATED	STATUS	PORTS	<CONTAINER_NAME>

List of inactive docker images with image names:

	runningImages=$(docker ps --format {{.Image}})
	docker images --format {{.Repository}}:{{.Tag}} | grep -v "$runningImages"

List of docker images in the system:

	sudo docker image ls
	REPOSITORY/<IMAGE_NAME>	TAG	<IMAGE_ID>	CREATED	SIZE

### [Flow-1]: Pull Docker Image from Docker Hub and Run it

#### [Step-1]: Verify Docker version and also login to Docker Hub

	sudo docker version
	sudo docker login

#### [Step-2]: Pull Image from Docker Hub

	sudo docker pull stacksimplify/dockerintro-springboot-helloworld-rest-api:1.0.0-RELEASE

#### [Step-3]: Run the downloaded Docker Image & Access the Application

Fetch the docker image from Docker Hub:

	sudo docker run <IMAGE_NAME>

Example by the book:

	sudo docker run --name app1 -p 80:8080 -d stacksimplify/dockerintro-springboot-helloworld-rest-api:1.0.0-RELEASE
	http://localhost/hello

Example modified from the book!

	sudo docker run --rm -it -u $(id -u):$(id -g) -e USER=$USER -e HOME=$HOME -v $HOME:$HOME -v $PWD:$PWD -w $PWD dokken/fedora-31 /bin/bash

Real example (simple one)!

	sudo docker run <dokken/fedora-31 <<===== IMAGE_NAME>
	sudo docker run dokken/fedora-31 /bin/bash

Real example (complex one)!

	sudo docker run --rm -it -e USER=$USER -h $HOSTNAME -e HOME=$HOME -v $HOME:$HOME -v $PWD:$PWD -w $PWD dokken/fedora-31 /bin/bash
	sudo docker exec -it <pedantic_yalow <<===== CONTAINER_NAME> /bin/bash

### [Flow-2]: Handling containers!

#### [Step-4]: List Running Containers

	sudo docker ps
	sudo docker ps -a
	sudo docker ps -a -q

#### [Step-5]: Exit Docker Container without Stopping It

	If you want to exit the container's interactive shell
	session, but do not want to interrupt the processes
	running in it, press Ctrl+P followed by Ctrl+Q.

	This operation detaches the container and allows you
	to return to your system's shell.

	Exit Running Docker Container: Ctrl+P and Ctrl+Q.

#### [Step-6]: Connect to Container Terminal

	sudo docker exec -it <CONTAINER_NAME> /bin/bash

##### Integrated Example

	sudo docker ps
	sudo docker ps -a
	sudo docker run --rm -it -e USER=$USER -h $HOSTNAME -e HOME=$HOME -v $HOME:$HOME -v $PWD:$PWD -w $PWD <dokken/fedora-31 <<===== IMAGE_NAME> /bin/bash
	<Ctrl+P & Ctrl+Q> (exit from running container)
	sudo docker ps -a
	sudo docker exec -it <pedantic_yalow <<===== CONTAINER_NAME> /bin/bash
	OR
	sudo docker attach <container-name-or-id>

#### [Step-7]: Container Stop, Start

	sudo docker stop <CONTAINER_NAME>
	sudo docker start <CONTAINER_NAME>

#### [Step-8]: Remove Container

	sudo docker stop <CONTAINER_NAME>
	sudo docker rm <CONTAINER_NAME>
	sudo docker rm -f <CONTAINER_NAME> (forced removal)

	sudo docker rm <crazy_golick <<===== CONTAINER_NAME>

#### [Step-9]: Remove Image

	sudo docker images
	sudo docker rmi <IMAGE_NAME/IMAGE_ID>

	sudo docker rmi <77310cafb345 <<===== IMAGE_ID>
