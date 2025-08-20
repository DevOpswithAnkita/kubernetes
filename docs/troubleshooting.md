# Troubleshooting Kubernetes Setup

## Common Issues

1. **Nodes show as NotReady**
   - Check if CNI plugin (Flannel/Calico) is installed.
   - Run `kubectl get pods -n kube-system` to debug.

2. **kubeadm join fails**
   - Ensure token is valid: `kubeadm token create --print-join-command`.

3. **Pods stuck in ContainerCreating**
   - Verify networking (CNI) is installed and running.
