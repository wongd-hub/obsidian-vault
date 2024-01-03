#course_datacamp-docker #docker 

- Docker containers don't make everything automatically secure. In this lesson we'll look at the security measures Docker provides us inherently, as well as what we can do to make them more secure.

- Docker inherently provides security over running apps locally because there is an extra layer of isolation between the application and the OS. This makes it safer to open an unknown application/from an unknown source in a container.
    - However, a malicious payload can still escape the container and infect the host.

![[Pasted image 20231101084929.png]]

- Attackers breaking out of the container into the host operating system is the main security concern for containers. 
    - Docker and other container providers spend extensive resources to ensure that their containers are secure as possible.
    - There are things we can do as the creators and users of images and containers to make them more secure. This is particularly important when running containers on remote machines and allowing external access.
## Steps for security

- **Build image from trusted source**: The first step to building a secure image is choosing the right image to start from. 
    - The official Docker registry provides three ways to filter for Trusted Content, ensure that you are using these images as the start point for your own.

![[Pasted image 20231101085312.png]]

- **Keep software up-to-date**: Even OS updates may not be incorporated into Docker images built on those OS's immediately. It is best practice to update our software from within the Docker image itself. 

```Dockerfile
RUN apt-get update # See all packages that need an update
RUN apt-get upgrade # Download and install those updates
```

- **Keeping it minimal**: What's better than making sure all of the software in our image is updated? Having less software. There's no safer piece of software than one we haven't installed.

- **Don't run applications as root**: All previous measures are of little use if we allow full access to the users of our container which allows them to install whatever they want. At the very least, we should change the user just before command (`CMD`) is given to the user of the image.

```Dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install python3
USER repl
CMD python3
```

