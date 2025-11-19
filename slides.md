---
# try also 'default' to start simple
theme: default
presenter: true
title: 'Cloud Computing | Virtualization: VMs and Containers'
titleTemplate: '%s - CPIT-490'
# apply any windi css classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: | 
    Cloud Computing | Virtualization: VMs and Containers
# page transition
transition: slide-left
# use UnoCSS
css: unocss
# Make content selectable/copyable
selectable: true
favicon: '/images/favicon.ico'
# Make slides downloadable as PDF
download: true
exportFilename: virtualization-slides
export:
  format: pdf
  timeout: 30000
  dark: false
  withClicks: false
  withToc: false
# enable slide recording and drawing
record: build
drawings:
  enabled: true
  persist: false
  presenterOnly: false
  syncAll: true
hideInToc: true
---

# Virtualization: VMs and Containers

### CPIT-490/632: Cloud Computing


<div class="absolute left-30px bottom-30px">
Spring 2024 &copy; Khalid Alharbi, Ph.D.
</div>


---
hideInToc: true
---

# Table of Contents

<Toc columns="2" maxDepth="1" mode="all" class="toc-list"/>

---
layout: fact
---


# Virtualization

## Virtualization enables running multiple operating system instances concurrently on the same physical machine.



---

## Virtualization

- Virtualization software manages computing resources (CPU, memory, and I/O) and their use among **guest** operating systems.
- Virtualization increases efficiency because the hardware is utilized to handle multiple guests simultaneously.
- Guest operating systems are isolated from each other and access the hardware through the hypervisor.

---

# Virtualization and Hypervisors
- A **Hypervisor** is a software layer that sits between the virtual machines (VMs) and the underlying hardware to handle sharing resources among guest operating systems.
- For example, you can have Linux (CentOS, Ubuntu, etc.), Unix (FreeBSD), and Windows running alongside.
- There are two types of hypervisors: Type 1 and Type 2

---

# Type 1 and Type 2 Hypervisors
<br>

![](/images/hypervisor-typ1-and-2.png)


---

# Type 1 Hypervisor (Bare Metal)

- Runs directly on the host's hardware to control the hardware and to manage guest operating systems.
- Offers better performance and efficiency because it has direct access to hardware.
- Generally used in cloud provider environments where performance, visibility, scalability, and reliability are critical.
- Examples include VMware ESXi, Microsoft Hyper-V, and Xen.

---

# Type 2 Hypervisor (Hosted)

- Runs on a host operating system just like other applications on the system.
- Performance is considered lower because it has to go through the host operating system to access hardware resources.
- Generally used in desktop environment for testing or personal use.
- Examples include Oracle VirtualBox, VMware Workstation, and Parallels Desktop.


---

## Machine Image
A machine image is a single static unit that contains a pre-configured operating system and installed software packages which are used to create new running machines.


---


# The Works on my Machine Problem (I)
- One common problem in software development is "works on my machine" phenomenon.
- Many developers set up a development environment with tools and libraries.
- The environment grows up over time with more added libraries and more changes to configuration options.
- The local development environment becomes very different from the target production environment.
- Libraries that are present on the development system may not exist on the production system.
- This leads into an unexpected errors and runtime behaviors.

---

## Solutions to the Works on my Machine Problem
- Create an isolated, dedicated development environment locally for each project that matches the production environment.
  - **Option 1**: Provision a custom VM with all the required development environment locally using virtualization software.
  - **Option 2**: Develop in a more managed, smaller, and isolated environment such as a _Container_ environment.



---


# Infrastructure as Code Approach
- All the previous solutions are central to the **infrastructure as code** approach.
- Infrastructure as code is the practice of managing and provisioning OS environments through machine-readable definition files, rather than manually applying changes to VM images and installing dependency libraries and tools.


---

# Creating and Managing Custom VM Images
- There're many virtualization systems that facilitate creating and managing virtual machine images.
  -  vSphere, the VMware virtualization suite of products.
- There're special tools for building and managing portable virtual machine images and environments.
  - [Vagrant](https://www.vagrantup.com/)
  - [Packer](https://packer.io)
  

---

# Vagrant
- Vagrant is an open source tool for provisioning and managing virtual machine environments.
- Vagrant can manage development environments running on a local virtualized platform such as VirtualBox or VMware, in the cloud via AWS, Azure, or any other provider, or in containers such as Docker.
- Vagrant can be used to create and manage an Infrastructure as a Service (IaaS)-like environment locally.


---

## Vagrant: Getting Started (I)
1. Install a hypervisor such as Oracle VirtualBox or VMWare Fusion.
2. [Install Vagrant](https://www.vagrantup.com) on your local environment.
3. Install a plugin for your hypervisor:
  - Vagrant comes with support out of the box for VirtualBox. However, if you are using VMware, you will need to install a plugin `vagrant plugin install vagrant-vmware-desktop`
4. You will need to pick a Vagrant Box, the package format for Vagrant environments.
5. Boxes can be installed locally from an image or from the [publicly available catalog of Vagrant boxes](https://vagrantcloud.com/boxes/search). 


---

## Vagrant: Getting Started (II)
- You will need to use the following commands:
  - `vagrant init <name_of_vm_image>`
  - `vagrant up`
  - `vagrant ssh`

---
layout: center
---

##  Using Vagrant (I) `vagrant init`

<br>

```bash
vagrant init debian/bookworm64
```

- The command above will create a file called _Vagrantfile_ that contains all the provisioning options to customize your VM (e.g., the amount of memory on the VM).
- `debian/bookworm64` is the name of the box, which is [Debian 12 (Bookworm)](https://app.vagrantup.com/debian/boxes/bookworm64).
- A box is a package format for Vagrant environments


---
layout: center
---

##  Using Vagrant (II) `vagrant up`

<br>

```bash
vagrant up
```

- The command above will download, start, and provision the vagrant VM environment.


---
layout: center
---

##  Using Vagrant (III) `vagrant ssh`

<br>

```bash
vagrant ssh
```

- The command above will connect to the local VM from the host OS via SSH
- To shut down the VM instance, use the command `vagrant halt`


---
layout: center
---

## Vagrant Demo


---

# Packer (I)

<br>

- [Packer](https://packer.io/) is an open source tool for building virtual machine images for multiple platforms from a single configuration file.
- Packer can be used to generate new machine images for multiple platforms with the changes made to them.
- This is in particular helpful in keeping development and production environments identical.
- To create an image, Packer launches a VM instance from a source image of your choice.
- Packer can customize the VM instance by running commands and scripts that may install libraries or customize configuration files.
- Finally, packer saves a copy of the Virtual Machine's state as an image.

---

## Packer (II)

<br>

- Packer and Vagrant are both tools developed by HashiCorp and they serve different but complementary purposes.
  - Vagrant creates and manages development environments abstracting away the need for direct interaction with the graphical user interface of the hypervisor (e.g., VirtualBox or VMware).
  - Packer Packer creates identical machine images for multiple platforms from a single configuration file.
- Packer uses a template configuration file in JSON format.
- Packer's template file defines how to create an image, add provisioning options, and install software and libraries for the image.
- To build the custom image, run the command `packer build <custom_image.json>`


---


## Packer: Getting Started

<br>

1. Download the Packer binary file from [https://packer.io](https://packer.io/)
2. Create a packer template file
3. Run 

```bash
packer build
```

---
layout: center
---

## Packer: Demo


---
layout: center
---

## Wrapping Up: Vagrant and Packer

- You can use Packer to create machine images with your application and all of its dependencies, and then use Vagrant to manage environments where these images run locally. 

- Both Vagrant and Packer are powerful tools to create identical and reproducible development, testing, and production environments.


---

---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## Docker: Up & Running
  A Comprehensive Guide to Containerization with Hands-on Activities
drawings:
  persist: false
transition: slide-left
title: Docker Up & Running - Enhanced Edition
---

# Linux Containers and Docker
## A Complete Training Guide with Practical Exercises

Building production-ready containerized applications from scratch

---
layout: center
---

# Agenda

- **Part I**: Docker Fundamentals and Core Concepts
- **Part II**: Installation and Environment Setup
- **Part III**: Working with Docker Images
- **Part IV**: Container Operations and Management
- **Part V**: Docker Compose for Multi-Container Applications
- **Part VI**: Production Deployment Best Practices
- **Part VII**: Container Orchestration Overview
- **Part VIII**: Advanced Topics and Optimization

**Hands-on Activities:**
- Activity 1: Creating Docker Compose Applications
- Activity 2: Building and Publishing to Docker Hub
- Activity 3: Deploying to Azure Container Services

---
layout: section
---

# Part I: Understanding Docker Fundamentals

---

# What is Docker?

Docker is a platform for developing, shipping, and running applications inside containers.

**The Problem Docker Solves:**
- "Works on my machine" syndrome
- Dependency conflicts between applications
- Inconsistent environments across dev/staging/production
- Complex deployment procedures
- Resource inefficiency with traditional VMs

**Docker's Solution:**
- Package applications with all dependencies
- Guarantee consistent behavior across environments
- Lightweight isolation using OS-level virtualization
- Simple, repeatable deployment process
- Efficient resource utilization

**Historical Context:**
- Launched by Solomon Hykes at PyCon 2013
- Open-sourced and gained rapid adoption
- Became the industry standard for containerization

---

# The Shipping Container Metaphor

## Physical Shipping Containers Changed Global Trade

**Before containers (1950s):**
- Manual loading/unloading of cargo
- Different handling for each type of goods
- Weeks to load/unload ships
- High breakage and theft rates

**After standardization (1960s+):**
- Standard 20ft and 40ft containers
- Automated cranes and handling
- Hours to load/unload ships
- Secure, trackable shipments

---

## Docker Applies This to Software

**Before Docker:**
- Manual configuration of servers
- Different setup for each application
- Complex deployment procedures
- "Works on my machine" problems

**With Docker:**
- Standardized container format
- Automated deployment tools
- Consistent behavior everywhere
- Predictable, reliable deployments

---
layout: two-cols-header
---

# Containers vs Virtual Machines

## Understanding the Fundamental Difference

::left::

```
┌─────────────────────────────────────────────────────────────┐
│                    VIRTUAL MACHINES                         │
├─────────────────────────────────────────────────────────────┤
│  App A   │  App B   │  App C |                              │
│  Bins    │  Bins    │  Bins  |                              │
│  Libs    │  Libs    │  Libs  |                             │
├──────────┼──────────┼──────────┐                            │
│  Guest   │  Guest   │  Guest   │                            │
│  OS      │  OS      │  OS      │  Each VM = Full OS (GB)    │
├──────────┴──────────┴──────────|                            │
│       Hypervisor (VMware, Hyper-V, VirtualBox)              │
├─────────────────────────────────────────────────────────────┤
│              Host Operating System                          │
├─────────────────────────────────────────────────────────────┤
│              Physical Hardware                              │
└─────────────────────────────────────────────────────────────┘
```

::right::

```
┌─────────────────────────────────────────────────────────────┐
│                       CONTAINERS                            │
├─────────────────────────────────────────────────────────────┤
│  App A   │  App B   │  App C                                │
│  Bins    │  Bins    │  Bins                                 │
│  Libs    │  Libs    │  Libs                                 │
├──────────┴──────────┴─────────┤                             │
│      Docker Engine            │  Containers = Process (MB)  │
├─────────────────────────────────────────────────────────────┤
│              Host Operating System                          │
├─────────────────────────────────────────────────────────────┤
│              Physical Hardware                              │
└─────────────────────────────────────────────────────────────┘
```

---

**Key Differences:**

| Aspect | Virtual Machines | Containers |
|--------|------------------|------------|
| **Isolation** | Full OS virtualization | Process-level isolation |
| **Size** | GBs (includes full OS) | MBs (shares host kernel) |
| **Startup Time** | Minutes | Seconds or milliseconds |
| **Performance** | Overhead from hypervisor | Near-native performance |
| **Volumes** | 10s per host | 100s or 1000s per host |
| **Portability** | Platform-dependent | Highly portable |
| **Use Case** | Complete OS isolation | Application isolation |

---

# Core Benefits of Docker (I)

## 1. Packaging and Dependency Management

**Before Docker:**
```bash
# Manual installation instructions
sudo apt-get install python3.9
sudo apt-get install postgresql-12
sudo apt-get install nginx
pip install django==3.2
pip install celery==5.1
# ... dozens more steps
```

**With Docker:**
```dockerfile
FROM python:3.9
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
# Everything needed is packaged
```

---

## 2. Environmental Consistency

- **Development**: Same container on all developer machines
- **Testing**: CI/CD runs exact production container
- **Production**: Deploy the same tested artifact

**Result**: "Works on my machine" becomes "Works everywhere"

## 3. Resource Efficiency

- Containers share host kernel (no guest OS overhead)
- Start/stop in milliseconds
- Higher density: run more apps on same hardware
- Lower costs in cloud environments

---

## 4. Microservices Architecture Enabler

**Monolithic Application:**
```
┌─────────────────────────────┐
│   Single Large Application  │
│  - User Management          │
│  - Payments                 │
│  - Inventory                │
│  - Notifications            │
└─────────────────────────────┘
```

**Microservices with Docker:**
```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Users      │  │   Payments   │  │  Inventory   │
│  Container   │  │  Container   │  │  Container   │
└──────────────┘  └──────────────┘  └──────────────┘
```

---

## 5. Developer Productivity

- **Quick Environment Setup**: `docker-compose up` vs. hours of manual setup
- **Clean Development**: No conflicts between project dependencies
- **Easy Experimentation**: Try new technologies without polluting your system
- **Faster Onboarding**: New developers productive in minutes

## 6. Simplified CI/CD

- Build once, deploy anywhere
- Automated testing in identical environments
- Rollback is just deploying previous image version
- Blue-green deployments made simple

---

# Docker Architecture

## Client-Server Model

```
┌───────────────────────────────────────────────────────────┐
│                    Docker Architecture                    │
└───────────────────────────────────────────────────────────┘

┌─────────────────┐                    ┌──────────────────┐
│  Docker Client  │                    │  Docker Registry │
│                 │                    │   (Docker Hub)   │
│  $ docker run   │                    │                  │
│  $ docker build │                    │  - nginx         │
│  $ docker pull  │◄──────────────────►│  - postgres      │
│                 │   Push/Pull Images │  - custom images │
└────────┬────────┘                    └──────────────────┘
         │
         │ REST API calls
         │ (over Unix socket or TCP)
         ▼
┌─────────────────────────────────────────────────────────┐
│              Docker Daemon (dockerd)                    │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐  │
│  │   Images     │  │  Containers  │  │   Volumes   │  │
│  │              │  │              │  │             │  │
│  │ - Base layers│  │ - Running    │  │ - Data      │  │
│  │ - App layers │  │ - Stopped    │  │ - Persistent│  │
│  └──────────────┘  └──────────────┘  └─────────────┘  │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐                    │
│  │   Networks   │  │  Build Cache │                    │
│  │              │  │              │                    │
│  │ - bridge     │  │ - Layers     │                    │
│  │ - overlay    │  │ - Speed up   │                    │
│  └──────────────┘  └──────────────┘                    │
└─────────────────────────────────────────────────────────┘
```
---

**Components Explained:**

1. **Docker Client (CLI)**: User-facing tool for issuing commands
2. **Docker Daemon (Server)**: Background process that manages Docker objects
3. **REST API**: Interface between client and daemon
4. **Docker Registry**: Storage and distribution system for images

---

# Docker Core Concepts

## 1. Docker Image

**Definition**: A read-only template with instructions for creating a container.

**Characteristics:**
- Layered filesystem structure
- Contains application code, runtime, libraries, dependencies
- Immutable (never changes after built)
- Portable across systems

**Think of it as:** A program downloaded on the disk (i.e. not running)

```
Image Example: nginx:1.21-alpine
├── Layer 4: nginx configuration (2 MB)
├── Layer 3: nginx binary (5 MB)
├── Layer 2: Alpine packages (3 MB)
└── Layer 1: Alpine Linux base (5 MB)
Total: 15 MB
```

---

## 2. Docker Container

**Definition**: A runnable instance of an image.

**Characteristics:**
- Created from an image
- Has its own writable layer
- Can be started, stopped, moved, deleted
- Isolated from other containers and the host
- Can connect to networks and storage

**Think of it as:** A process (a running program)

```
Container Lifecycle:
Created → Running → Paused → Stopped → Removed
```

---
layout: two-cols-header
---


## 3. Docker Registry

::left::
**Docker Registry**: A storage and distribution system for Docker images.

**Types:**
- **Docker Hub**: Public registry (hub.docker.com)
- **Private Registries**: Self-hosted or cloud-based
  - Amazon ECR (Elastic Container Registry)
  - Azure Container Registry (ACR)
  - Google Container Registry (GCR)
  - Harbor, JFrog Artifactory

::right::

**Registry vs Repository:**
- **Registry**: The service hosting images (e.g., Docker Hub)
- **Repository**: A collection of related images (e.g., `nginx`)
- **Tag**: A specific version of an image (e.g., `nginx:1.21`)

---

## 4. Dockerfile

**Dockerfile**: A text file containing instructions to build a Docker image.

**Example:**
```dockerfile
FROM ubuntu:20.04                    # Base image
RUN apt-get update && \              # Execute commands
    apt-get install -y python3
COPY app.py /app/                    # Copy files
WORKDIR /app                         # Set working directory
CMD ["python3", "app.py"]            # Default command
```

**Think of it as:** A build script or blueprint for your image

---

# Understanding Image Layers

## How Layering Works

Every instruction in a Dockerfile creates a new layer:

```dockerfile
FROM alpine:3.17                     # Layer 1 (Base)
RUN apk add --no-cache python3       # Layer 2 (Add Python)
COPY requirements.txt /app/          # Layer 3 (Add requirements)
RUN pip3 install -r /app/requirements.txt  # Layer 4 (Install deps)
COPY . /app/                         # Layer 5 (Add app code)
```

---

**Layer Structure:**
```
┌─────────────────────────────┐ ◄── Container Layer (Writable)
├─────────────────────────────┤
│ Layer 5: Application code   │ ◄── Top Image Layer
├─────────────────────────────┤
│ Layer 4: Python packages    │
├─────────────────────────────┤
│ Layer 3: requirements.txt   │
├─────────────────────────────┤
│ Layer 2: Python runtime     │
├─────────────────────────────┤
│ Layer 1: Alpine Linux       │ ◄── Bottom Image Layer
└─────────────────────────────┘
```

---

**Benefits of Layering:**
1. **Efficiency**: Layers are reused across images
2. **Speed**: Only changed layers need rebuilding
3. **Storage**: Shared layers stored once
4. **Bandwidth**: Only new/changed layers downloaded

**Example Scenario:**
```
Image A uses: Alpine + Python + Flask
Image B uses: Alpine + Python + Django

Shared: Alpine + Python layers (saved once)
Unique: Flask vs Django layers
```

---
layout: section
---

# Part II: Docker Installation & Setup

---

# Installation Overview

Docker is available for multiple platforms with different installation methods.

**Linux (Recommended for Production):**
- Native Docker Engine
- Direct kernel integration
- Best performance
- Distributions: Ubuntu, Debian, Fedora, CentOS, RHEL, etc.

**macOS and Windows (Development):**
- Docker Desktop
- Includes GUI management
- Uses virtualization layer
- Easy setup with updates

---

# Docker Desktop Installation

## macOS Installation

**Requirements:**
- macOS 11 or newer
- Apple Silicon (M1/M2) or Intel processor
- At least 4GB RAM

**Installation Steps:**

1. Download Docker Desktop from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Open the `.dmg` file
3. Drag Docker to Applications
4. Launch Docker from Applications
5. Follow setup wizard
6. Verify installation:
```bash
docker --version
docker compose version
```

## Windows Installation

**Requirements:**
- Windows 11 64-bit or Windows 10 64-bit (Pro, Enterprise, Education)
- WSL 2 feature enabled
- BIOS virtualization enabled

**Installation Steps:**

1. Enable WSL 2:
```powershell
wsl --install
```
2. Download Docker Desktop for Windows
3. Run installer
4. Restart computer
5. Launch Docker Desktop
6. Choose WSL 2 backend (recommended)

---

# Docker Desktop Features

## What's Included

**Core Components:**
- Docker Engine
- Docker CLI
- Docker Compose
- Docker Content Trust
- Kubernetes (optional)
- Credential Helper

## Key Features

**1. GUI Dashboard**
- View running containers
- Manage images and volumes
- Monitor resource usage
- View container logs
- Start/stop with one click

**2. Development Tools**
- Built-in file sharing
- Port forwarding to localhost
- Volume management
- Dev Environments (Beta)

**3. Extensions**
- Disk usage analyzer
- Resource saver
- Logs explorer
- Security scanning

**4. Resource Management**
```
Settings → Resources:
- CPU allocation
- Memory limits
- Swap size
- Disk image size
```

---

# Verifying Your Installation

## Essential Verification Commands

### 1. Check Docker Version

```bash
# Display Docker version information
docker version

# Expected output:
# Client: Docker Engine - Community
#  Version:           24.0.7
#  API version:       1.43
#  ...
# Server: Docker Engine - Community
#  Engine:
#   Version:          24.0.7
#   API version:      1.43 (minimum version 1.12)
```
---

### 2. View System Information

```bash
# Display system-wide information
docker info

# Key information shown:
# - Containers (running, paused, stopped)
# - Images count
# - Server version
# - Storage driver
# - Plugins
# - Memory and CPU
```

---

### 3. Run Test Container

```bash
# Download and run the hello-world image
docker run hello-world

# What happens:
# 1. Docker client contacts Docker daemon
# 2. Daemon pulls "hello-world" image from Docker Hub
# 3. Daemon creates new container from image
# 4. Daemon streams output to Docker client
# 5. Container exits after displaying message
```

---

# Understanding docker CLI

## Command Structure

```bash
docker [OPTIONS] COMMAND [ARG...]
```

## Command Categories

**Container Management:**
```bash
docker container run      # Create and start a container
docker container ls       # List containers
docker container stop     # Stop container
docker container rm       # Remove container
docker container logs     # View logs
docker container exec     # Execute command in container
```

**Image Management:**
```bash
docker image ls           # List images
docker image pull         # Download image
docker image build        # Build image from Dockerfile
docker image push         # Upload image to registry
docker image rm           # Remove image
docker image tag          # Tag an image
```

**System Management:**
```bash
docker system info        # Display system information
docker system df          # Show disk usage
docker system prune       # Remove unused data
```

**Getting Help:**
```bash
docker --help             # General help
docker container --help   # Help for container commands
docker run --help         # Help for specific command
```

---

# Docker Client-Server Communication

## How Commands Work

When you run a Docker command:

```
┌──────────────────────────────────────────────────────┐
│ Step 1: User enters command                          │
│ $ docker container run nginx                         │
└────────────────────┬─────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────┐
│ Step 2: Docker Client (CLI)                          │
│ - Parses command                                     │
│ - Validates syntax                                   │
│ - Converts to API call                               │
└────────────────────┬─────────────────────────────────┘
                     │
                     │ REST API (JSON)
                     │ Unix socket: /var/run/docker.sock
                     │ TCP: localhost:2375 (if enabled)
                     ▼
┌──────────────────────────────────────────────────────┐
│ Step 3: Docker Daemon (dockerd)                      │
│ - Receives API request                               │
│ - Authenticates                                      │
│ - Executes operation                                 │
│ - Returns response                                   │
└────────────────────┬─────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────┐
│ Step 4: Docker Engine performs action                │
│ - Check if image exists locally                      │
│ - Pull from registry if needed                       │
│ - Create container                                   │
│ - Start container                                    │
└────────────────────┬─────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────┐
│ Step 5: Result returned to client                    │
│ - Container ID displayed                             │
│ - Logs streamed (if -it flag used)                   │
└──────────────────────────────────────────────────────┘
```

---

# Your First Docker Commands

## Exploring Docker with Hands-On Examples

### Example 1: Interactive Shell

```bash
# Run an Ubuntu container with interactive shell
docker container run -it ubuntu:22.04 /bin/bash

# What the flags mean:
# -i: Interactive (keep STDIN open)
# -t: Allocate pseudo-TTY (terminal)
# ubuntu:22.04: Image name and tag
# /bin/bash: Command to run

# Inside the container:
root@abc123:/#  cat /etc/os-release
root@abc123:/#  apt-get update
root@abc123:/#  apt-get install curl
root@abc123:/#  curl https://example.com
root@abc123:/#  exit  # Exit and stop container
```

### Example 2: Running in Detached Mode

```bash
# Run nginx web server in background
docker container run -d -p 8080:80 --name my-nginx nginx:alpine

# Flags explained:
# -d: Detached (run in background)
# -p 8080:80: Map port 8080 on host to port 80 in container
# --name my-nginx: Give container a friendly name
# nginx:alpine: Image (nginx web server on Alpine Linux)

# Visit http://localhost:8080 in browser

# View running containers
docker container ls

# View container logs
docker container logs my-nginx

# Stop the container
docker container stop my-nginx
```

---

# More First Commands

### Example 3: Inspecting Containers and Images

```bash
# List all images
docker image ls

# List running containers
docker container ls

# List all containers (including stopped)
docker container ls -a

# Get detailed information about a container
docker container inspect my-nginx

# View resource usage statistics
docker container stats my-nginx

# View processes running in container
docker container top my-nginx
```
---

### Example 4: Cleaning Up

```bash
# Remove a stopped container
docker container rm my-nginx

# Force remove a running container
docker container rm -f my-nginx

# Remove an image
docker image rm nginx:alpine

# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune -a

# Remove everything (containers, images, volumes, networks)
docker system prune -a --volumes
```

---
layout: section
---

# Part III: Working with Docker Images

---

# Understanding Docker Images

## What is a Docker Image?

A Docker image is a **lightweight, standalone, executable package** that includes everything needed to run a piece of software:

**Contents of an Image:**
- Application code
- Runtime environment (Node.js, Python, Java, etc.)
- System libraries
- System tools
- Configuration files
- Environment variables

---

**Key Characteristics:**

1. **Read-only**: Images never change after built
2. **Layered**: Built from layers stacked on top of each other
3. **Portable**: Same image runs anywhere Docker runs
4. **Versioned**: Tagged with versions (e.g., `nginx:1.21`)
5. **Shareable**: Stored in registries and distributed

**Image Naming Convention:**
```
[registry/][namespace/]repository:tag

Examples:
nginx:latest                    # Docker Hub official image
docker.io/library/nginx:1.21    # Full format
mycompany/webapp:v2.3.1         # Custom image with version
gcr.io/project/app:latest       # Google Container Registry
```

---

# Image Layers

## How Layers Work

Each instruction in a Dockerfile creates a new layer:

```dockerfile
FROM node:18-alpine              # Layer 1: Base (5 MB)
WORKDIR /app                     # Layer 2: Metadata only (0 MB)
COPY package*.json ./            # Layer 3: Package files (1 KB)
RUN npm install                  # Layer 4: Dependencies (50 MB)
COPY . .                         # Layer 5: Source code (2 MB)
EXPOSE 3000                      # Layer 6: Metadata only (0 MB)
CMD ["node", "server.js"]        # Layer 7: Metadata only (0 MB)
```

**Visual Representation:**
```
┌─────────────────────────────┐
│ Container Layer (Writable)  │ ← Your running app can write here
├─────────────────────────────┤
│ CMD ["node", "server.js"]   │ ← Layer 7 (metadata)
├─────────────────────────────┤
│ EXPOSE 3000                 │ ← Layer 6 (metadata)
├─────────────────────────────┤
│ COPY . .                    │ ← Layer 5 (2 MB - app code)
├─────────────────────────────┤
│ RUN npm install             │ ← Layer 4 (50 MB - node_modules)
├─────────────────────────────┤
│ COPY package*.json ./       │ ← Layer 3 (1 KB - package files)
├─────────────────────────────┤
│ WORKDIR /app                │ ← Layer 2 (metadata)
├─────────────────────────────┤
│ FROM node:18-alpine         │ ← Layer 1 (5 MB - base OS)
└─────────────────────────────┘
```

**Layer Benefits:**
- **Caching**: Unchanged layers reused
- **Storage Efficiency**: Shared layers stored once
- **Build Speed**: Only rebuild changed layers
- **Distribution**: Only transfer new layers

---

# Anatomy of a Dockerfile

## Dockerfile Instructions

A Dockerfile is a text document containing all commands to assemble an image.

### Base Image Instructions

```dockerfile
# FROM: Specify the base image (required, must be first)
FROM node:18-alpine
# Alpine = lightweight Linux distribution (~5MB)

# FROM can also use multi-stage builds
FROM node:18 AS builder    # Build stage
FROM node:18-alpine        # Runtime stage (new base)
```

### Configuration Instructions

```dockerfile
# LABEL: Add metadata to image
LABEL maintainer="devops@example.com"
LABEL version="1.0"
LABEL description="My web application"

# ENV: Set environment variables
ENV NODE_ENV=production
ENV PORT=3000
ENV DATABASE_URL=postgres://db:5432/app

# ARG: Build-time variables (not available in container)
ARG VERSION=1.0.0
ARG BUILD_DATE
```
---

### File and Directory Instructions

```dockerfile
# WORKDIR: Set working directory (creates if doesn't exist)
WORKDIR /app

# COPY: Copy files from build context to image
COPY package.json package-lock.json ./
COPY src/ ./src/
COPY public/ ./public/

# ADD: Like COPY but also handles URLs and tar extraction
# (Prefer COPY for clarity)
ADD https://example.com/file.tar.gz /tmp/
```

---

# More Dockerfile Build Instructions

### Build Instructions

```dockerfile
# RUN: Execute commands during build
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Chain commands with && for efficiency
# Clean up in same layer to reduce size

# RUN can use different forms
RUN ["executable", "param1", "param2"]  # Exec form
RUN command param1 param2                # Shell form
```

---

### Runtime Instructions

```dockerfile
# EXPOSE: Document which ports the container listens on
# (Does NOT actually publish the port)
EXPOSE 3000
EXPOSE 8080 8443

# CMD: Default command to run (can be overridden)
CMD ["node", "server.js"]
# Only one CMD per Dockerfile (last one wins)

# ENTRYPOINT: Main executable (harder to override)
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["node", "server.js"]  # Arguments to ENTRYPOINT

# USER: Set user for RUN, CMD, ENTRYPOINT
USER node:node
# Security best practice: don't run as root

# VOLUME: Create mount point for external storage
VOLUME ["/data"]
# Data persists even when container is removed
```

---

# Complete Dockerfile Example

## Node.js Application

```dockerfile
# Use official Node.js image as base
FROM node:18-alpine AS base

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production && \
    npm cache clean --force

# Copy application code
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001 && \
    chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Expose application port
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Health check
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD node healthcheck.js

# Start application
CMD ["node", "server.js"]
```
---

**Best Practices Applied:**
-  Use specific base image version
-  Minimize layers (combine RUN commands)
-  Copy dependencies before code (caching)
-  Run as non-root user (security)
-  Clean up package manager cache
-  Include health check