# Always Free Oracle Infrastructure

This project documents the setup and usage of Oracle Cloud's **Always Free Tier** resources.  
The infrastructure is optimized for CI/CD, code quality, artifact storage, and security testing. 

---

## 🔹 Resource Breakdown per VM

### VM1: Wazuh Manager (via Docker Compose)
- **vCPU:** 1  
- **RAM:** 1 GB (limit container to 512 MB, reserve 256 MB)  
- **Disk:** 50 GB  

⚠️ Wazuh Manager alone can run here, but avoid running **Wazuh Indexer** (too heavy for this VM).

---

### VM2: Elasticsearch + Kibana
- **vCPU:** 1  
- **RAM:** 1 GB (split heap between Elasticsearch + Kibana)  
- **Disk:** 50 GB (logs + Elasticsearch data)  

⚠️ Use **Elasticsearch 7.10** (lighter than newer versions).  
Heap size should be capped at **256 MB**.

---

### VM3: Nginx + Fail2Ban + WireGuard
- **vCPU:** 1  
- **RAM:** 1 GB  
- **Disk:** 50 GB  

Notes:  
- WireGuard in a container is recommended — keeps footprint small.  
- Fail2Ban uses Python but is lightweight.

---

### VM4: Sonatype Nexus (for GitHub artifacts)
- **vCPU:** 1  
- **RAM:** 1 GB  
- **Disk:** 50 GB  

⚠️ Nexus can be heavy — JVM tuning required:  
- Set `-Xms256m -Xmx512m`

If Nexus proves too heavy, consider alternatives like **Harbor** or **Gitea Packages**.

---

## 🔹 Extra Optimizations
- Enable **2 GB swap** on each VM to reduce crashes during memory spikes.  
- Monitor container usage with:  
  ```bash
  docker stats
  ```
- Offload and rotate logs aggressively (use external storage if possible).  
- For artifact management, consider **lighter alternatives** if Nexus consumes too many resources.

---
