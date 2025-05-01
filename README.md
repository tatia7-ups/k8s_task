# Legal Term API Deployment Guide

## Overview
This project deploys a Python REST API (`legal-term-api`) to a Kubernetes cluster using Minikube for local testing. It includes a Dockerfile, Kubernetes manifests, and a GitHub Actions CI/CD pipeline.

## Prerequisites
- Docker
- Minikube
- kubectl
- GitHub account
- Docker Hub account

## Deployment Instructions
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/tatia7-ups/k8s_task
   cd k8s_task


Build and Push the Docker Image (Manual):
docker build -t tatia7-ups/legal-term-api:latest .
docker push tatia7-ups/legal-term-api:latest


Start Minikube:
minikube start
minikube addons enable ingress


Apply Kubernetes Manifests:
kubectl apply -f k8s/


Access the Application:

Add to /etc/hosts: echo "$(minikube ip) legal-term-api.local" | sudo tee -a /etc/hosts
Test: curl http://legal-term-api.local/docs or open http://legal-term-api.local/docs.



CI/CD Pipeline

The GitHub Actions workflow (.github/workflows/deploy.yml) builds and pushes the Docker image to Docker Hub on pushes to master.
It updates k8s/deployment.yaml with the new image tag and commits the change.
For local testing, pull and apply the updated manifests:git pull origin master
kubectl apply -f k8s/


In production, a cluster would pull from Docker Hub and apply manifests automatically.

Monitoring

Pod Status: kubectl get pods
Logs: kubectl logs -l app=legal-term-api
Service: kubectl get services
Ingress: kubectl get ingress
HPA: kubectl get hpa
Health Checks: Liveness/readiness probes on /health.

Security

Container runs as a non-root user (in Dockerfile).
No hardcoded secrets; CI/CD uses GitHub Secrets.
Resource limits prevent resource exhaustion.

Scalability

HorizontalPodAutoscaler scales pods (2–10) based on 70% CPU usage.
Multiple replicas ensure resilience.

Notes

Local testing uses Minikube with local images for cost-free development.
Assumes /health endpoint; adjust probes if the application uses a different health check.
CI/CD pipeline automates image builds and manifest updates, with manual application for local Minikube testing.




