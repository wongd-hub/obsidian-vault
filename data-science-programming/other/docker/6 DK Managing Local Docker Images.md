#course_datacamp-docker #docker 

- All images used to create containers so far have been provided to us. In reality, we will be working with containers we created ourselves, or have downloaded from the community.

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

