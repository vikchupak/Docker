# CMD vs ENTRYPOINT

In Docker, both `CMD` and `ENTRYPOINT` define the default commands that are executed when a container starts. However, they have slightly different purposes and behavior. Understanding their differences helps in determining how to structure your Dockerfile based on the intended behavior of the container.

### 1. **CMD**
- **Purpose**: The `CMD` instruction specifies the default command to be run when the container starts. It's mainly intended to provide **default arguments** for the `ENTRYPOINT` command or act as the main command if `ENTRYPOINT` is not specified.
- **Behavior**:
  - If the user provides a command when starting the container (e.g., `docker run myimage <command>`), the command specified in `CMD` will be **overridden**.
  - If you provide multiple `CMD` instructions in a Dockerfile, only the last one will take effect.

- **Example**:
  ```Dockerfile
  FROM ubuntu
  CMD ["echo", "Hello, World!"]
  ```

  In this example:
  - When the container runs, it will execute `echo "Hello, World!"` by default.
  - If you run `docker run myimage ls`, it will execute `ls` instead of the default `CMD` because the command `ls` overrides `CMD`.

- **Formats**:
  - **Shell form**: `CMD echo "Hello, World!"`
  - **Exec form**: `CMD ["echo", "Hello, World!"]`

### 2. **ENTRYPOINT**
- **Purpose**: The `ENTRYPOINT` instruction specifies the main executable that will always run when the container starts. It is used when you want the container to behave like an executable, where any additional arguments passed to `docker run` are treated as arguments to the `ENTRYPOINT` command.
- **Behavior**:
  - Unlike `CMD`, `ENTRYPOINT` **cannot be easily overridden** by arguments passed to `docker run`. The command passed will be treated as arguments to the `ENTRYPOINT` command.
  - You can override `ENTRYPOINT` at runtime using the `--entrypoint` flag in `docker run`.
  - You can combine `ENTRYPOINT` with `CMD` to provide **default arguments**.

- **Example**:
  ```Dockerfile
  FROM ubuntu
  ENTRYPOINT ["echo"]
  CMD ["Hello, World!"]
  ```

  In this example:
  - When the container runs, it will execute `echo "Hello, World!"` by default.
  - If you run `docker run myimage Goodbye`, it will execute `echo "Goodbye"`, where `"Goodbye"` overrides the `CMD` argument but not the `ENTRYPOINT`.

- **Formats**:
  - **Shell form**: `ENTRYPOINT echo "Hello, World!"`
  - **Exec form**: `ENTRYPOINT ["echo"]`

### **CMD vs ENTRYPOINT Comparison**

| Aspect                | CMD                              | ENTRYPOINT                         |
|-----------------------|----------------------------------|------------------------------------|
| Purpose               | Provides default arguments or a command to run. | Defines the main executable that should always run. |
| Overridable?          | Yes, can be overridden by `docker run <command>`. | No, unless overridden by `--entrypoint` option. |
| When to use?          | Use when the command can be overridden frequently. | Use when you want a fixed command with optional arguments. |
| Combination with the other? | Can be combined with `ENTRYPOINT` to supply default arguments. | Can be combined with `CMD` to provide default arguments. |

### **Combining ENTRYPOINT and CMD**

You can use both `ENTRYPOINT` and `CMD` in a Dockerfile. In this case:
- `ENTRYPOINT` defines the fixed command that will run.
- `CMD` provides default arguments to the `ENTRYPOINT` command.

**Example**:
```Dockerfile
FROM ubuntu
ENTRYPOINT ["curl"]
CMD ["http://example.com"]
```
- When you run `docker run myimage`, it will execute `curl http://example.com`.
- If you run `docker run myimage http://google.com`, it will execute `curl http://google.com`, where `http://google.com` replaces the `CMD` argument but not the `ENTRYPOINT`.
