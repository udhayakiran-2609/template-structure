# Deployment

This service is automatically deployed to **Google Cloud Run** via GitHub Actions on every push to `main`.

## CI/CD Pipeline

```
Push to main
    │
    ▼
┌─────────┐
│  Tests  │ ── npm test + lint
└────┬────┘
     │
     ├──────────────────┐
     ▼                  ▼
┌─────────┐      ┌───────────┐
│  Build  │      │ TechDocs  │
│ Docker  │      │  Publish  │
└────┬────┘      └───────────┘
     │
     ▼
┌──────────┐
│  Deploy  │ ── Cloud Run
└──────────┘
```

## GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| `GCP_PROJECT_ID` | Your GCP project ID |
| `GCP_SA_KEY` | Service account JSON key |
| `BACKSTAGE_NAMESPACE` | Backstage namespace (usually `default`) |

## Manual Deploy

```bash
# Authenticate
gcloud auth login
gcloud config set project YOUR_PROJECT_ID

# Build and push
docker build -t us-central1-docker.pkg.dev/PROJECT_ID/backstage/${{ values.name }}:latest .
docker push us-central1-docker.pkg.dev/PROJECT_ID/backstage/${{ values.name }}:latest

# Deploy
gcloud run deploy ${{ values.name }} \
  --image us-central1-docker.pkg.dev/PROJECT_ID/backstage/${{ values.name }}:latest \
  --region us-central1 \
  --allow-unauthenticated
```

## Cloud Run Configuration

| Setting | Value |
|---------|-------|
| Region | us-central1 |
| Memory | 512Mi |
| CPU | 1 |
| Min instances | 0 |
| Max instances | 10 |
| Port | 8080 |
