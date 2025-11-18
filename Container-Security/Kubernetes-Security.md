# TryHackMe - Intro to Kubernetes

![TryHackMe](https://img.shields.io/badge/TryHackMe-Intro%20to%20Kubernetes-red)
![Category](https://img.shields.io/badge/Category-Container%20Security-blue)
![Level](https://img.shields.io/badge/Level-Intermediate-green)
![Time](https://img.shields.io/badge/Time-60%20min-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Premium](https://img.shields.io/badge/Premium-Room-gold)

## 📋 Table of Contents
- [Room Overview](#room-overview)
- [Learning Objectives](#learning-objectives)
- [Task 1: Introduction](#task-1-introduction)
- [Task 2: Kubernetes 101](#task-2-kubernetes-101)
- [Task 3: Kubernetes Architecture](#task-3-kubernetes-architecture)
- [Task 4: Kubernetes Landscape](#task-4-kubernetes-landscape)
- [Task 5: Kubernetes Configuration](#task-5-kubernetes-configuration)
- [Task 6: Kubectl](#task-6-kubectl)
- [Task 7: Kubernetes & DevSecOps](#task-7-kubernetes--devsecops)
- [Task 8: Hands-on with Kubernetes](#task-8-hands-on-with-kubernetes)
- [Key Definitions](#key-definitions)
- [Self-Reflection](#self-reflection)

## 🎯 Room Overview

This premium room provides a comprehensive introduction to Kubernetes (K8s), covering fundamental concepts, architecture, configuration, and practical hands-on experience with cluster management and security.

## 🎓 Learning Objectives

- Understand why Kubernetes is needed and its benefits
- Learn basic Kubernetes architecture and components
- Master key Kubernetes landscape concepts
- Use kubectl for cluster traversal and management
- Apply Kubernetes security best practices as a DevSecOps engineer

## 📖 Task 1: Introduction

**Objective:** Introduction to Kubernetes and learning prerequisites.

**Key Concepts:**
- Kubernetes as container orchestration system
- K8s as numeronym for Kubernetes
- Foundation for container security module
- Building on previous container knowledge

**Answer:** 
```
No answer needed
```

## 🚀 Task 2: Kubernetes 101

### Evolution from Monolithic to Microservices
- **Monolithic Architecture:** Single application unit
- **Microservices Architecture:** Broken into components with different business functions
- **Netflix Case Study:** 2009 transition due to scaling demands

### Kubernetes Definition
- **Container Orchestration System:** Manages and organizes containers
- **Orchestra Metaphor:** Containers as instruments, Kubernetes as conductor
- **Scalability:** Automatically spins up new containers to handle traffic

### Key Benefits
- **High Availability & Scalability:** Load balancing and redundancy
- **Highly Portable:** Runs anywhere on any infrastructure
- **Popular & Compatible:** Extensive ecosystem and tool integration

**Questions & Answers**

**Which benefit of Kubernetes means that it can run anywhere on any type of infrastructure?**
```
highly portable
```

**Fill in the blank "Kubernetes is a _________ _____________ system".**
```
container orchestration
```

## 🏗️ Task 3: Kubernetes Architecture

### Core Components

**Pods**
- Smallest deployable unit in Kubernetes
- Group of one or more containers
- Share storage and network resources
- Unit of replication for scaling

**Nodes**
- **Control Plane (Master Node):** Manages worker nodes and pods
- **Worker Nodes:** Run application workloads
- Virtual or physical machines

**Cluster**
- Set of nodes working together
- Highest level of Kubernetes organization

### Control Plane Components
| Component | Purpose |
|-----------|---------|
| **kube-apiserver** | Front end for Kubernetes API |
| **etcd** | Key/value store for cluster data |
| **kube-scheduler** | Assigns pods to nodes |
| **kube-controller-manager** | Runs controller processes |
| **cloud-controller-manager** | Cloud provider communication |

### Worker Node Components
| Component | Purpose |
|-----------|---------|
| **kubelet** | Ensures containers run in pods |
| **kube-proxy** | Network communication within cluster |
| **Container runtime** | Runs containers (Docker, rkt, runC) |

**Questions & Answers**

**What is the smallest deployable unit of computing you can create in Kubernetes?**
```
pod
```

**Which control plane component is a key/value store which contains data pertaining to the cluster and its current state?**
```
etcd
```

**Which worker node component is responsible for network communication within the cluster?**
```
kube-proxy
```

## 🗺️ Task 4: Kubernetes Landscape

### Key Kubernetes Concepts

**Namespaces**
- Isolate groups of resources in single cluster
- Resource grouping by component or tenant
- Unique resource names within namespace

**ReplicaSet**
- Maintains set of replica pods
- Guarantees availability of identical pods
- Managed by deployments

**Deployments**
- Define desired state for applications
- Manage ReplicaSets and pods
- Declarative updates for pod management

**StatefulSets**
- For stateful applications (databases)
- Persistent unique pod IDs
- Master/slave pod configuration
- Ordered pod creation and deletion

**Services**
- Expose pods with static IP address
- Load balancing between pod replicas
- Types: ClusterIP, LoadBalancer, NodePort, ExternalName

**Ingress**
- Single access point to cluster
- Traffic routing between services
- Centralized routing rules

### DevOps vs. DevSecOps Responsibilities
- **DevOps:** Building and maintaining cluster
- **DevSecOps:** Securing cluster and applications
- Overlap depending on company structure

**Questions & Answers**

**Which Kubernetes component exposes pods and serves as an access point?**
```
service
```

**Which Kubernetes component can guarantee the availability of X number of pods?**
```
replicaset
```

**What Kubernetes component is used to define a desired state?**
```
deployments
```

## ⚙️ Task 5: Kubernetes Configuration

### YAML Configuration Basics
- **File Format:** YAML (human-readable) or JSON
- **Required Fields:** apiVersion, kind, metadata, spec

### Required Fields Explained
| Field | Purpose | Example |
|-------|---------|---------|
| **apiVersion** | Kubernetes API version | `v1`, `apps/v1` |
| **kind** | Type of object | `Deployment`, `Service` |
| **metadata** | Object identification | `name`, `namespace` |
| **spec** | Desired state | `replicas: 3` |

### Service Configuration Example
```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
  type: ClusterIP
```

### Deployment Configuration Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

**Questions & Answers**

**In a config file, you have just declared that you want 4 nginx pods. In which one of the 'required fields' has this been declared?**
```
spec
```

**The configuration file is for a deployment. In which one of the 'required fields' is this declared?**
```
kind
```

**The pods in this deployment will be exposed by a service. In the service configuration file, the target port was set to 80. What should you put as the 'containerPort'?**
```
80
```

## 🛠️ Task 6: Kubectl

### Essential kubectl Commands

**kubectl apply**
- Applies configuration from YAML files
- Creates or updates resources
- `kubectl apply -f deployment.yaml`

**kubectl get**
- Checks resource status
- Versatile for various resource types
- `kubectl get pods -n namespace`

**kubectl describe**
- Shows detailed resource information
- Useful for troubleshooting
- `kubectl describe pod pod-name`

**kubectl logs**
- Views application logs from containers
- Security analysis and debugging
- `kubectl logs pod-name`

**kubectl exec**
- Accesses container shell
- Interactive command execution
- `kubectl exec -it pod-name -- sh`

**kubectl port-forward**
- Creates secure tunnel to pods
- Local machine to cluster communication
- `kubectl port-forward service/service-name 8090:8080`

**Questions & Answers**

**...troubleshoot a pod by gathering some details about it?**
```
describe
```

**...access the container's shell?**
```
exec
```

**...check the status of running pods?**
```
get
```

**...turn a defined configuration (YAML file) into a running process?**
```
apply
```

## 🛡️ Task 7: Kubernetes & DevSecOps

### Kubernetes Security Considerations
- Emerging technology with popularity
- Network communication between pods
- Default pod-to-pod communication
- New attack surface introduction

### Kubernetes Hardening Areas

**Pod Security**
- Non-root container privileges
- Immutable filesystems
- Regular vulnerability scanning
- Prevent privileged containers
- Pod Security Standards (PSS) and Pod Security Admission (PSA)

**Network Security**
- Control plane access restriction
- TLS certificate communication
- Explicit deny policies
- Encrypted secrets management

**Authentication & Authorization**
- Disable anonymous access
- Strong user authentication
- RBAC policy implementation

**Monitoring & Logging**
- Audit logging enablement
- Log monitoring and alerting
- Threat detection systems

**Maintenance & Updates**
- Quick security patch application
- Regular vulnerability scanning
- Obsolete component removal

### Key Security Practices

**RBAC (Role-Based Access Control)**
- Regulates cluster access based on roles
- Users, groups, or service accounts
- YAML configuration for rules

**Secrets Management**
- Stores sensitive information securely
- Base64 encoded by default
- Encryption at rest configuration
- Least privilege access

**PSA & PSS**
- **Pod Security Standards:** Three levels (privileged, baseline, restricted)
- **Pod Security Admission:** Enforces PSS policies
- Replaced Pod Security Policies (PSP)

**Questions & Answers**

**Which best container security practice is used to regulate access to a Kubernetes cluster and its resources?**
```
RBAC
```

**What is used to define security policies at 3 levels?**
```
Pod Security Standards
```

**What enforces these policies?**
```
Pod Security Admission
```

**What Kubernetes object can be used to store sensitive information and should, therefore, be managed securely?**
```
Secret
```

## 🎮 Task 8: Hands-on with Kubernetes

### Practical Exercise Overview

**Phase 1: Explore**
- Start Minikube cluster: `minikube start`
- Check running pods: `kubectl get pods -A`
- Apply deployment and service configurations
- Verify pod creation

**Phase 2: Interact**
- Port forward to service: `kubectl port-forward service/nginx-service 8090:8080`
- Access web application at `http://localhost:8090/`
- Retrieve credentials from Kubernetes secret
- Use `kubectl get secrets` and `kubectl describe secret`
- Base64 decode secret data

**Phase 3: Secure**
- Create service accounts: `kubectl create sa`
- Configure RBAC with Role and RoleBinding
- Apply security configurations
- Verify access restrictions with `kubectl auth can-i`

### Configuration Files Used
- **nginx-deployment.yaml:** Defines web application deployment
- **nginx-service.yaml:** Exposes application via NodePort service
- **role.yaml:** Defines secret access permissions
- **role-binding.yaml:** Binds role to service account

**Questions & Answers**

**Can you master the basics of Kubernetes and retrieve the flag?**
```
THM{k8s_k3nno1ssarus}
```

**What apiVersion is used for the RoleBinding?**
```
rbac.authorization.k8s.io/v1
```

## 📚 Key Definitions

**Kubernetes (K8s):** Container orchestration system for automating deployment, scaling, and management of containerized applications.

**Pod:** Smallest deployable unit in Kubernetes, containing one or more containers.

**Node:** Worker machine in Kubernetes cluster, either physical or virtual.

**Cluster:** Set of nodes running containerized applications managed by Kubernetes.

**kubectl:** Command-line tool for communicating with Kubernetes cluster.

**RBAC:** Role-Based Access Control for regulating access to Kubernetes resources.

**Deployment:** Kubernetes object that defines desired state for applications.

**Service:** abstraction that defines a logical set of pods and policy to access them.

## 📝 Self-Reflection

### My Kubernetes Learning Journey

This room provided me with a comprehensive foundation in Kubernetes, transforming it from a mysterious buzzword into a practical technology I can understand and work with. The structured approach from theoretical concepts to hands-on practice was particularly effective.

**Key Technical Insights:**

**Architectural Understanding:** Learning about Kubernetes components like etcd, kube-apiserver, and kube-proxy gave me insight into how container orchestration works at a fundamental level, which is crucial for both deployment and security.

**Practical Configuration Skills:** Working with YAML configuration files and kubectl commands provided me with practical skills I can immediately apply in real-world scenarios. Understanding the relationship between deployments, services, and pods is essential for effective cluster management.

**Security Mindset Development:** The DevSecOps focus helped me understand the security implications of Kubernetes deployments. Learning about RBAC, secrets management, and pod security standards gave me the security perspective needed for modern containerized environments.

**Hands-on Experience:** The practical exercise was invaluable for reinforcing theoretical concepts. From starting a Minikube cluster to configuring RBAC policies, I gained confidence in working with Kubernetes environments.

This knowledge forms a solid foundation for advancing to more complex Kubernetes security topics and prepares me for working with container orchestration in professional DevSecOps roles.

---

**Tags:** `#TryHackMe` `#Kubernetes` `#K8s` `#ContainerOrchestration` `#DevSecOps` `#ClusterSecurity` `#HandsOnLearning`
