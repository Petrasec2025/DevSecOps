# TryHackMe - Intro to Pipeline Automation

![TryHackMe](https://img.shields.io/badge/TryHackMe-Intro%20to%20Pipeline%20Automation-red)
![Category](https://img.shields.io/badge/Category-Pipeline%20Security-blue)
![Level](https://img.shields.io/badge/Level-Fundamental-green)
![Time](https://img.shields.io/badge/Time-60%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: DevOps Pipelines Explained](#task-2-devops-pipelines-explained)
- [Task 3: Source Code and Version Control](#task-3-source-code-and-version-control)
- [Task 4: Dependency Management](#task-4-dependency-management)
- [Task 5: Automated Testing](#task-5-automated-testing)
- [Task 6: Continuous Integration and Delivery](#task-6-continuous-integration-and-delivery)
- [Task 7: Environments](#task-7-environments)
- [Task 8: Challenge](#task-8-challenge)
- [Task 9: Conclusion](#task-9-conclusion)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This room provides an introduction to DevOps pipeline automation and the potential security concerns associated with automated development processes.

## 🎓 Learning Objectives

- Introduction to the DevOps pipeline
- Introduction to DevOps tools and automation
- Introduction to security principles for the DevOps pipeline

## 📖 Task 1: Introduction

**Objective:** Understand the benefits and risks of pipeline automation in software development.

**Key Concepts:**
- Automation increases development efficiency but introduces new security risks
- Attackers can target the pipeline itself rather than individual developers
- Security must be integrated into automated processes

**Answer:** 
```
No answer needed
```

## 🔄 Task 2: DevOps Pipelines Explained
<img width="1202" height="682" alt="Screenshot 2025-11-17 at 12 34 44 PM" src="https://github.com/user-attachments/assets/e3266eec-0c10-413d-af25-6c82a2ed68b3" />


**Objective:** Understand the components of a typical DevOps pipeline.

**Pipeline Components:**
- Source Code and Version Control
- Dependency Management
- Automated Testing
- CI/CD (Continuous Integration/Continuous Delivery)
- Environments

**Questions & Answers**

**Where in the pipeline is our end product deployed?**
```
Production Environment
```

## 💾 Task 3: Source Code and Version Control

### Source Code Storage Considerations
- Access control implementation
- Change tracking capabilities
- Development tool integration
- Multiple version support
- Hosting location (internal vs external)

### Version Control Importance
- Managing continuous updates in Agile development
- Coordinating multiple developers' work
- Maintaining different application versions

### Common Tools
- **Git:** Distributed source control (GitHub, GitLab)
- **SVN:** Centralized source control (TortoiseSVN, Apache SVN)

### Security Considerations
- Authentication and access control for source code
- Avoiding secret storage in version control
- Understanding that "Git never forgets" - historical commits remain accessible

### Questions & Answers

**Who is the largest online provider of Git?**
```
Github
```

**What popular Git product is used to host your own Git server?**
```
Gitlab
```

**What tool can be used to scan the commits of a repo for sensitive information?**
```
GittyLeaks
```

## 📦 Task 4: Dependency Management

### Types of Dependencies

**Internal Dependencies:**
- Organization-developed libraries
- Legacy software risks
- Internal package manager security responsibility

**External Dependencies:**
- Publicly available libraries (PyPi, NuGet, Gems)
- Supply chain attack risks
- 0day vulnerability impacts

### Common Tools
- **External:** PyPi (Python), NuGet (.NET), Gems (Ruby)
- **Internal:** JFrog Artifactory, Azure Artifacts

### Security Considerations
- Dependencies represent code outside organizational control
- Vulnerability in dependencies = vulnerability in application
- Difficulty tracking numerous dependencies

### Questions & Answers

**What do we call the type of dependency that was created by our organisation?**
```
Internal
```

**What type of dependency is JQuery?**
```
External
```

**What is the name of Python's public dependency repo?**
```
PyPi
```

**What dependency 0day vulnerability set the world ablaze in 2021?**
```
Log4j
```

## 🧪 Task 5: Automated Testing

### Testing Types

**Unit Testing:**
- Tests small application components
- Quality gates in CI/CD
- Focuses on functionality, not security

**Integration Testing:**
- Tests component interactions
- Includes regression testing
- Not typically security-focused

**Security Testing:**
- **SAST (Static Application Security Testing):** Source code analysis
- **DAST (Dynamic Application Security Testing):** Runtime code execution testing
- **Penetration Testing:** Manual testing for contextual vulnerabilities

### Security Testing Limitations
- SAST/DAST cannot fully replace manual penetration testing
- Contextual vulnerabilities hard to detect automatically
- Business logic and access control flaws better found manually

### Common Tools
- GitHub/GitLab built-in SAST
- Snyk, Sonarqube for SAST/DAST

### Implementation Considerations
- Performance impact assessment
- Integration point evaluation
- Result calibration
- Quality/security gate implementation

### Questions & Answers

**What type of tool scans code to look for potential vulnerabilities?**
```
SAST
```

**What type of tool runs code and injects test cases to look for potential vulnerabilities?**
```
DAST
```

**Can SAST and DAST be used as a replacement for penetration tests?**
```
nay
```

## 🚀 Task 6: Continuous Integration and Delivery

### CI/CD Pipeline Elements
- **Starting Trigger:** Action initiating pipeline (e.g., branch push)
- **Building Actions:** Project and feature compilation
- **Testing Actions:** Feature integration validation
- **Deployment Actions:** Environment deployment
- **Delivery Actions:** Post-deployment monitoring

### Infrastructure Components
- **Build Orchestrator:** Controls build processes
- **Build Agents:** Execute build actions

### Security Concerns
- Large attack surface in CI/CD pipelines
- Misconfiguration risks
- Shared DEV/PROD build agent dangers

### Common Tools
- GitHub/GitLab CI/CD capabilities
- Jenkins for complex builds

### Questions & Answers

**What does CI in CI/CD stand for?**
```
Continuous Integration
```

**What does CD in CI/CD stand for?**
```
Continuous Delivery
```

**What do we call the build infrastructure element that controls all builds?**
```
Build Orchestrator
```

**What do we call the build infrastructure element that performs the build?**
```
Build Agent
```

## 🏗️ Task 7: Environments

### Environment Types

| Environment | Stability | Security Posture | Customer Data |
|-------------|-----------|------------------|---------------|
| **DEV** | Unstable | Weakest | No |
| **UAT** | Semi-Stable | Second Weakest | No |
| **PreProd** | Stable | Second Strongest | No |
| **PROD** | Stable | Strongest | Yes |
| **DR/HA** | Stable | Strongest | Yes |

### Specialized Environments
- **Green/Blue:** Deployment strategy with two PROD instances
- **Canary:** Gradual user migration to new versions

### Security Considerations
- Security hardening increases toward PROD
- Infrastructure vulnerabilities affect application security
- Developer bypasses must not reach PROD

### Questions & Answers

**Which environment usually has the weakest security configuration?**
```
DEV
```

**Which environment is used to test the application?**
```
UAT
```

**Which environment is similar to PROD but is used to verify that everything is working before it is pushed to PROD?**
```
PrePROD
```

**What is a common class of vulnerabilities that is discovered in PROD due to insecure code creeping in from DEV?**
```
Developer Bypasses
```

## 🎮 Task 8: Challenge

**Objective:** Apply learned concepts to build a secure pipeline in an interactive challenge.

**Challenge Focus:**
- Identifying valid security concerns at each pipeline stage
- Implementing appropriate security controls
- Understanding risk assessment in automated pipelines

### Questions & Answers

**What is the flag received after successfully building your pipeline?**
<img width="1440" height="785" alt="Screenshot 2025-11-17 at 1 09 47 PM" src="https://github.com/user-attachments/assets/ad823ee9-518d-4463-8627-de8bedf2892b" />
<img width="1440" height="780" alt="Screenshot 2025-11-17 at 1 12 32 PM" src="https://github.com/user-attachments/assets/57a2f766-42e3-470c-9029-b70a32410f04" />
<img width="1430" height="755" alt="Screenshot 2025-11-17 at 1 13 08 PM" src="https://github.com/user-attachments/assets/51e36fc5-f559-49ff-8cfa-fbd579970e09" />

```
THM{Pipeline.Automation.Is.Fun}
```

## ✅ Task 9: Conclusion

**Key Takeaways:**
- Pipeline automation enables rapid development but increases attack surface
- Security must be integrated throughout the pipeline
- Each pipeline component has unique security considerations

**Answer:** 
```
No answer needed
```

## 📚 Key Definitions

**CI/CD (Continuous Integration/Continuous Delivery):** Automated processes for integrating, testing, and deploying code changes.

**SAST (Static Application Security Testing):** Security testing methodology that analyzes source code for vulnerabilities without executing the program.

**DAST (Dynamic Application Security Testing):** Security testing methodology that analyzes running applications for vulnerabilities by injecting test cases.

**Build Orchestrator:** Infrastructure component that controls and coordinates build processes across multiple agents.

**Build Agent:** Infrastructure component that executes specific build tasks as directed by the orchestrator.

**Developer Bypasses:** Code features that allow developers to skip security controls during testing, which can accidentally reach production.

## 📝 Self-Reflection

### My Learning Journey

This room provided me with a comprehensive foundation in pipeline automation security concepts. I gained valuable insights into how automated development processes introduce both efficiency benefits and security risks.

**Key Realizations:**

**Pipeline Complexity:** I learned that modern DevOps pipelines involve multiple interconnected components, each with unique security considerations. Understanding the entire pipeline ecosystem is crucial for effective security implementation.

**Dependency Risks:** The Log4j case study particularly highlighted how vulnerable dependencies can create widespread security impacts across countless applications, emphasizing the importance of dependency management.

**Testing Limitations:** I now understand that while automated security testing (SAST/DAST) is valuable, it cannot replace manual penetration testing for contextual and business logic vulnerabilities.

**Environment Security:** The different security postures required for various environments (DEV, UAT, PrePROD, PROD) showed me the importance of security progression through the development lifecycle.

**Practical Application:** The interactive challenge helped me apply theoretical knowledge to practical pipeline building, reinforcing how security concerns vary at different pipeline stages.

This knowledge forms a solid foundation for my continued learning in pipeline security and will be essential as I progress through more advanced DevSecOps topics.

---

**Tags:** `#TryHackMe` `#PipelineSecurity` `#DevOps` `#CI/CD` `#SAST` `#DAST` `#AutomationSecurity`
