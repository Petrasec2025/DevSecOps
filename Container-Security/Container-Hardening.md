# TryHackMe - Container Hardening

![TryHackMe](https://img.shields.io/badge/TryHackMe-Container%20Hardening-red)
![Category](https://img.shields.io/badge/Category-Container%20Security-blue)
![Level](https://img.shields.io/badge/Level-Intermediate-green)
![Time](https://img.shields.io/badge/Time-40%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Premium](https://img.shields.io/badge/Premium-Room-gold)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Prerequisites](#prerequisites)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: Protecting the Docker Daemon](#task-2-protecting-the-docker-daemon)
- [Task 3: Implementing Control Groups](#task-3-implementing-control-groups)
- [Task 4: Preventing "Over-Privileged" Containers](#task-4-preventing-over-privileged-containers)
- [Task 5: Seccomp & AppArmor 101](#task-5-seccomp--apparmor-101)
- [Task 6: Reviewing Docker Images](#task-6-reviewing-docker-images)
- [Task 7: Compliance & Benchmarking](#task-7-compliance--benchmarking)
- [Task 8: Practical](#task-8-practical)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This premium room focuses on implementing security mechanisms to secure Docker containers, covering daemon protection, privilege management, security profiles, and compliance frameworks.

## 🎓 Learning Objectives

- Secure Docker daemon to prevent unauthorized interaction
- Correctly assign privileges and capabilities to containers
- Prevent resource exhaustion through control groups
- Utilize Seccomp and AppArmor security features
- Review Docker images for vulnerabilities
- Implement compliance frameworks and benchmarking tools

## 📚 Prerequisites

- Completion of Intro to Docker room
- Familiarity with Docker components
- Understanding of container fundamentals

## 📖 Task 1: Introduction

**Objective:** Introduction to container hardening concepts and security mechanisms.

**Key Concepts:**
- Docker daemon security
- Privilege and capability management
- Resource control implementation
- Security profile configuration
- Vulnerability assessment

**Answer:** 
```
No answer needed
```

## 🔒 Task 2: Protecting the Docker Daemon

### Docker Daemon Security Risks
- Remote management capabilities
- Common in CI/CD pipelines
- Unauthorized container and image access
- Critical vulnerability if exposed

### Secure Communication Methods

**SSH Authentication**
- Docker contexts for remote management
- Secure encrypted communication
- Requires SSH access and Docker permissions

**Context Creation:**
```bash
docker context create --docker host=ssh://user@remotehost --description="Environment" context-name
```

**Context Switching:**
```bash
docker context use context-name
```

**TLS Encryption**
- HTTP/S communication with Docker
- Certificate-based authentication
- Encrypted data transmission

**TLS Configuration:**
```bash
# Server side
dockerd --tlsverify --tlscacert=myca.pem --tlscert=myserver-cert.pem --tlskey=myserver-key.pem -H=0.0.0.0:2376

# Client side
docker --tlsverify --tlscacert=myca.pem --tlscert=client-cert.pem --tlskey=client-key.pem -H=SERVERIP:2376 info
```

### TLS Arguments
| Argument | Purpose |
|----------|---------|
| `--tlscacert` | Certificate authority certificate |
| `--tlscert` | Device identification certificate |
| `--tlskey` | Private key for decryption |

**Questions & Answers**

**What would the command be if we wanted to create a Docker profile?**
```
docker context create
```

**What would the command be if we wanted to switch to a Docker profile?**
```
docker context use
```

## ⚖️ Task 3: Implementing Control Groups

### Control Groups (cgroups) Purpose
- Linux kernel feature for resource restriction
- Process prioritization and limitation
- System stability improvement
- Resource usage tracking

### cgroups Implementation
- Not enabled by default in Docker
- Must be specified per container
- Prevents resource exhaustion attacks

### Resource Limitation Arguments
| Resource Type | Argument | Example |
|---------------|----------|---------|
| **CPU** | `--cpus` | `docker run -it --cpus="1" mycontainer` |
| **Memory** | `--memory` | `docker run -it --memory="20m" mycontainer` |

### Runtime Updates
```bash
docker update --memory="40m" mycontainer
```

### Container Inspection
```bash
docker inspect containername
```

### Namespace Security
- Process isolation mechanism
- Individual "rooms" for processes
- Security through isolation

**Questions & Answers**

**What argument would we provide when running a Docker container to enforce how many CPU cores the container can utilise?**
```
--cpus
```

**What would the command be if we wanted to inspect a docker container named "Apache"?**
```
docker inspect apache
```

## 🛡️ Task 4: Preventing "Over-Privileged" Containers

### Privileged Container Risks
- Unchecked host access
- Bypasses container isolation
- Full root privileges
- Container escape vulnerability

### Linux Capabilities
- Granular privilege assignment
- Fine-tuned process permissions
- Alternative to all-or-nothing root access

### Key Capabilities
| Capability | Purpose | Use Case |
|------------|---------|----------|
| **CAP_NET_BIND_SERVICE** | Bind to ports <1024 | Web server on port 80 |
| **CAP_SYS_ADMIN** | Administrative privileges | System automation tasks |
| **CAP_SYS_RESOURCE** | Modify resource limits | Resource control |

### Capability Management
**Assign Specific Capability:**
```bash
docker run -it --rm --cap-drop=ALL --cap-add=NET_BIND_SERVICE mywebserver
```

**Check Assigned Capabilities:**
```bash
capsh --print
```

### Security Best Practices
- Avoid `--privileged` flag
- Assign capabilities individually
- Regular capability reviews
- Principle of least privilege

**Questions & Answers**

**What is the name of the capability that allows services to bind to ports (specifically those under 1024)?**
```
CAP_NET_BIND_SERVICE
```

**What argument would we provide when starting a Docker container to add a capability?**
```
--cap-add
```

**Finally, what command (with argument) would we use to print the capabilities assigned to a process?**
```
capsh --print
```

## 🔐 Task 5: Seccomp & AppArmor 101

### Seccomp (Secure Computing Mode)
- Restricts program actions
- System call filtering
- Process-level security
- Default profile applied by Docker

### Seccomp Profile Example
```json
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "name": "socket",
      "action": "SCMP_ACT_ERRNO",
      "args": []
    },
    {
      "name": "read",
      "action": "SCMP_ACT_ALLOW",
      "args": []
    }
  ]
}
```

### Applying Seccomp Profile
```bash
docker run --rm -it --security-opt seccomp=/path/to/profile.json mycontainer
```

### AppArmor (Application Armor)
- Mandatory Access Control (MAC) system
- Operating system level enforcement
- Resource access control

### AppArmor Status Check
```bash
sudo aa-status
```

### AppArmor Profile Example
```apparmor
/usr/sbin/httpd {
  capability setgid,
  capability setuid,
  /var/www/** r,
  network tcp,
  deny /bin/**
}
```

### AppArmor Profile Management
**Import Profile:**
```bash
sudo apparmor_parser -r -W /path/to/profile.json
```

**Apply Profile:**
```bash
docker run --rm -it --security-opt apparmor=/path/to/profile.json mycontainer
```

### Security Feature Comparison
- **AppArmor:** Resource access control (files, network, etc.)
- **Seccomp:** System call restriction (CPU, OS functions)
- **Combination:** Layered security approach

**Questions & Answers**

**If we wanted to enforce the container to only be able to read files located in /home/tryhackme, what type of profile would we use? Seccomp or AppArmor?**
```
AppArmor
```

**If we wanted to disallow the container from a system call (such as clock_adjtime), what type of profile would we use? Seccomp or AppArmor?**
```
Seccomp
```

**Finally, what command would we use if we wanted to list the status of AppArmor?**
```
aa-status
```

## 🔍 Task 6: Reviewing Docker Images

### Image Security Importance
- Prevent malicious code execution
- Avoid cryptomining and backdoors
- Production environment safety

### Review Methods

**Docker Hub Analysis**
- Layer inspection during build
- Dockerfile code review
- Open-source repository access

**Reverse Engineering Tools**
- **Dive:** Layer-by-layer image analysis
- Code auditing capabilities
- Vulnerability identification

### Best Practices
- Review Dockerfiles before use
- Analyze image layers
- Use trusted sources
- Regular security scanning

**Answer:** 
```
No answer needed
```

## 📊 Task 7: Compliance & Benchmarking

### Compliance Frameworks
- Regulatory standards adherence
- Industry-specific requirements
- Security best practices

### Key Frameworks
| Framework | Purpose | URL |
|-----------|---------|-----|
| **NIST SP 800-190** | Container security guidance | [NIST](https://csrc.nist.gov/publications/detail/sp/800-190/final) |
| **ISO 27001** | Information security management | [ISO](https://www.iso.org/standard/27001) |

### Benchmarking Tools
| Tool | Purpose | URL |
|------|---------|-----|
| **CIS Docker Benchmark** | CIS framework compliance | [CIS](https://www.cisecurity.org/benchmark/docker) |
| **OpenSCAP** | Multiple framework assessment | [OpenSCAP](https://www.open-scap.org/) |
| **Docker Scout** | Docker image vulnerability scanning | [Docker Scout](https://docs.docker.com/scout/) |
| **Anchore** | Comprehensive compliance checking | [Anchore](https://github.com/anchore/anchore-engine) |
| **Grype** | Fast vulnerability scanning | [Grype](https://github.com/anchore/grype) |

### Docker Scout Example
```bash
docker scout cves local://nginx:latest
```

**Questions & Answers**

**What is the name of the framework published by the National Institute of Standards and Technology?**
```
NIST SP 800-190
```

**What is the name of the analysis tool provided by Docker?**
```
Docker Scout
```

## 🎮 Task 8: Practical

### Grype Vulnerability Scanner
- Docker image analysis
- Container filesystem scanning
- Fast vulnerability detection

### Grype Commands
| Purpose | Command |
|---------|---------|
| **Scan Docker image** | `grype imagename --scope all-layers` |
| **Scan exported filesystem** | `grype /path/to/image.tar` |

### Practical Exercises
- List running containers
- Analyze "struts2" image
- Scan exported container filesystem
- Identify vulnerability severities

**Questions & Answers**

**Use Docker to list the running containers on the system. What is the name of the container that is currently running?**
```
couchdb
```

**Use Grype to analyse the "struts2" image. What is the name of the library marked as "Critical"?**
```
struts2-core
```

**Use Grype to analyse the exported container filesystem located at /root/container.tar. What severity is the "CVE-2023-45853" rated as?**
```
High
```

## 📚 Key Definitions

**Control Groups (cgroups):** Linux kernel feature for restricting and prioritizing system resources.

**Linux Capabilities:** Granular privilege assignment system for processes.

**Seccomp:** Security feature that restricts system calls a program can make.

**AppArmor:** Mandatory Access Control system that enforces resource access rules.

**Docker Context:** Profile for managing remote Docker daemon connections.

**Compliance Framework:** Set of standards and regulations for security practices.

**Benchmarking:** Process of assessing adherence to security best practices.

## 📝 Self-Reflection

### My Container Hardening Learning Journey

This room provided me with comprehensive knowledge of Docker container security hardening techniques, transforming theoretical concepts into practical security implementations.

**Key Security Skills Developed:**

**Docker Daemon Protection:** I learned how to secure Docker daemon communication through SSH contexts and TLS encryption, understanding the critical importance of preventing unauthorized remote access.

**Resource Management:** Implementing control groups gave me practical experience in preventing resource exhaustion attacks and maintaining system stability through proper resource allocation.

**Privilege Management:** Understanding Linux capabilities and avoiding over-privileged containers taught me the principle of least privilege in container security, a fundamental concept for secure deployments.

**Security Profiles:** Working with Seccomp and AppArmor provided hands-on experience in creating layered security through system call restrictions and resource access controls.

**Compliance and Assessment:** Learning about compliance frameworks and benchmarking tools equipped me with the knowledge to assess container security against industry standards and identify vulnerabilities.

This knowledge significantly enhances my ability to design, implement, and maintain secure containerized environments in professional settings.

---

**Tags:** `#TryHackMe` `#ContainerHardening` `#DockerSecurity` `#Seccomp` `#AppArmor` `#Compliance` `#DevSecOps`
