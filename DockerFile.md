## `ARG` build-time arguments (build-time configurations)

```dockerfile
ARG <name>[=<default value>]
```

- **ARG values are available only during the build process. They do not persist in the final image.**
- You can reference an ARG value in subsequent instructions like RUN, ENV, or LABEL by prefixing it with $ (e.g., $ARG_NAME).
- You can override the default value by passing a new value using the --build-arg flag during the build: `docker build --build-arg APP_ENV=production -t my-app .`
- If a default value is not defined and no value is passed during the build, the ARG will remain undefined (empty string).
- Docker does not raise an error if you pass a --build-arg for an argument that is not declared in the Dockerfile. It silently ignores the argument.

## `ENV` runtime environment variables (runtime configurations)

```dockerfile
FROM ubuntu:latest
ENV APP_ENV=production
CMD echo "Running in $APP_ENV mode"
# Output: Running in production mode
```

- The ENV instruction in a Dockerfile is used to define environment variables that persist within the container at runtime.
- ENV can be overridden when running the container `docker run -e APP_ENV=development my-app`

## `USER`

The USER command in a Dockerfile is used to specify the user that will run subsequent commands inside the container. By default, Docker containers run as the root user.

```dockerfile
FROM ubuntu:latest

# Create a new user
RUN useradd -m myuser

# Switch to the new user
USER myuser

# Commands after this will run as 'myuser'
CMD ["whoami"]
```

```dockerfile
FROM node:18

# Create a non-root user
RUN useradd --create-home --shell /bin/bash appuser

# Set working directory and switch user
WORKDIR /home/appuser
USER appuser

# Copy files and install dependencies
COPY --chown=appuser:appuser package.json .
RUN npm install

CMD ["node", "app.js"]
```

```dockerfile
FROM jenkins
# if we want to install via apt
USER root
RUN apt-get update && apt-get install -y ruby make more-thing-here
USER jenkins # drop back to the regular jenkins user - good practice
```

## `WORKDIR`

```dockerfile
WORKDIR <path>
```

- The WORKDIR instruction in a Dockerfile sets the working directory for any subsequent instructions in the Dockerfile (e.g., RUN, CMD, COPY, and ADD) and for the container runtime when it starts.
- If the directory does not exist, Docker automatically creates it.
- When the container starts, it will begin execution in the WORKDIR.

## `RUN`

```dockerfile
FROM ubuntu:20.04

# Install curl (modifies the image)
RUN apt-get update && apt-get install -y curl
```

- The RUN instruction is used to execute commands at build time to modify the image. These commands create a new layer in the image.
- The results of RUN commands are saved into the image (e.g., installed packages, modified files).

## `COPY`

```dockerfile
COPY [--chown=<user>:<group>] <source> <destination>
```

- The COPY instruction in a Dockerfile is used to copy files or directories from the host machine into the image's filesystem.
- Changes the ownership of the copied files to the specified user and group.

## `ADD`

```dockerfile
ADD [--chown=<user>:<group>] <source> <destination>
```

- The ADD instruction in a Dockerfile is used to copy files or directories from the build context or a remote URL into the image's filesystem.
- While similar to COPY, it has additional features, such as **extracting compressed files** and **downloading files from URLs**.

```dockerfile
FROM ubuntu:20.04

# Add a file to the image
ADD myapp.py /app/

# Set the working directory and execute the file
WORKDIR /app
CMD ["python3", "myapp.py"]
```

```dockerfile
FROM ubuntu:20.04

# Add and extract a .tar.gz archive
ADD files.tar.gz /data/
```

```dockerfile
FROM ubuntu:20.04

# Download and add a file from a URL
ADD https://example.com/app.tar.gz /app/
```

Best practices:
- Prefer COPY Over ADD. Use COPY when you don't need the extra features of ADD.
- Avoid using ADD for downloading files from the internet whenever possible. Instead, use RUN curl or RUN wget for more control and transparency.
  ```dockerfile
  RUN curl -o /app/app.tar.gz https://example.com/app.tar.gz
  ```
- If you need to extract archives, consider explicitly extracting them using RUN tar for better visibility and control.
  ```dockerfile
  COPY files.tar.gz /data/
  RUN tar -xzf /data/files.tar.gz -C /data/
  ```
