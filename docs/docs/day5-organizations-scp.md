# Day 5 – Organizations & SCP Guardrails (concept-only)

## What SCPs are
- **Service Control Policies (SCPs)** are *org-level guardrails* that set the **maximum** permissions an account (all its users/roles) can ever have.
- They **do not grant** permissions; IAM still grants. SCPs only **limit** what IAM *could* allow.

## Why use them
- Enforce global rules across all accounts (e.g., “No public S3 buckets”, “No IAM user creation”, “Only specific regions”).
- Central control by the Org management account/security team.

## How they evaluate (mental model)
1. **Org SCPs** (root → OU → account) determine the **max-allow**.
2. **IAM policies** (identity/resource/session/boundary) must also allow.
3. If **any** layer denies → the action is denied.
   - SCP explicit Deny beats everything.

## Typical patterns
- Deny actions except when caller is a break-glass role:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [{
      "Sid": "DenyS3PutUnlessBreakGlass",
      "Effect": "Deny",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "StringNotLike": {
          "aws:PrincipalArn": "arn:aws:iam::*:role/BreakGlass"
        }
      }
    }]
  }

### Region restrictions (example SCP)

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {
        "aws:RequestedRegion": ["us-east-1","us-west-2"]
      }
    }
  }]
}
