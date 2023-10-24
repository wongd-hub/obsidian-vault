#course_datacamp-docker #docker 

- `FROM`, `COPY`, `RUN` instructions only affect the file system of the image, and do not modify each other.
- Some instructions can influence others directly, the ones we'll discuss in this lesson are `WORKDIR` and `USER` used to change the working directory and executing user respectively.
## Changing the working directory

- Up until now, when we've needed to specify a path in our Docker image, we've used a full path. This can get lengthy and unwieldy. Instead, we can specify a working directory and then use relative paths from that like so.

```Dockerfile
COPY /projects/pipeline/ /my/long/path/app/

# Becomes
WORKDIR /my/long/path/
COPY /projects/pipeline/ app/
```

- This also affects our subsequent `RUN` commands. 