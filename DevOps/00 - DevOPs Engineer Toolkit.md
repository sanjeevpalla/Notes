# 🛠️ DevOps Engineer Toolkit — Tools, Workflow & Connectivity

A complete map of the tools a DevOps Engineer should learn, organized by lifecycle stage, with flow diagrams showing how everything connects.

---

## 🔄 1. The DevOps Lifecycle (Infinite Loop)

```mermaid
flowchart LR
    A[Plan] --> B[Code]
    B --> C[Build]
    C --> D[Test]
    D --> E[Release]
    E --> F[Deploy]
    F --> G[Operate]
    G --> H[Monitor]
    H -->|Feedback| A

    style A fill:#4A90D9,color:#fff
    style B fill:#4A90D9,color:#fff
    style C fill:#50C878,color:#fff
    style D fill:#50C878,color:#fff
    style E fill:#F5A623,color:#fff
    style F fill:#F5A623,color:#fff
    style G fill:#D0021B,color:#fff
    style H fill:#D0021B,color:#fff
```

The loop never stops — **Monitor** feeds insights back into **Plan**, driving continuous improvement (CI/CD/CM: Continuous Integration, Delivery, Monitoring).

---

## 🧰 2. Tools by Lifecycle Stage

### 📋 2.1 Plan — Collaboration & Project Management
| Tool | Purpose |
|---|---|
| **Jira** | Agile issue/sprint tracking |
| **Confluence** | Documentation & knowledge base |
| **Trello / Linear** | Lightweight task boards |
| **Slack / MS Teams** | Team communication, ChatOps |

### 💻 2.2 Code — Source Control & Standards
| Tool | Purpose |
|---|---|
| **Git** | Distributed version control (foundation of everything below) |
| **GitHub / GitLab / Bitbucket** | Git hosting, PR/MR reviews, code hosting |
| **Pre-commit / Husky** | Local hooks for lint/format before commit |
| **SonarQube** | Static code quality & security analysis |

### ⚙️ 2.3 Build & CI — Continuous Integration
| Tool | Purpose |
|---|---|
| **Jenkins** | Extensible, self-hosted CI/CD automation server |
| **GitHub Actions** | Native CI/CD in GitHub, YAML-based |
| **GitLab CI/CD** | Native pipelines in GitLab |
| **CircleCI / Travis CI** | Cloud-native CI platforms |
| **Maven / Gradle / npm / pip** | Build & dependency management per language |

### 🧪 2.4 Test — Quality Assurance
| Tool | Purpose |
|---|---|
| **JUnit / PyTest / Jest** | Unit testing frameworks |
| **Selenium / Cypress / Playwright** | UI/end-to-end testing |
| **Postman / Newman** | API testing |
| **JMeter / k6 / Locust** | Load & performance testing |
| **OWASP ZAP / Snyk / Trivy** | Security (SAST/DAST/SCA) scanning |

### 📦 2.5 Package & Artifact Management
| Tool | Purpose |
|---|---|
| **Docker** | Containerize applications |
| **Nexus / JFrog Artifactory** | Store build artifacts & container images |
| **Docker Hub / Amazon ECR / GCR** | Container image registries |

### 🚀 2.6 Release & Deploy — CD + Orchestration
| Tool | Purpose |
|---|---|
| **Kubernetes (K8s)** | Container orchestration at scale |
| **Helm** | Kubernetes package manager (templated manifests) |
| **ArgoCD / Flux** | GitOps — declarative continuous delivery to K8s |
| **Spinnaker** | Multi-cloud continuous delivery |
| **AWS CodeDeploy / Azure DevOps** | Cloud-native deployment services |

### 🏗️ 2.7 Infrastructure as Code (IaC)
| Tool | Purpose |
|---|---|
| **Terraform** | Cloud-agnostic infrastructure provisioning |
| **AWS CloudFormation** | AWS-native IaC |
| **Pulumi** | IaC using general-purpose languages |
| **Ansible** | Configuration management & app deployment |
| **Chef / Puppet** | Configuration management (agent-based) |

### ☁️ 2.8 Cloud Platforms
| Tool | Purpose |
|---|---|
| **AWS** | Compute, storage, networking, managed services |
| **Microsoft Azure** | Enterprise cloud ecosystem |
| **Google Cloud Platform (GCP)** | Data/AI-friendly cloud services |

### 📊 2.9 Operate & Monitor — Observability
| Tool | Purpose |
|---|---|
| **Prometheus** | Metrics collection & alerting |
| **Grafana** | Metrics visualization dashboards |
| **ELK Stack (Elasticsearch, Logstash, Kibana)** | Centralized logging & search |
| **Datadog / New Relic** | Full-stack SaaS observability (APM + logs + metrics) |
| **PagerDuty / Opsgenie** | Incident alerting & on-call management |

### 🔐 2.10 Security (DevSecOps — cross-cutting)
| Tool | Purpose |
|---|---|
| **HashiCorp Vault** | Secrets management |
| **Snyk / Trivy** | Vulnerability scanning (code, containers) |
| **OPA (Open Policy Agent)** | Policy-as-code enforcement |
| **AWS IAM / Azure AD** | Identity & access management |

---

## 🔗 3. Tool Connectivity — How They Talk to Each Other

```mermaid
flowchart TD
    subgraph Plan["📋 Plan"]
        Jira[Jira]
    end

    subgraph Code["💻 Code"]
        Git[Git Repo]
        GH[GitHub/GitLab]
    end

    subgraph CI["⚙️ Build & Test - CI"]
        Jenkins[Jenkins / GH Actions]
        Test[JUnit / Selenium / SonarQube]
    end

    subgraph Artifact["📦 Package"]
        Docker[Docker Build]
        Registry[Artifactory / ECR]
    end

    subgraph CD["🚀 Release & Deploy"]
        Argo[ArgoCD / Helm]
        K8s[Kubernetes Cluster]
    end

    subgraph IaC["🏗️ Infrastructure"]
        TF[Terraform]
        Ansible[Ansible]
    end

    subgraph Cloud["☁️ Cloud"]
        AWS[AWS / Azure / GCP]
    end

    subgraph Ops["📊 Operate & Monitor"]
        Prom[Prometheus]
        Graf[Grafana]
        ELK[ELK Stack]
        Pager[PagerDuty]
    end

    Jira -->|tickets link to| Git
    Git -->|push/PR| GH
    GH -->|webhook triggers| Jenkins
    Jenkins -->|runs| Test
    Test -->|pass| Docker
    Docker -->|push image| Registry
    Registry -->|pull image| Argo
    Argo -->|deploy| K8s
    TF -->|provisions| Cloud
    Ansible -->|configures| K8s
    K8s -->|hosted on| Cloud
    K8s -->|emits metrics| Prom
    K8s -->|emits logs| ELK
    Prom -->|visualized in| Graf
    Prom -->|alerts| Pager
    Pager -->|feedback loop| Jira

    style Jira fill:#4A90D9,color:#fff
    style Jenkins fill:#50C878,color:#fff
    style Docker fill:#50C878,color:#fff
    style Argo fill:#F5A623,color:#fff
    style K8s fill:#F5A623,color:#fff
    style Prom fill:#D0021B,color:#fff
    style Graf fill:#D0021B,color:#fff
```

---

## 🔁 4. A Typical CI/CD Pipeline, Step by Step

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Git as Git/GitHub
    participant CI as CI Server (Jenkins/Actions)
    participant Reg as Artifact Registry
    participant CD as CD Tool (ArgoCD)
    participant K8s as Kubernetes
    participant Mon as Monitoring

    Dev->>Git: git push (feature branch)
    Git->>CI: Webhook trigger
    CI->>CI: Lint + Unit Tests + SonarQube scan
    CI->>CI: Build Docker image
    CI->>Reg: Push image (tagged)
    Reg-->>CD: New image available
    CD->>K8s: Sync desired state (GitOps)
    K8s->>K8s: Rolling deployment
    K8s->>Mon: Stream metrics & logs
    Mon-->>Dev: Alert if anomaly detected
```

---

## 🎓 5. Suggested Learning Path

```mermaid
flowchart TD
    L1["Level 1: Foundations<br/>Linux, Networking, Git, Bash/Python scripting"] --> L2
    L2["Level 2: CI/CD Basics<br/>Jenkins or GitHub Actions, Docker"] --> L3
    L3["Level 3: Orchestration<br/>Kubernetes, Helm"] --> L4
    L4["Level 4: Infrastructure as Code<br/>Terraform, Ansible"] --> L5
    L5["Level 5: Cloud Platform<br/>Pick one deeply: AWS / Azure / GCP"] --> L6
    L6["Level 6: Observability & Security<br/>Prometheus, Grafana, ELK, Vault, DevSecOps"] --> L7
    L7["Level 7: GitOps & Advanced Delivery<br/>ArgoCD, Spinnaker, Service Mesh (Istio)"]

    style L1 fill:#4A90D9,color:#fff
    style L2 fill:#50C878,color:#fff
    style L3 fill:#50C878,color:#fff
    style L4 fill:#F5A623,color:#fff
    style L5 fill:#F5A623,color:#fff
    style L6 fill:#D0021B,color:#fff
    style L7 fill:#9013FE,color:#fff
```

---

## ⚡ 6. Quick Reference — One Tool per Category (Minimum Viable Toolkit)

| Category | Pick One to Start |
|---|---|
| 💻 Version Control | Git + GitHub |
| ⚙️ CI/CD | GitHub Actions |
| 🐳 Containers | Docker |
| ☸️ Orchestration | Kubernetes |
| 🏗️ IaC | Terraform |
| 🔧 Config Management | Ansible |
| 📊 Monitoring | Prometheus + Grafana |
| 📜 Logging | ELK Stack |
| 🔐 Secrets | HashiCorp Vault |
| ☁️ Cloud | AWS |

> **Tip:** Depth beats breadth. Master this "minimum viable toolkit" end-to-end (i.e., actually deploy a real app through the full pipeline) before branching into alternatives like GitLab CI, Chef, or Pulumi.

---

## 🎯 7. Key Takeaway

DevOps tools aren't isolated — they form a **pipeline of pipelines**:

```
Code → Build → Test → Package → Provision Infra → Deploy → Monitor → Feedback → Code (again)
```

Each tool hands off an artifact (code, image, manifest, metric) to the next stage, and the entire chain is automated end-to-end so releases become fast, repeatable, and low-risk.