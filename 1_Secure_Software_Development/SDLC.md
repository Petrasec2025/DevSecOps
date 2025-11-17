# TryHackMe - SDLC Room Writeup

![TryHackMe](https://img.shields.io/badge/TryHackMe-SDLC-red)
![Category](https://img.shields.io/badge/Category-Secure%20Software%20Development-blue)
![Level](https://img.shields.io/badge/Level-Fundamental-green)
![Time](https://img.shields.io/badge/Time-120%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: SDLC](#task-2-sdlc)
- [Task 3: SDLC Phases Part 1](#task-3-sdlc-phases-part-1)
- [Task 4: SDLC Phases Part 2](#task-4-sdlc-phases-part-2)
- [Task 5: Keep CALMS](#task-5-keep-calms)
- [Task 6: DevOps Metrics](#task-6-devops-metrics)
- [Task 7: Production of the Droids](#task-7-production-of-the-droids)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This room provides a comprehensive introduction to the **Software Development Lifecycle (SDLC)** framework, covering its phases, importance, and how to integrate security into the development process.

## 🎓 Learning Objectives

- Understand the SDLC framework and its importance
- Learn about the phases of SDLC
- How to instill security into the SDLC
- Understand the Security SDLC phases and process

## 📖 Task 1: Introduction

**Objective:** Introduction to the SDLC framework and its components.

**Key Concepts:**
- SDLC as a standardized framework for building software applications
- Methodology to improve software quality and development process
- Framework for measuring and improving development efficiency

**Answer:** 
```
No answer needed
```

## 🔄 Task 2: SDLC

### What is Software Development Lifecycle (SDLC)?

**Definition:** SDLC is a set of practices that form a framework to standardize building software applications. It defines tasks to perform at each development stage to improve quality, meet deadlines, and control costs.

### How SDLC Works

SDLC splits development into measurable phases that can be tracked and monitored. The aim is to establish repeatable processes and predictable outcomes.

**SDLC Phases Visualized:**
![SDLC Phases Diagram](https://github.com/user-attachments/assets/08232b24-cd33-48e4-b29e-ba35f86c6d09)

**SDLC Phases:** Typically 6-8 phases including:
1. Planning
2. Requirements Definition  
3. Design & Prototyping
4. Software Development
5. Testing
6. Deployment
7. Operations & Maintenance

### Questions & Answers

**How many phases can an SDLC have? (Format X-Y)**
```
6-8
```

## 🏗️ Task 3: SDLC Phases Part 1

### First Four Phases

**1. Planning (Feasibility Stage)**
- Determines scope and purpose
- Defines system problems and requirements
- Resource allocation and project scheduling
- Crucial for time-to-market goals

**2. Requirements Definition**
- Defines prototype ideas and gathers details
- Creates Software Requirement Specification (SRS)
- Identifies end-user needs through research
- Defines resources needed for the project

**3. Design and Prototyping**
- Outlines application details: user interfaces, system interfaces, network requirements, databases
- Converts SRS into logical structures
- Creates Architecture Design Review (ADR)
- Defines security measures and communication methods

**4. Software Development**
- Code writing and application development
- Documentation creation (playbooks, guides)
- Adherence to coding guidelines
- Incorporation of secure coding practices

### Questions & Answers

**What phase focuses on determining the first idea for a prototype?**
```
Requirements Definition
```

**What stage is also known as the "Feasibility Stage"?**
```
Planning Stage
```

**When do you outline the user interfaces and network requirements?**
```
Design and Prototyping
```

## 🚀 Task 4: SDLC Phases Part 2

### Final Three Phases

**5. Testing**
- Critical pre-release validation
- Automated tooling and source code scanners
- Software Testing Lifecycle phases:
  - Test case design and development
  - Test environment setup  
  - Test execution
- Regression testing and bug reporting

**6. Deployment**
- Software made available to users
- Automated deployment tools (Netlify, Argo CD)
- Release management and rollback capabilities
- Feature releases and version updates

**7. Operations and Maintenance**
- Handling issues reported by end-users
- Implementing post-deployment changes
- Focus on stability and uptime
- Self-service enablement and tool standardization

### Questions & Answers

**What phase focuses on handling issues or bugs reported by end-users?**
```
Operations and Maintenance
```

**What phase involves releasing new versions of software?**
```
Deployment
```

**What phase ensures software meets the standards defined in the requirements phase?**
```
Testing
```

## 🤝 Task 5: Keep CALMS

### CALMS Framework

**Definition:** A framework assessing company's ability to adopt DevOps processes, standing for:

**C - Culture**
- DevOps as cultural change
- Adoption across all teams (development, QA, product management, operations)
- Shift from waterfall to agile sprints

**A - Automation**
- Focus on automated processes
- Continuous delivery and integration
- Configuration as code
- Streamlined production-ready code

**L - Lean**
- Breaking tasks into small components
- Early application versions to users
- Constant improvement through user feedback

**M - Measurement**
- Metrics for process effectiveness
- Continuous improvement tracking
- Essential for DevOps success

**S - Sharing**
- Shared responsibility across teams
- Collaborative approach to final solution
- Better product outcomes through teamwork

### Questions & Answers

**What does CALMS stand for?**
```
Culture, Automation, Lean, Measurement, Sharing
```

## 📊 Task 6: DevOps Metrics

### Essential DevOps Metrics

**MTTP (Mean Time to Production)**
- Time between code commit and deployment
- Improved through test automation and small batches
- Fast feedback for developers

**Failure Rate**
- Percentage of code changes requiring hot fixes
- Measures production failures
- Essential for security requirement validation

**Deployment Frequency**
- How often new code is deployed to production
- Enables on-demand deployment
- Requires automated testing pipelines

**MTTR (Mean Time to Recovery)**
- Recovery time after service interruption
- Requires continuous monitoring and alerting
- Critical for operations team efficiency

**Deployment Agility**
- Combination of deployment speed and frequency
- Key metric for development efficiency

### Risk Communication
- Different risk definitions across teams
- Security vs. operational risk perspectives
- Finding common ground for security implementation

### Questions & Answers

**What 2 metrics are used to measure deployment agility?**
```
Deployment speed and frequency
```

**What is an essential rate for engineers in Production environments to know if code meets security requirements?**
```
Failure rate
```

**What is the measurement for recovery time after a failure?**
```
MTTR
```

## 🤖 Task 7: Production of the Droids

### Interactive Challenge

**Scenario:** Control a droid production factory with $1,000,000 seed funding. Goal: Double the empire's investment through effective SDLC management.

**Key Principles:**
- Mythical Man Month principle (Brooks's Law): Adding resources to late projects makes them later
- Optimal allocation of developers and sprints
- Strategic split of sprints across SDLC phases
- Balancing factory effectiveness with resource allocation

### Challenge Screenshots:

**Factory Overview:**
![Factory Setup](https://github.com/user-attachments/assets/d830381d-6e02-4788-b7f9-e846d5c8edde)

**Resource Allocation:**
![Resource Management](https://github.com/user-attachments/assets/43fe5ef4-29e2-41db-b520-44c80ae230c2)

**Phase Distribution:**
![Phase Allocation](https://github.com/user-attachments/assets/3f69d9e8-87d7-499f-95b3-1394ad82618d)

**Production Results:**
![Production Outcome](https://github.com/user-attachments/assets/0cdd5f78-f936-4ce1-ba54-c03473fc0157)

**Success Achievement:**
![Success Screen](https://github.com/user-attachments/assets/8a4e4624-3c99-4aae-9005-3cb0d645beb8)

**Final Results:**
![Final Score](https://github.com/user-attachments/assets/58fcd8e1-e699-4f82-a498-44faade7c5a7)

### Challenge Strategy
- Careful developer hiring decisions
- Strategic sprint allocation across phases
- Attention to factory effectiveness metrics
- Achieving beyond double investment through optimization

### Questions & Answers

**What is the flag that you receive once you have doubled the empire's investment?**
```
THM{Ruler.of.the.SDLC.Droids}
```

## 📚 Key Definitions

**SDLC (Software Development Lifecycle):** Framework standardizing software application building through defined phases and tasks.

**SRS (Software Requirement Specification):** Document outlining application requirements and functionality.

**ADR (Architecture Design Review):** Document ensuring alignment across development teams on application architecture.

**CALMS Framework:** Assessment framework for DevOps adoption covering Culture, Automation, Lean, Measurement, Sharing.

**MTTP (Mean Time to Production):** Metric measuring time from code commit to deployment.

**MTTR (Mean Time to Recovery):** Metric measuring recovery time after service failures.

**Brooks's Law:** Principle stating that adding human resources to late software projects delays them further.

## 📝 Self-Reflection

### My Learning Journey Through SDLC

This room provided me with fundamental knowledge about SDLC frameworks and their critical importance in modern software development. The structured approach to understanding each phase helped me see how security can be integrated throughout the entire development lifecycle rather than being treated as an afterthought.

**Key Takeaways for Me:**

**Understanding SDLC Phases:**
I learned how each phase builds upon the previous one, creating a comprehensive framework for software development. The clear breakdown from Planning through Operations & Maintenance gave me a holistic view of the entire process.

**CALMS Framework Application:**
The CALMS framework introduced me to a practical way to assess DevOps readiness. I realized that successful implementation requires not just technical improvements but significant cultural change across all teams. This framework will be invaluable in my future work with development teams.

**Metrics That Matter:**
The metrics section was particularly enlightening for me. Understanding MTTP, MTTR, failure rates, and deployment agility helped me see how to measure development efficiency effectively. I now appreciate how these metrics can bridge communication gaps between security and development teams.

**Practical Application:**
The interactive droid factory challenge was an excellent way for me to apply the theoretical concepts. Making decisions about resource allocation and phase balancing gave me hands-on experience with the real-world implications of development decisions. It reinforced the importance of strategic planning in SDLC management.

**Broader Implications:**
This foundation in SDLC principles has prepared me to advance through the DevSecOps learning path with a better understanding of how to implement security at every stage of software development. I now see SDLC not just as a process, but as a mindset that can significantly improve software quality and security when properly implemented.

The room has equipped me with both the theoretical knowledge and practical insights needed to contribute meaningfully to secure software development practices in real-world scenarios.
