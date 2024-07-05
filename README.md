Docker
=====


## Index
- [Basic docker commands](#Basic-docker-commands)
- [Building and running image](#building-and-running-image)
- [Other docker commands](#other-docker-commands)
- [Docker hub](#docker-hub)
- [Docker volumes](#docker-volumes)
- [Docker Networks](#docker-networks)

## Basic docker commands

1. HELP 🆘
```
docker help
```

2. Current running docker processes (containers) 🏃
```
docker ps

docker ps -a
```
- -a  -->  to get all the running containers
- -q  -->  to get just the container id

3. Stopping and Removing 🛑
```
docker stop [container_id]
docker rm [docker_id]
``` 

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```
----


## Building and running image

1. For building docker image 🏗️
```
docker build -t [image_name] [dir_of_Dockerfile]
```

2. For creating & running docker image 🏃
```
docker run --nane [container_name] [image_name]
```

- can add **-rm** for stooping and removing docker as u exit
- can add **-d** for detaching it from terminal after running (if ur using default bridge network)
- add **-p [host_port]:[docker_port]** for mapping a specific docker port to host port

3. For executing something inside docker image 🖱️
```
docker exec [file/command_wanna_run] [container_name]
```
----


## Other docker commands

1. KILL IT 🔪💀
```
docker kill [container_id]
```

2. Logs
```
docker logs [container_id]
```
----


## Docker hub
1. Pull
```
docker pull [image_name]:[tag]
````
- list of docker image
    ```
    docker image ls
    ```

2. Tag
```
docker tag [image_id] [image_name]:[tag]
```

3. Push
```
docker push [image_name]:[tag]    # default tag is "latest"
```
----


## Docker volumes
1. create
```
docker volume create [volume_name]
```
2. manage
 ```
docker volume list
docker colume inspect [volume_name]
```

3. mount
```
# for mounting docker volume 
docker run -v [volume_name]:[path_in_container] [docker_image]

# for mounting host directry
docker run -v "[host_directry]":[volume_name] [docker_image]
```

4. remove
```
docker volume rm [volume_name]

docker volume prune   # to remove all unuced volumes
```
----


Docker Networks
=====
## Default networks
1. Bridge network (default)
2. Host network
3. None

## 1. Bridge Network





