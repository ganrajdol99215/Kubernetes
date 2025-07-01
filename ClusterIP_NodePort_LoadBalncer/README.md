# 📘 Kubernetes Networking & Service Types Explained (Simple Terms)

This guide explains in plain language:

✅ How Kubernetes assigns IPs  
✅ What CIDRs are  
✅ How ClusterIP, NodePort, and LoadBalancer services work  

---

## 🌿 1️⃣ How IP Addresses Are Assigned in Kubernetes

**Kubernetes has different kinds of IP addresses:**

1. **Node IPs**
   - Each worker node has an IP on your cloud or network.
   - Example:
     - Node1: 192.168.1.10
     - Node2: 192.168.1.11

2. **Pod IPs**
   - Each pod gets a unique IP from the **Pod CIDR range**.
   - Example Pod CIDR: `10.244.0.0/16`
   - Example pod IPs:
     - PodA: 10.244.1.5
     - PodB: 10.244.1.6

3. **Service IPs (ClusterIP)**
   - Each Service gets a **virtual IP** from the **Service CIDR range**.
   - Example Service CIDR: `10.96.0.0/12`
   - Example ClusterIP: 10.96.33.8

✅ **Important:**
- Pod IPs are **internal only**.
- Service IPs (ClusterIP) are **internal only** unless you create NodePort or LoadBalancer.

---

## 📦 2️⃣ What Is CIDR?

**CIDR** = a way to define an IP range.

- **Pod CIDR:** Range for all pod IPs.
- **Service CIDR:** Range for all ClusterIPs.

**Example:**
```
Pod CIDR = 10.244.0.0/16
Service CIDR = 10.96.0.0/12
```

Kubernetes picks IPs from these ranges automatically.

---

## 🔵 3️⃣ ClusterIP Service (Internal Access)

✅ **Default service type.**

- Gives a **stable internal IP** to reach your pods.
- No external access.

**How it works:**
1. You create a Deployment (pods).
2. You create a ClusterIP Service.
3. Kubernetes assigns an internal IP (e.g., 10.96.33.8).
4. Other pods can connect via that IP or DNS name.

**Example YAML:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

✅ To access:
```
http://my-app:80
```

✅ **Use Case:**
- Internal communication between pods.

---

## 🟢 4️⃣ NodePort Service (Basic External Access)

✅ **Makes your service reachable via the node’s IP and a high port number.**

- Kubernetes picks a port in range **30000–32767**.
- You can connect to `NodeIP:NodePort` externally.

**How it works:**
1. Create a NodePort Service.
2. Kubernetes assigns a port, e.g., 30080.
3. Traffic comes to node’s IP on that port and forwards to pods.

**Example YAML:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-nodeport
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30080
```

✅ To access:
```
http://<NodeIP>:30080
```

✅ **Use Case:**
- Quick external testing without a load balancer.

---

## 🟣 5️⃣ LoadBalancer Service (Public Access)

✅ **Easiest way to get a public IP in the cloud.**

- Creates an external load balancer (AWS ELB, Azure LB, etc.).
- Allocates a public cloud IP automatically.
- Routes traffic to your pods.

**How it works:**
1. Create a LoadBalancer Service.
2. Cloud provider assigns a public IP.
3. Requests hit that IP and are load-balanced to pods.

**Example YAML:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-lb
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

✅ To access:
```
http://<PublicIP>
```

✅ **Use Case:**
- Production apps needing public access.

---

## 🛠️ 6️⃣ How IP Assignments Happen

**Step by step:**

✅ **Pod IP assignment:**
- Kubelet on each node talks to the CNI plugin.
- CNI allocates an IP from the pod CIDR for that node.

✅ **Service IP assignment:**
- Kubernetes control plane picks a free IP from the Service CIDR.
- That becomes the ClusterIP.

✅ **NodePort assignment:**
- Kubernetes picks a port in 30000–32767 (or you specify it).
- Nodes listen on that port.

✅ **LoadBalancer assignment:**
- Cloud provider assigns a public IP and sets up a load balancer.

---

## ✨ Summary Table

| Service Type     | Internal IP (ClusterIP) | External IP        | How to Access                      |
|------------------|-------------------------|---------------------|-------------------------------------|
| ClusterIP        | Yes                     | No                  | Inside cluster only                |
| NodePort         | Yes                     | Node IP:NodePort    | Outside via node IP and port       |
| LoadBalancer     | Yes                     | Public IP           | Outside via cloud load balancer    |

---

## 🎯 Quick Example Flow

1️⃣ You create a Deployment (pods get Pod IPs).  
2️⃣ You create a Service:  
   - ClusterIP → Internal-only  
   - NodePort → NodeIP:Port  
   - LoadBalancer → Public IP  

---

✅ **Tips for Beginners:**

- Always start with ClusterIP for internal communication.
- Use NodePort or LoadBalancer for external access.
- Know your Pod CIDR and Service CIDR ranges.
- Use `kubectl get svc` to see assigned IPs.

---

🙌 Thanks for reading!  
Feel free to adapt this README to your projects or learning materials.