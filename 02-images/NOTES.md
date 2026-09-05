# Docker Images

## Introduction

A Docker image is a read only template/package used to create Docker containers.

An image contains the files, dependencies, libraries, configuration, and instructions required to run an application.

Simple flow:

```text
Docker Image
     ↓
Docker Container
     ↓
Running Application
```

---

## What is a Docker Image?

A Docker image is a packaged environment that contains everything required to run an application.

For example, a Node.js application may require:

```text
Node.js
npm
Application code
Dependencies
Configuration
```

A Docker image can package the required environment so that it can be used to create containers.

---

## Image as a Template

A useful way to understand images is:

```text
Image     = Template / Blueprint
Container = Instance created from the image
```

For example:

```text
Node.js Image
     ↓
┌────┼────┐
↓    ↓    ↓
C1   C2   C3
```

The same image can be used to create multiple containers.

---

## Docker Hub

Docker Hub is a public registry where Docker images can be stored and shared.

For example, Docker Hub provides images such as:

```text
ubuntu
node
nginx
python
```

We can download these images using:

```bash
docker pull <image>
```

---

## Pull an Image

The `docker pull` command downloads an image from a registry.

Example:

```bash
docker pull node
```

This downloads the Node.js image.

Another example:

```bash
docker pull ubuntu
```

---

## Image Tags

Docker images can have tags.

A tag usually identifies a specific version or variant of an image.

Example:

```text
node:22
```

Here:

```text
node
 ↓
Image name

22
 ↓
Tag
```

Another example:

```text
ubuntu:24.04
```

Here:

```text
ubuntu
 ↓
Image name

24.04
 ↓
Tag
```

---

## Why Use Tags?

Tags allow us to specify which version of an image we want.

For example:

```bash
docker pull node:22
```

This requests the Node.js image with the `22` tag.

Instead of depending on an unspecified version, explicitly using a tag can make the environment more predictable.

---

## Pull a Specific Image Version

Example:

```bash
docker pull node:22
```

You can then check the downloaded image:

```bash
docker images
```

---

## List Docker Images

Use:

```bash
docker images
```

This displays images available locally.

You can also use:

```bash
docker image ls
```

Both commands list local Docker images.

Example output:

```text
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
node         22        abc123         2 days ago    ...
ubuntu       latest    def456         5 days ago    ...
```

---

## Understanding Image Information

The output of:

```bash
docker images
```

contains information such as:

### Repository

The image name.

Example:

```text
node
```

### Tag

The version or variant of the image.

Example:

```text
22
```

### Image ID

A unique identifier for the image.

Example:

```text
abc123
```

### Created

Shows when the image was created.

### Size

Shows the size of the image.

---

## Image Name and Tag

Docker commonly identifies an image using:

```text
<repository>:<tag>
```

Example:

```text
node:22
```

Another example:

```text
ubuntu:24.04
```

If no tag is specified, Docker commonly uses:

```text
latest
```

For example:

```bash
docker pull ubuntu
```

is commonly interpreted as:

```text
ubuntu:latest
```

It is better to understand that `latest` is a tag name, not necessarily the newest version in every technical sense.

---

## Inspect an Image

Docker provides detailed information about an image using:

```bash
docker image inspect <image>
```

Example:

```bash
docker image inspect node:22
```

This can show information such as:

* Image configuration
* Architecture
* Operating system
* Environment variables
* Entry point
* Command
* Layers
* Metadata

---

## Remove a Docker Image

To remove an image:

```bash
docker rmi <image>
```

Example:

```bash
docker rmi node:22
```

You can also use:

```bash
docker image rm node:22
```

Both commands can remove an image.

---

## Image Cannot Be Removed

If a container is using an image, Docker may not allow the image to be removed.

For example:

```text
Image
  ↓
Container
```

The container may need to be removed first before removing the image, depending on its state and usage.

Check containers:

```bash
docker ps -a
```

Then remove the relevant container if it is no longer needed:

```bash
docker rm <container-id>
```

Then remove the image:

```bash
docker rmi <image>
```

---

## Docker Image Layers

Docker images are built using layers.

Conceptually:

```text
Application Layer
       ↓
Dependency Layer
       ↓
Base Image Layer
       ↓
Docker Image
```

Each instruction in a Dockerfile can create a layer.

Layers help Docker reuse existing data instead of rebuilding everything from scratch.

This can make image building more efficient.

We will explore layers more deeply when learning Dockerfiles.

---

## Image vs Container

It is important not to confuse these two.

### Image

```text
Read-only template/package
```

### Container

```text
Running or stopped instance of an image
```

Simple example:

```text
node:22
   ↓
Docker Image
   ↓
┌──────────┬──────────┐
↓          ↓
Container  Container
```

---

## Important Commands

### List images

```bash
docker images
```

### Alternative command

```bash
docker image ls
```

### Pull an image

```bash
docker pull node
```

### Pull a specific tag

```bash
docker pull node:22
```

### Inspect an image

```bash
docker image inspect node:22
```

### Remove an image

```bash
docker rmi node:22
```

### Alternative remove command

```bash
docker image rm node:22
```

---

## Practical Practice

### Practice 1 — Check Docker Version

```bash
docker --version
```

---

### Practice 2 — List Existing Images

```bash
docker images
```

Observe the images currently available on your machine.

---

### Practice 3 — Pull Node.js Image

Run:

```bash
docker pull node:22
```

Docker will download the Node.js 22 image from the configured registry.

---

### Practice 4 — Verify the Image

Run:

```bash
docker images
```

You should see something similar to:

```text
REPOSITORY   TAG
node         22
```

---

### Practice 5 — Inspect the Image

Run:

```bash
docker image inspect node:22
```

Look through the output and identify information such as:

```text
Architecture
OS
Environment
Entrypoint
Cmd
Layers
```

---

### Practice 6 — Remove the Image

If you no longer need the image:

```bash
docker rmi node:22
```

Then verify:

```bash
docker images
```

---

## Common Mistakes

### 1. Confusing Image and Container

Remember:

```text
Image     = Template
Container = Instance
```

---

### 2. Forgetting the Tag

These are different references:

```text
node
node:22
```

`node` commonly refers to:

```text
node:latest
```

while:

```text
node:22
```

specifically requests the `22` tag.

---

### 3. Thinking `docker pull` Creates a Running Container

This command:

```bash
docker pull node:22
```

only downloads the image.

It does not start a container.

To create and start a container, we use:

```bash
docker run node:22
```

---

### 4. Trying to Remove an Image Being Used

If an image is being used by a container, Docker may prevent its removal.

Check:

```bash
docker ps -a
```

---

### 5. Assuming `latest` Always Means Newest Version

`latest` is simply a tag.

It does not technically guarantee that it represents the newest version at every moment.

---

## Interview Questions

### Q1. What is a Docker image?

A Docker image is a read only template/package used to create Docker containers.

### Q2. What is the difference between an image and a container?

An image is a template, while a container is an instance created from that image.

### Q3. What is Docker Hub?

Docker Hub is a public registry used to store and share Docker images.

### Q4. What does `docker pull` do?

It downloads an image from a Docker registry to the local machine.

### Q5. What does `docker images` do?

It lists Docker images available locally.

### Q6. What is a Docker image tag?

A tag identifies a particular version or variant of an image.

Example:

```text
node:22
```

Here `22` is the tag.

### Q7. What is the purpose of `docker image inspect`?

It displays detailed information about a Docker image.

### Q8. How do you remove a Docker image?

```bash
docker rmi <image>
```

### Q9. Does `docker pull` create a container?

No. It only downloads the image.

### Q10. Can multiple containers be created from one image?

Yes. One image can be used to create multiple containers.

---

## Quick Revision

```text
Docker Image
     ↓
Read-only template/package

Docker Hub
     ↓
Registry for Docker images

docker pull
     ↓
Download image

docker images
     ↓
List local images

docker image inspect
     ↓
Show image details

docker rmi
     ↓
Remove image

Image
     ↓
Container
```

### Example

```bash
docker pull node:22
```

```bash
docker images
```

```bash
docker image inspect node:22
```

```bash
docker rmi node:22
```