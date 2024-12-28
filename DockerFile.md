## Build-time arguments (ARG)

```dockerfile
ARG <name>[=<default value>]
```

- ARG values are available only during the build process. They do not persist in the final image.
- You can reference an ARG value in subsequent instructions like RUN, ENV, or LABEL by prefixing it with $ (e.g., $ARG_NAME).
- You can override the default value by passing a new value using the --build-arg flag during the build: `docker build --build-arg APP_ENV=production -t my-app .`
- If a default value is not defined and no value is passed during the build, the ARG will remain undefined (empty string).
- Docker does not raise an error if you pass a --build-arg for an argument that is not declared in the Dockerfile. It silently ignores the argument.
