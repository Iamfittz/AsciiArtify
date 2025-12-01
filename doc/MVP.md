## 1. Check the possibility of automatic restarting through ArgoCD when making changes to the Git repository.

Components:
- Kubernetes cluster (k3d for PoC)
- ArgoCD for synchronization
- Simple service (nginx) as a placeholder for the upcoming AsciiArtify API/UI
- GitHub repository with k8s

## 2. Flow

1. The change takes away from the `main` repository.
2. ArgoCD installs the repository and automatically compiles: 
- synchronization of manifestos 
- storage of changes in Kubernetes 
- self-renewal 

## 3. Setting up ArgoCD Application

### Basic parameters
- **repoURL:** `https://github.com/Iamfittz/AsciiArtify`
- **path:** `k8s`
- **namespace:** `asciiartify`
- **syncPolicy:** automatic (prune + selfHeal)


## 4. Kubernetes Manifests

The `k8s/` directory has:
- `deployment.yaml` — Deployment (nginx placeholder)
- `service.yaml` - Service (ClusterIP)
- other basic resources (upon request)

Container stock (for MVP):
```yaml
containers: 
- name: asciiartify 
image:nginx:latest 
ports: 
- containerPort: 8080
```

## 5. Automatic demonstration

### Step 1 - change in image from Deployment
```yaml
image: nginx:1.27
```

### Step 2 - commit + push in main
ArgoCD automatically detects changes.

### Step 3 - result
- Pod restarts
- ReplicaSet is being updated
- Start to switch to **Synced** + **Healthy**
- Many manual `kubectl` commands are not vikorista

## 6. Cluster status after synchronization

```bash
kubectl get pods -n asciiartify
```

An example of the result:
```
NAME READY STATUS RESTARTS AGE
asciiartify-56b8d96ff5-w6rlb 1/1 Running 0 1m
```