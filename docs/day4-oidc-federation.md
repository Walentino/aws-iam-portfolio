# Day 4 — OIDC Federation (GitHub Actions → AWS)

## Goal
Use GitHub Actions OIDC to assume an AWS role (no stored AWS keys).

## Steps
1) Create OIDC IdP (GitHub) in IAM
2) Create IAM role trusted by the IdP
3) Add a GitHub Actions workflow that assumes the role
4) Capture CLI evidence

## Evidence
### STS identity from the workflow

```json

{
  "UserId": "...:GitHubActions",
  "Account": "457664479040",
  "Arn": "arn:aws:sts::457664479040:assumed-role/gha-oidc-demo/GitHubActions"
}

