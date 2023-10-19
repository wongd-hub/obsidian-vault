#course_datacamp-docker #docker 

- Summary of covered commands:

| Command                                     | Usage                                               |
| ------------------------------------------- | --------------------------------------------------- |
| `docker pull <private-registry-url>/<image-name>:<ver>`              | Pull an image from a private registry              |
| `docker tag <image-name>:<ver> <private-registry-url>/<image-name>:<ver>` | Rename an image for pushing to a private registry |
| `docker image push <image-name>`                             | Push an image to a registry   |
| `docker login <private-registry-url>`              | Authenticate with a private registry
| `docker save -o <file-name>.tar <image-name>`                     | Save an image to a local file           |
| `docker load -i <file-name>.tar`                                            | Load an image from a local file |
## Private registries

- Images are distributed through the Docker Hub but images can be hosted privately in private registry servers as well. You can tell that an image is from a private registry as the name will start with the URL of the registry.
    - You pull these the same way as you pull any other image.

```shell
docker pull <private-registry-url>/<image-name>:<ver>
```

## Pushing to a registry

- To push an image to a registry run `docker image push <image-name>`. To direct it to a private registry, we need to rename the image and prepend the URL of the private repository with `docker tag`. We can then run `docker image push <new-image-name>`.

```shell
docker image push <image-name>
docker tag <image-name>:<ver> <private-registry-url>/<image-name>:<ver>
```

## Authenticating with a private registry

- Docker Hub images can be pulled without authentication, but private registries may be set up to require it. We can do this with `docker login <private-registry-url>`.

## Docker images as files

- If we want to pass an image around to only a small number of people, we can save it as a file with `docker save`. You can then load it with `docker load`.

```shell
docker save -o <file-name>.tar <image-name>
docker load -i <file-name>.tar
```