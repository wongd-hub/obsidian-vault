#course_datacamp-docker #docker

- This lesson focuses on starting, stopping, and looking at the output of containers.
- We'll be using the following shell commands:

| Command               | Usage                                                  |
| --------------------- | ------------------------------------------------------ |
| `nano <file-name>`    | Opens `<file-name>` in the nano text editor            |
| `touch <file-name>`   | Creates an empty file with the specified `<file-name>` |
| `echo "<text>"`       | Print `<text>` to the console                          |
| `<command> >> <file>` | Push the output of `<command>` to the end of `<file>`  |
| `<command> -y`                      | Automatically respond `yes` to all prompts from `<command>`                                                       |

- Summary of covered commands:

| Command                       | Usage                                                        |
| ----------------------------- | ------------------------------------------------------------ |
| `docker run <image-name>`    | Run a Docker image                                           |
| `docker run -it <image-name>` | Run a Docker image and gain access to an interactive shell   |
| `docker run -d <image-name>`  | Run a Docker image in the background with no printed outputs |
| `docker ps`                   | List all running Docker containers                           |
| `docker stop <container-id>`                              | Stop a Docker container         |

## The Docker CLI

- The Docker command line interface sends instructions to the Docker daemon. The daemon generally provides a response in the console to let you know the status of your command.
    - We start all docker CLI commands with `docker`
- Before we can start a container, we'll need an *image*. Images are blueprints/recipes for how the container is configured and what is installed. A container is just a running instance of an image.
    - For example, there is an `ubuntu` image which contains the full Ubuntu OS. This full OS will be available once your container is running, and can then be interacted with from the shell.

![[Pasted image 20231012225930.png]]

## Docker container output and interactivity

- `docker run <image-name>` will run an image. By default, Docker starts the container and shows you the container's output as it's running. For example, running Docker's `hello-world` image with `docker run hello-world`:

![[Pasted image 20231012230306.png]]

- When creating an image, the creator can choose what happens when you start a container from it.
    - This is how the message above was generated
    - The Ubuntu image will start, then shut down again immediately without emitting any output
        - Instead, the Ubuntu image is made to work with the `-it` flag. This flag allows us to start our container and get an interactive shell in it - i.e. `docker run -it ubuntu`. The resulting shell gives us access to a clean Ubuntu environment, isolated from the host machine. Run `exit` to get out of the container. 

## Running a container detached

- We've seen a container whose primary purpose is to print text, and one which is geared towards using interactively. There is a third type of container that processes data or can be interacted with in some way externally. For example, a data processing script or a container with a database like Postgres.
    - We run these with the `-d` flag; this ensures that the image runs in a detached state without emitting output to the shell - i.e. `docker run -d postgres`

## Listing and stopping running containers

- After a container has started, you can run `docker ps` to see it and any other running containers. The first column shows the container's unique ID.

![[Pasted image 20231012232809.png]]

- To stop a container, run `docker stop <container-id>`.