# Deployment Runbook

## Overview
This runbook covers standard deployment procedures for the modernization-demo app running on GKE with Cloud SQL.

## Normal Deployment (Automated)
Every push to `main` triggers the GitHub Actions pipeline automatically.
No manual steps required.

## Manual Deployment (if pipeline is down)

### 1. Build and push image
```bash
cd ~/Documents/modernization-demo

docker buildx build \
  --platform linux/amd64 \
  -t gcr.io/modernization-demo/modernization-demo-app:v2 \
  --push .
```

### 2. Deploy to GKE
```bash
gcloud container clusters get-credentials modernization-demo \
  --zone us-central1-a

kubectl set image deployment/demo-app \
  app=gcr.io/modernization-demo/modernization-demo-app:v2

kubectl rollout status deployment/demo-app
```

### 3. Verify deployment
```bash
curl http://35.193.13.15/health
```
Expected response: `{"status":"ok"}`

## Verify Everything is Healthy
```bash
# check pods are running
kubectl get pods

# check service has external IP
kubectl get service demo-service

# check logs for errors
kubectl logs deployment/demo-app -c app --tail=50
```