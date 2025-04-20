
# 🌟 Local Kubernetes with KIND & NGINX

Welcome! This guide walks you through deploying **NGINX on a local Kubernetes cluster** using **KIND (Kubernetes IN Docker)**. You'll set up:

- 🚀 A single NGINX Pod
- 📦 A Deployment with 3 replicas of NGINX
- 🌐 A NodePort Service accessible on port `30001`

---

## 🔁 Step 1: Clone the Repository

```bash
git clone https://github.com/vivekdalsaniya12/kubernetes-kind.git
cd kubernetes-kind
```

---

## 🛠️ Step 2: Setup Essentials

### ➤ Install `kubectl`

```bash
sudo apt update
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### ➤ Install `kind`

```bash
# For x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

---

## 📦 Step 3: Create and Manage Your KIND Cluster

### ▶️ Create Cluster

```bash
kind create cluster --name my-cluster --config cluster.yml
```

### 🔍 Verify Cluster

```bash
kubectl cluster-info --context kind-my-cluster
```

### 📄 Show Clusters

```bash
kind get clusters
```

### ❌ Remove Cluster

```bash
kind delete cluster --name my-cluster
```

---

## 📂 Step 4: Files Inside This Repo

- `nginx-deployment.yaml` – 3 replica Deployment of NGINX
- `nginx-service.yaml` – NodePort Service on port `30001`

---

## 🚀 Step 5: Deploy to Cluster

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

---

## ✅ Step 6: Check Everything is Running

```bash
kubectl get nodes
kubectl get pods -o wide
kubectl get svc
```

Extra info:

```bash
kubectl describe deployment nginx-deployment
kubectl describe service nginx-service
```

---

## 🌐 Step 7: View NGINX in Action

Because `kind` runs containers, access NGINX using your local machine:

```bash
curl http://localhost:30001
```

Or via browser:

```
http://localhost:30001
```

---

## 🧹 Step 8: Clean Up Resources

To remove NGINX deployment and service:

```bash
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml
```

To delete everything:

```bash
kubectl delete -f .
```

---

## 📚 Useful Links

- 📘 [KIND Docs](https://kind.sigs.k8s.io/)
- 📘 [Kubernetes Docs](https://kubernetes.io/docs/)
