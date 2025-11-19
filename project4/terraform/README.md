
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

<img width="1898" height="999" alt="Screenshot 2025-11-19 162451" src="https://github.com/user-attachments/assets/f7822bb4-d7de-439b-ab63-a4bb23eb2f73" />


---
## setting the alert for cpu usage with threshold 40% 

<img width="1913" height="1000" alt="Screenshot 2025-11-19 180351" src="https://github.com/user-attachments/assets/fc823584-cef1-4176-b3be-25ea7a5aa33b" />


---
---
## after making stress on the instance by stress command 
![WhatsApp Image 2025-11-19 at 18 06 27_4a678625](https://github.com/user-attachments/assets/9350b399-8b98-4e52-ae71-7ce28d82c219)

---

## but how the telegram bot create as contact point for your alert 



📡 Telegram Contact Point Setup in Grafana

This guide explains how to configure Telegram alerts in Grafana step-by-step with simple instructions and helpful emojis.

🚀 Step 1 — Create a Telegram Bot

Open Telegram and search “BotFather” 🔍

Send: /start ▶️

Create a new bot using: /newbot 🤖

Provide a bot name and username

BotFather will generate your:
👉 Bot Token (keep it safe) 🔐

Example: 8572456242:AAHWFNHrPI2JX_9yF8R9ipREtdTynUFHxwI

🆔 Step 2 — Get Your Telegram Chat ID

Open Telegram → search:
@userinfobot or @chatid_echo_bot 👤

Start the bot

It will display your:
👉 Chat ID

Example: 7335043

🧪 Step 3 — Test Your Bot (Optional but Recommended)

Test sending a message using this API URL in your browser:

https://api.telegram.org/bot<BOT_TOKEN>/sendMessage?chat_id=<CHAT_ID>&text=Hello


Example:

https://api.telegram.org/bot8572456242:AAH.../sendMessage?chat_id=7735043&text=Hello


✔️ If you receive the message in Telegram → token & chat ID are valid.

📡 Step 4 — Create Telegram Contact Point in Grafana

Open Grafana

Go to:
Alerting → Contact Points → + Add contact point ➕

Name the contact point:

Telegram-Alerts


Choose Telegram as the contact method

Fill in the following fields:

Field	Description
🔐 Bot Token	Your Telegram bot token
🆔 Chat ID	Your Telegram chat ID
🎉 Done!

You have successfully created a Telegram contact point in Grafana.
Your alerts will now be delivered directly to Telegram.
