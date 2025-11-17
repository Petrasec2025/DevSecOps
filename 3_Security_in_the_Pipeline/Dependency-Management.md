# TryHackMe - Dependency Management

![TryHackMe](https://img.shields.io/badge/TryHackMe-Dependency%20Management-red)
![Category](https://img.shields.io/badge/Category-Pipeline%20Security-blue)
![Level](https://img.shields.io/badge/Level-Intermediate-orange)
![Time](https://img.shields.io/badge/Time-120%20min-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Premium](https://img.shields.io/badge/Premium-Room-gold)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: What are dependencies?](#task-2-what-are-dependencies)
- [Task 3: Internal vs External](#task-3-internal-vs-external)
- [Task 4: Securing External Dependencies](#task-4-securing-external-dependencies)
- [Task 5: Securing Internal Dependencies](#task-5-securing-internal-dependencies)
- [Task 6: Theory of a Dependency Confusion](#task-6-theory-of-a-dependency-confusion)
- [Task 7: Practical Exploitation of Dependency Confusion](#task-7-practical-exploitation-of-dependency-confusion)
- [Task 8: Conclusion](#task-8-conclusion)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This premium room focuses on security concerns regarding dependency management in automated DevOps pipelines, covering both external and internal dependencies, supply chain attacks, and dependency confusion vulnerabilities.

## 🎓 Learning Objectives

- Security principles of dependency management
- Securing external and internal dependencies
- Dependency confusion attacks
- Practical exploitation techniques

## 📖 Task 1: Introduction

**Objective:** Introduction to dependency management security concepts.

**Key Concepts:**
- Modern applications extensively use libraries and SDKs
- Dependencies form part of the application's attack surface
- Secure dependency management is crucial for application security

**Answer:** 
```
No answer needed
```

## 📦 Task 2: What are dependencies?

### The Scale of Dependencies
- Dependencies can make up 99.99% of application code
- A simple `import numpy` can execute thousands of lines of dependency code
- Developers typically write only 0.01% of total application code

### Dependency Management Importance
- Better application support and maintenance
- Version locking for stability
- Golden images for faster deployments
- Developer onboarding efficiency
- Security monitoring and updates

### Common Tools
- **JFrog Artifactory:** Central dependency management
- Integration into DevOps pipelines
- Build server dependency resolution

**Questions & Answers**

**What do we call the libraries and SDKs that are imported into our application?**
```
Dependencies
```

## 🔄 Task 3: Internal vs External

### External Dependencies
- Developed and maintained outside the organization
- Examples: PyPi packages, NPM libraries, Google ReCaptcha SDK
- External parties responsible for maintenance

### Internal Dependencies
- Developed within the organization
- Examples: Authentication libraries, data-source connection libraries
- Organization responsible for maintenance and security

### Security Considerations
- Different attack surfaces for internal vs external dependencies
- Different security measures required for each type

**Questions & Answers**

**Would an authentication library that we created be considered an internal or external dependency?**
```
Internal
```

**Would JQuery be considered an internal or external dependency?**
```
External
```

## 🌐 Task 4: Securing External Dependencies

### Security Concerns

**Public CVEs**
- Vulnerabilities publicly disclosed in dependencies
- Patching challenges due to version locking
- Dependency chain vulnerabilities (like Log4j)

**Supply Chain Attacks**
- **MageCart APT Group:** Notorious for supply chain attacks
- British Airways: $230M fine from payment portal compromise
- Targeting dependencies rather than applications directly
- Compromising insecurely hosted dependencies

### Practical Exploitation: Supply Chain Attack

**Attack Steps:**
1. Identify dependency hosted on S3 bucket
2. Check for world-writeable permissions
3. Upload malicious JavaScript with credential skimmer
4. Intercept user credentials
5. Authenticate to recover flag

**Defense Strategies:**
- Regular dependency updates and patching
- Internal hosting of dependencies
- Sub-resource integrity for JS libraries
- Hash verification for loaded libraries

**Questions & Answers**

**Which Advance Persistent Threat group is notorious for their supply chain attacks?**
```
MageCart
```

**What is the password of the intercepted credentials?**
```
supersecretpassword12345@
```

**What is the value of the flag you receive on the website after authenticating with the intercepted credentials?**
```
THM{Supply.Chain.Attacks.Are.Super.Powerful}
```

## 🏢 Task 5: Securing Internal Dependencies

### Security Concerns
- Vulnerabilities affect multiple applications
- Legacy code and documentation issues
- Access control for dependency modification
- Dependency manager security

### Common Tools
- **Internal PyPi servers** for single-language environments
- **JFrog Artifactory** for multi-language dependency management
- Centralized dependency management

**Questions & Answers**

**What do we call a dependency that we have created ourselves and are responsible for maintaining?**
```
Internal dependency
```

## 🎯 Task 6: Theory of a Dependency Confusion

### Background
- **Discovered by:** Alex Birsan (2021)
- Race condition between internal and external dependencies
- Package managers choose highest version number

### Traditional Supply Chain Attacks
- **Typosquatting:** Malicious packages with similar names
- **Source Injection:** Vulnerabilities in legitimate packages
- **Domain Expiry:** Taking over expired maintainer domains

### Dependency Confusion Attack
1. Discover internal dependency names
2. Host malicious package with same name externally
3. Use higher version number (e.g., 9000.0.2)
4. Build systems install malicious package

### Attack Vectors
- Public forum posts revealing internal package names
- Package.json files in compiled applications
- Any disclosure of internal dependency names

**Questions & Answers**

**What is the name of a common supply chain attack that relies on users mistyping the names of packages that they wish to install?**
```
Typosquatting
```

**Dependency confusion relies on a race condition of what of a package?**
```
Version
```

## 💻 Task 7: Practical Exploitation of Dependency Confusion

### Attack Environment
- Internal Pypi server
- Build server with Docker-Compose
- API application rebuilding every 5 minutes

### Attack Steps

**1. Dependency Discovery**
- Developer forum post reveals `datadbconnect` package name

**2. Malicious Package Creation**
- Create package with same name but higher version
- Inject reverse shell in post-installation hook
- Build package using `python3 setup.py sdist`

**3. Package Upload**
- Upload to external PyPI repository
- Use `twine upload` with higher version number

**4. Exploitation**
- Start listener for reverse shell
- Wait for build process to trigger installation
- Gain remote code execution on build server

### Technical Details
- Uses `--extra-index-url` instead of `--index-url`
- Package managers compare versions from all sources
- Highest version wins the installation race

### Defense Strategies
- Use `--index-url` instead of `--extra-index-url`
- Protect packages with controlled scopes
- Client-side verification features
- Register internal package names externally
- Reference only private feeds

**Questions & Answers**

**What is the name of the attack that can be launched to create a race condition between internal and external dependencies with the same name?**
```
Dependency Confusion
```

**What is the flag's value that is stored in the /root/ directory on the docker container where you get remote code execution?**
```
THM{RCE.Through.Dependency.Confusion}
```

## ✅ Task 8: Conclusion

### Key Security Principles
- **Awareness:** Know all dependencies and their dependencies
- **Updates:** Always use latest dependency versions
- **Configuration:** Secure dependency manager configurations
- **Attack Surface:** Include dependencies in security assessments

**Answer:** 
```
No answer needed
```

## 📚 Key Definitions

**Dependencies:** Libraries and SDKs imported into applications that provide functionality.

**External Dependencies:** Dependencies developed and maintained outside the organization.

**Internal Dependencies:** Dependencies created and maintained within the organization.

**Supply Chain Attacks:** Attacks targeting dependencies rather than applications directly.

**Dependency Confusion:** Attack exploiting version race conditions between internal and external dependencies.

**Typosquatting:** Attack using malicious packages with names similar to legitimate ones.

**MageCart:** APT group notorious for supply chain attacks against payment systems.

## 📝 Self-Reflection

### My Dependency Management Security Journey

This room provided me with comprehensive insights into the critical security aspects of dependency management in modern software development. The hands-on exercises demonstrated real-world attack vectors that I can now both exploit and defend against.

**Key Technical Learnings:**

**Supply Chain Attack Understanding:** I gained practical experience in identifying and exploiting insecurely hosted dependencies, understanding how attackers can compromise entire applications by targeting their dependencies rather than the applications themselves.

**Dependency Confusion Mastery:** The dependency confusion attack was particularly enlightening. I learned how to create malicious packages, exploit version comparison mechanisms, and gain remote code execution through build process vulnerabilities.

**Defense Strategy Development:** Beyond exploitation, I learned critical defense strategies including proper dependency manager configuration, package scoping, version locking, and the importance of using `--index-url` over `--extra-index-url`.

**Real-World Impact Awareness:** The MageCart case studies and Log4j vulnerability examples showed me the massive real-world impact of dependency security failures, reinforcing the importance of robust dependency management practices.

**Professional Skill Enhancement:** This room has significantly enhanced my ability to assess dependency security, implement secure dependency management practices, and contribute to software supply chain security in professional environments.

The practical nature of this room, combined with comprehensive theoretical coverage, has equipped me with both offensive and defensive perspectives on dependency security that I can immediately apply in real-world scenarios.

---

**Tags:** `#TryHackMe` `#DependencyManagement` `#SupplyChainSecurity` `#DependencyConfusion` `#DevSecOps` `#PipelineSecurity` `#HandsOnLearning`
