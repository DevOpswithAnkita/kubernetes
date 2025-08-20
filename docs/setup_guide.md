# Kubernetes Cluster Setup Commands (Master + Worker Nodes)

## 1. Common Setup (Run on All Nodes)
```bash
sudo su
apt-get update
apt-get install -y apt-transport-https curl

# Install Docker
apt install docker.io -y
docker --version
systemctl start docker
systemctl enable docker

# Add Kubernetes signing key
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

# Add Kubernetes repo
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

# Update and install Kubernetes
apt-get update
apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

## If above commands are not working then go with Options to install Kubernetes on Ubuntu 24.04

## Option 1: Use official Kubernetes binaries

Instead of apt packages, you can download the binaries directly:

```bash
# Download kubeadm, kubelet, kubectl binaries
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubeadm"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubelet"

# Make them executable
chmod +x kubectl kubeadm kubelet

# Move them to /usr/local/bin
sudo mv kubectl kubeadm kubelet /usr/local/bin/

# Verify
kubectl version --client
kubeadm version
kubelet --version


This bypasses apt entirely and works on any Ubuntu version, including 24.04.
```

## Option 2: Use containerized Kubernetes (Kind or K3s)

K3s is lightweight and works well on new Ubuntu versions:
```bash
curl -sfL https://get.k3s.io | sh -
sudo k3s kubectl get nodes

```

Kind runs Kubernetes in Docker containers, perfect for testing:
```bash
sudo apt install -y docker.io
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.25.0/kind-linux-amd64
chmod +x ./kind
sudo mv kind /usr/local/bin/
kind create cluster

```
Recommendation: For Ubuntu 24.04, the binary installation or K3s is the simplest because the official apt repo doesn’t yet support “noble”.

## Create a kubelet systemd service manually

# Create a file /etc/systemd/system/kubelet.service:
```bash
sudo tee /etc/systemd/system/kubelet.service > /dev/null <<EOF
[Unit]
Description=kubelet: The Kubernetes Node Agent
Documentation=https://kubernetes.io/docs/
After=network.target
[Service]
ExecStart=/usr/local/bin/kubelet
Restart=always
StartLimitInterval=0
RestartSec=10
[Install]
WantedBy=multi-user.target
EOF
```bash
## Enable and start kubelet
```bash
sudo systemctl daemon-reload
sudo systemctl enable kubelet
sudo systemctl status kubelet
```

## 2. Master Node Setup
```bash
# Initialize Kubernetes Master
kubeadm init

# Setup kubeconfig for kubectl commands
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# Apply pod network (choose one)
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
# OR
kubectl apply -f https://raw.githubusercontent.com/coreos/weaveworks-k8s/master/weave-daemonset-k8s.yaml

# Get join command for worker nodes
kubeadm token create --print-join-command
```

## 3. Worker Node Setup
```bash
# Run the join command copied from master
kubeadm join <MASTER-IP>:6443 --token <TOKEN> \
--discovery-token-ca-cert-hash <HASH>
```

## 4. Verification (Run on Master)
```bash
# Check if nodes joined successfully
kubectl get nodes
```
