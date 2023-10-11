#course_datacamp-docker #docker

- Docker is an open-source tool that allows us to create, run, and manage containers.
    - Docker's creation in 2013 caused containers (which already existed) to explode in popularity.
    - *Docker Engine* is all you need to create and run containers, but is part of a wider ecosystem which contains (amongst other things):
        - *Docker Compose*: A tool for managing multi-container Docker applications
        - *Kubernetes*: A tool for scheduling and management of containers

## Docker Engine
- The Docker Engine is composed of a client and a server:
    - The client is called Docker Client, and is a command-line interface used to interact with the Docker server
        - Docker Engine provides API specifications which define how a programmer or other applications can interact with the server
    - The server requires no user interaction and runs in the background
        - This is often referred to as the Docker daemon (or daemon)

![[Pasted image 20230904220447.png]]
### The Docker Daemon
- The daemon is responsible for managing Docker objects (i.e. containers, images)
- We can't tell the daemon directly what to do, we need to use the client to provide us a human-usable interface to it
    - The Docker command-line is one, however there is also Docker Desktop which provides a GUI

![[Pasted image 20230904220506.png]]
### Images and Containers
- An image is like a snapshot of a computer
    - Think of this like a recipe to perform our workflow
- A container is simply a running image
    - Think of this like a running process; when we start a container, a process is started, just like if we open an application. The difference is that the container has different permissions to the file system, memory, and network.
    - In fact, access is not only restricted, but also undetectable to the process. So if a container is told to only work on a specific folder, it will only be able to see that folder and nothing else. This allows for *isolation* to the rest of the machine.

![[Pasted image 20230904220837.png]]

- This level of isolation for the container means that we can run an entirely separate operating system from inside the container. The Docker daemon ensures that our container is unaware of the host OS and other containers.
    - The operating system inside a container can start and manage its own processes without interfering with any processes on the host OS.

![[Pasted image 20230904221053.png]]