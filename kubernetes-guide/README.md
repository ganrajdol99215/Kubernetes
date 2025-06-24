
# 🐳 Kubernetes in Lemon Terms - Beginner to Intermediate

This guide explains Kubernetes core resources—Pods, Services, Deployments, and Ingress—in very simple terms. YAML files are included for practical reference.

---

## 🔹 What’s What in Kubernetes?

| Real-World Analogy | Kubernetes Term | Description |
|--------------------|------------------|-------------|
| Apartment Room     | **Pod**          | Smallest unit, holds 1+ containers (apps). |
| Building           | **Node**         | Machine where Pods live (like an EC2 or VM). |
| Architect          | **Deployment**   | Decides how many rooms (Pods) are needed. |
| Door/Entrance      | **Service**      | Makes Pods reachable (inside/outside). |
| Street Address     | **Ingress**      | Routes external traffic via domain/path. |
| House Plan         | **YAML File**    | Instructions for Kubernetes to follow. |

---

## 📦 Pod - `pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: myapp
spec:
  containers:
    - name: mycontainer
      image: nginx
      ports:
        - containerPort: 80
```

**Explanation:**
- `labels`: Important for Services to match and connect.
- `containers.image`: Runs nginx app.

---

## 🛠 Deployment - `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: mycontainer
          image: nginx
          ports:
            - containerPort: 80
```

**Why use Deployment?**
- Manages Pods (replicas, rolling updates).
- Automatically replaces crashed Pods.

---

## 🌐 Service - `service-nodeport.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
```

### Types of Service

| Type        | Behavior |
|-------------|----------|
| `ClusterIP` | Default. Internal traffic only (Pod ↔ Pod). |
| `NodePort`  | Opens a port on each Node (e.g., 30000–32767). |
| `LoadBalancer` | For cloud. Assigns public IP and load balances traffic. |

---

## 🌍 Ingress - `ingress.yaml`

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myingress
spec:
  rules:
    - host: mysite.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: myservice
                port:
                  number: 80
```

**How Ingress Works:**
- Needs an Ingress Controller (like NGINX Ingress).
- Maps domain/path to services.

---

## 🔄 Connecting All Resources

```text
[Browser] --> [Ingress] --> [Service: NodePort/ClusterIP] --> [Deployment] --> [Pods] --> [Containers]
```

- Deployment creates Pods with `labels: app: myapp`.
- Service selects Pods with matching label.
- Ingress routes based on domain/path to Services.

---

## 📂 Files Included

- `pod.yaml`
- `deployment.yaml`
- `service-nodeport.yaml`
- `ingress.yaml`

Use with: `kubectl apply -f <filename>`


