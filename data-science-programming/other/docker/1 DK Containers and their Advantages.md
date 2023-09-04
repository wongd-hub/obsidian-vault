#course_datacamp-docker #docker

## Advantages of Containers
### Containers
- A container is a portable computing environment, it contains everything needed (including dependencies, code, and configuration) to run a workflow or an application.
- Imagine you set up an empty computer with code, dependencies and configuration, then back it up. This is similar to the setup of a container.
    - However, that's where the similarities end. There are many advantages to a container over having a conventional backup.

![[Pasted image 20230904214639.png]]

### Reproducibility
- Containers provide *reproducibility* since they run identically every time. The same input provides an identical output regardless of time since the last run.
    - Extending this, this means that containers are *portable* since they are designed to run the same on your computer, a colleague's computer, and in the cloud.

![[Pasted image 20230904214719.png]]

### Isolation
- This reproducibility and portability is in part allowed by the concept of *isolation*. Running a container on a host device will have no interaction with anything outside of the container, allowing multiple containers to be run in parallel with no interaction between them.
    - Containers have limited resource access to the operating system they're running on.

![[Pasted image 20230904214946.png]]

### Security
- An effect of isolation means that containers are *secure*. One container being compromised doesn't mean that the other containers are too.
    - This allows for safely deploying applications and rapidly prototyping workflows.

![[Pasted image 20230904215137.png]]

### Lightweight
- Containers use few extra resources in comparison to running an application outside of a container. There is little overhead compared to other options (something that sets this apart from virtual machines).

## Containers and Data Science
- These attributes mean that:
    - **Containers allow ease of reproducibility**. Dependencies are automatically included, and datasets can be included too.
    - **Containers being lightweight** means that they're easier to share than alternatives.