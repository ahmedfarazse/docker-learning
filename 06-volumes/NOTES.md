# Docker Volumes

## Introduction

Containers are designed to be replaceable and temporary.

If important data is stored only inside a container's writable layer, that data can be lost when the container is removed.

Docker Volumes provide a way to store data separately from the container.

The main purpose of a volume is:

> Persistent storage for containerized applications.

---

## What is a Docker Volume?

A Docker Volume is storage managed by Docker that can be mounted inside one or more containers.

Simple structure:

```text
Container
    |
    | writes data
    v
Docker Volume
    |
    | persists independently
    v
Data remains after container removal
```

The important idea is:

```text
Container lifecycle != Data lifecycle
```

The container can be deleted while the volume remains.

---

## Why Do We Need Volumes?

Suppose a container contains:

```text
/data
    └── notes.txt
```

If the data exists only inside the container and the container is removed, the data may also be lost.

With a volume:

```text
Container
    |
    v
/data
    |
    v
Docker Volume
```

The data is stored in the volume and can be reused by another container.

---

## Create a Volume

Use:

```bash
docker volume create my-volume
```

Example:

```bash
docker volume create practice-volume
```

---

## List Volumes

To see all Docker volumes:

```bash
docker volume ls
```

Example output:

```text
DRIVER    VOLUME NAME
local     practice-volume
```

---

## Inspect a Volume

Use:

```bash
docker volume inspect my-volume
```

This shows information such as:

* Volume name
* Driver
* Mountpoint
* Scope

---

## Mount a Volume

A volume must be mounted into a container to be used by the application.

### Using `--mount`

```bash
docker run -it --mount source=my-volume,target=/data ubuntu bash
```

Here:

```text
source=my-volume
```

means the Docker volume.

```text
target=/data
```

means the location inside the container.

So:

```text
my-volume
     |
     v
/data
```

---

## Using `-v`

There is also a shorter syntax:

```bash
docker run -it -v my-volume:/data ubuntu bash
```

The format is:

```text
-v VOLUME:CONTAINER_PATH
```

Example:

```bash
-v my-volume:/data
```

means:

```text
my-volume ---> /data
```

---

## `--mount` vs `-v`

Both can be used to mount volumes.

### `--mount`

```bash
docker run -it --mount source=my-volume,target=/data ubuntu bash
```

### `-v`

```bash
docker run -it -v my-volume:/data ubuntu bash
```

The `--mount` syntax is more explicit and easier to understand when working with multiple mount options.

The `-v` syntax is shorter and commonly seen in Docker commands.

---

# Practical Example

## Step 1 — Create Volume

```bash
docker volume create practice-volume
```

Check it:

```bash
docker volume ls
```

---

## Step 2 — Start Container

```bash
docker run -it --mount source=practice-volume,target=/data ubuntu bash
```

You are now inside the Ubuntu container.

---

## Step 3 — Move to Volume Directory

Inside the container:

```bash
cd /data
```

Check the directory:

```bash
pwd
```

Expected:

```text
/data
```

---

## Step 4 — Create Data

Create a file:

```bash
echo "Docker Volumes Practice" > notes.txt
```

Check it:

```bash
cat notes.txt
```

Expected:

```text
Docker Volumes Practice
```

---

## Step 5 — Exit Container

```bash
exit
```

---

## Step 6 — Find the Container

```bash
docker ps -a
```

Find the container you just used.

---

## Step 7 — Remove Container

```bash
docker rm <container-id>
```

The container has now been removed.

---

## Step 8 — Create Another Container

Use the same volume:

```bash
docker run -it --mount source=practice-volume,target=/data ubuntu bash
```

Move into the volume:

```bash
cd /data
```

Check files:

```bash
ls
```

You should see:

```text
notes.txt
```

Read it:

```bash
cat notes.txt
```

Expected:

```text
Docker Volumes Practice
```

This demonstrates data persistence.

---

# Important Concept

Removing the container does not automatically remove the named volume.

Example:

```text
Container 1
     |
     v
practice-volume
     |
     v
notes.txt
```

Container 1 is removed:

```text
Container 1  X
     |
     v
practice-volume
     |
     v
notes.txt
```

A new container can use the same volume:

```text
Container 2
     |
     v
practice-volume
     |
     v
notes.txt
```

---

## Remove a Volume

To remove a volume:

```bash
docker volume rm practice-volume
```

Docker normally requires the volume to no longer be in use.

You can check volumes first:

```bash
docker volume ls
```

---

## Volume vs Container

| Container                    | Volume                          |
| ---------------------------- | ------------------------------- |
| Runs the application         | Stores persistent data          |
| Can be temporary             | Designed for persistent storage |
| Can be removed/recreated     | Can survive container removal   |
| Contains application runtime | Contains application data       |

---

## Common Use Cases

Volumes are commonly useful for:

* Databases
* Uploaded files
* Application data
* Persistent configuration
* Development environments
* Shared data between containers

For example:

```text
PostgreSQL Container
        |
        v
PostgreSQL Volume
        |
        v
Database Data
```

If the PostgreSQL container is recreated, the database data can remain in the volume.

---

## Common Mistakes

### Mistake 1 — Thinking Every Container File Is Persistent

Not every file inside a container should be treated as persistent storage.

Important data should be stored using an appropriate persistent storage mechanism such as a volume.

---

### Mistake 2 — Removing the Volume Instead of the Container

These are different operations.

Remove container:

```bash
docker rm <container-id>
```

Remove volume:

```bash
docker volume rm my-volume
```

Be careful when deleting volumes because they may contain important data.

---

### Mistake 3 — Wrong Mount Path

Example:

```bash
docker run -it -v my-volume:/data ubuntu bash
```

The volume is mounted at:

```text
/data
```

If you create the file somewhere else, it may not be stored in the volume.

Correct:

```bash
cd /data
```

---

### Mistake 4 — Confusing Volume With Image

An image contains the application environment and files needed to create containers.

A volume is primarily used for persistent data.

```text
Image
  |
  v
Container
  |
  v
Volume
```

---

# Interview Questions

## 1. What is a Docker Volume?

A Docker Volume is Docker managed storage used to persist data independently from a container's lifecycle.

---

## 2. Why are Docker Volumes used?

They are used to preserve important data when containers are stopped, removed, or recreated.

---

## 3. Does removing a container remove a named volume?

Normally, removing a container does not automatically remove a named volume.

The volume can be reused by another container.

---

## 4. How do you create a Docker Volume?

```bash
docker volume create my-volume
```

---

## 5. How do you list Docker Volumes?

```bash
docker volume ls
```

---

## 6. How do you inspect a Volume?

```bash
docker volume inspect my-volume
```

---

## 7. How do you mount a Volume?

Using `--mount`:

```bash
docker run -it --mount source=my-volume,target=/data ubuntu bash
```

Or using `-v`:

```bash
docker run -it -v my-volume:/data ubuntu bash
```

---

## 8. What is the difference between `-v` and `--mount`?

Both can mount volumes, but `--mount` uses a more explicit key-value syntax while `-v` provides a shorter syntax.

---

## 9. Can multiple containers use the same volume?

Yes.

A Docker volume can be mounted by multiple containers, depending on the application's storage and access requirements.

---

## Quick Revision

### Create

```bash
docker volume create my-volume
```

### List

```bash
docker volume ls
```

### Inspect

```bash
docker volume inspect my-volume
```

### Mount with `--mount`

```bash
docker run -it --mount source=my-volume,target=/data ubuntu bash
```

### Mount with `-v`

```bash
docker run -it -v my-volume:/data ubuntu bash
```

### Remove

```bash
docker volume rm my-volume
```

### Core Concept

```text
Container = Application Runtime
Volume    = Persistent Data
```

Remember:

```text
Container can be removed
        ↓
Volume can remain
        ↓
New container can reuse the volume
```

