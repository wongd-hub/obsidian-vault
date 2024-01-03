#course_datacamp-docker #docker 

- Building Docker images can take a significant amount of time, but building them a second time can be much faster. 
- During the build process, the image keeps track of which command in the `Dockerfile` caused which changes in the file system. 
    - We can view a `Dockerfile` as a list of consecutive changes to the file system, with each entry in the list being a `RUN` command.

> [!tip]
> All changes to the file system for a single instruction in the `Dockerfile` are called a layer of the image. There is also some metadata, such as the command passed to `CMD`.

![[Pasted image 20231020223955.png]]

- When building an image we've built before, we'll see the logs for each layer that hasn't changed prepended with `CACHED` to indicate that nothing is being done for that layer as the instruction hasn't changed.
    - Docker will only use a cached layer for each layer if it 1. hasn't changed, and 2. all the previous layers are also the same as before.
    - If you change an instruction, then change it back to how it was before, the cache will still be there.
    - There are some consequences of this behaviour:
        - Consider a scenario where we have `RUN apt-get install python3` in our image. We build it once then wait a while. If a new version of `python3` is released and we build this image again, Docker will not capture the new version of `python3`. All it will see is that the instruction didn't change and hence it treats it as cached and skips over it.
        - Knowing this behaviour, we should put the instructions that we're less likely to change at the start of the `Dockerfile`, since they will remain cached.
            - For example, we generally put packages at the start and file copies/downloads at the end since those are more likely to change.