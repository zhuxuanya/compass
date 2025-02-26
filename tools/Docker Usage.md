# Docker Usage

Docker is an open-source application container engine that allows developers to package the applications along with all necessary dependencies into a portable container. These containers allow different applications inside to operate independently across various systems, simplifying software development and testing.

## Images

Docker images serve as templates for creating Docker containers. One image can be used to instantiate multiple containers.

### Pull an Image

To download an existing image from a Docker registry such as Docker Hub. This is useful for obtaining pre-built images on setup.

```bash
docker pull [REPOSITORY]:[TAG]
```

`[REPOSITORY]` is the name where the image is stored.

`[TAG]` specifies the version of the image with default `latest`.

### Build an Image from a Dockerfile

A Dockerfile is a text document that contains all the commands to assemble an image. Use `docker build`  can create an automated build that executes several command-line instructions in succession.

```bash
docker build -t [REPOSITORY]:[TAG] -f /path/to/a/Dockerfile .
```

`-f` followed by the path to the Dockerfile. This tells Docker exactly which file to use as the Dockerfile for the build process.

`.` specifies the build context, which includes all files in the current directory. These files are sent to the Docker daemon to build the image.

### List All Images

This command shows following information about each image.

```bash
docker image ls
# REPOSITORY              TAG                     IMAGE ID       CREATED          SIZE
```

### Remove an Image

To delete an image from local storage.

```bash
docker rmi [IMAGE ID]
```

## Containers

Containers are running instances of images. Each container can be seen as a simplified Linux environment. 

### Create a Container

The `run` command is the most straightforward method to create and start a single container. Following command will start an Nginx server in a container named `webserver`, accessible through port 80 on the host.

```bash
docker run -d -p 80:80 --name webserver nginx
```

`-d` runs the container in detached mode (in the background).

`-p 80:80` maps port 80 of the host to port 80 in the container.

`--name webserver` assigns a name to the container.

`nginx` is the name of the image to use.

### List All Containers

This command shows following information about running containers.

```bash
docker ps
# or
docker container ls
# CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

`-a` will list all running and stopped containers.

### Start/Stop a Container

 Stop a running container instead of deleting. Restart an existing container.

```bash
docker start [CONTAINER ID]
docker stop [CONTAINER ID]
```

### Remove a Container

Remove a stopped container. Use `-f` to force the removal of a running container.

```bash
docker rm [CONTAINER ID]
```

### Execute Commands Within a Container

This command allows interaction with running containers by executing commands inside directly, which is particularly useful for managing and debugging applications without having to stop and restart the container. Use `bash` as an example.

```bash
docker exec -it [CONTAINER ID] bash
```

## Volumes

Volumes are used for data storage of container data. Volumes allow for data to be shared and reused among containers and also make it possible to persist data beyond the life of a single container.

### List All Volumes

List all the volumes currently managed on the host system.

```bash
docker volume ls
# DRIVER    VOLUME NAME
```

### Remove a Volume

Delete the volume if it is not currently attached to any containers.

```bash
docker volume rm [VOLUME NAME]
```

## Networks

Docker networking allows containers to connect to other containers and to the outside world.

### List All Networks

List all networks created on the host.

```bash
docker network ls
# NETWORK ID     NAME                    DRIVER    SCOPE
```

### Remove a Network

Remove a network. Containers attached to the network must be stopped or disconnected from the network first.

```bash
docker network rm [NETWORK ID]
```

## Cleanup

Maintaining a clean Docker environment is crucial, especially to avoid consuming excessive disk space with unused Docker objects such as containers, images, networks, and volumes. 

### System Prune

This basic command is used to clean up unused Docker objects. This can help reclaim disk space and keep the system tidy.

```bash
docker system prune
# WARNING! This will remove:
#         - all stopped containers
#         - all networks not used by at least one container
#         - all dangling images
#         - unused build cache
```

In addition to `system prune`, Docker also provides specific prune commands for removing all unused images, containers, volumes and networks.

``` bash
docker image prune
docker container prune
docker volume prune
docker network prune
```

### Options

```bash
docker system prune -a --volumes
# WARNING! This will remove:
#         - all stopped containers
#         - all networks not used by at least one container
#         - all anonymous volumes not used by at least one container
#         - all images without at least one container associated to them
#         - all build cache
```

`-a` stands for "all". It is much more aggressive and will free up more space but should be used with caution.

`--volumes` will include unused volumes in the prune operation.

For another advanced option `--filter`, refer to the [docker.docs](https://docs.docker.com/reference/cli/docker/system/prune/).

## Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure the application's services, networks, and volumes, and then creates and starts all the specified components.

An official example refers to [Docker Compose Quickstart](https://docs.docker.com/compose/gettingstarted/).

### Start All Services

`up` is used to start all the services.

```bash
docker compose -f /path/to/a/YAML/file up
```

`-f` is used to specify a YAML file. If not specified, `docker-compose.yaml` will be used by default.

### Stop/Remove All Services

`down` is used to stop and remove all the containers, networks, and volumes.

```bash
docker compose -f /path/to/a/YAML/file down
```
