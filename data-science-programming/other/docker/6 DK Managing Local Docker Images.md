#course_datacamp-docker #docker 

- All images used to create containers so far have been provided to us. In reality, we will be working with containers we created ourselves, or have downloaded from the community.
- Summary of covered commands:

| Command                                     | Usage                                               |
| ------------------------------------------- | --------------------------------------------------- |
| `docker pull <image-name>`                  | Pull an image from Docker Hub                       |
| `docker pull <image-name>:<version-number>` | Pull an image from Docker Hub of a specific version |
| `docker images`                             | List all installed images                           |
| `docker image rm <image-name>`              | Remove a specific installed image                   |
| `docker image prune -a`                     | Prune all unused and dangling images                |
| `docker container prune`                                            | Remove all stopped containers                                                    |
## Docker Hub

- Docker Hub is the main registry of community-made Docker images, you are almost guaranteed to find a Docker image for any common use-case.
- Downloading an image from the Hub is referred to as 'pulling' it. Pull an image with `docker pull <image-name>`. 
    - Pulling an image this way will always pull the latest version
    - Append a version number to pull a specific version (`<image-name>:<version-number>`)

```shell
docker pull <image-name>
docker pull <image-name>:<version-number>
```
## Listing images

- List the images you've downloaded with `docker images`. This also tells us when the image was created, what size it is on disk, and the image's unique ID.
- Since images take up space, we should remove any that we're not using. We can do this with `docker image rm <image-name>`.
    - Since a container is a running image, if we have any containers running off that image we will get an error message. This message tells us the ID of the containers we would need to stop. We can then use `docker container rm <container-id>` to [[5 DK Working with Docker containers#Cleaning up containers|remove them]].
    - It is common to have multiple containers running off a single image. It might be easier to remove all stopped containers, which we can do with `docker container prune`.

![[Pasted image 20231017212110.png]]

- We can then remove all unused images with `docker image prune -a`, the `-a` flag stands for all so that the command removes all unused images, not just dangling ones.
    - Dangling images are images that no longer have a name since their name was reused for another image. This frequently occurs when we iteratively work on a single image, creating new versions of it - each of which takes the image name.

```shell
docker images
docker image rm <image-name>
docker container prune
docker image prune -a
```