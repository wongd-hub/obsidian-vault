#course_datacamp-docker #docker 

- Summary of covered commands:

| Command                       | Usage                                                        |
| ----------------------------- | ------------------------------------------------------------ |
| `docker run --name <container-name> <image-name>`    | Run a Docker and give it a `<container-name>`
| `docker ps -f "name=<container-name>"` | Filter list of running container based on name   |
| `docker logs <container-name>`  | See logs for container
| `docker logs -f <container-name>`                   | Follow logs live for container
| `docker container rm <container-id>`                              | Completely remove a Docker container
## Listing containers

- `docker ps` is useful when wor;'//king with a handful of containers, but quickly becomes difficult to use when you have more.
    - You can provide names for a container upon runtime which will let you reference them easier in any command that ordinarily takes a container ID. This name shows up in the last column of the output of `docker ps`.

```shell
docker run --name <container-name> <image-name>
```

## Filtering running containers

- To filter the output of `docker ps`, pass the `-f` (i.e. filter) flag, and also the name of the container you want to find.

```shell
docker ps -f "name=<container-name>"
```

## Container logs

- To see the logs of a running container (e.g. for debugging), use `docker logs`. To follow along with the live logs, pass the `-f` (i.e. follow) flag

```shell
docker logs <container-name>
docker logs -f <container-name>
```

![[Pasted image 20231016122649.png]]

## Cleaning up containers

- We saw how to stop a container with `docker stop <container-id>`, however a stopped container isn't actually fully gone. It still exists and is taking space on your hard drive. To completely remove it, run `docker container rm <container-id>`

```shell
docker container rm <container-id>
```

