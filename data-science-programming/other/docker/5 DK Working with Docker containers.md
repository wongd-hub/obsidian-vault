#course_datacamp-docker #docker 

## Listing containers

- `docker ps` is useful when working with a handful of containers, but quickly becomes difficult to use when you have more.
    - You can provide names for a container upon runtime which will let you reference them easier in any command that ordinarily takes a container ID. This name shows up in the last column of the output of `docker ps`.

```bash
docker run --name <container-name> <image-name>
```

## Filtering running containers

