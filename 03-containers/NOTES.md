# Docker Containers

## Introduction

A Docker container is a running or stopped instance of a Docker image.

Docker images are templates, while containers are created from those images to run applications or processes.

Simple flow:

```text
Docker Image
     ↓
Docker Container
     ↓
Running Application
```

---

## What is a Docker Container?

A container is an isolated environment where an application or process runs.

For example:

```text
Node.js Image
     ↓
Node.js Container
     ↓
Node.js Application
```

A container provides an isolated environment while sharing the host operating system's kernel.

---

## Image vs Container

It is important to understand the difference.

### Docker Image

An image is a template or package used to create containers.

```text
Image = Template / Blueprint
```

### Docker Container

A container is an instance created from an image.

```text
Container = Instance
```

Example:

```text
Node.js Image
     ↓
┌──────────┬──────────┬──────────┐
↓          ↓          ↓
Container  Container  Container
```

One image can create multiple containers.

---

## Create and Run a Container

The basic command is:

```bash
docker run <image>
```

Example:

```bash
docker run hello-world
```

When we run this command, Docker generally:

1. Checks whether the image exists locally.
2. Downloads the image if required.
3. Creates a container.
4. Starts the container.
5. Runs the configured command.

---

## `docker run`

`docker run` is used to create and start a new container from an image.

Example:

```bash
docker run hello-world
```

Important:

```text
docker run
    ↓
Create new container
    +
Start container
```

It does not simply start an old container.

---

## List Running Containers

Use:

```bash
docker ps
```

This shows currently running containers.

Example:

```text
CONTAINER ID   IMAGE   STATUS
abc123         nginx   Up 10 seconds
```

If no containers are currently running, the output may not show any container rows.

---

## List All Containers

Use:

```bash
docker ps -a
```

This shows:

* Running containers
* Stopped containers
* Exited containers

Example:

```text
CONTAINER ID   IMAGE         STATUS
abc123         nginx         Up 10 seconds
def456         hello-world   Exited
```

---

## `docker ps` vs `docker ps -a`

```text
docker ps
    ↓
Running containers only

docker ps -a
    ↓
All containers
Running + Stopped + Exited
```

This distinction is very important.

---

## Interactive Containers

We can run a container and interact with its terminal.

Example:

```bash
docker run -it ubuntu bash
```

Here:

```text
docker run
    ↓
Create and start container

-it
    ↓
Interactive terminal

ubuntu
    ↓
Image

bash
    ↓
Command executed inside container
```

---

## What Does `-i` Mean?

The `-i` option means interactive.

It keeps standard input open so that we can interact with the process.

```bash
docker run -i ubuntu
```

---

## What Does `-t` Mean?

The `-t` option allocates a pseudo terminal.

```bash
docker run -t ubuntu
```

When combined:

```bash
docker run -it ubuntu bash
```

we get an interactive terminal session inside the container.

---

## Working Inside a Container

Run:

```bash
docker run -it ubuntu bash
```

After entering the container, try:

```bash
whoami
```

Then:

```bash
pwd
```

Then:

```bash
ls
```

You can also check the operating system:

```bash
cat /etc/os-release
```

These commands are executed inside the Ubuntu container.

---

## Exit a Container

To leave an interactive container:

```bash
exit
```

After exiting, check:

```bash
docker ps -a
```

You will usually see that the container has stopped because its main process (`bash`) has exited.

---

## Start a Stopped Container

If a container already exists but is stopped, use:

```bash
docker start <container-id>
```

Example:

```bash
docker start abc123
```

This starts an existing container.

Important difference:

```text
docker run
    ↓
Creates a new container

docker start
    ↓
Starts an existing container
```

---

## Start and Attach to a Container

For an interactive container, we can use:

```bash
docker start -ai <container-id>
```

Here:

```text
-a
↓
Attach

-i
↓
Interactive
```

Example:

```bash
docker start -ai abc123
```

This allows us to attach to the container's interactive terminal.

---

## Stop a Running Container

Use:

```bash
docker stop <container-id>
```

Example:

```bash
docker stop abc123
```

You can also use the container name:

```bash
docker stop my-container
```

Stopping a container does not delete it.

---

## Remove a Container

To remove a stopped container:

```bash
docker rm <container-id>
```

Example:

```bash
docker rm abc123
```

You can also remove it using its name:

```bash
docker rm my-container
```

After removal, the container no longer exists.

---

## Container ID and Name

Every Docker container has:

* Container ID
* Container name

Example:

```text
CONTAINER ID   IMAGE    NAMES
abc123         ubuntu   my-ubuntu
```

We can use either the ID or name with many Docker commands.

For example:

```bash
docker stop abc123
```

or:

```bash
docker stop my-ubuntu
```

---

## Container Lifecycle

A basic container lifecycle looks like this:

```text
Docker Image
     ↓
docker run
     ↓
Container Created
     ↓
Container Started
     ↓
Running
     ↓
docker stop
     ↓
Stopped
     ↓
docker start
     ↓
Running Again
     ↓
docker rm
     ↓
Container Removed
```

---

## Container Status

Containers can have different states.

Common examples include:

```text
Created
Running
Exited
```

You can check the state with:

```bash
docker ps -a
```

---

## Running Multiple Containers

One image can be used to create multiple containers.

For example:

```bash
docker run -it ubuntu bash
```

Exit the container:

```bash
exit
```

Then run another:

```bash
docker run -it ubuntu bash
```

Now two different containers can exist even though both were created from the same Ubuntu image.

---

## Important Commands

### Run a container

```bash
docker run hello-world
```

### List running containers

```bash
docker ps
```

### List all containers

```bash
docker ps -a
```

### Run interactive container

```bash
docker run -it ubuntu bash
```

### Start a stopped container

```bash
docker start <container-id>
```

### Start and attach

```bash
docker start -ai <container-id>
```

### Stop a container

```bash
docker stop <container-id>
```

### Remove a container

```bash
docker rm <container-id>
```

---

## Practical Practice

### Practice 1 — Run Hello World

Run:

```bash
docker run hello-world
```

Then:

```bash
docker ps
```

Then:

```bash
docker ps -a
```

Observe the container status.

---

### Practice 2 — Run Ubuntu Container

Run:

```bash
docker run -it ubuntu bash
```

Inside the container:

```bash
whoami
```

```bash
pwd
```

```bash
ls
```

```bash
cat /etc/os-release
```

Then exit:

```bash
exit
```

---

### Practice 3 — Find the Container

After exiting:

```bash
docker ps -a
```

Find the Ubuntu container ID and name.

---

### Practice 4 — Start the Container Again

Use:

```bash
docker start -ai <container-id>
```

Then exit:

```bash
exit
```

---

### Practice 5 — Stop a Container

If you have a running container:

```bash
docker ps
```

Then:

```bash
docker stop <container-id>
```

Verify:

```bash
docker ps -a
```

---

### Practice 6 — Remove a Container

Find a stopped container:

```bash
docker ps -a
```

Then:

```bash
docker rm <container-id>
```

Verify:

```bash
docker ps -a
```

---

## Common Mistakes

### 1. Confusing `docker run` and `docker start`

Remember:

```text
docker run
    ↓
Create + start a new container

docker start
    ↓
Start an existing container
```

---

### 2. Thinking `docker stop` Deletes a Container

This:

```bash
docker stop <container-id>
```

only stops the container.

To remove it:

```bash
docker rm <container-id>
```

---

### 3. Using `docker ps` to Find Stopped Containers

`docker ps` only shows running containers.

Use:

```bash
docker ps -a
```

to see all containers.

---

### 4. Confusing Image and Container

Remember:

```text
Image     = Template
Container = Instance
```

---

### 5. Forgetting `-it` for Interactive Terminal

If you want to work interactively inside a container, commonly use:

```bash
docker run -it ubuntu bash
```

---

## Interview Questions

### Q1. What is a Docker container?

A Docker container is an isolated runtime environment created from a Docker image.

### Q2. What is the difference between an image and a container?

An image is a template/package, while a container is an instance created from that image.

### Q3. What does `docker run` do?

It creates and starts a new container from an image.

### Q4. What does `docker ps` do?

It lists currently running containers.

### Q5. What does `docker ps -a` do?

It lists all containers, including stopped containers.

### Q6. What is the difference between `docker run` and `docker start`?

`docker run` creates a new container, while `docker start` starts an existing stopped container.

### Q7. How do you stop a Docker container?

```bash
docker stop <container-id>
```

### Q8. How do you remove a Docker container?

```bash
docker rm <container-id>
```

### Q9. What does `-it` mean?

`-i` enables interactive input and `-t` allocates a pseudo terminal.

### Q10. Can multiple containers be created from one image?

Yes. A single image can be used to create multiple containers.

---

## Quick Revision

```text
Image
  ↓
Template

Container
  ↓
Instance of an image

docker run
  ↓
Create + start new container

docker ps
  ↓
Running containers

docker ps -a
  ↓
All containers

docker start
  ↓
Start existing container

docker stop
  ↓
Stop container

docker rm
  ↓
Remove container

docker run -it ubuntu bash
  ↓
Interactive Ubuntu container
```

