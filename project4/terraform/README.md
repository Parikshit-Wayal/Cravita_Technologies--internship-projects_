
## terraform main.tf file (working code ) => took from terraform registry

## (while i am doing i get stuck at sometimes so) => by help of chatgpt

---

i set CPU usage (requests and limits) per pod to ensure efficient cluster resource management, avoid node overload, and prevent any single pod from consuming excessive resources. It helps with fair scheduling, prevents pod crashes due to resource starvation .

```
terraform {
  required_providers {
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.0"
    }
    helm = {
      source  = "hashicorp/helm"
      version = "~> 2.0"
    }
  }
}

provider "kubernetes" {
  config_path = "~/.kube/config"
}

provider "helm" {
  kubernetes {
    config_path = "~/.kube/config"
  }
}

# -----------------------------
# PROMETHEUS (correct keys for prometheus-community/prometheus)
# -----------------------------
resource "helm_release" "prometheus" {
  name             = "prometheus"
  repository       = "https://prometheus-community.github.io/helm-charts"
  chart            = "prometheus"
  namespace        = "monitoring"
  create_namespace = true

  # Disable unwanted components (correct keys)
  set {
    name  = "alertmanager.enabled"
    value = "false"
  }
  set {
    name  = "pushgateway.enabled"
    value = "false"
  }
  set {
    name  = "nodeExporter.enabled"
    value = "false"
  }
  set {
    name  = "kubeStateMetrics.enabled"
    value = "false"
  }

  # Expose Prometheus server via NodePort (correct path)
  set {
    name  = "server.service.type"
    value = "NodePort"
  }

  # Disable PVC for server
  set {
    name  = "server.persistentVolume.enabled"
    value = "false"
  }

  # Lightweight resources
  set {
    name  = "server.resources.requests.cpu"
    value = "100m"
  }
  set {
    name  = "server.resources.requests.memory"
    value = "256Mi"
  }
  set {
    name  = "server.resources.limits.cpu"
    value = "200m"
  }
  set {
    name  = "server.resources.limits.memory"
    value = "512Mi"
  }
}

# -----------------------------
# GRAFANA
# -----------------------------
resource "helm_release" "grafana" {
  name       = "grafana"
  repository = "https://grafana.github.io/helm-charts"
  chart      = "grafana"
  namespace  = "monitoring"
  depends_on = [helm_release.prometheus]
  timeout    = 600

  # Expose Grafana as NodePort
  set {
    name  = "service.type"
    value = "NodePort"
  }

  # Disable persistence
  set {
    name  = "persistence.enabled"
    value = "false"
  }

  # Lightweight resources
  set {
    name  = "resources.requests.cpu"
    value = "100m"
  }
  set {
    name  = "resources.requests.memory"
    value = "128Mi"
  }
  set {
    name  = "resources.limits.cpu"
    value = "200m"
  }
  set {
    name  = "resources.limits.memory"
    value = "256Mi"
  }
}




```
## like this K8s objects or Resources get made via terraform
---
<img width="952" height="552" alt="Screenshot 2025-11-19 151024" src="https://github.com/user-attachments/assets/2b97a1ec-9686-4bd5-8b7d-e89a849707d6" />

---

