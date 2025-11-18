# TryHackMe - Intro to Docker

![TryHackMe](https://img.shields.io/badge/TryHackMe-Intro%20to%20Docker-red)
![Category](https://img.shields.io/badge/Category-Container%20Security-blue)
![Level](https://img.shields.io/badge/Level-Intermediate-green)
![Time](https://img.shields.io/badge/Time-120%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Hands-On](https://img.shields.io/badge/Hands--On-Practical%20Labs-yellow)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: Basic Docker Syntax](#task-2-basic-docker-syntax)
- [Task 3: Running Your First Container](#task-3-running-your-first-container)
- [Task 4: Intro to Dockerfiles](#task-4-intro-to-dockerfiles)
- [Task 5: Intro to Docker Compose](#task-5-intro-to-docker-compose)
- [Task 6: Intro to the Docker Socket](#task-6-intro-to-the-docker-socket)
- [Task 7: Practical](#task-7-practical)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This hands-on room provides comprehensive practical experience with Docker containers, covering everything from basic commands to advanced container orchestration with Docker Compose.

## 🎓 Learning Objectives

- Master basic Docker syntax and commands
- Run and deploy first containers
- Understand Docker image distribution
- Create custom images using Dockerfiles
- Use Docker Compose for container orchestration
- Apply knowledge in practical scenarios

## 📖 Task 1: Introduction

**Objective:** Introduction to Docker hands-on learning and prerequisites.

**Key Concepts:**
- Practical Docker container deployment
- Dockerfile creation and image building
- Docker Compose orchestration
- Linux fundamentals required

**Answer:** 
```
No answer needed
```

## 🖥️ Task 2: Basic Docker Syntax

### Docker Command Categories
- Running containers
- Managing & inspecting containers
- Managing Docker images
- Docker daemon stats and information

### Image Management Commands

**docker pull**
- Downloads images from repositories
- Syntax: `docker pull [IMAGE_NAME]`
- Tags specify image versions (e.g., `ubuntu:22.04`)

**docker image ls**
- Lists all images on local system
- Shows repository, tag, image ID, creation date, size

**docker image rm**
- Removes images from local system
- Requires image name with tag: `docker image rm ubuntu:22.04`

### Tag Usage Examples
| Image | Tag | Command | Explanation |
|-------|-----|---------|-------------|
| ubuntu | latest | `docker pull ubuntu` | Latest version |
| ubuntu | 22.04 | `docker pull ubuntu:22.04` | Specific version |
| ubuntu | 20.04 | `docker pull ubuntu:20.04` | Older version |

**Questions & Answers**

**If we wanted to pull a docker image, what would our command look like?**
```
docker pull
```

**If we wanted to list all images on a device running Docker, what would our command look like?**
```
docker image ls
```

**Let's say we wanted to pull the image "tryhackme" (no quotations); what would our command look like?**
```
docker pull tryhackme
```

**Let's say we wanted to pull the image "tryhackme" with the tag "1337" (no quotations). What would our command look like?**
```
docker pull tryhackme:1337
```

## 🚀 Task 3: Running Your First Container

### Docker Run Syntax
- `docker run [OPTIONS] IMAGE_NAME [COMMAND] [ARGUMENTS...]`
- Options determine container behavior
- Commands specify what to execute in container

### Common Run Options
| Option | Explanation | Example |
|--------|-------------|---------|
| `-d` | Run in detached mode (background) | `docker run -d nginx` |
| `-it` | Interactive mode with shell | `docker run -it ubuntu /bin/bash` |
| `-v` | Mount host directory to container | `docker run -v /host:/container nginx` |
| `-p` | Bind host port to container port | `docker run -p 80:80 nginx` |
| `--rm` | Remove container after execution | `docker run --rm alpine` |
| `--name` | Assign friendly name to container | `docker run --name web nginx` |

### Container Management
**docker ps**
- Lists running containers
- Shows container ID, image, command, status, ports, names

**docker ps -a**
- Lists all containers (including stopped)
- Useful for troubleshooting and cleanup

**Questions & Answers**

**What would our command look like if we wanted to run a container interactively?**
```
docker run -it
```

**What would our command look like if we wanted to run a container in "detached" mode?**
```
docker run -d
```

**Let's say we want to run a container that will run and bind a webserver on port 80. What would our command look like?**
```
docker run -p 80:80
```

**How would we list all running containers?**
```
docker ps
```

**Now, how would we list all containers (including stopped)?**
```
docker ps -a
```

## 🏗️ Task 4: Intro to Dockerfiles

### Dockerfile Instructions
| Instruction | Description | Example |
|-------------|-------------|---------|
| `FROM` | Sets base image (required) | `FROM ubuntu:22.04` |
| `RUN` | Executes commands in container | `RUN apt-get update` |
| `COPY` | Copies files from host to container | `COPY app/ /var/www/` |
| `WORKDIR` | Sets working directory | `WORKDIR /app` |
| `CMD` | Command to run when container starts | `CMD ["python", "app.py"]` |
| `EXPOSE` | Documents which ports to publish | `EXPOSE 80` |

### Dockerfile Example
```dockerfile
FROM ubuntu:22.04
RUN apt-get update -y && apt-get install apache2 -y
EXPOSE 80
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

### Building Images
- `docker build -t [IMAGE_NAME] [PATH]`
- `-t` tags the image with a name
- Path specifies Dockerfile location (`.` for current directory)

### Optimization Techniques
- Chain RUN commands to reduce layers
- Use minimal base images
- Remove cached files and documentation
- Install only essential packages

**Questions & Answers**

**What instruction would we use to specify what base image the container should be using?**
```
FROM
```

**What instruction would we use to tell the container to run a command?**
```
RUN
```

**What docker command would we use to build an image using a Dockerfile?**
```
build
```

**Let's say we want to name this image; what argument would we use?**
```
-t
```

## 🔄 Task 5: Intro to Docker Compose

### Docker Compose Overview
- Orchestrates multiple containers as services
- Defines container relationships and networking
- Uses YAML configuration files
- Simplifies multi-container deployments

### Common Commands
| Command | Purpose | Example |
|---------|---------|---------|
| `up` | Create and start containers | `docker-compose up` |
| `start` | Start existing containers | `docker-compose start` |
| `down` | Stop and remove containers | `docker-compose down` |
| `stop` | Stop containers | `docker-compose stop` |
| `build` | Build images | `docker-compose build` |

### docker-compose.yml Structure
```yaml
version: '3.3'
services:
  web:
    build: ./web
    networks:
      - ecommerce
    ports:
      - '80:80'
  
  database:
    image: mysql:latest
    networks:
      - ecommerce
    environment:
      - MYSQL_ROOT_PASSWORD=password

networks:
  ecommerce:
```

### Key Instructions
- `version`: Compose file version
- `services`: Container definitions
- `build`: Dockerfile location
- `ports`: Port mappings
- `volumes`: Directory mounts
- `environment`: Environment variables
- `networks`: Container networking

**Questions & Answers**

**I want to use docker-compose to start up a series of containers. What argument allows me to do this?**
```
up
```

**I want to use docker-compose to delete the series of containers. What argument allows me to do this?**
```
down
```

**What is the name of the .yml file that docker-compose uses?**
```
docker-compose.yml
```

## 🔌 Task 6: Intro to the Docker Socket

### Docker Architecture
- **Docker Client**: Command-line interface
- **Docker Server**: API that manages containers
- **Docker Socket**: Communication channel between client and server

### Interprocess Communication (IPC)
- Sockets allow processes to communicate
- Docker uses socket for client-server communication
- Can be network socket or file socket

### Security Considerations
- Docker socket allows remote container management
- Misconfiguration can lead to security vulnerabilities
- Useful for remote administration when properly secured

**Questions & Answers**

**What does the term "IPC" stand for?**
```
Interprocess Communication
```

**What technology can the Docker Server be equalled to?**
```
API
```

## 🎮 Task 7: Practical

### Hands-on Exercises
- Connect to provided virtual machine
- Identify running containers
- Deploy web server container
- Access web application to retrieve flag

### Practical Application
- Container inspection and management
- Port mapping and service deployment
- Real-world Docker usage scenarios

**Questions & Answers**

**Connect to the machine. What is the name of the container that is currently running?**
```
CloudIsland
```

**Use Docker to start a web server with the "webserver" image (no quotations). You will need to run the container with port 80. After starting the container, try to connect to https://10-10-185-199.p.thmlabs.com/ in your browser. What is the flag?**
```
THM{WEBSERVER_CONTAINER}
```

## 📚 Key Definitions

**Dockerfile:** Text file containing instructions for building Docker images.

**Docker Compose:** Tool for defining and running multi-container Docker applications.

**Docker Socket:** Communication channel between Docker client and server.

**Image:** Read-only template with instructions for creating containers.

**Container:** Runnable instance of a Docker image.

**Tag:** Label used to version Docker images.

**Orchestration:** Management of multiple containers working together.

## 📝 Self-Reflection

### My Docker Learning Journey

This room provided me with comprehensive, hands-on experience with Docker container technology. The practical approach from basic commands to advanced orchestration gave me confidence in deploying and managing containerized applications.

**Key Technical Skills Gained:**

**Command Proficiency:** I mastered essential Docker commands for image management, container execution, and system inspection. The hands-on exercises reinforced my understanding of Docker's command-line interface.

**Dockerfile Creation:** Learning to create optimized Dockerfiles was particularly valuable. Understanding layer reduction, minimal base images, and efficient command chaining will help me create production-ready containers.

**Multi-Container Management:** Docker Compose introduced me to container orchestration, showing how to manage complex applications with multiple interdependent services through declarative configuration.

**Architectural Understanding:** The Docker socket explanation gave me insight into Docker's client-server architecture and the importance of secure configuration in production environments.

**Practical Application:** The final practical exercise allowed me to apply all learned concepts in a realistic scenario, from container inspection to service deployment and verification.

This foundation in Docker technology is essential for modern application deployment and will be crucial as I progress to more advanced container security topics.

---

**Tags:** `#TryHackMe` `#Docker` `#Containerization` `#DevOps` `#DockerCompose` `#ContainerSecurity` `#HandsOnLearning`
