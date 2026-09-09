# Docker Compose

## Introduction

Modern applications often consist of multiple services.

For example:

```text
Frontend
Backend
Database
Redis
```

Managing every container manually with separate `docker run` commands can become difficult.

Docker Compose provides a way to define and manage multiple services using a single configuration file.

---

## What is Docker Compose?

Docker Compose is a tool for defining and managing multi container Docker applications.

Instead of running many commands manually, services can be defined in a Compose file.

Example:

```yaml
services:
  backend:
    image: node:22

  database:
    image: postgres
```

Docker Compose can then manage these services together.

---

# Compose File

A modern Compose project commonly uses:

```text
compose.yml
```

It can also use:

```text
docker-compose.yml
```

Example project:

```text
docker-compose-practice/
├── app.js
├── Dockerfile
└── compose.yml
```

---

# Basic Compose Structure

Example:

```yaml
services:
  app:
    image: nginx
    ports:
      - "8080:80"
```

Here:

```text
services
   |
   └── app
        |
        ├── image
        └── ports
```

---

# Services

A service represents a containerized component of an application.

Example:

```yaml
services:
  backend:
    image: my-backend

  database:
    image: postgres
```

There are two services:

```text
backend
database
```

Compose manages both services.

---

# `docker compose up`

Start the application:

```bash
docker compose up
```

Compose will read the Compose file and create/start the required services.

---

# Detached Mode

To run services in the background:

```bash
docker compose up -d
```

The `-d` option means:

```text
detached mode
```

The terminal remains available while the containers continue running.

---

# Build Before Starting

If the Compose file uses a Dockerfile:

```yaml
services:
  app:
    build: .
```

Run:

```bash
docker compose up --build
```

This builds the image before starting the service.

---

# Check Running Services

Use:

```bash
docker compose ps
```

This shows the services belonging to the current Compose project.

---

# Stop Services

To stop Compose services:

```bash
docker compose stop
```

This stops the containers without removing them.

---

# Docker Compose Down

Use:

```bash
docker compose down
```

This stops and removes the containers and the networks created for the Compose project.

Important:

```text
stop = stop containers

down = stop + remove Compose resources
```

---

# Compose Logs

View logs:

```bash
docker compose logs
```

Follow logs in real time:

```bash
docker compose logs -f
```

View logs for a specific service:

```bash
docker compose logs app
```

---

# Port Mapping

Ports can be defined inside Compose.

Example:

```yaml
services:
  app:
    image: nginx
    ports:
      - "8080:80"
```

Meaning:

```text
Host Port       Container Port
8080       ---> 80
```

The application can be accessed through:

```text
http://localhost:8080
```

---

# Compose Networking

Docker Compose automatically creates a network for the services in a Compose project.

Example:

```yaml
services:
  backend:
    image: my-backend

  database:
    image: postgres
```

Conceptually:

```text
             Compose Network
                  |
          ┌───────┴───────┐
          v               v
       backend         database
```

The services can communicate with each other through the Compose network.

---

# Service Name as Hostname

Suppose the database service is:

```yaml
services:
  database:
    image: postgres
```

The backend can use:

```text
database
```

as the database hostname when both services are connected to the same Compose network.

For example:

```text
DB_HOST=database
```

Do not normally use:

```text
DB_HOST=localhost
```

for reaching another container.

Inside a container, `localhost` refers to that container itself.

---

# Environment Variables

Environment variables can be defined in Compose.

Example:

```yaml
services:
  backend:
    image: my-backend
    environment:
      NODE_ENV: production
      PORT: 3000
```

The application can read these values from its environment.

For sensitive values such as passwords and API keys, avoid committing secrets directly into the Compose file.

---

# Volumes in Compose

Docker volumes can be defined in Compose.

Example:

```yaml
services:
  database:
    image: postgres
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

Concept:

```text
PostgreSQL Container
        |
        v
postgres-data
        |
        v
Persistent Database Data
```

The volume allows database data to persist independently of the container lifecycle.

---

# `depends_on`

A service can declare another service as a dependency.

Example:

```yaml
services:
  backend:
    image: my-backend
    depends_on:
      - database

  database:
    image: postgres
```

This tells Compose about the startup dependency.

Important:

`depends_on` does not automatically guarantee that the database is fully ready to accept application connections.

For production applications, health checks and proper readiness handling may also be required.

---

# Build From Dockerfile

Instead of using an existing image:

```yaml
services:
  app:
    image: my-app
```

we can build an image from a Dockerfile:

```yaml
services:
  app:
    build: .
```

Here:

```text
compose.yml
     |
     v
build: .
     |
     v
Dockerfile
     |
     v
Docker Image
     |
     v
Container
```

---

# Practical Project

Create:

```text
docker-compose-practice/
├── app.js
├── Dockerfile
└── compose.yml
```

---

## `app.js`

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.end("Hello from Docker Compose!");
});

server.listen(3000, "0.0.0.0", () => {
  console.log("Server running on port 3000");
});
```

---

## `Dockerfile`

```dockerfile
FROM node:22

WORKDIR /app

COPY app.js .

EXPOSE 3000

CMD ["node", "app.js"]
```

---

## `compose.yml`

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
```

---

# Run the Project

Inside the project directory:

```bash
docker compose up --build
```

The image will be built and the container will start.

Expected log:

```text
Server running on port 3000
```

Open:

```text
http://localhost:3000
```

Expected response:

```text
Hello from Docker Compose!
```

---

# Run in Background

Stop the current process if necessary and run:

```bash
docker compose up --build -d
```

Check:

```bash
docker compose ps
```

---

# View Logs

```bash
docker compose logs
```

Live logs:

```bash
docker compose logs -f
```

Specific service:

```bash
docker compose logs app
```

---

# Stop the Project

```bash
docker compose stop
```

Check:

```bash
docker compose ps
```

---

# Remove the Project

```bash
docker compose down
```

Check:

```bash
docker compose ps
```

---

# Docker Run vs Docker Compose

Without Compose:

```bash
docker build -t my-app .
docker run -d --name app -p 3000:3000 my-app
```

With Compose:

```bash
docker compose up --build -d
```

Configuration:

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
```

Compose becomes particularly useful when an application contains multiple services.

---

# Multi-Container Example

A typical application could contain:

```text
Frontend
Backend
PostgreSQL
Redis
```

Compose can define all of them:

```yaml
services:
  frontend:
    build: ./frontend

  backend:
    build: ./backend

  database:
    image: postgres

  redis:
    image: redis
```

Conceptually:

```text
                 Docker Compose
                       |
        ┌──────────────┼──────────────┐
        |              |              |
        v              v              v
    Frontend        Backend       Database
                       |
                       v
                     Redis
```

Compose manages the services as one application stack.

---

# Docker Concepts Connection

At this point, the Docker topics connect together:

```text
Dockerfile
    |
    v
Image
    |
    v
Container
    |
    ├── Ports
    ├── Volumes
    └── Networks
            |
            v
         Compose
```

Docker Compose brings these concepts together for multi container applications.

---

# Common Mistakes

## Mistake 1 — Wrong Compose Command

Modern Docker Compose is commonly used as:

```bash
docker compose up
```

Do not automatically assume:

```bash
docker-compose up
```

is the command you should use on every modern Docker installation.

---

## Mistake 2 — Wrong Port Mapping

Remember:

```text
HOST:CONTAINER
```

Example:

```yaml
ports:
  - "8080:3000"
```

means:

```text
8080 -> 3000
```

---

## Mistake 3 — Using `localhost` for Another Container

If backend needs to connect to a database service named:

```text
database
```

use:

```text
database
```

as the hostname on the Compose network.

`localhost` normally refers to the current container.

---

## Mistake 4 — Thinking `depends_on` Means "Ready"

`depends_on` expresses a service dependency and affects startup ordering, but it does not by itself guarantee that the dependency is application-ready.

---

## Mistake 5 — Forgetting `-d`

This:

```bash
docker compose up
```

runs attached to the terminal.

For background execution:

```bash
docker compose up -d
```

---

# Interview Questions

## 1. What is Docker Compose?

Docker Compose is a tool for defining and managing multi container Docker applications using a Compose configuration file.

---

## 2. What is a service in Docker Compose?

A service represents a containerized application component that Compose manages.

Examples:

```text
backend
database
redis
frontend
```

---

## 3. What does `docker compose up` do?

It creates and starts the services defined in the Compose file.

---

## 4. What does `docker compose down` do?

It stops and removes the containers and networks created for the Compose project.

---

## 5. What is the difference between `docker compose stop` and `docker compose down`?

`stop` stops services.

`down` stops and removes the Compose created containers and networks.

---

## 6. How do you run Compose in the background?

```bash
docker compose up -d
```

---

## 7. How do you view Compose logs?

```bash
docker compose logs
```

For live logs:

```bash
docker compose logs -f
```

---

## 8. How do services communicate in Docker Compose?

Services can communicate through the Compose created network and can normally use service names as hostnames.

---

## 9. Why is `localhost` usually wrong for connecting to another Compose service?

Because `localhost` inside a container normally refers to that same container.

---

## 10. What does `build: .` mean?

It tells Compose to build the service image using the Dockerfile and build context in the specified directory.

---

# Quick Revision

Start:

```bash
docker compose up
```

Background:

```bash
docker compose up -d
```

Build + start:

```bash
docker compose up --build
```

Check:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs
```

Live logs:

```bash
docker compose logs -f
```

Stop:

```bash
docker compose stop
```

Stop + remove:

```bash
docker compose down
```

---

## Core Concept

```text
Dockerfile
    ↓
Image
    ↓
Container
    ↓
Ports / Volumes / Networks
    ↓
Docker Compose
    ↓
Multi Container Application
```

The most important Compose idea:

> **Define your services once in a Compose file and manage the application stack with Docker Compose commands.**


