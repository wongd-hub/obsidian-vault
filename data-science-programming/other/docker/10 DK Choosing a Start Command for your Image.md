#course_datacamp-docker #docker 

- The images we've created so far do not do anything upon running them as containers. In this lesson we'll talk about having the image execute commands once it starts as a container.

## What is a start command?

- The `hello-world` container will print a "Hello World" message upon instantiation, then automatically exit. A `python3-sandbox` container will likely open up a `python3` interpreter upon run.
- To choose what our container does upon running we use the `CMD` instruction. This instruction accepts a single argument, the shell command we want to run when the image is started as a container.
    - The instruction in `CMD` is not run upon image build, only when the image is started as a container.
    - Adding `CMD` instructions to an image will not affect the image's size, nor affect build time.
    - If you add multiple `CMD` instructions to a `Dockerfile`, only the last one will be used.

- When providing a `CMD` instruction, it makes sense to start an application related to the container's particular use-case. For example, opening a Python interpreter or a Python script, or instantiating a database.
    - We can also run a script that will open up multiple applications.

```Dockerfile
CMD python3 hello.py
CMD postgres
```

- Note that the container will stay running until the command passed to `CMD` exits, i.e. when the user exits the shell, or when the database crashes.

## Overriding the default start command

- We can override the default start command by passing a shell command when running an image. Generally we also want to run such an image interactively, so we also pass the `-it` flag.

```shell
docker run -t <image-name> <shell-command>
```

