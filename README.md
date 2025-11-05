# 🐳 Docker

## 📋 Table of Contents
- [1️⃣ Definition](#1️⃣-definition)
- [2️⃣ Docker Architecture](#2️⃣-docker-architecture)
  - [🧱 2.1 Docker Image](#21-docker-image)
  - [📦 2.2 Docker Container](#22-docker-container)
  - [🗄️ 2.3 Docker Registry](#23-docker-registry)
  - [💻 2.4 Docker Client](#24-docker-client)
  - [⚙️ 2.5 Docker Daemon](#25-docker-daemon)
  - [🌐 2.6 Docker Namespace](#26-docker-namespace)
  - [🛠️ 2.7 Docker Workflow](#27-docker-workflow)

---

## 1️⃣ Definition

**Docker** is a platform that lets you **package, ship, and run applications in isolated environments called containers**.

Think of it like putting your app and everything it needs (code, libraries, configs, etc.) into a lightweight, portable box that works the same way no matter where you run it—on your laptop, a server, or in the cloud.

### 🔹 Simple analogy

Imagine you're shipping furniture. Instead of trying to load random pieces loosely into a truck (which might break or not fit), you pack everything neatly into a **standard shipping container**.
Docker does the same for software—it puts your app in a **standard container** so it runs smoothly anywhere Docker is installed.

### 🔑 Key points

* **Containers** are lightweight and fast (unlike full virtual machines).
* Docker ensures **"it works on my machine"** isn't a problem anymore.
* You define your app's environment in a file called `Dockerfile`.
* Multiple containers can work together using **Docker Compose**.
* Widely used in development, testing, and production.

---

## 2️⃣ Docker Architecture

---

### 🧱 2.1 Docker Image

An **image** is a read-only template with instructions for creating a Docker container.
You may build an image which is based on the Ubuntu , SQL Server ,nginx or python ... .

It contains:

* The application code
* Runtime (e.g., Python, .NET)
* Libraries, dependencies
* Environment variables
* Configuration files (if baked in)
* Instructions for how to run the app (`CMD`, `ENTRYPOINT`)

---

#### 💡 **"based on Ubuntu or SQL Server" what does it mean?**

You can create your own Docker image by starting from an existing base image—like one that already has Ubuntu (a Linux OS) or SQL Server (a database) installed.

**Example:**

```docker
# Start FROM a base image: Ubuntu
FROM ubuntu:22.04

# Install Python on top of Ubuntu
RUN apt-get update && apt-get install -y python3

# Copy your app code
COPY myapp.py /app/

# Set the command to run your app
CMD ["python3", "/app/myapp.py"]
```

✅ **Why do this?**

* Saves time: Don't reinvent the wheel—use trusted base images.
* Consistency: Everyone builds from the same Ubuntu/SQL version.
* Security & updates: Official base images (like `ubuntu:` or `mssql/server`) are maintained by experts.

---

#### ❓ **But what mean read-only if can add my config?**

✅ **During build (via Dockerfile):**
You can add files, install software, and configure settings—this defines the image.

❌ **After build:**
The image becomes locked—you cannot change anything inside it directly (becomes like an ISO).

🔄 **At runtime:**
When you run a container, Docker adds a temporary writable layer on top of the read-only image so your app can write logs, temp files, etc.—but the original image stays untouched.

---

### 📦 2.2 Docker Container

A container is a running instance of a Docker image—a lightweight, isolated, and secure environment where your application actually executes.

💡 **Real-Life Use Case**

You build a custom image for your Python app → `my-python-app`.

Then you run:
- 1 container for development
- 3 containers for testing (different configs)
- 10 containers in production (scaled behind a load balancer)

✅ All from the same read-only image → guaranteed consistency!  
✅ Each has its own writable space for logs, user uploads, etc.

---

### 🗂️ 2.3 Docker Registry

A Docker registry is a storage and distribution system for Docker images.

**Think of it like:**
- GitHub for Docker images
- App Store for container templates
- A warehouse where you push (upload) and pull (download) images.

**🌐 Types of Docker Registries:**
1. **Docker Hub (The Default Public Registry)**  
   🌍 URL: https://hub.docker.com  
2. **Private / Self-Hosted Registries**  
   You can run your own registry:
   - Docker Registry (open-source, by Docker)
   - **Cloud-Based Private Registries:**
     - AWS: Amazon ECR (Elastic Container Registry)
     - Azure: Azure Container Registry (ACR)
     - Google Cloud: Google Container Registry (GCR) / Artifact Registry
     - GitLab / GitHub: Also offer container registries

**🔑 Key Concepts:**
- **Repository (repo)**: A collection of images with the same name but different tags (e.g., `my-app:1.0`, `my-app:latest`)
- **Tag**: A label for a specific version of an image (e.g., `v1.2`, `latest`, `alpine`)
- **Image Name Format**: `[registry-host/][username/]image-name[:tag]`

---

### 💻 2.4 Docker Client

The Docker Client is your interface to Docker — it lets you control containers and images by sending commands to the Docker Daemon.

---

### ⚙️ 2.5 Docker Daemon

The Docker Daemon (process name: `dockerd`) is the background service that does all the real work in Docker.

- It listens for commands from the Docker Client (like `docker run`, `docker build`).
- It manages Docker objects: images, containers, networks, and volumes.
- It communicates directly with the Linux kernel (or Windows container subsystem) to create and run containers.

💡 **Simple Analogy:**  
→ The Docker Client is your voice.  
→ The Docker Daemon is your brain + body — it hears your commands and makes things happen.

**What is the difference between Docker Daemon and Docker Engine?**  

Docker Engine is made up of:
- **Docker Daemon (`dockerd`)**  
  → The background service that manages Docker objects (images, containers, etc.).
- **Docker Client (`docker` CLI)**  
  → The command-line tool you use to talk to the Daemon.
- **Docker REST API**  
  → The interface that lets the Client (or other tools) communicate with the Daemon.

**In summary:**

| Term | What It Is |
|------|------------|
| **Docker Engine** | The **whole system**: Daemon + CLI + APIs + container runtime |
| **Docker Daemon** (`dockerd`) | The **central "brain"** that manages containers, images, networks, etc. |

---

### 🌐 2.6 Docker Namespace

Docker namespaces are a Linux kernel feature that Docker uses to isolate containers from each other and from the host system.

They give each container its own private view of the system — like having your own little world inside the same machine (like Filesystem, Network, Hostname...)
---
### 🛠️ 2.7 Docker Workflow

![Docker Workflow](https://github.com/yassin-elkhamlichi/Docker/blob/main/Docker%20Workflow.png)
