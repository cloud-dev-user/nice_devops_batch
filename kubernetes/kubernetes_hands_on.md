---

# Kubernetes Hands-On Lab: Minimal Setup & Core Concepts

---

## Step 1: Install Kubernetes Using kubeadm (Single-node Cluster)

### Prerequisites

* Ubuntu 20.04+ VM or physical machine (2 CPUs, 2GB+ RAM)
* Disable swap:

  ```bash
  sudo swapoff -a
  sudo sed -i '/ swap / s/^/#/' /etc/fstab
  ```
* Install Docker or containerd (container runtime)

---

### Install kubeadm, kubelet, kubectl

```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https curl

curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl
```

---

### Initialize Kubernetes Master Node

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

---

### Configure kubectl for current user

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

### Deploy Pod Network (Flannel)

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

---

### (Optional) Allow scheduling pods on master node (for single-node)

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

---

## Step 2: Create a Pod Using YAML

`pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

Apply:

```bash
kubectl apply -f pod.yaml
kubectl get pods
```

Check logs:

```bash
kubectl logs nginx-pod
```

---

## Step 3: Create Deployment Using YAML

`deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

Apply:

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
```

---

## Step 4: Selectors & Labels

* Labels tag objects; selectors query/filter objects by labels.

Check pods with label:

```bash
kubectl get pods -l app=nginx
```

Describe deployment and verify selector:

```bash
kubectl describe deployment nginx-deployment
```

---

## Step 5: Working with Jobs

`job.yaml`:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi-job
spec:
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

Apply job:

```bash
kubectl apply -f job.yaml
kubectl get jobs
kubectl logs job/pi-job
```

---

## Step 6: ReplicaSets & Rolling Updates

### View ReplicaSets (managed by Deployment)

```bash
kubectl get rs
```

### Perform Rolling Update (update nginx image)

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.21.6
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
```

Rollback if needed:

```bash
kubectl rollout undo deployment/nginx-deployment
```

---

## Step 7: Scheduling Applications on Specific Nodes

Use `nodeSelector` in Pod spec to schedule pods on specific node labels.

Example: add this to Pod or Deployment spec template:

```yaml
spec:
  nodeSelector:
    kubernetes.io/hostname: your-node-name
```

Verify nodes and labels:

```bash
kubectl get nodes --show-labels
```

---

## Step 8: Create Services to Expose Pods

`service.yaml` (ClusterIP type):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Apply:

```bash
kubectl apply -f service.yaml
kubectl get svc
```

### For External Access: Use `NodePort` or `LoadBalancer`

Example NodePort:

```yaml
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

Access app via `http://<node-ip>:30080`

---

## Cleanup

```bash
kubectl delete -f pod.yaml
kubectl delete -f deployment.yaml
kubectl delete -f job.yaml
kubectl delete -f service.yaml
```

---


