## `ARG` build-time arguments

```dockerfile
ARG <name>[=<default value>]
```

- ARG values are available only during the build process. They do not persist in the final image.
- You can reference an ARG value in subsequent instructions like RUN, ENV, or LABEL by prefixing it with $ (e.g., $ARG_NAME).
- You can override the default value by passing a new value using the --build-arg flag during the build: `docker build --build-arg APP_ENV=production -t my-app .`
- If a default value is not defined and no value is passed during the build, the ARG will remain undefined (empty string).
- Docker does not raise an error if you pass a --build-arg for an argument that is not declared in the Dockerfile. It silently ignores the argument.

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
