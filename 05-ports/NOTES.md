# Docker Ports

## Introduction

Docker containers are isolated from the host machine.

If an application is running inside a container, its port is not automatically accessible from the host.

Docker provides **port mapping** to connect a host port with a container port.

---

## What is a Port?

A port is a logical endpoint used by applications to communicate over a network.

For example, a Node.js application may run on:

```text
Port 3000
```

Inside Docker, the application can listen on port `3000` inside the container.

---

## Host Port vs Container Port

There are two important ports:

### Host Port

The port on the host machine.

Example:

```text
localhost:3000
```

### Container Port

The port on which the application is listening inside the container.

Example:

```text
3000
```

---

## Docker Port Mapping

Docker maps a host port to a container port using the `-p` option.

Syntax:

```bash
docker run -p HOST_PORT:CONTAINER_PORT IMAGE
```

Example:

```bash
docker run -p 3000:3000 docker-port-app
```

This means:

```text
Host Port       Container Port
3000       ---> 3000
```

Now the application can be accessed from:

```text
http://localhost:3000
```

---

## Using Different Host and Container Ports

The host and container ports do not have to be the same.

Example:

```bash
docker run -p 8080:3000 docker-port-app
```

Here:

```text
Host Port       Container Port
8080       ---> 3000
```

The application still runs on port `3000` inside the container.

But from the host machine, we access it through:

```text
http://localhost:8080
```

---

## Understanding `-p`

Example:

```bash
docker run -p 8080:3000 docker-port-app
```

Breakdown:

```text
-p
│
└── Port mapping

8080
│
└── Host port

3000
│
└── Container port
```

---

## EXPOSE Instruction

Dockerfiles can contain the `EXPOSE` instruction.

Example:

```dockerfile
EXPOSE 3000
```

This tells Docker that the application inside the container is expected to use port `3000`.

However:

**`EXPOSE` does not publish the port to the host.**

You still need port mapping when running the container:

```bash
docker run -p 3000:3000 docker-port-app
```

---

## Practical Example

Create a project:

```text
docker-port-practice/
├── app.js
└── Dockerfile
```

### app.js

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.end("Hello from Docker Port Mapping!");
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

### Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## Build the Image

Inside the project directory:

```bash
docker build -t docker-port-app .
```

Check the image:

```bash
docker images
```

---

## Run the Container

```bash
docker run -p 3000:3000 docker-port-app
```

The Node.js application is now running inside the container.

Open the browser:

```text
http://localhost:3000
```

Expected output:

```text
Hello from Docker Port Mapping!
```

---

## Check Port Mapping

First check running containers:

```bash
docker ps
```

You should see a port mapping similar to:

```text
0.0.0.0:3000->3000/tcp
```

You can also use:

```bash
docker port <container-id>
```

Example:

```bash
docker port docker-port-app
```

If a container name is available:

```bash
docker port <container-name>
```

---

## Running on Another Host Port

Stop the existing container and run:

```bash
docker run -p 8080:3000 docker-port-app
```

Now open:

```text
http://localhost:8080
```

The application is still listening on:

```text
3000
```

inside the container.

Only the host side port changed.

---

## Port Mapping Diagram

```text
Browser
   |
   | http://localhost:8080
   v
Host Machine
   |
   | Port 8080
   v
Docker Port Mapping
   |
   | 8080 -> 3000
   v
Container
   |
   | Port 3000
   v
Node.js Application
```

---

## Important Difference

### Without Port Mapping

```bash
docker run docker-port-app
```

The application may be running inside the container, but the host cannot access it through `localhost:3000`.

### With Port Mapping

```bash
docker run -p 3000:3000 docker-port-app
```

The host can access the application through:

```text
http://localhost:3000
```

---

## Common Mistakes

### Mistake 1: Thinking EXPOSE Publishes the Port

```dockerfile
EXPOSE 3000
```

`EXPOSE` alone does not make the application available on the host.

Use:

```bash
docker run -p 3000:3000 image-name
```

---

### Mistake 2: Reversing the Ports

Incorrect:

```bash
docker run -p 3000:8080 docker-port-app
```

if the application is actually listening on container port `3000`.

Remember:

```text
-p HOST:CONTAINER
```

---

### Mistake 3: Application Listening on the Wrong Interface

For containerized applications, the application should generally listen on:

```text
0.0.0.0
```

rather than only:

```text
127.0.0.1
```

For example:

```javascript
server.listen(3000, "0.0.0.0");
```

This allows the application to accept connections through the container's network interface.

---

## Interview Questions

### 1. What is Docker port mapping?

Docker port mapping connects a port on the host machine to a port inside a container.

Example:

```bash
docker run -p 8080:3000 image-name
```

---

### 2. What does `-p 8080:3000` mean?

It means:

```text
Host port 8080 -> Container port 3000
```

---

### 3. What does `EXPOSE` do?

`EXPOSE` documents the port that the application is expected to use inside the container. It does not publish that port to the host by itself.

---

### 4. Can host and container ports be different?

Yes.

Example:

```bash
docker run -p 8080:3000 image-name
```

---

### 5. How can you check a container's port mapping?

Use:

```bash
docker port <container-id>
```

or:

```bash
docker ps
```

---

### 6. Why might a mapped port still not work?

Possible reasons include:

* Wrong container port
* Application is not running
* Application is listening only on `127.0.0.1`
* Incorrect port mapping
* Container stopped
* Application crashed

---

## Quick Revision

```text
-p HOST:CONTAINER
```

Example:

```bash
docker run -p 8080:3000 my-app
```

Means:

```text
localhost:8080
      |
      v
container:3000
```

Important commands:

```bash
docker run -p 3000:3000 image-name
docker run -p 8080:3000 image-name
docker ps
docker port <container-id>
```

Important Dockerfile instruction:

```dockerfile
EXPOSE 3000
```

Remember:

```text
EXPOSE != Publish
```

Port publishing is done with:

```bash
-p HOST_PORT:CONTAINER_PORT
```

