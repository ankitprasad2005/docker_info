 
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
docker stop <container_id>
docker rm <docker_id>
``` 

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```



## Commands for constructing and running image

1. For building docker image 🏗️
```
docker build -t <image_name> <dir_of_Dockerfile>
```

2. For creating & running docker image 🏃
```
docker run --nane <container_name> <image_name>
```

- can add **-rm** for stooping and removing docker as u exit
- can add **-d** for detaching it from terminal after running (if ur using default bridge network)
- add **-p <host_port>:<docker_port>** for mapping a specific docker port to host port

3. For executing something inside docker image 🖱️
```
docker exec <file/command_wanna_run> <container_name>
```



## Other docker commands

1. KILL IT 🔪💀