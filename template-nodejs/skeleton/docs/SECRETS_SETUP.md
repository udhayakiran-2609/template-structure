# GitHub Secrets Setup Guide

Every repo scaffolded from the Node.js Backstage template needs these
GitHub Secrets set to enable the CI/CD pipeline.

---

## Required Secrets

Go to: **GitHub Repo → Settings → Secrets and variables → Actions → New repository secret**

| Secret Name | Value | How to get it |
|-------------|-------|---------------|
| `GCP_PROJECT_ID` | Your GCP project ID | `gcloud config get-value project` |
| `GCP_SA_KEY` | Full JSON of service account key | See below |
| `BACKSTAGE_NAMESPACE` | Backstage namespace | Usually `default` |

---

## Creating the GCP_SA_KEY

```powershell
# Create the key file
gcloud iam service-accounts keys create sa-key.json `
  --iam-account=backstage-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com `
  --project=YOUR_PROJECT_ID

# Print the content (paste this as the secret value)
Get-Content sa-key.json

# Delete local file after copying!
Remove-Item sa-key.json
```

Paste the **entire JSON content** as the value of `GCP_SA_KEY`.

---

## Setting Secrets via GitHub CLI (faster for many repos)

```bash
# Install gh CLI if needed
# https://cli.github.com/

gh secret set GCP_PROJECT_ID --body "your-project-id" --repo ORG/REPO
gh secret set GCP_SA_KEY < sa-key.json --repo ORG/REPO
gh secret set BACKSTAGE_NAMESPACE --body "default" --repo ORG/REPO
```

---

## Pipeline Flow

```
Push to main
     │
     ▼
  [Tests] ──────────────────────────────┐
     │                                  │
     ▼                                  ▼
  [Build Docker]                  [TechDocs Publish]
     │                              → GCS Bucket
     ▼
  [Deploy Cloud Run]
     │
     ▼
  [Summary]
```

## Pipeline runs on

- ✅ Every push to `main` → full pipeline (test + build + deploy + techdocs)
- ✅ Every PR to `main` → tests only (no deploy)
