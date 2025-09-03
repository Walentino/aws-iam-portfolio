# Week 5 — Day 3: SCP — Block IAM user access keys org-wide

## Goal
Prevent creation/rotation of IAM **user** access keys to enforce role-based access.

## SCP
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyAccessKeyCreationAndUpdate",
    "Effect": "Deny",
    "Action": ["iam:CreateAccessKey","iam:UpdateAccessKey"],
    "Resource": "*"
  }]
}

POLICY_NAME="Deny-IAM-AccessKeys"
POLICY_ID=$(aws organizations create-policy --type SERVICE_CONTROL_POLICY \
  --name "$POLICY_NAME" --description "Deny creating/updating IAM user access keys" \
  --content file://scp-deny-accesskeys.json --query 'Policy.PolicySummary.Id' --output text)

ROOT_ID=$(aws organizations list-roots --query 'Roots[0].Id' --output text)
aws organizations attach-policy --policy-id "$POLICY_ID" --target-id "$ROOT_ID"

aws organizations list-policies-for-target --target-id "$ROOT_ID" --filter SERVICE_CONTROL_POLICY \
  --query 'Policies[?Name==`Deny-IAM-AccessKeys`].[Name,Id]' --output table

aws organizations detach-policy --policy-id "$POLICY_ID" --target-id "$ROOT_ID"
aws organizations delete-policy --policy-id "$POLICY_ID"
