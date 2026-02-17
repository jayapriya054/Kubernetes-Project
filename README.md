_This project demonstrates how a Python Flask application is containerized with Docker, pushed to a registry, deployed on a local k3d Kubernetes cluster, and finally deployed on Google Kubernetes Engine (GKE) with autoscaling, security, storage, and ingress._


1. Created a Python Flask application (app.py) that listens on port 5000 and returns Hello, World!.

2. Containerized the application using Docker with the base image python:3.12-slim and exposed port 5000 inside the container.

3. Built and pushed the Docker image jayapriya054/frontend:latest to Docker Hub so it can be pulled by any Kubernetes cluster.

4. Created a local Kubernetes cluster named frontend-cluster using k3d, which runs Kubernetes inside Docker and allows testing the application in a real cluster environment before deploying to the cloud.

5. Configured persistent storage using:

     PersistentVolume: frontend-app-pv (10Gi, hostPath /tmp/frontend-data)

     PersistentVolumeClaim: frontend-app-pvc (8Gi, StorageClass: standard)

6. Implemented RBAC security by creating:

     ServiceAccount: developer-sa

     ClusterRole: frontend-app-clusterrole

     ClusterRoleBinding: frontend-app-clusterrolebinding

7. Deployed the application using a Kubernetes Deployment named frontend-app in namespace team1 with 3 replicas, pulling image jayapriya054/frontend:latest and mounting the PVC at /var/lib/store.

8. Exposed the app using a NodePort Service named frontend-app-service on:

     Port 80 → container port 5000

     NodePort 30080 (for local access in k3d)

9. Enabled automatic scaling using an HPA named frontend-app that scales from 1 to 10 pods when memory usage exceeds 70%.

10. Deployed the same manifests to Google Kubernetes Engine (GKE), used a GCE Ingress named frontend-ingress, and accessed the app using the external IP provided by Google Cloud Load Balancer.
