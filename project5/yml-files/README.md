## 🧾 Project Source Code (Mono to Microservice Conversion)

This section documents all key files used in the mono-to-microservice architecture project including Dockerfiles, Kubernetes manifests, and app source code.

---

### ===== Dockerfile.flask =====
```dockerfile
FROM python:3.10-alpine
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

---

### ===== Dockerfile.node =====
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "server.js"]
```

---

### ===== app.py =====
```python
from flask import Flask
app = Flask(__name__)

@app.route('/health')
def health():
    return "Product service is running"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

### ===== server.js =====
```javascript
const express = require('express');
const app = express();

app.get('/health', (req, res) => res.send('User service is running'));

app.listen(3000, () => console.log('User service listening on port 3000'));
```

---

### ===== package.json =====
```json
{
  "name": "user-service",
  "version": "1.0.0",
  "description": "A simple User microservice",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "author": "Your Name",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.4"
  }
}
```

---

### ===== requirements.txt =====
```txt
Flask
```

---

### ===== product-deployement.yml =====
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: product-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: product
  template:
    metadata:
      labels:
        app: product
    spec:
      containers:
      - name: product
        image: parikshit1212/flaskimg:latest
        ports:
        - containerPort: 5000
        volumeMounts:
        - name: product-storage
          mountPath: /data
      volumes:
      - name: product-storage
        persistentVolumeClaim:
          claimName: product-pvc
```

---

### ===== product-service.yml =====
```yaml
apiVersion: v1
kind: Service
metadata:
  name: product-service
spec:
  selector:
    app: product
  ports:
  - port: 5000
    targetPort: 5000
  type: NodePort
```

---

### ===== product-hpa.yml =====
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: product-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: product-deployment
  minReplicas: 1
  maxReplicas: 3
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

---

### ===== pv.yml =====
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: product-pv
spec:
  capacity:
    storage: 500Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

---

### ===== pvc.yml =====
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: product-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 200Mi
```

---

### ===== user-deployment.yml =====
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: user
  template:
    metadata:
      labels:
        app: user
    spec:
      containers:
      - name: user
        image: parikshit1212/nodeimg:latest
        ports:
        - containerPort: 3000
```

---

### ===== user-service.yml =====
```yaml
apiVersion: v1
kind: Service
metadata:
  name: user-service
spec:
  selector:
    app: user
  ports:
  - port: 3000
    targetPort: 3000
  type: NodePort
```

---

### ===== user-hpa.yml =====
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: user-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: user-deployment
  minReplicas: 1
  maxReplicas: 4
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

---

⭐ All these files are part of the full-stack containerized microservice app for **product** and **user** services using Flask + Node.js deployed on Kubernetes with Docker and HPA support.
