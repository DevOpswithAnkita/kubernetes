# Kubernetes Pod Networking

After setting up your cluster, you must install a networking solution (CNI).  
Options include:

- Flannel  
- Weave Net  
- Calico  

Example (Flannel):
```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
