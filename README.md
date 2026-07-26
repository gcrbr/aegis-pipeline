# 🛡️ Aegis Pipeline

<div align="center">
  <img src="assets/logo.png" alt="Aegis Logo" width="100"/>
  <br>
  <img src="assets/brands.png" alt="Aegis Logo" width="300"/>
</div>

This project is the result of a project work carried out in collaboration with [Fides Group](https://www.fidesgroup.com/it) and students from the **Cisco DTLab**.

This repository serves a **demonstrative and educational** purpose. It contains the implementation of a secure and automated **DevSecOps** pipeline for validating, containerizing, and deploying cloud-native workloads on Kubernetes clusters.


## 🏗️ Architecture & Modules

The project covers the 3 required modules as follows:

### Module 1: DevOps and Orchestration with Kubernetes
- **Containerization**: The pipeline builds the image and pushes it to Docker Hub only if all security checks pass.
- **K8s Manifests**: The `k8s-examples/` folder contains examples of a Deployment (with Probes and Resource Limits), Service, ConfigMap, PVC, Secret, and RBAC/ServiceAccount configurations to manage cluster privileges.
- **GitOps CD**: Delegated to _Argo CD_ to visualize state, manage synchronizations, detect configuration drift, and easily handle rollbacks.

### Module 2: Cloud-Native Networking and Security with Cilium
- **Default Deny & Microsegmentation**: Implementation of `CiliumNetworkPolicy` rules (versioned in the repository) starting with a *Default Deny* approach for the namespace.
- **L7 Traffic Control**: Authorization of strictly necessary traffic via Labels (e.g., blocking direct database access from the frontend, authorizing specific API calls) restricted to allowed HTTP paths.

### Module 3: DevSecOps and Observability
- **Secure Pipeline**:
  - `Trufflehog`: Secret scanning to prevent accidental commits of plaintext credentials.
  - `Trivy`: Filesystem and dependency scanning, container image vulnerability scanning (OS and libraries), and formal security validation on Kubernetes YAML manifests.
- **In-Cluster Security (OPA Gatekeeper)**: Declarative rules in `k8s-examples/opa-policies` (ConstraintTemplates and Constraints) to validate resources during admission, preventing insecure instantiations such as privileged containers.
- **Observability**: Configured in-cluster utilizing modern stacks (Prometheus/Grafana for workload metrics, Hubble/Cilium for network flow mapping and drop analysis).

## 🛠️ Technology Stack
- **CI Pipeline**: GitHub Actions
- **Containerization**: Docker
- **Security Scanning**: Aqua Trivy, TruffleHog
- **Orchestration**: Kubernetes, Helm / Kustomize
- **GitOps CD**: Argo CD
- **Networking & CNI**: Cilium, Hubble
- **Policy Engine**: OPA Gatekeeper

## 📁 Repository Structure

```text
.
├── .github/
│   └── workflows/
│       └── aegis.yaml           # DevSecOps CI/CD Pipeline
├── k8s-examples/
│   ├── deployment.yaml          # Secure workload example with probes & limits
│   ├── service.yaml             # Service exposure
│   ├── configmap.yaml / pvc.yaml / secret.yaml # Data & persistence management
│   ├── rbac.yaml / serviceaccount.yaml         # In-cluster permissions
│   ├── network-policies.yaml    # Cilium Policies (Default Deny & L7 traffic)
│   └── opa-policies/            # OPA Gatekeeper Policies (e.g., no-privileged)
└── README.md
```

## 🔗 Demonstration
This repository focuses on demonstrating the infrastructure and security configurations. The fully functional CI/CD pipeline, including the active **Argo CD** deployment, is implemented in a separate repository hosting a custom-built **3-tier mock application** (Frontend, Backend, Database) used as our live workload.

👉 [**View the Live Implementation Repository here**](https://github.com/gcrbr/aegis-mock-app)