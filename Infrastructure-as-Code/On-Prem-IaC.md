# On-Premises Infrastructure as Code (IaC) - TryHackMe Room Summary

## Badges Earned
[![TryHackMe](https://img.shields.io/badge/TryHackMe-On--Premises%20IaC-blue)](https://tryhackme.com/room/onpremisesiac)
[![DevSecOps](https://img.shields.io/badge/DevSecOps-IaC%20Security-green)](https://tryhackme.com/room/onpremisesiac)
## Overview
This premium room provides comprehensive security guidance for on-premises infrastructure as code (IaC) deployments, covering practical implementations using Vagrant and Ansible, security concerns, and hands-on offensive security techniques against vulnerable IaC pipelines.

## Key Learning Objectives
- Understand reasons and use cases for on-premises IaC
- Master Vagrant fundamentals and provisioning
- Master Ansible playbooks, roles, and templates
- Build complete on-prem IaC workflows
- Identify and exploit security vulnerabilities in IaC pipelines
- Understand IaC security hardening principles

## Detailed Summary

### Task 1: Introduction
**Core Focus:** Security-focused on-premises IaC deployments in regulated environments

**Prerequisites:**
- Basic offensive security techniques
- Introductory IaC knowledge
- Understanding of DevOps/DevSecOps principles

**Real-world Context:** Heavy emphasis on heavily regulated industries (financial, governmental) where cloud deployments may not be feasible due to compliance requirements.

### Task 2: Why On-Premises IaC
**Key Decision Factors:**

| Aspect | On-Premises IaC | Cloud-Based IaC |
|--------|-----------------|------------------|
| Control | Complete control over pipeline | Shared responsibility model |
| Customization | Fully customizable for specific needs | Limited by provider offerings |
| Data Protection | Easier compliance with regulations | Third-party data handling |
| Cost Structure | High initial investment, no monthly fees | Pay-as-you-go model |
| Scalability | Limited by physical hardware | Virtually unlimited elasticity |

**Case Study Analysis:** GitHub (SaaS) vs Self-hosted GitLab comparison highlighting control vs convenience trade-offs.

**Decision Framework:**
- Use on-prem IaC when: Need full control, have strict data regulations, doing small deployments
- Use cloud IaC when: Need flexible scalability, want lower initial investment

### Task 3: Vagrant Basics
**Core Concepts & Terminology:**

| Term | Definition | Practical Example |
|------|------------|------------------|
| Provider | Virtualization technology (Docker, VirtualBox, VMware) | `config.vm.provider "docker"` |
| Box | Base image for provisioning | `config.vm.box = "ubuntu/bionic64"` |
| Vagrantfile | Main provisioning configuration | Ruby-based configuration file |
| Provision | Action execution on hosts | File uploads, script execution |
| Network Config | Interface and IP configuration | Private networks, static IPs |

**Key Commands:**
```bash
vagrant up                    # Provision all hosts
vagrant up webserver          # Provision specific host
vagrant status               # Check provisioning status
vagrant destroy              # Destroy all provisioned hosts
```

**Example Vagrantfile Structure:**
```ruby
Vagrant.configure("2") do |config|
  config.vm.define "webserver" do |cfg|
    cfg.vm.box = "ubuntu/bionic64"
    cfg.vm.hostname = "testserver"
    cfg.vm.provider :virtualbox do |v|
      v.cpus = 2
      v.memory = 4096
    end
    cfg.vm.network :private_network, ip: "192.168.56.10"
  end
end
```

### Task 4: Ansible Basics
**Architecture & Components:**

**Core Terminology:**
- **Playbook**: YAML file defining provisioning steps and desired state
- **Role**: Reusable collection of tasks, templates, and variables
- **Template**: Configuration files with Jinja2 variable placeholders
- **Variable**: Dynamic values injected during provisioning
- **Handler**: Tasks triggered by notifications from other tasks

**Standard Folder Structure:**
```
.
├── playbook.yml
├── roles/
│   └── webapp/
│       ├── tasks/
│       │   ├── main.yml
│       │   ├── db-setup.yml
│       │   └── app-setup.yml
│       ├── templates/
│       │   ├── config.j2
│       │   └── script.sql.j2
│       ├── defaults/
│       │   └── main.yml
│       └── vars/
│           └── main.yml
└── group_vars/
    └── all.yml
```

**Vagrant vs Ansible Comparison:**

| Feature | Vagrant | Ansible |
|---------|---------|---------|
| Configuration Language | Ruby | YAML |
| Execution Model | Procedural | Declarative |
| Primary Use Case | Development environments | Enterprise infrastructure |
| Scalability | Smaller scale | Highly scalable |
| Integration | With provisioners like Ansible | Standalone or CI/CD tools |

**Example Playbook:**
```yaml
- name: Configure web server
  hosts: webservers
  become: yes
  vars:
    http_port: 80
  tasks:
    - name: Ensure Apache is installed
      apt:
        name: apache2
        state: present
    - name: Copy configuration template
      template:
        src: templates/apache.conf.j2
        dest: /etc/apache2/sites-available/000-default.conf
```

### Task 5: Building On-Prem IaC Workflow
**Hands-on Implementation:**

**Vagrantfile Analysis:**
- Two-machine deployment: `dbserver` and `webserver`
- Docker provider with MySQL and custom Ansible images
- Network configuration with static IP assignments (172.20.128.2/3)
- SSH key authentication setup
- Ansible playbook integration through shell provisioner

**Key Components:**
1. **Database Server**: MySQL container with root password configuration
2. **Web Server**: Custom Ansible image with SSH access and shared folders
3. **Network**: Private Docker network with internal communication
4. **Provisioning**: Ansible playbook execution on web server

**Ansible Playbook Structure:**
```yaml
- hosts: localhost
  connection: local
  roles:
    - webapp
```

**Webapp Role Tasks:**
- Database setup with SQL script template injection
- Web application file deployment
- Variable propagation through Jinja2 templates
- Service configuration and initialization

**Variable Injection Example:**
```sql
-- templates/createdb.sql.j2
CREATE DATABASE {{ db_name }};
USE {{ db_name }};

CREATE TABLE {{ db_name }}.tbl_user (
  'user_id' BIGINT AUTO_INCREMENT,
  'user_name' VARCHAR(45) NULL,
  PRIMARY KEY ('user_id'));
```

**Execution Commands:**
```bash
# Start the IaC pipeline
vagrant up

# Check running containers
docker ps

# Start web application
vagrant docker-exec -it webserver -- python3 /app/app.py
```

**Achievement:** Successfully deployed functional web application with database backend through automated IaC pipeline, accessible at http://172.20.128.2/

### Task 6: Security Concerns in On-Prem IaC
**Critical Security Considerations:**

**1. Dependency Management**
- Base image vulnerabilities propagation through the pipeline
- Third-party software risks in provisioned services
- Outdated package dependencies in container images
- Lack of vulnerability scanning in CI/CD pipeline

**2. Default Credentials**
- Vendor default accounts (Vagrant's `vagrant:vagrant`)
- Service default credentials (Jenkins `jenkins:jenkins`, MySQL root passwords)
- Lack of credential rotation post-deployment
- Hardcoded secrets in configuration files

**3. Insufficient Hardening**
- Services used for provisioning left enabled (WinRM, SSH with weak configurations)
- Missing security patches and updates in base images
- Inadequate access controls and firewall rules
- Excessive permissions on service accounts

**4. Remote Code Execution as Feature**
- Pipeline compromise leads to complete infrastructure compromise
- Inadequate secret management leading to credential exposure
- Privilege escalation through poorly configured services
- Lack of audit trails for provisioning activities

**Security Best Practices:**
- Implement proper secret management (HashiCorp Vault, AWS Secrets Manager)
- Apply principle of least privilege to all services and accounts
- Include security hardening in provisioning steps
- Regular security scanning of IaC code and dependencies
- Implement network segmentation and access controls
- Maintain audit logs for all provisioning activities

### Task 7: Attacking On-Prem IaC (Challenge)
**Offensive Security Exercise:**

**Initial Access:**
- SSH credentials: `entry:entry`
- Target IP: `10.10.144.22`

**Methodology:**
1. **IaC Script Analysis**: Comprehensive review of Vagrantfile and Ansible configurations for vulnerabilities
2. **Network Enumeration**: Identify deployed services, network topology, and trust relationships
3. **Vulnerability Identification**: Find security misconfigurations in provisioning scripts and deployed services
4. **Lateral Movement**: Exploit trust relationships between provisioned hosts and services
5. **Pipeline Compromise**: Gain control over IaC deployment mechanisms to achieve persistence

**Flags Captured:**
1. **Flag 1**: `THM{Dev.Bypasses.and.Checks.can.be.Dangerous}` - Related to development bypasses and insufficient checks
2. **Flag 2**: `THM{IaC.Deployment.Keys.Must.be.Removed}` - Related to exposed deployment keys and credentials
3. **Flag 3**: `THM{IaC.Shares.Should.be.Restricted}` - Related to insecure file shares and permissions
4. **Flag 4**: `THM{Provisioners.Usually.Have.Privileged.Access}` - Related to privileged access of provisioner accounts

**Key Attack Vectors Identified:**
- Default credentials and hardcoded secrets in provisioning scripts
- Insecure file shares with excessive permissions
- Privileged service accounts with unnecessary access
- Lack of network segmentation between components
- Insufficient input validation in deployed applications
- Exposed administrative interfaces and services

**Tools and Techniques Used:**
- Nmap for network enumeration and service discovery
- SSH for remote access and port forwarding
- SCP for file transfer and data exfiltration
- Manual code review of IaC configuration files
- Privilege escalation through misconfigured services

## Key Definitions Expanded

**On-Premises IaC**: Managing and provisioning infrastructure using code-driven approaches within an organization's own data centers rather than cloud environments, providing complete control over the entire stack.

**Vagrant Provider**: The underlying virtualization technology (Docker, VirtualBox, VMware, AWS) that Vagrant uses to create and manage virtual machines or containers.

**Ansible Role**: A predefined collection of tasks, variables, files, and templates that can be reused across multiple playbooks and hosts, promoting code reuse and consistency.

**Immutable Infrastructure**: Infrastructure paradigm where components are replaced rather than modified after deployment, ensuring consistency and reducing configuration drift.

**Declarative Provisioning**: Infrastructure definition approach that specifies the desired end state rather than the specific steps to achieve it, enabling idempotent operations.

**Idempotent Operations**: Operations that can be applied multiple times without changing the result beyond the initial application, a key characteristic of declarative IaC tools.

## Self-Reflection & Key Insights

### Technical Mastery Gained
- **Vagrant Proficiency**: Deep understanding of Vagrantfile structure, multi-machine configurations, provider integrations, and network configuration
- **Ansible Expertise**: Mastery of playbook creation, role development, template injection, variable management, and conditional execution
- **Pipeline Integration**: Skills in combining Vagrant and Ansible for comprehensive infrastructure deployment and management
- **Docker Integration**: Experience using Docker as a Vagrant provider for container-based deployments and understanding of container networking
- **Infrastructure Design**: Ability to design multi-tier applications with proper service separation and network architecture

### Security Mindset Development
- **Attack Surface Recognition**: Ability to identify vulnerabilities in IaC pipelines before deployment through code review and analysis
- **Defensive Posturing**: Understanding of security measures required to protect IaC pipelines, including secret management and access controls
- **Compliance Awareness**: Recognition of regulatory implications in different deployment models and industries
- **Risk Assessment**: Skills in evaluating security trade-offs between control, convenience, and functionality
- **Threat Modeling**: Ability to model potential attacks against IaC pipelines and deployed infrastructure

### Architectural Understanding
- **Control vs Scalability**: Deep appreciation of the fundamental trade-off between on-prem control and cloud scalability in different scenarios
- **Tool Selection Criteria**: Framework for choosing between Vagrant, Ansible, or combined approaches based on specific use cases and requirements
- **Lifecycle Management**: Understanding of complete IaC lifecycle from development, testing, deployment, to decommissioning
- **Integration Patterns**: Knowledge of how IaC fits into broader DevOps/DevSecOps practices and CI/CD pipelines
- **Hybrid Approaches**: Understanding of when to combine different IaC tools for optimal results

### Practical Security Lessons
1. **Secrets Management is Critical**: Hardcoded credentials in IaC scripts create massive attack surfaces that can compromise entire infrastructures
2. **Default Configurations are Dangerous**: Vendor defaults must be explicitly disabled or modified to prevent easy exploitation
3. **Dependency Management is Non-negotiable**: Outdated base images and packages propagate vulnerabilities throughout deployed environments
4. **Access Control is Fundamental**: Principle of least privilege applies equally to IaC pipelines and must be rigorously enforced
5. **Monitoring is Essential**: IaC deployments require continuous security validation and monitoring for anomalous activities
6. **Documentation Matters**: Well-documented IaC code enables better security reviews and maintenance
7. **Testing is Crucial**: Comprehensive testing of IaC code prevents misconfigurations and security gaps

## Industry Applications Identified

### Regulated Industries
- **Financial Services**: Data residency requirements and regulatory compliance forcing on-prem deployments with strict security controls
- **Government**: Classified information handling mandating physical control and air-gapped infrastructure
- **Healthcare**: HIPAA compliance requiring specific data handling procedures and audit trails
- **Defense**: Weapon systems and classified infrastructure deployments with stringent security requirements

### Enterprise Scenarios
- **Legacy System Integration**: On-prem systems requiring cloud-like automation while maintaining existing infrastructure
- **Hybrid Cloud Strategies**: Bridge between cloud and physical infrastructure for gradual migration or specific workloads
- **Disaster Recovery**: Rapid infrastructure recreation in emergency scenarios with predictable outcomes
- **Development Environments**: Consistent, reproducible development setups that mirror production environments
- **Testing Laboratories**: Isolated environments for security testing, performance validation, and quality assurance

## Skills Validation
- **Practical IaC Deployment**: Successfully built and deployed multi-tier application with database backend
- **Security Assessment**: Identified and exploited IaC vulnerabilities in a realistic challenge environment
- **Tool Proficiency**: Mastered both Vagrant and Ansible in realistic deployment scenarios
- **Problem Solving**: Solved complex infrastructure challenges through systematic analysis and troubleshooting
- **Security Hardening**: Understood and applied defensive measures to protect IaC pipelines
- **Code Review**: Developed skills in reviewing IaC code for security misconfigurations and vulnerabilities
- **Network Design**: Understanding of network architecture in multi-machine deployments

## Future Learning Path
- **Advanced Ansible**: Complex role development, dynamic inventory management, and custom module creation
- **Terraform Integration**: Multi-cloud and hybrid deployment strategies with infrastructure state management
- **Kubernetes with IaC**: Container orchestration provisioning and management using IaC principles
- **Policy as Code**: Automated compliance and security enforcement using tools like Open Policy Agent
- **GitOps**: Infrastructure management through Git workflows and pull request-based deployments
- **Security Scanning**: Integration of security scanning tools into IaC pipelines for continuous validation
- **Performance Optimization**: Advanced techniques for optimizing IaC deployment speed and resource usage

## Room Completion Details

**Room Status**: Completed  
**Difficulty Level**: Intermediate  
**Time Investment**: 3-4 hours  
**Practical Exercises**: Hands-on IaC deployment and exploitation  
**Skills Validated**: Vagrant, Ansible, IaC Security, Offensive Security  
**Key Achievement**: Successfully attacked and compromised vulnerable IaC pipeline  
**Real-world Application**: Directly applicable to regulated industry environments  
**Security Mindset**: Developed both offensive and defensive IaC security perspectives  

**Overall Assessment**: This room provides an excellent balance of theoretical knowledge and practical skills, with particularly valuable insights into IaC security concerns and offensive techniques. The hands-on challenge effectively reinforces the security concepts taught throughout the room, making it highly relevant for security professionals working with infrastructure automation.

---
