# Docker

## Purpose

This document aims to answer the following questions:

1. What is Docker?
2. Why are we using Docker?
3. What alternatives exist?
4. How do we install and verify Docker?

---

# 1. What is Docker?

Before understanding Docker, you must first understand what a **Container** is.

## What is a Container?

A container is an isolated environment that packages an application together with everything it needs to run.

Unlike a Virtual Machine, a container **does not have its own operating system**. Instead, it shares the host operating system's kernel while maintaining its own isolated filesystem, processes, and networking.

### Container Characteristics

- Lightweight
- Portable
- Isolated
- Contains an application and its dependencies
- Shares the host OS kernel

---

## What is Docker?

Docker is a container platform used to:

- Build containers
- Download images
- Run containers
- Network containers together
- Store images
- Manage storage volumes

Docker allows applications to be packaged into isolated, reproducible environments that are easy to deploy, update, and scale.

---

## Without Docker

Example Ubuntu Server

```
Ubuntu
├── MySQL
├── PostgreSQL
├── Python
├── NodeJS
└── Various dependencies
```

Imagine PostgreSQL is upgraded.

Another application was expecting the previous version.

Now the application breaks because multiple services are sharing the same environment.

---

## With Docker

Example architecture for **Your Fantasy**

```
Azure VM
│
├── Docker
│
├── PostgreSQL Container
│   ├── Accounts
│   ├── Characters
│   └── Inventory
│
├── Login API Container
│   ├── Login
│   ├── Registration
│   └── Authentication
│
├── Redis Container (Future)
│   ├── Sessions
│   └── Cache
│
└── Unreal Dedicated Server
```

Each service is isolated and maintains its own dependencies.

Updating one service does not directly affect another.

---

## Docker Image vs Docker Container

### Docker Image

A Docker Image is a blueprint or template for an application.

An image contains:

- Application
- Dependencies
- Configuration
- Instructions on how to start

Images are stored:

- Locally
- Docker Hub
- Private Container Registries

One Docker Image can create many Docker Containers.

---

### Docker Container

A Docker Container is a running (or stopped) instance created from a Docker Image.

Each container has its own:

- Filesystem
- Processes
- Network
- Environment Variables

Multiple containers can be created from the same image.

---

# 2. Use Case

Docker is used to package and deploy backend services independently.

For **Your Fantasy**, Docker will eventually host:

- PostgreSQL Database
- Login API
- Redis Cache
- Monitoring Tools
- Admin Dashboard
- Other backend microservices

### Note

The Unreal Engine Dedicated Server will initially run directly on the Azure VM for simplicity.

It may later be containerized if scaling requirements change.

---

## Benefits of Isolation

Separating services into containers provides several advantages:

- Updating the Login API does not affect PostgreSQL
- Updating PostgreSQL does not affect Redis
- Services can be restarted independently
- Easier debugging
- Easier deployments
- Dependency conflicts are minimized
- Services can be scaled independently

---

# 3. Alternatives

Docker is the most popular container platform, but alternatives include:

- Podman
- containerd
- CRI-O
- LXC/LXD

Docker was selected for **Your Fantasy** because of:

- Large community support
- Excellent documentation
- Docker Compose
- Industry adoption
- Ease of learning

---

# 4. Installation & Verification

## Step 1 - Download the Installation Script

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```

### Command Breakdown

| Option | Description |
|---------|-------------|
| curl | Downloads a file from the internet |
| -f | Fails silently if an error occurs |
| -s | Silent mode (no progress bar) |
| -S | Displays errors if they occur |
| -L | Follows redirects automatically |
| -o get-docker.sh | Saves the downloaded file locally |

---

### Verify the Download

```bash
head get-docker.sh
```

Displays the first few lines of the installer to verify that the script downloaded correctly.

---

## Step 2 - Execute the Installer

Docker can be installed in two ways.

### Option A

Execute the script using the shell.

```bash
sudo sh get-docker.sh
```

Explanation

- sudo → Execute as administrator
- sh → Execute using the shell

---

### Option B (Recommended)

Give the script execute permissions.

```bash
chmod +x get-docker.sh
```

Then execute it directly.

```bash
sudo ./get-docker.sh
```

Explanation

- chmod +x → Adds execute permission
- ./ → Execute the file in the current directory

---

## Complete Installation

```bash
curl -fsSL https://get.docker.com -o get-docker.sh

head get-docker.sh

chmod +x get-docker.sh

sudo ./get-docker.sh
```

---

# Verification

## Verify Docker CLI

```bash
docker --version
```

Expected Output

```
Docker version XX.XX.X
```

Confirms the Docker CLI is installed.

---

## Verify Docker Compose

```bash
docker compose version
```

Expected Output

```
Docker Compose version v2.xx.x
```

Confirms Docker Compose is installed.

---

## Verify Docker Service

Docker consists of two primary components:

- Docker CLI
- Docker Daemon

```
User
   │
Docker Command
   │
Docker Daemon
   │
Containers
```

Check the daemon status.

```bash
sudo systemctl status docker
```

Expected Output

```
Active: active (running)
```

Press **Q** to exit.

---

## Verify Container Creation

```bash
sudo docker run hello-world
```

Docker performs the following steps:

1. Searches for the image locally.
2. Image is not found.
3. Downloads the image from Docker Hub.
4. Creates a container.
5. Executes the application.
6. Prints the success message.
7. Stops the container.

---

## View Installed Images

```bash
docker images
```

Displays:

- Repository
- Tag
- Image ID

---

## View Containers

```bash
docker ps -a
```

Displays all containers, including stopped containers.

---

# Summary

Docker provides a standardized way to package, deploy, and manage backend services.

For **Your Fantasy**, Docker will serve as the foundation for hosting backend infrastructure including:

- PostgreSQL
- Login API
- Redis
- Monitoring
- Additional microservices

The Unreal Dedicated Server will initially remain outside Docker but may later be containerized as the project grows.

Docker was selected because it offers portability, isolation, scalability, and a workflow that closely aligns with modern DevOps practices.
