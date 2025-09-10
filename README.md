# Always Free Oracle Infrastructure

This project documents the setup and usage of Oracle Cloud's **Always Free Tier** resources.  
The infrastructure is optimized for CI/CD, code quality, artifact storage, and security testing.  

---

## 📦 Resources

- **Total allocation**  
  - **RAM**: 24 GB  
  - **vCPU**: 4  
  - **Storage**: 200 GB  

- **VM Division**  
  | VM  | RAM   | vCPU | Boot Drive | Block Storage | Purpose
   
  | VM1 | 6 GB  | 1    | 30 GB      | -             | Nginx + WireGuard (reverse proxy + VPN)
  
  | VM2 | 6 GB  | 1    | 30 GB      | -             | SonarQube + Nexus
  
  | VM3 | 12 GB | 2    | 60 GB      | 80 GB         | Security stack (Sliver C2, Wazuh, Wine, Shellter) 

---

## 🛠️ Software Stack

- **CI/CD & Dev Tools**
  - Jenkins (automation server)
  - SonarQube (code quality)
  - Nexus (artifact repository)
  - 
## Offloading Jenkins job to GitHub CICD instead to reduce RAM consumption.

- **CI/CD**
  - **GitHub Actions** (replaces Jenkins)  

- **Code Quality & Artifacts**
  - SonarQube (hosted on VM2)  
  - Nexus Repository (hosted on VM2)  

- **Web & Networking**
  - Nginx (reverse proxy & TLS termination, VM1)  
  - WireGuard (firewall + VPN, VM1)  

- **Security Tools**
  - Sliver C2  
  - Wazuh Manager (SIEM)  
  - Wine + Shellter  

---

## 🚀 Setup Guidelines

### 1. Provision Oracle VMs
- Create 3 VMs as per the resource split above.  
- Configure **security lists** in Oracle Cloud:  
  - Allow HTTPS (443) for Nginx.  
  - Allow WireGuard (51820/UDP by default).  
  - Restrict SonarQube + Nexus ports to VPN-only access.  

### 2. Install Core Software
- **VM1 (Nginx + WireGuard)**  
  - Nginx: reverse proxy with Let’s Encrypt TLS certs.  
  - WireGuard: secure VPN access for private services.  

- **VM2 (SonarQube + Nexus)**  
  - Install Docker + Docker Compose.  
  - Run SonarQube + Nexus as containers.  

- **VM3 (Security Stack)**  
  - Deploy Sliver, Wazuh, Wine, and Shellter.  
  - Configure Wazuh to collect logs from VM1 + VM2.
  - App Deployment Tomcat (VM3)
  - Ansible (automation)  

### 3. GitHub Actions CI/CD
Instead of Jenkins, pipelines run **on GitHub Actions**:  

- **Build & Test**  
  - Code compiled and tested directly in GitHub runners.  

- **Static Analysis (SonarQube)**  
  - GitHub Actions sends analysis results to SonarQube:
  - SonarQube Quality Gate result using SonarSource/sonarcloud-github-action@master

- **Artifact Upload (Nexus)**  
  - Artifacts stored in Oracle-hosted Nexus:  

## Directory Structure

```plaintext
always-free-oracle/
├── .github/workflows/build.yml         # GitHub Actions pipelines
│   └── build.yml
├── ansible/
│   ├── site.yml                       # Master playbook (runs all VMs together)
│   ├── provision-vm1.yml              # Playbook for VM1 (Nginx + WireGuard)
│   ├── provision-vm2.yml              # Playbook for VM2 (SonarQube + Nexus)
│   ├── deploy-vm3.yml                 # Playbook for VM3 (Tomcat deployment)
│   ├── inventory.ini                  # Oracle Cloud VM inventory
│   ├── group_vars/
│   │   ├── vm1.yml                    # Variables for VM1
│   │   ├── vm2.yml                    # Variables for VM2
│   │   └── vm3.yml                    # Variables for VM3
│   └── configs/
│       ├── nginx-reverse-proxy.conf.j2    # Nginx reverse proxy template
│       └── docker-compose-sonarnexus.yml.j2 # Docker Compose for SonarQube + Nexus
|
├── docs/                               # Documentation & diagrams
└── README.md                           # This file
