# TryHackMe - Intro to Containerisation

![TryHackMe](https://img.shields.io/badge/TryHackMe-Intro%20to%20Containerisation-red)
![Category](https://img.shields.io/badge/Category-Container%20Security-blue)
![Level](https://img.shields.io/badge/Level-Fundamental-green)
![Time](https://img.shields.io/badge/Time-30%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: What is Containerisation](#task-2-what-is-containerisation)
- [Task 3: Introducing Docker](#task-3-introducing-docker)
- [Task 4: The History of Docker](#task-4-the-history-of-docker)
- [Task 5: The Benefits & Features of Docker](#task-5-the-benefits--features-of-docker)
- [Task 6: How does Containerisation Work?](#task-6-how-does-containerisation-work)
- [Task 7: Practical](#task-7-practical)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This room serves as the foundational introduction to containerisation technologies, focusing on Docker and the core concepts that make containerisation a powerful tool in modern software development and deployment.

## 🎓 Learning Objectives

- Understand what containerisation is and what containers are
- Learn where and why containerisation is used
- Gain fundamental understanding of Docker technology
- Understand what makes Docker popular
- Learn how containerisation works technically

## 📖 Task 1: Introduction

**Objective:** Introduction to containerisation concepts and the learning path ahead.

**Key Concepts:**
- Containerisation as a packaging technology
- Docker as a popular containerisation platform
- Practical benefits of containerisation

**Answer:** 
```
No answer needed
```

## 📦 Task 2: What is Containerisation

### Containerisation Definition
- Process of packaging applications with necessary resources into containers
- Makes applications portable and hassle-free to run
- Solves dependency conflicts and environment inconsistencies
<img width="713" height="492" alt="Screenshot 2025-11-18 at 11 54 46 AM" src="https://github.com/user-attachments/assets/791431e7-03d4-47de-b835-9b559329ed6e" />

### Problems Solved by Containerisation
- Difficult dependency installation across different environments
- Difficulty diagnosing environment-specific faults
- Dependency version conflicts (e.g., multiple Python versions)
- Application environment isolation

### Technical Foundation
- **Namespaces:** Kernel feature that allows processes to access OS resources without interacting with other processes
- Process isolation between containers
- Security benefits through namespace separation

### Comparison with Virtual Machines
- Containers share host OS kernel
- VMs require full OS installation per instance
- Containers are more resource-efficient

**Questions & Answers**

**What is the name of the kernel feature that allows for processes to use resources of the Operating System without being able to interact with other processes?**
```
namespace
```

**In a normal configuration, can other containers interact with each other? (yay/nay)**
```
nay
```

## 🐳 Task 3: Introducing Docker

### Docker Ecosystem
- Hassle-free, extensive open-source containerisation platform
- Works on Linux, Windows, and macOS
- Applications published as "images"
- Easy sharing and deployment through image pulling

### Docker Engine
- API that runs on host operating system
- Communicates between OS and containers
- Manages hardware resource access (CPU, RAM, networking, disk)

### Key Features
- **Container Connectivity:** Connect multiple containers (web app + database)
- **Image Management:** Export, import, and share applications
- **File Transfer:** Move files between OS and containers
- **Orchestration:** Manage multiple containers as groups

### Configuration
- Uses **YAML** syntax for container instructions
- Ensures portability and consistent behavior across systems
- Easy debugging through shared configuration

**Questions & Answers**

**What does an application become when it is published using Docker? Format: An xxxxx (fill in the x's)**
```
An Image
```

**What is the abbreviation of the programming syntax language that Docker uses?**
```
YAML
```

## 📜 Task 4: The History of Docker

### Docker Origins
- **Created by:** Solomon Hykes
- **Year:** 2013
- **Origin:** Internal project for dotCloud (PaaS provider)
- **First Showcase:** PyCon 2013
- **Status:** Open-source since initial release

### Containerisation History
- Original concepts started in 1979 with **Unix V7**
- Docker popularized and modernized containerisation
- Made benefits accessible to broader developer community

### Docker Popularity (As of April 2022)
- 13 million developers using Docker
- 7 million applications available as Docker images
- 13 billion applications downloaded monthly from official repository

**Questions & Answers**

**In what year was Docker originally created?**
```
2013
```

**Where was Docker first showcased?**
```
PyCon
```

**What version of Unix had the first concepts of containerisation?**
```
V7
```

## ⚡ Task 5: The Benefits & Features of Docker

### Key Advantages

**Cost & Accessibility**
- Free and open-source
- Business plans available but core functionality free

**Compatibility**
- Cross-platform support (Linux, macOS, Windows)
- Universal container execution on Docker Engine-supported devices

**Efficiency**
- Minimal resource usage compared to virtual machines
- Shared base images reduce storage requirements
- Example: Ubuntu minimal image ~77.8MB vs VM ~1GB

**Ease of Use**
- Comprehensive documentation and community support
- Easy-to-learn syntax
- Quick startup time for first container

**Portability & Sharing**
- Easy image sharing through public/private repositories
- Consistent behavior across different environments
- Support for DockerHub and GitHub integration

**Security & Minimalism**
- Reduced attack surface through minimal packages
- Explicit control over container contents
- Better vulnerability management

**Cost Effectiveness**
- Lower cloud computing costs
- Efficient resource utilization
- No virtualization hardware requirements

**Answer:** 
```
No answer needed
```

## 🔧 Task 6: How does Containerisation Work?

### Technical Foundation: Namespaces
- Segregate system resources (processes, files, memory)
- Each process assigned namespace and PID
- Processes can only "see" other processes in same namespace

### Process Isolation
- Each Docker container runs in new namespace
- Multiple applications in container share namespace
- Prevents process conflicts between containers

### Process Hierarchy
- **Process #0:** Started at system boot
- **Process #1:** System init (e.g., systemd in Ubuntu)
- All other processes controlled by process #1

### Security Implications
- Namespace isolation provides security boundary
- Container escape possible if sharing host namespace
- Privilege escalation opportunities through namespace manipulation

**Questions & Answers**

**What command can we use to view a list of running processes?**
```
ps aux
```

## 🎮 Task 7: Practical

### Hands-on Exercise
- Deploy static site attached to task
- Containerise applications to reveal flag
- Apply containerisation concepts learned throughout room

### Learning Application
- Practical implementation of containerisation principles
- Understanding container deployment process
- Experience with container configuration

**Questions & Answers**

**Containerise the applications in the static site. What is the flag?**
<img width="1405" height="723" alt="Screenshot 2025-11-18 at 12 17 31 PM" src="https://github.com/user-attachments/assets/33d98c79-f1b4-44f2-8bdc-48c0e6f7cf52" />

```
THM{APPLICATION_SHIPPED}
```

## 📚 Key Definitions

**Containerisation:** Process of packaging applications with dependencies into isolated, portable containers.

**Docker:** Open-source containerisation platform that simplifies container creation and management.

**Namespace:** Linux kernel feature that isolates processes and resources between containers.

**Docker Image:** Packaged application with instructions for container creation and execution.

**YAML:** Human-readable data serialization language used for Docker configuration files.

**Docker Engine:** Core component that manages container execution and resource allocation.

**Orchestration:** Management of multiple containers working together as a coordinated system.

## 📝 Self-Reflection

### My Containerisation Learning Journey

This room provided me with a solid foundation in containerisation concepts and Docker technology. The structured approach from theoretical concepts to practical application helped me understand both the "why" and "how" of containerisation.

**Key Insights Gained:**

**Fundamental Understanding:** I now clearly understand how containerisation solves real-world problems in software deployment, particularly around dependency management and environment consistency.

**Technical Mechanics:** Learning about Linux namespaces gave me insight into the underlying technology that makes container isolation possible, which is crucial for understanding both the benefits and potential security implications.

**Docker Ecosystem:** Understanding Docker's architecture, from images to the Docker Engine, has prepared me for more advanced container topics and practical Docker usage.

**Practical Application:** The hands-on exercise reinforced the theoretical concepts and demonstrated the practical value of containerisation in real deployment scenarios.

**Historical Context:** Knowing the evolution from Unix V7 concepts to modern Docker implementation helped me appreciate the technological progression and why Docker became so popular.

This foundational knowledge will be essential as I progress through more advanced container security topics in the subsequent rooms of this learning path.

---

**Tags:** `#TryHackMe` `#Containerisation` `#Docker` `#ContainerSecurity` `#DevOps` `#CloudComputing` `#Fundamentals`
