# Auth to DockerHub

We do not need to authenticate to DockerHub and provide hostname explicitly. It is used as default repository.
```bash
# By default, docker pulls from DockerHub
docker pull redis
# Is equivalent to
docker pull docker.io/library/redis:latest
```

# Authenticate to docker repositories

https://docs.docker.com/reference/cli/docker/login/

To login to a docker repository:\
`docker login <hostname[:port]>`

<img width="700" alt="01" src="https://github.com/user-attachments/assets/3f49aa53-1463-438e-b457-841ddd34c8a0">

### Docker Registry Protocols are implicit

- If the registry is reachable over **HTTPS** (the standard for security), Docker automatically uses it.
- If the registry is not using HTTPS and instead uses **HTTP**, Docker allows this but requires additional configuration due to security risks (more on this below).

### **Insecure Registries (HTTP)**
If your registry is using **HTTP** (an insecure protocol), Docker will block the connection by default due to security concerns. However, you can explicitly tell Docker to allow connections over HTTP by configuring it as an "insecure registry."

To use an HTTP registry, you must:

#### **Configure Docker for Insecure Registries**

**Edit Docker's daemon configuration**:
On most Linux systems, the configuration file for the Docker daemon is `/etc/docker/daemon.json`.

Add the following configuration to the file, specifying the registry you want to use with HTTP:

```json
{
 "insecure-registries": ["registry.example.com"]
}
```
On Docker-Desktop\
![image](https://github.com/user-attachments/assets/8830c787-b5e5-4952-bf9e-10bd0c709e64)

Even though no protocol is specified in the command, Docker will now allow HTTP connections to that registry.

### Summary

- Docker **implicitly assumes HTTPS** when logging into a registry, so you don't need to specify the protocol.
- If the registry uses **HTTP**, you must configure Docker to allow "insecure registries."

## Credentials storage

On your local host, Docker stores authentication credentials (including tokens) in a configuration file located in your home directory. This file, `config.json`, holds the login credentials (username and token or password) used to authenticate with Docker registries.

### Location of Docker Credentials

The credentials to authenticate against private docker repositories are stored in the `config.json` file, usually located at:

- **Linux and macOS**: `~/.docker/config.json`
- **Windows**: `C:\Users\<YourUsername>\.docker\config.json`

### Viewing the `config.json` File

You can open and view the contents of `config.json` to see your tokens or stored credentials. Here’s an example of how the file looks:

```json
{
  "auths": {
    "https://index.docker.io/v1/": {
      "auth": "dXNlcm5hbWU6dG9rZW4="
    }
  },
  "HttpHeaders": {
    "User-Agent": "Docker-Client/19.03.12 (linux)"
  },
  "credsStore": "desktop"  // Only present if using credential helpers. Here desktop - means use of Docker-Desktop store
}
```

In this file:

- The `auths` section contains the authentication information for each registry. The `auth` field contains a **Base64-encoded** string of your username and token (or password).
  
  To decode the `auth` value:
  ```bash
  echo "dXNlcm5hbWU6dG9rZW4=" | base64 --decode
  ```
  This will output:
  ```bash
  username:token
  ```

### Credential Helpers

In many cases, Docker uses **credential helpers** instead of storing the token directly in `config.json`. This is indicated by the presence of `"credsStore"` or `"credHelpers"` fields.

- **credHelpers**: This is used to store your credentials securely using an external program such as `docker-credential-pass`, `docker-credential-osxkeychain`, or `docker-credential-wincred`.
- If credential helpers are enabled, Docker does not store the token in plain text or even encoded in `config.json`. Instead, the credentials are stored securely by the respective OS or tool (e.g., macOS Keychain, Windows Credential Manager, or a password manager like `pass`).

For example:
```json
{
  "auths": {
    "https://index.docker.io/v1/": {}
  },
  "credsStore": "desktop"
}
```

In this case, Docker is using a credential store (e.g., macOS Keychain) to manage authentication, and the actual token is not visible in `config.json`.

***Note: On my ubuntu OS, Docker-Desktop uses `pass` with `docker-credential-helpers`. So knowing how to deal with pass, we can see the creds.***

### To summarize:
- **`~/.docker/config.json`** contains your stored credentials or references to external credential stores.
- You can decode Base64-encoded tokens, but if credential helpers are used, the tokens are stored securely elsewhere.
