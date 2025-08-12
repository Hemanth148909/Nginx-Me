DevOps Portfolio - Serving HTML Pages with Nginx on Kubernetes
This project demonstrates how to serve custom HTML pages using Nginx deployed on a Kubernetes cluster. The HTML pages are stored in ConfigMaps and mounted as volumes inside the Nginx pod. The service is exposed internally via a ClusterIP, and you can access the pages using port forwarding for local testing.

Project Structure
index.html — Root page (/): Welcome page with a brief intro and skills.

hemanth.html — Detailed profile page (/hemanth): About me, skills, projects, and contact info.

Nginx ConfigMap — Contains the HTML files and Nginx config to serve both pages correctly.

Deployment manifest — Deploys Nginx with mounted ConfigMap as a volume.

Service manifest — Exposes Nginx with a ClusterIP service.

Port forwarding — Forwards local port to Nginx service for easy access.

How It Works
ConfigMap contains your static website files (HTML and nginx configuration).

Deployment mounts the ConfigMap as a volume inside the Nginx pod, so Nginx serves your pages from there.

ClusterIP Service exposes the Nginx pod inside the cluster on a stable IP and port.

You use kubectl port-forward to forward the service port to your local machine for accessing the site.

Prerequisites
Kubernetes cluster up and running

kubectl configured to access your cluster

Basic knowledge of Kubernetes resources (ConfigMap, Deployment, Service)

Nginx Docker image (official)

Step-by-step Setup
1. Create ConfigMap with HTML and Nginx config
bash
Copy
Edit
kubectl create configmap nginx-config \
  --from-file=index.html=./index.html \
  --from-file=hemanth.html=./hemanth.html \
  --from-file=default.conf=./default.conf \
  -n your-namespace \
  --dry-run=client -o yaml > nginx-configmap.yaml
default.conf should configure Nginx to serve / from index.html and /hemanth from hemanth.html.

2. Deploy Nginx with ConfigMap mounted as volume
Apply your deployment.yaml manifest that:

Creates a Pod running Nginx

Mounts the ConfigMap volume at /usr/share/nginx/html (or wherever Nginx expects files)

Uses the custom default.conf as Nginx config

3. Create ClusterIP Service
Apply your service.yaml manifest to expose Nginx internally on port 80.

4. Access the app locally via port-forwarding
Run this command to forward port 8080 on your machine to the service’s port 80:

bash
Copy
Edit
kubectl port-forward svc/nginx-service 8080:80 -n your-namespace
Now open your browser and visit:

http://localhost:8080/ for root page

http://localhost:8080/hemanth for your profile page

Example Nginx default.conf
nginx
Copy
Edit
server {
  listen 80;

  location = / {
    root /usr/share/nginx/html;
    index index.html;
  }

  location = /hemanth {
    root /usr/share/nginx/html;
    index hemanth.html;
  }
}
Notes
You can update your HTML files by editing the ConfigMap and redeploying.

This setup is perfect for simple static sites and learning Kubernetes ConfigMaps and volume mounts.

For production, consider using Ingress or LoadBalancer service for external access.

Contact
If you want to connect or learn more about my DevOps journey:

LinkedIn: https://linkedin.com/in/hemanth

GitHub: https://github.com/hemanth

