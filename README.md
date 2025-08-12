# 🚀 DevOps Portfolio: Serving HTML Pages with Nginx on Kubernetes 🎉

Welcome to my **DevOps portfolio project**!  
This project shows how to serve **custom HTML pages** using **Nginx** inside a **Kubernetes** cluster.  
All website files are managed as **ConfigMaps** and mounted inside the Nginx Pod — a neat way to handle static websites in Kubernetes! 🎨✨

---

## 🌟 Project Overview

| Feature                  | Description                                         | Emoji      |
|--------------------------|-----------------------------------------------------|------------|
| **Root page `/`**        | Friendly welcome with intro & skills                | 👋         |
| **Profile page `/hemanth`** | Detailed profile, skills, projects, and contacts    | 📄         |
| **Nginx ConfigMap**      | Contains HTML files + custom Nginx config           | 📦         |
| **Deployment**           | Runs Nginx pod with ConfigMap volume mount          | 🐳         |
| **ClusterIP Service**    | Internal service exposing Nginx on port 80          | 🔗         |
| **Port Forwarding**      | Forward local port to access app during dev/testing | 🔄         |

---

## ⚙️ How It Works

1. **ConfigMap** holds your static website files (HTML + Nginx config).  
2. **Deployment** mounts ConfigMap as a volume inside the Nginx Pod — Nginx serves your pages directly.  
3. **ClusterIP Service** exposes Nginx internally within the cluster.  
4. Use `kubectl port-forward` to access the website locally. Easy peasy! 🍋

---

## 📋 Prerequisites

- A running Kubernetes cluster ⛵  
- `kubectl` configured for your cluster ⚙️  
- Basic understanding of Kubernetes resources (ConfigMap, Deployment, Service) 📚  
- Official Nginx Docker image (no need to build your own!) 🐋  

---

## 🚧 Step-by-step Setup Guide

### 1️⃣ Create ConfigMap with HTML & Nginx config

```bash
kubectl create configmap nginx-config \
  --from-file=index.html=./index.html \
  --from-file=hemanth.html=./hemanth.html \
  --from-file=default.conf=./default.conf \
  -n your-namespace \
  --dry-run=client -o yaml > nginx-configmap.yaml
