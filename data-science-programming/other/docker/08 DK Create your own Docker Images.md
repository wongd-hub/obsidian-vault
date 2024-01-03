#course_datacamp-docker #docker 
## Dockerfiles

- The recipe for each image is contained in a `Dockerfile`. 
    - The first line in any `Dockerfile` is always a `FROM` command. This tells Docker which image to start from when building our image. Often, this will be `FROM ubuntu` but it can be anything that matches your use-case. We can also [[06 DK Managing Local Docker Images#Docker Hub|specify a specific version using colon notation]] as seen in previous videos.
    - We can then customise from there by putting in `RUN` lines followed by valid shell commands, which run the given commands in the shell of the image. 
    - A simple `Dockerfile` might look like the following:

```Dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install -y python3
```

> [!tip] 
> Note that we cannot interact with the image while it is building, which means that any commands that require user input will not work. Note that we pass the `-y` flag in the third line to avoid any user input that are emitted by that command.
## Building our image

- We can use `docker build` to build an image from a `Dockerfile`, passing the `-t` (for tag) flag if we want to provide it with a name. This command takes the folder in which your `Dockerfile` resides as an argument.

```shell
docker build .
docker build -t <image-name> .
```

- Upon build, Docker also provides an ID for your image, generated using a hashing function.

![[Pasted image 20231018225209.png]]

- When building an image, Docker is actually running the shell commands specified with `RUN`. So the amount of time it takes to build an image is directly related to the commands specified in the `Dockerfile`.