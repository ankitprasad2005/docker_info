Docker
=====


## Index
- [Basic docker commands](#Basic-docker-commands)
- [Building and running image](#building-and-running-image)
- [Other docker commands](#other-docker-commands)
- [Docker hub](#docker-hub)
- [Docker volumes](#docker-volumes)

- [Docker Networks](#docker-networks)
  - [Bridge networks](#bridge-network)
  - [Host network](#host-network)
  - [None](#none)

- [Docker Web-Interface](#docker-web-interface)
- [Dockerfile](#dockerfile)
- [Docker compose](#docker-compose)

- [Sources](#sources)


# Installing docker
```bash
# Remove previous installs
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done


# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
```

***Pls dont use this***

```bash
## PLS DONT USE THS
# for debian/ubuntu users
sudo apt update
sudo apt install docker.io

# for fedora users
sudo dnf install docker

# for arch users
sudo pacman -S docker
```
Running docker daemon
```bash## PLS DONT USE THS
sudo systemctl enable --now docker.service
```

For adding user to docker group
```bash
sudo usermod -aG docker $USER
```


## Basic docker commands

1. HELP 🆘
```bash
docker help
```

2. Current running docker processes (containers) 🏃
```bash
docker ps

docker ps -a
```
- ```-a```  -->  to get all the running containers
- ```-q```  -->  to get just the container id

3. Stopping and Removing 🛑
```bash
docker stop [container_id]
docker rm [docker_id]
``` 

```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```
----


## Building and running image

1. For building docker image 🏗️
```bash
docker build -t [image_name] [dir_of_Dockerfile]
```

2. For creating & running docker image 🏃
```bash
docker run --nane [container_name] [image_name]
```

- can add ```-rm``` for stooping and removing docker as u exit
- can add ```-d``` for detaching it from terminal after running (if ur using default bridge network)
- add ```-p [host_port]:[docker_port]``` for mapping a specific docker port to host port

3. For executing something inside docker image 🖱️
```bash
docker exec [file/command_wanna_run] [container_name]
```
----


## Other docker commands

1. KILL IT 🔪💀
```bash
docker kill [container_id]
```

2. Logs 📃
```bash
docker logs [container_id]
```
----


## Docker hub
1. Pull
```bash
docker pull [image_name]:[tag]
```
- list of docker image
    ```bash
    docker image ls
    ```

2. Tag
```bash
docker tag [image_id] [image_name]:[tag]
```

3. Push
```bash
docker push [image_name]:[tag]    # default tag is "latest"
```
----


## Docker volumes
1. create
```bash
docker volume create [volume_name]
```
2. manage
```bash
docker volume list
docker colume inspect [volume_name]
```

3. mount
```bash
# for mounting docker volume 
docker run -v [volume_name]:[path_in_container] [docker_image]

# for mounting host directry
docker run -v "[host_directry]":[volume_name] [docker_image]
```

4. remove
```bash
docker volume rm [volume_name]

docker volume prune   # to remove all unused volumes
```
----


Docker Networks
=====

1. Creating network
```bash
docker network create [network_name]
```

2. Connect
```bash
docker network connect [network_name] [container_name]
```

3. Disconnect
```bash
docker network disconect [network_name] [container_name]
```
2. List & Inspect
```bash
docker network ls

docker inspect [network_name]
```

3. Delete network
```bash
docker network rm [network_name]

doceker network prune    #delete all unused network
```


## Default networks
1. [Bridge networks](#bridge-network)
2. [Host network](#host-network)
3. [None](#none)

## Other networks
4. macvlan
5. ipvlan
6. overlay   

(There are other possible P&C too....)

## Parameters (flags)
- ```--subnet 172.76.0.0/16``` --> to define subnet
- ```--gateway 172.76.0.1/16``` --> to define the gateway of the network
- ```-o parent=[network_interface]``` --> to define the physical network interface

### Bridge network
It's the default one no need for any flags.

### User-Defined Bridge Network
```bash
docker network create [network_name]
docker run --network [network_name] [container_id] [image_name]
```

### Host network
```bash
docker run --network host [container_id] [image_name]
```

### None
```bash
docker run --network none [container_id] [image_name]
```
----


## Docker Web-Interface
### portainer-ce

1. Instalation

Create a docker voume or direct mount to a host directry
```bash
docker volume create portainer_data
```

Run the portainer as a contatiner on the bridge network
```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.5
```

Sample docker-compose
```yaml
---
services:
  portainer:
    image: portainer/portainer:latest
    container_name: portainer
    environment:
      - PUID = ${UID}
      - PGID = ${GID}
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ${DATA}:/data
    ports:
      - 8000:8000
      # - 9000:9000 #HTTP port
      - 9443:9443 #HTTPS port
    restart: always

```

----
## Dockerfile
Common tags:
- `FROM`    --> Base image
- `COPY`    --> Commands to run
- `WORKDIR` --> current working dir in the container
- `EXPOSE`  --> expose the mentioned port
- `CMD`     --> run the final service (should not have a terminatig shell)

----
## Docker Compose
Common paramaters:

```yaml
- service
  - [service-name]
    - image: [image_name]
    - container_name: [any_name_for_container]
    - hostname: [hostname]
    - user: [GID]:[UID]
    # - env_file:
    #   - [.env_location]
    - environment:
      - [parameter]=[value]
    - volumes:
      - [host_location]\:[container_location]
    - ports:
      - [host_port]\:[container_port]
    - restart: [always, unless-stopped, ....]
```

----

## Sources
1. https://docs.docker.com/
2. https://dockerlabs.collabnix.com/
3. https://www.docker.com/blog/