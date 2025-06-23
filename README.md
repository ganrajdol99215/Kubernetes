# 🚀 Kubernetes: Basic to Advanced Commands with Concepts & Networking

This README provides a complete overview of Kubernetes from basic to advanced commands, along with key concepts, pod communication, and networking diagrams.

---

## 📦 Kubernetes CLI Setup

```bash
kubectl version --client
kubectl config view
```
> Ensure `kubectl` is installed and configured to talk to a cluster (e.g., minikube, EKS, GKE).

---

## 🔰 Basic Commands

```bash
kubectl cluster-info                     # Show cluster info
kubectl get nodes                        # List all nodes
kubectl get pods                         # List all pods in default namespace
kubectl get all                          # List all resources
kubectl describe pod <pod-name>         # Pod details
kubectl logs <pod-name>                 # Show pod logs
```

---

## 📄 Working with YAML Manifests

```bash
kubectl apply -f <file>.yaml            # Apply a manifest
kubectl delete -f <file>.yaml           # Delete resources from manifest
kubectl create -f <file>.yaml           # Create resources
kubectl edit deployment <name>          # Edit a live deployment
```

---

## ⚙️ Workload Management

```bash
kubectl run nginx --image=nginx          # Create a single pod
kubectl expose pod nginx --port=80 --type=NodePort
kubectl scale deployment nginx --replicas=3
kubectl rollout restart deployment nginx
kubectl rollout status deployment nginx
```

---

## 📁 Namespaces

```bash
kubectl get namespaces
kubectl create namespace demo
kubectl delete namespace demo
kubectl get pods -n kube-system
```

---

## 📦 Deployments, ReplicaSets, Services

```bash
kubectl create deployment web --image=nginx
kubectl get deployments
kubectl get rs                         # ReplicaSets
kubectl get svc                        # Services
kubectl expose deployment web --port=80 --target-port=80 --type=ClusterIP
```

---

## 🐳 Pods & Networking Types

### 🧩 Intra-Pod Communication (Containers inside a Pod)

- Share localhost
- Common `localhost`, `127.0.0.1`, and volumes

### 🌐 Inter-Pod Communication (Same Node / Cross Node)

- Each pod gets its own IP.
- Communication via Services (ClusterIP, NodePort, LoadBalancer)
- DNS-based service discovery

### 🔄 Sidecar Containers (Logging, Proxies)

- Run inside the same Pod
- Share network + volumes with main container

---

## 📊 Useful Commands for Debugging

```bash
kubectl exec -it <pod> -- /bin/bash      # Access pod shell
kubectl port-forward svc/my-service 8080:80
kubectl top pod                          # View resource usage
kubectl get events                       # Get recent events
```

---

## 📥 ConfigMaps & Secrets

```bash
kubectl create configmap my-config --from-literal=env=dev
kubectl get configmaps
kubectl describe configmap my-config

kubectl create secret generic my-secret --from-literal=password=admin123
kubectl get secrets
kubectl describe secret my-secret
```

---

## 🔐 RBAC & Service Accounts

```bash
kubectl create sa my-serviceaccount
kubectl get serviceaccounts
kubectl create rolebinding read-pods --clusterrole=view --serviceaccount=default:my-serviceaccount --namespace=default
```

---

## 📦 Helm Basics

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx
helm list
helm uninstall my-nginx
```

---

## 📈 Monitoring & Logs

- Use `kubectl logs`, `kubectl top`, or tools like:
  - Prometheus + Grafana
  - ELK Stack
  - Cloud-native services (e.g., AWS CloudWatch)

---

## 🧠 Kubernetes Network Overview (Diagram)

```plaintext
                     +----------------------+
                     |    External Client   |
                     +----------+-----------+
                                |
                                v
                        LoadBalancer (Optional)
                                |
                                v
                     +----------------------+
                     |      NodePort        |
                     +----------+-----------+
                                |
                                v
                  +-------------+-------------+
                  |        ClusterIP Service  |
                  +-------------+-------------+
                                |
        +-----------------------+------------------------+
        |                                                |
+-------------------+                        +--------------------+
|     Pod A         | ---- eth0 (10.0.x.x)   |      Pod B         |
|   [Container(s)]  | <--- Same Network ---> |   [Container(s)]   |
+-------------------+                        +--------------------+

```

---

## ✅ Tips

- Always use namespaces for separation
- Use `kubectl explain <resource>` for field-level help
- Combine `kubectl` with `jq` or `yq` for advanced filtering
- Use GitOps or CI/CD pipelines for applying manifests

---

## 🧪 Testing Locally

Use Minikube or Kind for local Kubernetes environments:

```bash
minikube start
kubectl get nodes
```

---

📌 Author: DevOps by Ganraj  
📅 Last Updated: 2025  
📘 License: MIT  
