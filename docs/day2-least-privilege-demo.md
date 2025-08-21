# Day 2 — Group-based Provisioning & Scoped S3 Write

**Goal:** Provision IAM via **groups**, then enforce **least privilege** by granting write to a single S3 bucket.

## Groups
- `baseline-self-service` → AWS managed: **IAMUserChangePassword**
- `dev-readonly` → AWS managed: **ReadOnlyAccess**
- `s3-nino-eiam-writer` → customer-managed: **S3Write-nino-iam-demo-eiam**

## Users (test matrix)
- `alice-readonly` → baseline + readonly (list/download; upload fails)
- `ben-s3writer` → baseline + scoped S3 writer (upload/download OK in target bucket; S3 bucket list may show AccessDenied)
- `cara-baseline` → baseline only (no S3)
- `nino-dev-demo` → initial testing identity

## Expected Results
- Upload to **nino-iam-demo-eiam** only works for users in `s3-nino-eiam-writer`.
- Bucket listing page may fail unless you add this convenience permission:
```json
{
  "Sid": "AllowListAllBucketsForConsole",
  "Effect": "Allow",
  "Action": "s3:ListAllMyBuckets",
  "Resource": "*"
}
