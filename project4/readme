
# 🚀 Project 4: Deploy Prometheus & Grafana on Kubernetes with Terraform & Helm

A concise summary and documentation for my monitoring setup using **Infrastructure as Code (IaC)**, **Prometheus**, and **Grafana** on **Kubernetes**.

---

## 📌 Overview

This project demonstrates how I set up **end-to-end Kubernetes monitoring** using:

- 🔧 **Terraform** for infrastructure provisioning and automation  
- 📦 **Helm** for deploying Prometheus and Grafana  
- 💻 **AWS EC2** as the Kubernetes cluster substrate (1 Master, 1 Worker Node)  
- 🌐 **NodePort services** for external UI access  
- 📂 Managed everything inside a dedicated `monitoring` namespace for clarity and isolation  

---

## 🧱 Step-by-Step with Screenshots

### ✅ 1. Terraform Provisioning for Infrastructure  
Provisioned EC2 instances and set up Kubernetes using Terraform.  
<img src="./images/terraformdone.png" alt="Terraform Done" width="600"/>

---

### 📦 2. Installed Prometheus & Grafana using Helm  
Used Helm charts to deploy Prometheus and Grafana inside the `monitoring` namespace.  
<img src="./images/helm-kube.png" alt="Helm Kube Install" width="600"/>

---

### 🌐 3. Accessed Grafana UI via NodePort  
Exposed Grafana using NodePort and accessed it via EC2's public IP.  
<img src="./images/graf.png" alt="Grafana UI" width="600"/>

---

### 📊 4. Accessed Prometheus UI via NodePort  
Prometheus was also exposed via NodePort for direct access.  
<img src="./images/prom.png" alt="Prometheus UI" width="600"/>

---

### 🔗 5. Added Prometheus as Data Source in Grafana  
Configured Prometheus inside Grafana for data queries.  
<img src="./images/prom-Graf.png" alt="Prometheus in Grafana" width="600"/>

---

### 📈 6. Created CPU Usage Panel in Grafana  
Built dashboards to visualize **CPU usage per pod/service**.  
<img src="./images/node_cpu.png" alt="Node CPU" width="600"/>

---

### 🧩 7. Final Grafana Dashboard with Widgets  
Multiple panels for monitoring were added in a clean layout.  
<img src="./images/dasboardadded.png" alt="Final Dashboard" width="600"/>

---

## 🧰 Infrastructure & Tools Used

| Component    | Description                                      |
|--------------|--------------------------------------------------|
| 🖥️ EC2        | Kubernetes master and worker nodes (AWS)         |
| ☸️ Kubernetes | Cluster orchestration, pod/service management    |
| 🪄 Helm       | Simplifies deploying Prometheus & Grafana        |
| 📜 Terraform  | IaC - Full automation of provisioning & Helm     |
| 📡 Prometheus | Collects cluster and node metrics                |
| 📊 Grafana    | Visualizes data, creates dashboards & alerts     |

---

## 🔧 What I Did

- Provisioned EC2-based Kubernetes cluster using Terraform  
- Initialized `kubeadm` to form the cluster  
- Installed Helm and added official Prometheus & Grafana chart repositories  
- Wrote Terraform config with `helm_release` to automate deployment  
- Exposed both tools using NodePort for browser access  
- Logged into Grafana, added Prometheus as a source  
- Built dashboard to monitor **pod-level CPU usage**  

---

## ✅ Key Success Checks

- ✅ Prometheus & Grafana pods were running in `monitoring` namespace  
- ✅ UIs accessible via EC2’s public IP + NodePort  
- ✅ Prometheus metrics successfully visualized in Grafana  
- ✅ All provisioning repeatable using Terraform  

---

## 📚 What I Learned

- Using **Terraform + Helm** is a powerful way to automate and version monitoring stacks  
- NodePort is a simple and effective way to test UIs on cloud VMs  
- Hands-on with monitoring tools gave me deeper insights into cluster health and debugging  
- This setup enhances production-readiness for Kubernetes clusters  

---

⭐ *Cloud-native monitoring, automated with code – built, tested, and deployed by me!*
