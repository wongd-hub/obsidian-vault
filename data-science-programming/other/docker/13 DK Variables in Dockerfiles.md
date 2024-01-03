#course_datacamp-docker #docker 

- Using variables inside our `Dockerfiles` makes them easier to maintain and update as well as being less verbose.
## Build-time variables

- Use the `ARG` command to set a variable, then refer to the variable using `$<var_name>`
    - Variables created with `ARG` are only scoped to the image build. They will not be available when your image is running.
    - Particularly useful for defining certain package versions, and commonly used directory paths (like a project folder)

```Dockerfile
ARG <var_name>=<value_value>
$<var_name>

# For example
FROM ubuntu
ARG python_ver=3.9.7-1+bionic1
RUN apt-get install python3=$python_ver
RUN apt-get install python3-dev=$python_ver
```

- You can override the value of a variable when building an image with the `--build-arg` flag.

```shell
docker build --build-arg python_ver=3.9.8-1 .
```

## Variables available upon image run

- Use `ENV` to set variables that are available both at build-time and after the image is built. The syntax is identical to `ARG`. 
    - Typical use-cases are setting starting variables for applications, or switching between dev and prod modes (`ENV MODE production`).
    - It is also not possible to override `ENV` variables at build time. However, you can override these variables when starting a container from the image. For example, there are several variables you can set this way when using the `postgres` container.

```shell
docker run --env <var_name>=<var_value> <image_name>
```
## Secrets in variables are not secure

- While variables seem like a convenient way to provide passwords into the process, variables defined this way are *not secure*. 
- Anyone can see the variables defined in the `Dockerfile` after build using the `docker history` command.
    - If we passed variables during build/start time with the `--build-arg` or `--env` flags, they will be available in the bash history.