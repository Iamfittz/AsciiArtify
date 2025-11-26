## 1. Short Overview of the Tools

### **minikube**
A classic and simple way to run Kubernetes on a laptop.  
Supports different drivers (Docker, VirtualBox, Hyper-V).  
Good for beginners.

### **kind**
Runs Kubernetes inside Docker containers.  
Often used in CI pipelines for testing manifests.  
Fast and lightweight.

### **k3d**
Runs lightweight Kubernetes (K3s) inside Docker containers.  
Extremely fast to create and delete clusters.  
Great for everyday local development.

---

## 2. Pros and Cons (short summary)

### **minikube**
**+** lots of documentation, addons, easy to start  
**–** can be heavy and slow, especially with VM drivers

### **kind**
**+** very fast, ideal for CI, easy to create multiple clusters  
**–** fully depends on Docker

### **k3d**
**+** fastest startup, minimal resource usage, great for microservices  
**–** depends on Docker; runs K3s (but fully compatible with Kubernetes API)

---

## 3. Comparison Table

| Criteria              | minikube         | kind           | k3d              |
|-----------------------|------------------|----------------|------------------|
| Startup speed         | medium           | fast           | very fast        |
| Resource usage        | higher           | low            | very low         |
| Depends on Docker     | optional         | yes            | yes              |
| Best for              | beginners        | CI / dev       | local dev        |

---

## 4. Short Demo (k3d example)

k3d was selected for practical testing because it is the fastest and easiest
to use for a quick Proof-of-Concept.

### 1. Create a cluster

```bash
k3d cluster create asciiartify --port "8080:80@loadbalancer"
kubectl get nodes
```
### 2. Deploy a simple application
hello.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
        - name: hello
          image: nginxdemos/hello
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  selector:
    app: hello
  ports:
    - port: 80
And apply it 
```bash
kubectl apply -f hello.yaml
kubectl get pods 
```

### 3. Run the app localy and open the browser
```bash
kubectl port-forward svc/hello 8081:80

http://localhost:8081
```