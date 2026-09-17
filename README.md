# Akanni Emmanuel — Cloud & DevOps Portfolio

Personal portfolio for Akanni Emmanuel, a software engineer focused on AWS cloud infrastructure, Terraform, Kubernetes (Amazon EKS) and CI/CD.

## How it's hosted

| Layer | Service |
|---|---|
| Primary hosting | Amazon S3, served through Amazon CloudFront |
| Infrastructure | Terraform (kept in a separate infrastructure repository) |
| Delivery | GitHub Actions, authenticating to AWS with OIDC (no stored AWS keys) |
| Backup | Netlify deployment of the same site |

Every push to `main` syncs the site to S3 and invalidates the CloudFront cache.

## Repository contents

- `index.html`: the whole site in one self-contained file (styles, scripts and images inlined)
- `.github/workflows/deploy.yml`: the S3 and CloudFront deployment pipeline
- `netlify.toml`: configuration for the Netlify backup

## Pipeline setup

In **Settings > Secrets and variables > Actions > Variables**, add:

| Variable | Example |
|---|---|
| `AWS_DEPLOY_ROLE_ARN` | `arn:aws:iam::123456789012:role/portfolio-github-deploy` |
| `AWS_REGION` | `us-east-1` |
| `S3_BUCKET` | `akanni-portfolio-site` |
| `CLOUDFRONT_DISTRIBUTION_ID` | `E1ABCDEFGHIJK` |

The IAM role's trust policy should allow `token.actions.githubusercontent.com` and be restricted to this repository, for example `repo:harkanni/portfolio:ref:refs/heads/main`. The role needs `s3:ListBucket`, `s3:PutObject` and `s3:DeleteObject` on the bucket, plus `cloudfront:CreateInvalidation` on the distribution.

The workflow uses a GitHub environment named `production`. Create it under **Settings > Environments**, or remove the `environment` line.

## Netlify backup

Connect this repository in Netlify with no build command and `.` as the publish directory. `netlify.toml` already sets this.

## Run locally

Open `index.html` in a browser. There is no build step.
