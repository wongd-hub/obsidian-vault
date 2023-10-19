#course_datacamp-docker #docker 

- We've seen how to create a Docker image using another image as a starting point, as well as how to run arbitrary shell commands upon build. This lesson will take this further and show you how to add/download files into your image.

## Copying files from your local machine

- Use the `COPY` command to copy files from your local machine into your Docker image.
    - You can copy entire directories across. Don't provide a file extension in your destination path to specify either copying across an entire folder (and its subdirectories) or to copy the single file you've selected to the selected folder with no name change.
    - Note that you cannot copy files from outside of your current working directory, even with `../`

```dockerfile
COPY <src-path-on-host> <dest-path-on-image>
```

## Downloading files

- To download files from the internet, you'll need to use `RUN` to run `curl` commands:
    - Download a file with `RUN curl <file-url> -o <destination>`
    - Unzip the file (if a zip) with `RUN unzip <zipped-file>`
    - Remove the zip file with `RUN rm <zipped-file>`

> [!tip]
> Note that each downloaded file adds to the size of the image, even if you delete the zip in a later command. The solution is to run all three commands in the same line using `\` and `&&` to keep adding on to the same line.

```dockerfile
RUN curl <file-url> -o <destination> \
    && unzip <zipped-file> \
    && rm <zipped-file>
```

