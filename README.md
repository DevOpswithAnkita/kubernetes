# Kubernetes Learning Repository

Welcome! 🚀  
This repository contains step-by-step commands, scripts, and examples to help you learn Kubernetes from scratch.

## 📂 Repository Structure
```
kubernetes/
│── README.md                 # Main introduction & usage guide
│── kubernetes_setup_commands.docx   # Word document with commands
│
├── docs/                     # Documentation files
│   ├── setup_guide.md        # Step-by-step cluster setup
│   ├── networking.md         # Pod networking (Flannel, Weave, Calico)
│   └── troubleshooting.md    # Common issues & fixes
│
├── scripts/                  # Useful scripts for automation
│   ├── master-setup.sh       # Automate master node setup
│   ├── worker-setup.sh       # Automate worker node setup
│   └── reset-cluster.sh      # Reset cluster script
│
├── manifests/                # Kubernetes YAML manifests
│   ├── nginx-deployment.yaml # Example Deployment
│   ├── service.yaml          # Example Service
│   └── namespace.yaml        # Example Namespace
│
├── examples/                 # Example apps for practice
│   ├── hello-world/          # Simple app deployment
│   └── guestbook/            # Multi-tier example app
│
└── images/                   # Screenshots/diagrams
```

## 📘 Setup Guide
👉 See full setup instructions in [docs/setup_guide.md](docs/setup_guide.md)
