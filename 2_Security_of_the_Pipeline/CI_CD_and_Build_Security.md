# TryHackMe - CI/CD and Build Security

![TryHackMe](https://img.shields.io/badge/TryHackMe-CI_CD%20and%20Build%20Security-red)
![Category](https://img.shields.io/badge/Category-Pipeline%20Security-blue)
![Level](https://img.shields.io/badge/Level-Advanced-orange)
![Time](https://img.shields.io/badge/Time-120%20min-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Hands-On](https://img.shields.io/badge/Hands--On-Practical%20Labs-yellow)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Prerequisites](#prerequisites)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: Setting up](#task-2-setting-up)
- [Task 3: What is CI/CD and Build Security?](#task-3-what-is-cicd-and-build-security)
- [Task 4: Creating your own Pipeline](#task-4-creating-your-own-pipeline)
- [Task 5: Securing the Build Source](#task-5-securing-the-build-source)
- [Task 6: Securing the Build Process](#task-6-securing-the-build-process)
- [Task 7: Securing the Build Server](#task-7-securing-the-build-server)
- [Task 8: Securing the Build Pipeline](#task-8-securing-the-build-pipeline)
- [Task 9: Securing the Build Environment](#task-9-securing-the-build-environment)
- [Task 10: Securing the Build Secrets](#task-10-securing-the-build-secrets)
- [Task 11: Conclusion](#task-11-conclusion)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This advanced room provides comprehensive coverage of CI/CD pipeline security, focusing on practical attacks against misconfigured build processes and implementing robust security measures throughout the software development lifecycle.

## 🎓 Prerequisites

- SDLC
- SSDLC  
- Intro to Pipeline Automation
- Dependency Management

## 🎯 Learning Objectives

- Understand CI/CD and build system security significance
- Explore risks of insecure CI/CD pipelines
- Learn to integrate security measures into build processes
- Practice attacks against misconfigured CI/CD pipelines

## 📖 Task 1: Introduction

**Objective:** Introduction to CI/CD security concepts and risks.

**Key Concepts:**
- Automation introduces new security risks
- Attackers can target pipelines directly
- Understanding build process vulnerabilities is essential

**Answer:** 
```
No answer needed
```

## 🔧 Task 2: Setting up

**Objective:** Configure the lab environment for hands-on exercises.

**Setup Steps:**
- Connect to the network via AttackBox or VPN
- Configure DNS entries for GitLab and Jenkins
- Register with MU-TH-UR 6000 (Mother) system
- Verify access to all required systems

**Questions & Answers**

**I am ready to start my CI/CD and Build security learning journey.**
```
No answer needed
```

**I am connected to the network using my preferred method of choice and can confirm that I can access the Gitlab server.**
```
No answer needed
```

**I have registered an account with Mother.**
```
No answer needed
```

## 🔄 Task 3: What is CI/CD and Build Security?

### CI/CD Fundamentals
- Single source repository
- Frequent check-ins to main branch
- Automated builds
- Self-testing builds
- Frequent iterations
- Stable testing environments
- Maximum visibility
- Predictable deployments

### Pipeline Components
- **Developer workstations** - Code development
- **Source code storage** - GitLab server
- **Build orchestrator** - Coordinates automation
- **Build agents** - Execute builds and tests
- **Environments** - DEV, TEST, PROD stages

### SolarWinds Case Study
- Supply chain attack through compromised updates
- Importance of isolation and segmentation
- Access control and permission management
- Network security implementation

**Questions & Answers**

**What element of a CI/CD pipeline coordinates and manages the automation of build and deployment environments?**
```
build orchestrator
```

**What element of a CI/CD pipeline builds, tests, and packages code?**
```
build agents
```

**What fundamental of CI/CD promotes developers in having access to the latest builds and code in order to understand and see the changes that have been made?**
```
maximum visibility
```

## 🏗️ Task 4: Creating your own Pipeline

### GitLab Setup
- Register account on GitLab instance
- Fork BasicBuild project
- Configure CI/CD pipeline using .gitlab-ci.yml

### Pipeline Components
- **Stages:** build, test, deploy
- **Jobs:** Individual tasks within stages
- **Scripts:** Commands executed during jobs
- **Runners:** Agents that execute pipeline jobs

### GitLab Runner Registration
- Install GitLab Runner application
- Register runner with project token
- Configure to run untagged jobs
- Execute builds on code commits

**Questions & Answers**

**What is the name of the build agent that can be used with Gitlab?**
```
Gitlab Runner
```

**What is the value of the flag you receive once authenticated to Timekeep?**
```
THM{Welcome.to.CICD.Pipelines}
```

## 🔐 Task 5: Securing the Build Source

### Source Code Security Risks
- **Unauthorized Tampering:** Unauthorized code changes
- **Unauthorized Disclosure:** Sensitive information exposure

### Common Misconfigurations
- Open registration on internal GitLab instances
- Publicly shared repositories containing sensitive data
- Lack of granular access controls

### Exploitation Techniques
- GitLab API enumeration
- Automated repository downloading
- Sensitive information extraction

### Protection Measures
- **.gitignore** for excluding sensitive files
- **Branch protection** to prevent direct pushes
- **Group-based access control**
- **Environment variables** for secrets
- **Regular access reviews**

**Questions & Answers**

**Which file specifies which directories and files should be excluded for version control?**
```
.gitignore
```

**What can you protect to ensure direct pushes and vulnerable code changes are avoided?**
```
branches
```

**What issue does lack of access control and unauthorised code changes lead to?**
```
unauthorised tampering
```

**What is the API key stored within the Mobile application that can be accessed by any Gitlab user?**
```
THM{You.Found.The.API.Key}
```

## ⚙️ Task 6: Securing the Build Process

### Build Process Security
- **Dependency Management:** Supply chain attacks and dependency confusion
- **Build Triggers:** What actions start builds
- **Access Control:** Who can initiate builds
- **Execution Environment:** Where builds occur

### Toxic Combinations
- **On-Merge Builds:** Builds triggered by merge requests
- Lack of environment segregation
- Shared build agents across sensitivity levels

### Exploitation: Merge Build Attack
- Fork target repository
- Modify Jenkinsfile with malicious code
- Create merge request to trigger build
- Gain code execution on build agent

### Protection Strategies
- **Isolation and Containerization**
- **Least Privilege** for CI/CD tools
- **Secret Management** for sensitive data
- **Immutable Artifacts** in secure registry
- **Dependency Scanning** integration
- **Pipeline as Code** version control

**Questions & Answers**

**Where should you store artefacts to prevent tampering?**
```
secure registry
```

**What mechanism should you always use to store and inject sensitive data?**
```
secret management
```

**What attack can malicious actors perform to inject malicious code in the build process?**
```
dependency confusion
```

**Authenticate to Mother and follow the process to claim Flag 1. What is Flag 1?**
```
THM{7753f7e9-6543-4914-90ad-7153609831c3}
```

## 🖥️ Task 7: Securing the Build Server

### Build Server Security Basics
- Default credential attacks (jenkins:jenkins)
- Access restriction implementation
- Multi-factor authentication
- Granular access controls

### Jenkins Exploitation
- Metasploit modules for Jenkins
- Script console code execution
- Manual Groovy script attacks

### Protection Measures
- **Build Agent Configuration:** Restricted communication
- **Private Network** placement
- **Firewall** implementation
- **VPN** for secure access
- **Token-Based Authentication**
- **SSH Keys** for build agents
- **Continuous Monitoring**
- **Regular Updates**
- **Security Audits**

**Questions & Answers**

**What can be used to ensure that remote access to the build server can be performed securely?**
```
VPN
```

**What can be used to add an additional layer of authentication security for build agents?**
```
Token-based authentication
```

**Authenticate to Mother and follow the process to claim Flag 2. What is Flag 2?**
```
THM{1769f776-e03c-40b6-b2eb-b298297c15cc}
```

## 🚪 Task 8: Securing the Build Pipeline

### Access Gates Implementation
- **Manual Approvals:** Human review requirements
- **Automated Tests:** Quality and security checks
- **Security Scans:** Vulnerability detection
- **Environment Validation:** Readiness verification

### Two-Person Concept
- No single user can pass all access gates
- Multiple approvals required
- Technical enforcement of separation

### Misconfigured Access Gates
- Protected branches without approval requirements
- Self-approval of merge requests
- Lack of technical enforcement for policies

### Protection Strategies
- **Limit Branch Access** to trusted developers
- **Review Merge Requests** with multiple approvers
- **CI/CD Variables** for secret management
- **Limit Runner Access** with tags
- **Access Control** configuration
- **Regular Audits** of permissions
- **Monitoring and Alerting** implementation

**Questions & Answers**

**What can we add so that merges are raised for review instead of pushing the changes to code directly?**
```
merge requests
```

**What should we do so that only trusted runners execute CI/CD jobs?**
```
limit runner access
```

**Authenticate to Mother and follow the process to claim Flag 3. What is Flag 3?**
```
THM{2411b26f-b213-462e-b94c-39d974e503e6}
```

## 🌐 Task 9: Securing the Build Environment

### Environment Segregation
- Different access levels per environment
- Pipeline side channel exploitation
- Shared build agent risks

### Shared Runner Exploitation
- Compromise DEV environment runner
- Intercept PROD builds on same runner
- Access sensitive production data

### Protection Measures
- **Isolate Environments** with separate runners
- **Restricted Access** for CI/CD jobs
- **Protected CI/CD Environments** feature
- **Monitoring and Alerting** for suspicious activity
- **Permission Reviews** and audits

**Questions & Answers**

**What should you do so that a compromised environment doesn't affect other environments?**
```
isolate environments
```

**Authenticate to Mother and follow the process to claim Flag 4 from the DEV environment. What is Flag 4?**
```
THM{28f36e4a-7c35-4e4d-bede-be698ddf0883}
```

**Authenticate to Mother and follow the process to claim Flag 5 from the PROD environment. What is Flag 5?**
```
THM{e9f99dbe-6bae-4849-adf7-18a449c93fe6}
```

## 🔒 Task 10: Securing the Build Secrets

### Secret Management Risks
- Inadequate variable scoping
- PROD secrets accessible in DEV builds
- Unmasked variable exposure in logs

### Secret Exploitation
- Access PROD variables from DEV environment
- Echo secret values in build logs
- Leverage broad variable permissions

### Protection Strategies
- **Masking Variables** using CI_JOB_TOKEN
- **Secure Variables** with masked option
- **Access Control** for variable visibility
- **Script Review** to prevent accidental exposure

**Questions & Answers**

**Is using environment variables enough to protect the build secrets? (yay or nay)**
```
nay
```

**What is the value of the PROD API_KEY?**
```
THM{Secrets.are.meant.to.be.kept.Secret}
```

## ✅ Task 11: Conclusion

### Key Security Principles
- **Pipeline Security Priority:** Essential for code and data integrity
- **Access Control Foundation:** First line of defense
- **Runner Security Essential:** Prevent system breaches
- **Secrets Management Vital:** Beyond environment variables
- **Environment Isolation:** Minimize cross-environment risks
- **Continuous Vigilance:** Ongoing security maintenance
- **Education Importance:** Team security awareness

**Questions & Answers**

**I understand CI/CD and Build Security!**
```
No answer needed
```

## 📚 Key Definitions

**CI/CD (Continuous Integration/Continuous Delivery):** Automated processes for building, testing, and deploying code changes.

**Build Orchestrator:** System that coordinates and manages build automation across multiple agents.

**Build Agent:** Machine that executes build, test, and packaging tasks.

**Access Gates:** Checkpoints in pipelines that enforce quality and security criteria before progression.

**Two-Person Rule:** Security principle requiring multiple approvals for sensitive actions.

**Masked Variables:** GitLab feature that hides sensitive values in job logs.

**Environment Segregation:** Separation of development, testing, and production environments.

## 📝 Self-Reflection

### My CI/CD Security Learning Journey

This room provided me with an incredibly comprehensive and practical understanding of CI/CD pipeline security. Through hands-on exploitation of real-world misconfigurations, I gained valuable insights into how attackers target automated build processes and what defensive measures are most effective.

**Key Technical Learnings:**

**Pipeline Component Security:** I learned to identify and secure each component of a CI/CD pipeline, from source code repositories to build servers and deployment environments. Understanding the attack surface of each component helped me appreciate the need for defense in depth.

**Practical Exploitation Techniques:** The hands-on exercises taught me how attackers leverage misconfigured merge builds, shared runners, and inadequate access controls to compromise pipelines. This practical experience will be invaluable for both offensive security testing and defensive implementations.

**Security Control Implementation:** I gained experience implementing critical security controls like access gates, environment segregation, secret management, and the two-person rule. Understanding how to technically enforce these controls rather than relying on policies was particularly valuable.

**Real-World Application:** The SolarWinds case study contextualized the importance of pipeline security, showing how supply chain attacks can have devastating consequences. This reinforced the critical nature of the security principles covered in the room.

**Professional Growth:** This room has significantly enhanced my ability to design, implement, and assess secure CI/CD pipelines. The skills gained are directly applicable to DevSecOps roles and will help me contribute meaningfully to securing modern software development practices.

The comprehensive coverage of both attack and defense perspectives has given me a well-rounded understanding of CI/CD security that I can apply immediately in professional environments.

---

**Tags:** `#TryHackMe` `#CI_CD` `#BuildSecurity` `#PipelineSecurity` `#DevSecOps` `#Jenkins` `#GitLab` `#HandsOnLearning`
