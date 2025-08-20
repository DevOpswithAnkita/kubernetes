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
