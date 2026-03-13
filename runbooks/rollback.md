# Rollback Runbook

## When to Roll Back
- Health check returning non-200
- Error rate spike after a deployment
- CrashLoopBackOff on pods after deploy

## Step 1 — Roll back the deployment
```bash
# rolls back to the previous working image instantly
kubectl rollout undo deployment/demo-app

# watch pods come back up
kubectl get pods -w
```

## Step 2 — Verify rollback worked
```bash
# check health endpoint
curl http://35.193.13.15/health

# check pods are stable
kubectl get pods

# confirm which image is now running
kubectl describe deployment demo-app | grep Image
```

## Step 3 — Roll back the database (if schema changed)
```bash
# go to project folder
cd ~/Documents/modernization-demo

# roll back one migration
alembic downgrade -1

# verify the schema is correct
alembic current
```

## Step 4 — Notify and document
- Note the time of rollback
- Note which commit caused the issue
- Open a GitHub issue describing what went wrong

## Contacts
- GCP Console: https://console.cloud.google.com
- GitHub Actions: https://github.com/armansingh21/modernization-demo/actions
- Cluster: modernization-demo, zone us-central1-a