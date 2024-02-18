# Docker
- Docker : namespace + cggroup + 
- Namespace - Limits what you can see. Isolation Ip.
- cgGroup - Limits how much you can use. Limit Memory.

## Installation
References
https://docs.docker.com/
https://docs.docker.com/engine/install/ubuntu/

> Way 1
```
# 1. Update apt
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
# 2. Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# 3.
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 4. install
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

```
> Easy Way
```
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
```

### Uninstalling 
```
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

## Commands
> Basic commands

| Command | Description | 
| --- | --- |
| docker inspect | Get addition details of specific container |
| docker images | list images |
| docker ps  | list container |
| docker ps -a | list all containers (including exited one) |
| docker rmi imageId/name | Remove image |
| docker rm ContainerId/name | Remove container |
| docker history imageId/name | history related to specific image |

> Commands related containers

| Command | Description | 
| --- | --- |
| docker run imageId/name | run image in attach mode |
|  -d |  Run in dettach/daemon mode in backgroud |
|  --name |  Give name to container |
| docker attach imageId/name | Re-attach container  |
| docker stop Name/containerId | Stop running container |
| docker exec Name/containerId <COMMANDS> | Execute commands |

> Run
```
docker run <OPTIONS> imageId/name [COMMAND]
	OPTIONS
	: -it interractive terminal
	: -p 5000:80 hostPort:containerPort
	: -v hostLoc:containerLoc  :-Volume or directory mapping	
	: -d Dettach/daemon mode / Run in backgroud
	: -e Set environment var , -e APP_COLOR=blue
	: --name Set name to container

#Eg types:
docker exec mysql-db mysql -pdb_pass123 -e 'use foo; select * from myTable' 
```

> Logs

docker logs <containerId>


## Create/Build Image
```
FROM 'base_os'
RUN 'command'
COPY SRC => DEST
ENTRYPOINT 'command to run when container is run'
```
> Build
```
docker build -t name:tag .
```
> [!NOTE]
> use ':alpine' base image for lightweight. 

> [!NOTE]
> CMD vs  ENTRYPOINT 
CMD:
- override default command
- Passed by arg: all CMD will be replaced.

ENTRYPOINT:
- Excecutes when container run 
- Passed by arg: Entrypont will be appended 
- --entrypoint :  to modify run time

- User can use both Entrypoint and CMD,  ENTRYPOINT-COMMAND + CMD-COMMAND


##  DOCKER HUB
```
docker login  # Will ask for credentials, Enter username and password
docker pull image-name
docker push image-name
```

## docker-compose.yml 
- Run combinition of images togeather like (Web+DB)
- Using '--link db:db' - link application
	- if used compose. every container has access to each other
sample file:
```
version: "3"
services:
  wordpress:
    image: "wordpress"
    ports:
      - "8085:80"
    links:
      - "db:db"
  db:
    image: "postgres"
    environment:
      - "POSTGRES_PASSWORD=mysecretpassword"
```

# Docker Engine 
> Docker Engine [ CLI <=> Rest API <=> Doker Daemon]
	- CLI : docker commands, Can be on onother host, eg : docker -H=remote-docker-engine:2375 run ngnix 
	- Rest API : Interface between CLI and Docker daemon 
	- Docker daemon : Actual docker operations.

> Containerization:
- Namespace - [PID, InterProcess, Network, Mount, Unix Timesharing]
- PID namespace: 
  - Pid is unique , But in container is apear PID start with 1 but its actull PID is 5 on host system.
  - PID from container is separated by its namespace
 
------ lINUX SYS ------
- PID 1  = systemd
- PID 2
- PID 3	: --ChildSystem(container)--
- PID 4	= PID 1
- PID 5	=	PID 2


------- cggroup ------
- Restrict hardware resorces usage
- docker run --cpus=.5 ubuntu
- dockder run --memory=100m ubuntu

# Docker Storage
- Storage Drivers: responsible to perform all layered arch: AUFS(ubuntu),ZFS, BTRFS, Device Mapper, Overlay,Overlay2 
- https://docs.docker.com/storage/storagedriver/
- https://docs.docker.com/storage/
/var/lib/docker 
	:aufs
	:containers
	:image
	:volumes

--------- Container Layer -------------------
- Read Write (COPY ON WRITE)
- User can only modify on Copy of image (container)
  
--------- Image Layer -----------------------
- Read only
- docker consume or use memory if already executed(build)


- Volume : volume-mounting(bind docker volume:/docker/volume/data_volume), bind-mounting(bind custom vol loc)
  
```
# docker volume create data_volume 
docker run -v data_volume:/var/lib/mysql mysql

# new way usind mount:
docker run --mount type=bind,source=/data/data-vol/,target=/var/lib/mysql
```

# Docker Network 

Bridge: default net
- Private internal network created by docker
- IP range : 172.17.*.*
- docker run ubuntu --network=none/host
- docker network ls
none: full isolated, No access to outside net/container
host: Access container ouside by exposing port. webapp

Create Custom network:
```
docker network create \
	--driver bridge \
	--subnet 182.18.0.0/16 custom-network
```
- docker isolates container using network namespace


# Docker Registry 

image: nginx/nginx
	 : UserAc/ImageRepository
```
docker 	run -d -p 5000:5000 --name registry registry:2
docker image tag nginx localhost:5000/my-nginx
docker push localhost:5000/my-nginx
```

# Security
By Default all commands are executed by root user
- Change/Set different user 
by Image : "USER <user-name>"
by command : --user <user-name>

Add/remove Privilages:
```
docker run --cap-add MAC_ADMIN ubuntu
docker run --cap-drop KILL ubuntu
# Add full access
docker run --privileged ubuntu
```


