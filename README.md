# 🚀 Azure AKS Portfolio Project (AZ-104)

## Overview
This project demonstrates the design, deployment, and operation of an Azure Kubernetes Service (AKS) cluster using cost-aware, quota-conscious design principles aligned with the AZ-104 Microsoft Azure Administrator certification.

The focus of this project is Azure infrastructure administration rather than application development.

---

## Objectives
- Deploy a functional AKS cluster using minimal resources
- Work within regional and VM family vCPU quota constraints
- Administer AKS using Azure CLI and kubectl
- Deploy and expose a containerized workload
- Enable and validate Azure Monitor and logging
- Apply cost-optimised design decisions

---

## Architecture Diagram

![AKS Architecture Diagram](diagrams/aks-architecture.png)

---

## Architecture

### Core Azure Resources
- Azure Kubernetes Service (AKS)
- Virtual Machine Scale Set (system node pool)
- Azure Load Balancer (Service type: LoadBalancer)
- Azure Monitor / Log Analytics
- Azure Cloud Shell

### Region
France Central

### Resource Group
rg-aks-portfolio-frc

---

## AKS Cluster Design

Cluster Name: aks-portfolio-frc  
Region: France Central  
Preset: Dev/Test  
Node Pool Type: System  
VM Size: Standard_D2s_v4  
vCPUs: 2  
Memory: 8 GiB  
Node Count: 1  
Autoscaling: Disabled  
Availability Zones: None  
Identity: System-assigned managed identity  
Networking: Azure CNI (default)  
Monitoring: Azure Monitor (Container Insights enabled)

---

## Design Decisions & Rationale

### Compute Sizing
B-series virtual machines were initially considered for cost efficiency.  
However, B-series is not supported for AKS system node pools.

Standard_D2s_v5 was not available due to VM family quota restrictions.  
Standard_D2s_v4 was selected as a non-burstable, AKS-compliant alternative.

This ensured compatibility with AKS system requirements while remaining within available quotas.

---

### Quota Awareness
This project explicitly accounts for:
- Regional vCPU quotas
- Per-VM-family quotas

The final design reflects real-world Azure subscription constraints rather than idealised lab conditions.

---

### Cost Optimisation
- Single-node cluster
- Autoscaling disabled
- Dev/Test preset
- No availability zones
- Resources created temporarily for demonstration

---

## Deployment Summary
1. Validated regional and VM family vCPU quotas
2. Created a resource group in France Central
3. Deployed AKS with a system-assigned managed identity
4. Connected to the cluster using Azure Cloud Shell
5. Verified node and system workloads
6. Deployed and exposed a containerized workload
7. Validated monitoring, logs, and cost visibility

---

## Application Deployment

A lightweight NGINX container was deployed to validate workload scheduling and networking.

Create deployment:

    kubectl create deployment nginx-demo --image=nginx

Expose service:

    kubectl expose deployment nginx-demo --type=LoadBalancer --port=80

The application was accessed through a public Azure Load Balancer IP, confirming external connectivity.

---

## Monitoring & Logging

### Kubernetes
Node and pod health were verified using kubectl.  
Application logs were retrieved using:

    kubectl logs <pod-name>

---

### Azure Monitor
Container Insights was enabled during cluster creation.  
Log Analytics was used to verify telemetry:

    KubePodInventory
    | summarize count() by Namespace

---

## Security & Identity
- System-assigned managed identity
- Kubernetes RBAC enabled
- No service principals or secrets manually managed
- Default network security configuration retained

---

## Cleanup & Cost Control
After testing:

    kubectl delete svc nginx-demo
    kubectl delete deployment nginx-demo

Final cleanup consisted of deleting the entire resource group to avoid ongoing costs.

---

## AZ-104 Skills Demonstrated
- AKS deployment and administration
- Azure CLI and Cloud Shell usage
- Kubernetes fundamentals
- Compute sizing and quota management
- Monitoring and log analysis
- Cost-aware infrastructure design
- Azure best-practice decision making

---

## Key Takeaways
AKS system node pools require non-burstable VM sizes.  
VM family quotas are as important as total vCPU quotas.  
Default Azure configurations frequently represent best practices.  
Cost governance is a core Azure Administrator responsibility.

---

## Future Enhancements
- Additional user node pool
- Microsoft Entra ID integration
- Private AKS cluster
- Internal Load Balancer exposure
- Infrastructure as Code using Bicep or Terraform

---

## Author
Shaun Barnett  
AZ-104 Azure Administrator Portfolio Project
