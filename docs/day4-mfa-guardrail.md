# Day 4 — MFA Guardrail with Explicit Deny

**Goal:** Enforce MFA across non-admin users. If MFA is not present, deny everything except the minimum needed to enroll MFA.

## Policy

`policies/DenyIfNoMFA.json` is attached to:
- `baseline-self-service`
- `dev-readonly`
- `s3-nino-eiam-writer`

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyAllRequestsIfNoMFA",
    "Effect": "Deny",
    "NotAction": [
      "iam:CreateVirtualMFADevice",
      "iam:EnableMFADevice",
      "iam:GetUser",
      "iam:ListMFADevices",
      "iam:ListVirtualMFADevices",
      "iam:ResyncMFADevice",
      "sts:GetSessionToken"
    ],
    "Resource": "*",
    "Condition": { "BoolIfExists": { "aws:MultiFactorAuthPresent": "false" } }
  }]
}
md
