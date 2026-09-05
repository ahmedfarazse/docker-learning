# Docker Introduction

## Introduction

Docker is a platform used to build, package, ship, and run applications in containers.

Docker helps developers create a consistent environment for applications so that the application behaves more reliably across different systems.

A common problem in software development is:

```text
"It works on my machine."
```

Docker helps reduce this problem by packaging an application together with the environment and dependencies it needs.

---

## What is Docker?

Docker is a containerization platform.

It allows us to package an application with its dependencies into a container and run that container on different systems that support Docker.

Simple concept:

```text
Application
     +
Dependencies
     +
Configuration
     ↓
Docker Image
     ↓
Docker Container
     ↓
Running Application
```

---

## What is Containerization?

Containerization is a way of packaging an application and its required dependencies into an isolated environment called a container.

For example, suppose an application requires:

```text
Node.js
npm packages
Environment variables
Application code
Configuration
```

Instead of manually setting everything up on another machine, Docker can package the required environment into an image and run it as a container.

---

## What is a Container?

A container is an isolated environment in which an application or process runs.

Containers are created from Docker images.

```text
Docker Image
     ↓
Docker Container
```

Example:

```text
Node.js Image
     ↓
Node.js Container
     ↓
Node.js Application
```

A container is lightweight compared to a traditional virtual machine because containers share the host operating system's kernel.

---

## What is a Docker Image?

A Docker image is a read only template/package used to create containers.

An image contains the required files, dependencies, configuration, and instructions needed to run an application.

Example images:

```text
ubuntu
node
nginx
python
```

Simple analogy:

```text
Image     = Blueprint
Container = Building created from the blueprint
```

One image can be used to create multiple containers.

```text
          Docker Image
               ↓
       ┌───────┼───────┐
       ↓       ↓       ↓
 Container  Container  Container
```

---

## What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

Example:

```dockerfile
FROM node:22

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "app.js"]
```

The Dockerfile tells Docker how the image should be built.

Simple flow:

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Docker Container
```

---

## What is Docker Engine?

Docker Engine is the core technology that allows Docker containers to be created and managed.

It is responsible for tasks such as:

* Building images
* Running containers
* Managing containers
* Managing networks
* Managing storage

Simple concept:

```text
Docker CLI
     ↓
Docker Engine
     ↓
Images / Containers / Networks / Volumes
```

---

## What is Docker CLI?

Docker CLI stands for:

```text
Docker Command Line Interface
```

It allows us to interact with Docker using commands.

Examples:

```bash
docker --version
```

```bash
docker images
```

```bash
docker ps
```

```bash
docker run hello-world
```

We will use the Docker CLI extensively throughout this repository.

---

## What is Docker Hub?

Docker Hub is a public registry where Docker images can be stored and shared.

Developers can pull publicly available images from Docker Hub.

For example:

```bash
docker pull ubuntu
```

```bash
docker pull node
```

```bash
docker pull nginx
```

Simple concept:

```text
Docker Hub
     ↓
Docker Image
     ↓
docker pull
     ↓
Local Machine
```

---

## Docker vs Virtual Machine

Docker containers and virtual machines are both used for isolation, but they work differently.

### Virtual Machine

A virtual machine includes a complete guest operating system.

```text
Physical Machine
       ↓
Hypervisor
       ↓
Virtual Machine
       ↓
Guest Operating System
       ↓
Application
```

### Docker Container

Containers share the host operating system's kernel.

```text
Physical Machine
       ↓
Operating System
       ↓
Docker Engine
       ↓
Container
       ↓
Application
```

### Main Difference

```text
Virtual Machine
= Application + Guest OS

Container
= Application + Dependencies
  sharing Host OS Kernel
```

Containers are generally more lightweight and faster to start than full virtual machines.

---

## Docker Architecture — Basic View

A simplified Docker architecture looks like this:

```text
User
 ↓
Docker CLI
 ↓
Docker Engine
 ├── Images
 ├── Containers
 ├── Networks
 └── Volumes
```

The Docker CLI sends commands to the Docker Engine, and the Docker Engine performs the requested operations.

---

## Important Docker Terms

### Docker

Platform for building, packaging, and running applications in containers.

### Container

An isolated environment where an application or process runs.

### Image

A template/package used to create containers.

### Dockerfile

A file containing instructions for building an image.

### Docker Engine

The core component responsible for running and managing Docker resources.

### Docker CLI

Command-line interface used to interact with Docker.

### Docker Hub

Public registry for storing and sharing Docker images.

### Volume

A mechanism used to persist data generated or used by containers.

### Network

Allows containers and other systems to communicate with each other.

### Docker Compose

A tool used to define and run multi container applications.

---

## Basic Docker Workflow

A common Docker workflow is:

```text
Write Application
       ↓
Create Dockerfile
       ↓
Build Docker Image
       ↓
Run Container
       ↓
Test Application
       ↓
Push Image to Registry
```

Later, we will practice each important part of this workflow.

---

## Basic Docker Commands

Check Docker version:

```bash
docker --version
```

Get Docker information:

```bash
docker info
```

Show Docker help:

```bash
docker --help
```

Run a test container:

```bash
docker run hello-world
```

List images:

```bash
docker images
```

List running containers:

```bash
docker ps
```

These commands will be explained in detail in the upcoming topics.

---

## Practical Practice

### Practice 1 — Check Docker Version

Run:

```bash
docker --version
```

Observe the installed Docker version.

---

### Practice 2 — Check Docker Information

Run:

```bash
docker info
```

This displays information about the Docker environment.

---

### Practice 3 — Run Hello World

Run:

```bash
docker run hello-world
```

Docker will download the image if it is not already available locally and then create and run a container from it.

---

## Common Mistakes

### 1. Confusing Image and Container

Remember:

```text
Image = Template
Container = Instance
```

---

### 2. Thinking Docker is a Virtual Machine

Docker containers are not complete virtual machines.

Containers share the host operating system's kernel.

---

### 3. Thinking Dockerfile is the Container

A Dockerfile is only a set of instructions used to build an image.

```text
Dockerfile
     ↓
Image
     ↓
Container
```

---

### 4. Thinking Docker Hub is Docker

Docker Hub is a registry for storing and sharing images.

Docker itself is the containerization platform.

---

## Interview Questions

### Q1. What is Docker?

Docker is a platform used to build, package, and run applications in containers.

### Q2. What is containerization?

Containerization is the process of packaging an application and its dependencies into an isolated container.

### Q3. What is a Docker container?

A container is an isolated runtime environment created from a Docker image.

### Q4. What is a Docker image?

A Docker image is a read-only template/package used to create containers.

### Q5. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### Q6. What is Docker Hub?

Docker Hub is a public registry used to store and share Docker images.

### Q7. What is the difference between a container and a virtual machine?

A virtual machine includes a complete guest operating system, while containers share the host operating system's kernel.

### Q8. What is Docker Engine?

Docker Engine is the core component that builds and runs containers and manages Docker resources.

### Q9. What is Docker CLI?

Docker CLI is the command-line interface used to interact with Docker.

### Q10. Can multiple containers be created from one image?

Yes. One Docker image can be used to create multiple containers.

---

## Quick Revision

```text
Docker
  ↓
Containerization Platform

Image
  ↓
Template/Package

Container
  ↓
Running/isolated instance of an image

Dockerfile
  ↓
Instructions to build an image

Docker Engine
  ↓
Runs and manages Docker resources

Docker CLI
  ↓
Command line interface for Docker

Docker Hub
  ↓
Registry for Docker images

Basic Flow:

Dockerfile
    ↓
Image
    ↓
Container
    ↓
Application
```

