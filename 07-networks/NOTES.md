# Docker Networks

## Introduction

Docker containers are isolated environments.

When multiple containers are part of the same application, they often need to communicate with each other.

For example:

```text
Frontend
   |
   v
Backend
   |
   v
Database
```

Docker Networks provide the networking layer that allows containers to communicate.

---

## What is a Docker Network?

A Docker Network is a virtual network managed by Docker that allows containers to communicate with each other and, depending on the configuration, with the outside world.

Simple example:

```text
Container 1
     |
     |
     v
Docker Network
     |
     |
     v
Container 2
```

---

## Why Do We Need Docker Networks?

Suppose an application has:

```text
Backend Container
Database Container
```

The backend needs to communicate with the database.

A Docker network can connect them:

```text
Backend
   |
   | Docker Network
   v
Database
```

This is especially useful in multi container applications.

---

# Docker Network Commands

## List Networks

```bash
docker network ls
```

This shows the Docker networks available on the system.

Typical default networks include:

```text
bridge
host
none
```

---

## Create a Network

```bash
docker network create my-network
```

Check:

```bash
docker network ls
```

You should see:

```text
my-network
```

---

## Inspect a Network

```bash
docker network inspect my-network
```

This provides information about the network, including connected containers.

---

# Running Containers on a Network

A container can be connected to a specific network when it is created.

Example:

```bash
docker run -d --name app1 --network my-network nginx
```

Another container:

```bash
docker run -d --name app2 --network my-network nginx
```

Now:

```text
        my-network
       /          \
      v            v
    app1          app2
```

Both containers are connected to the same network.

---

# Container-to-Container Communication

One of the important features of a user defined Docker network is that containers can communicate using container names.

Suppose we have:

```text
app1
app2
```

Both are connected to:

```text
my-network
```

From `app1`, `app2` can be addressed by its name:

```text
app2
```

For an HTTP service, this could be:

```text
http://app2
```

Docker's embedded DNS resolves the container name to the appropriate container IP on the network.

---

# Practical Example

## Step 1 — Create Network

```bash
docker network create practice-network
```

Check:

```bash
docker network ls
```

---

## Step 2 — Start First Container

```bash
docker run -d --name app1 --network practice-network nginx
```

---

## Step 3 — Start Second Container

```bash
docker run -d --name app2 --network practice-network nginx
```

---

## Step 4 — Check Containers

```bash
docker ps
```

You should see:

```text
app1
app2
```

---

## Step 5 — Inspect Network

```bash
docker network inspect practice-network
```

You should find both containers connected to the network.

---

# Testing Container Communication

Open a shell inside `app1`:

```bash
docker exec -it app1 bash
```

Inside the container, check whether `curl` exists:

```bash
which curl
```

If it is available:

```bash
curl http://app2
```

Nginx should return an HTTP response.

This demonstrates:

```text
app1
 |
 | HTTP request
 v
Docker Network
 |
 v
app2
 |
 v
Nginx
```

Exit:

```bash
exit
```

---

# Connect an Existing Container

A running or existing container can be connected to a network.

Command:

```bash
docker network connect my-network <container-name>
```

Example:

```bash
docker network connect my-network app1
```

---

# Disconnect a Container

To disconnect a container:

```bash
docker network disconnect my-network app1
```

This removes the network connection but does not automatically remove the container.

---

# Remove a Network

To remove a network:

```bash
docker network rm my-network
```

If containers are still connected to the network, Docker may require them to be disconnected or removed first.

---

# Docker Network Drivers

Docker supports different network drivers.

Important drivers include:

```text
bridge
host
none
overlay
```

---

## Bridge

`bridge` is the common network driver for containers on a single Docker host.

Example:

```bash
docker network create my-network
```

By default, a user created network is typically a bridge network.

---

## Host

With the host network mode, the container shares the host's network stack.

Example:

```bash
docker run --network host nginx
```

This is different from normal bridge networking because there is no separate container network namespace in the usual sense.

---

## None

The `none` network mode provides a container with very limited networking.

Example:

```bash
docker run --network none nginx
```

This is useful when network access is intentionally not required.

---

## Overlay

Overlay networks are designed for communication across multiple Docker hosts.

They are associated with distributed container environments such as Docker Swarm.

For the current Docker learning roadmap, understanding `bridge` networks is the main priority.

---

# Default Bridge vs Custom Bridge

Docker provides a default `bridge` network.

However, user defined bridge networks provide better features for application setups, including convenient container name based communication through Docker's embedded DNS.

Example custom network:

```bash
docker network create my-network
```

Then:

```bash
docker run -d --name backend --network my-network nginx
docker run -d --name frontend --network my-network nginx
```

The containers can communicate using their names.

---

# Docker Network vs Port Mapping

These concepts are different.

## Port Mapping

Port mapping allows the host machine to access a container service.

Example:

```bash
docker run -p 8080:3000 my-app
```

Meaning:

```text
Host
8080
 |
 v
Container
3000
```

---

## Docker Network

Docker networking allows containers to communicate with each other.

Example:

```text
Backend
   |
   | Docker Network
   v
Database
```

---

## Combined Example

A real application might look like:

```text
Browser
   |
   | localhost:8080
   v
Backend Container
   |
   | Docker Network
   v
Database Container
```

Here:

* Port mapping connects the host to the backend.
* Docker networking connects the backend to the database.

---

# Real-World Example

Imagine a web application with three services:

```text
Frontend
Backend
PostgreSQL
```

They can be placed on a custom Docker network:

```text
             app-network
        ┌────────┼────────┐
        |        |        |
        v        v        v
    Frontend  Backend  PostgreSQL
```

The backend can communicate with PostgreSQL through the database container's network name and database port.

This pattern becomes especially important when working with Docker Compose.

---

# Common Mistakes

## Mistake 1 — Confusing Network with Port Mapping

Network:

```text
Container <-> Container
```

Port mapping:

```text
Host <-> Container
```

They solve different problems.

---

## Mistake 2 — Using the Wrong Container Name

If the container is named:

```bash
--name database
```

then the other container should use:

```text
database
```

as the hostname on the same user defined network.

---

## Mistake 3 — Containers Are Not on the Same Network

If two containers need to communicate, make sure they share an appropriate network.

Example:

```bash
docker run -d --name app1 --network my-network nginx
docker run -d --name app2 --network my-network nginx
```

---

## Mistake 4 — Assuming `localhost` Means Another Container

Inside a container:

```text
localhost
```

normally refers to **that same container**, not another container.

For example, from `backend`:

```text
localhost
```

means the backend container itself.

If the database container is named:

```text
database
```

the backend should normally connect to:

```text
database
```

rather than:

```text
localhost
```

when both are on the same user defined network.

This is a very important concept.

---

# Useful Commands

List networks:

```bash
docker network ls
```

Create:

```bash
docker network create my-network
```

Inspect:

```bash
docker network inspect my-network
```

Connect:

```bash
docker network connect my-network <container>
```

Disconnect:

```bash
docker network disconnect my-network <container>
```

Remove:

```bash
docker network rm my-network
```

Run container on network:

```bash
docker run -d --name app1 --network my-network nginx
```

---

# Interview Questions

## 1. What is a Docker Network?

A Docker Network is a virtual networking mechanism that allows Docker containers to communicate with each other and, depending on configuration, with external networks.

---

## 2. What is the default Docker network driver?

The commonly used default network driver for containers is `bridge`.

---

## 3. How do you create a custom Docker network?

```bash
docker network create my-network
```

---

## 4. How do you connect a container to a network?

```bash
docker network connect my-network container-name
```

---

## 5. How do containers communicate on a user-defined bridge network?

They can communicate using container names because Docker provides embedded DNS based name resolution on user defined networks.

---

## 6. What is the difference between Docker Network and Port Mapping?

Docker networking primarily enables communication between containers, while port mapping publishes a container port through a port on the host.

---

## 7. What does `localhost` mean inside a container?

`localhost` normally refers to the current container itself.

It does not automatically refer to another container.

---

## 8. What is a bridge network?

A bridge network provides networking between containers on the same Docker host.

---

# Quick Revision

### List networks

```bash
docker network ls
```

### Create network

```bash
docker network create my-network
```

### Run container on network

```bash
docker run -d --name app1 --network my-network nginx
```

### Inspect

```bash
docker network inspect my-network
```

### Connect

```bash
docker network connect my-network app1
```

### Disconnect

```bash
docker network disconnect my-network app1
```

### Remove

```bash
docker network rm my-network
```

---

## Core Concept

Remember these two:

```text
Port Mapping
Host <-> Container
```

```text
Docker Network
Container <-> Container
```

And this:

```text
localhost
   |
   v
Current Container
```

While on the same user defined network:

```text
backend
   |
   | request
   v
database
```

The backend can use:

```text
database
```

as the database hostname instead of relying on a fixed container IP.


