# Week 4 — Review & Wrap-Up (KMS + Least-Privilege)

## What I built
- **Day 1 – KMS CMK:** Created symmetric key, clean key policy (root/admin + app role), verified Encrypt/Decrypt.
- **Day 2 – S3 with SSE-KMS:** Bucket forces `aws:kms` with my CMK; uploads without headers are denied.
- **Day 4 – Policy Generation:** Generated least-privilege from CloudTrail for `gha-oidc-demo`, trimmed & validated.
- **Day 5 – KMS Alerts:** EventBridge → SNS email on sensitive KMS actions (PutKeyPolicy, DisableKey, ScheduleKeyDeletion, grants).
- **Day 6 – Access Reviews:** Service-Last-Access report for `gha-oidc-demo`; listed “never used” services to prune.

## Evidence (snippets)
```bash
# Key status + rotation
aws kms describe-key --key-id "$KEY_ID" --region "$REGION" \
  --query 'KeyMetadata.[Arn,Enabled,KeyManager]' --output table
aws kms get-key-rotation-status --key-id "$KEY_ID" --region "$REGION"

# S3 object shows SSE-KMS + correct key
aws s3api head-object --bucket <YOUR_BUCKET> --key ok.txt \
  --query '{SSE:ServerSideEncryption,KMS:SSEKMSKeyId}' --output json

# Alerts wired
aws events describe-rule --name kms-sensitive-actions --region "$REGION" \
  --query '{Name:Name,State:State}' --output table
