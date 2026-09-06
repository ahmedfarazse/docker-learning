# Dockerfile

## Introduction

A Dockerfile is a text file that contains instructions for building a Docker image.

Instead of manually creating an image, we can define the required environment and commands inside a Dockerfile.

Basic flow:

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
    ↓
Application
```

---

## What is a Dockerfile?

A Dockerfile contains instructions that tell Docker how to build an image.

For example:

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

CMD ["node", "app.js"]
```

Docker reads these instructions from top to bottom and uses them to build an image.

---

## Why Use Dockerfile?

Without a Dockerfile, creating the same application environment repeatedly can be difficult.

A Dockerfile allows us to define the environment as code.

For example:

```text
Node.js version
Application files
Dependencies
Working directory
Startup command
```

can all be defined inside the Dockerfile.

This makes the image build process repeatable and easier to share.

---

## Dockerfile Naming

The standard filename is:

```text
Dockerfile
```

Usually there is no file extension.

Correct:

```text
Dockerfile
```

Not:

```text
Dockerfile.txt
```

---

## Basic Dockerfile Structure

A simple Node.js Dockerfile:

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["node", "app.js"]
```

The instructions are executed in order during the image build process.

---

# Dockerfile Instructions

## 1. FROM

`FROM` specifies the base image.

Example:

```dockerfile
FROM node:22
```

This tells Docker to use the Node.js 22 image as the starting point.

Other examples:

```dockerfile
FROM ubuntu:24.04
```

```dockerfile
FROM python:3.12
```

```dockerfile
FROM nginx
```

`FROM` is normally the first instruction in a Dockerfile.

---

## 2. WORKDIR

`WORKDIR` sets the working directory inside the image/container.

Example:

```dockerfile
WORKDIR /app
```

After this instruction, commands and file operations can use `/app` as the working directory.

For example:

```dockerfile
WORKDIR /app

COPY app.js .
```

The `app.js` file will be copied into:

```text
/app/app.js
```

---

## 3. COPY

`COPY` copies files or directories from the build context into the image.

Example:

```dockerfile
COPY app.js .
```

This copies:

```text
Host
 ↓
app.js
 ↓
Image
 ↓
/app/app.js
```

Another example:

```dockerfile
COPY . .
```

This copies the contents of the build context into the current working directory in the image.

---

## 4. RUN

`RUN` executes a command while the Docker image is being built.

Example:

```dockerfile
RUN npm install
```

This installs the Node.js dependencies during image building.

Another example:

```dockerfile
RUN apt-get update
```

`RUN` is mainly used to prepare the image.

---

## 5. CMD

`CMD` defines the default command that runs when a container starts.

Example:

```dockerfile
CMD ["node", "app.js"]
```

When a container is started from the image, Docker runs:

```bash
node app.js
```

---

# RUN vs CMD

This is an important Docker concept.

### RUN

Runs during image building.

```dockerfile
RUN npm install
```

Flow:

```text
Dockerfile
    ↓
docker build
    ↓
RUN executes
    ↓
Image created
```

### CMD

Runs when the container starts.

```dockerfile
CMD ["node", "app.js"]
```

Flow:

```text
Image
    ↓
docker run
    ↓
CMD executes
    ↓
Application starts
```

Simple difference:

```text
RUN
↓
Build time

CMD
↓
Container runtime
```

---

# Dockerfile Build Context

When we run:

```bash
docker build -t my-node-app .
```

the final `.` represents the current directory.

This directory is called the build context.

Example:

```text
dockerfile-practice/
├── Dockerfile
└── app.js
```

If we run:

```bash
docker build -t my-node-app .
```

Docker uses the current directory as the build context.

This allows instructions such as:

```dockerfile
COPY app.js .
```

to access the file.

---

# Build a Docker Image

The basic command is:

```bash
docker build -t <image-name> .
```

Example:

```bash
docker build -t my-node-app .
```

Here:

```text
docker build
    ↓
Build an image

-t
    ↓
Assign a name/tag

my-node-app
    ↓
Image name

.
    ↓
Current directory as build context
```

---

# Check the Image

After building:

```bash
docker images
```

You should see:

```text
REPOSITORY     TAG
my-node-app    latest
```

---

# Run the Custom Image

After building:

```bash
docker run my-node-app
```

If `app.js` contains:

```javascript
console.log("Hello from Docker!");
```

the output should be:

```text
Hello from Docker!
```

---

# Complete Example

Project:

```text
dockerfile-practice/
├── Dockerfile
└── app.js
```

### app.js

```javascript
console.log("Hello from Docker!");
```

### Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

CMD ["node", "app.js"]
```

Build:

```bash
docker build -t my-node-app .
```

Run:

```bash
docker run my-node-app
```

Output:

```text
Hello from Docker!
```

---

# Node.js Application with Dependencies

For a Node.js application that has dependencies, we can use:

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["node", "app.js"]
```

The general flow is:

```text
Base Node.js Image
        ↓
Set /app
        ↓
Copy package files
        ↓
Install dependencies
        ↓
Copy application
        ↓
Start application
```

---

# Dockerfile Instructions Learned

| Instruction | Purpose                                       |
| ----------- | --------------------------------------------- |
| `FROM`      | Selects the base image                        |
| `WORKDIR`   | Sets the working directory                    |
| `COPY`      | Copies files into the image                   |
| `RUN`       | Executes commands during image build          |
| `CMD`       | Defines the default container startup command |

---

# Dockerfile Workflow

A typical workflow is:

```text
1. Create application
        ↓
2. Create Dockerfile
        ↓
3. Write Dockerfile instructions
        ↓
4. Build image
        ↓
5. Check image
        ↓
6. Run container
        ↓
7. Test application
```

Commands:

```bash
docker build -t my-node-app .
```

```bash
docker images
```

```bash
docker run my-node-app
```

---

# Practical Practice

## Practice 1 — Create Project

Create a directory:

```bash
mkdir dockerfile-practice
```

Enter it:

```bash
cd dockerfile-practice
```

---

## Practice 2 — Create app.js

Create:

```bash
nano app.js
```

Add:

```javascript
console.log("Hello from Docker!");
```

Save the file.

---

## Practice 3 — Create Dockerfile

Create:

```bash
nano Dockerfile
```

Add:

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

CMD ["node", "app.js"]
```

Save the file.

---

## Practice 4 — Build Image

Run:

```bash
docker build -t my-node-app .
```

Check:

```bash
docker images
```

You should find:

```text
my-node-app
```

---

## Practice 5 — Run Container

Run:

```bash
docker run my-node-app
```

Expected output:

```text
Hello from Docker!
```

---

## Practice 6 — Check Container

Run:

```bash
docker ps -a
```

You should see the container created from:

```text
my-node-app
```

---

# Common Mistakes

## 1. Wrong Dockerfile Name

Use:

```text
Dockerfile
```

instead of:

```text
Dockerfile.txt
```

---

## 2. Running `docker build` in the Wrong Directory

If your Dockerfile is in:

```text
dockerfile-practice/
```

run:

```bash
docker build -t my-node-app .
```

from that directory.

---

## 3. Confusing RUN and CMD

Remember:

```text
RUN
↓
Build time

CMD
↓
Container runtime
```

---

## 4. Forgetting the Build Context

This command:

```bash
docker build -t my-node-app .
```

uses:

```text
.
```

as the build context.

---

## 5. Forgetting to Build the Image

After changing the Dockerfile, build the image again:

```bash
docker build -t my-node-app .
```

Then run:

```bash
docker run my-node-app
```

---

## 6. Confusing Dockerfile with Docker Image

They are different:

```text
Dockerfile
    ↓
Instructions

Docker Image
    ↓
Built package/template

Container
    ↓
Running instance
```

---

# Interview Questions

### Q1. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### Q2. What does `FROM` do?

`FROM` specifies the base image for the Docker image.

### Q3. What does `WORKDIR` do?

`WORKDIR` sets the working directory inside the image/container.

### Q4. What does `COPY` do?

`COPY` copies files or directories from the build context into the image.

### Q5. What does `RUN` do?

`RUN` executes a command during the image build process.

### Q6. What does `CMD` do?

`CMD` defines the default command executed when a container starts.

### Q7. What is the difference between `RUN` and `CMD`?

`RUN` executes during image building, while `CMD` executes when a container starts.

### Q8. What does `docker build` do?

It builds a Docker image using a Dockerfile and build context.

### Q9. What does `-t` mean in `docker build -t my-node-app .`?

`-t` assigns a name and optionally a tag to the image.

### Q10. What does `.` mean in `docker build -t my-node-app .`?

It specifies the current directory as the build context.

---

# Quick Revision

```text
Dockerfile
    ↓
Contains build instructions

FROM
    ↓
Base image

WORKDIR
    ↓
Working directory

COPY
    ↓
Copy files into image

RUN
    ↓
Execute command during build

CMD
    ↓
Default command when container starts

docker build
    ↓
Dockerfile → Image

docker run
    ↓
Image → Container
```

## Most Important Example

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

CMD ["node", "app.js"]
```

Build:

```bash
docker build -t my-node-app .
```

Run:

```bash
docker run my-node-app
```
