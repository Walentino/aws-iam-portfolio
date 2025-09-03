# Week 5 — Day 4: SCP — Protect CloudTrail

## Goal
Prevent org-wide CloudTrail tampering (Stop/Delete/Update/Create).

## SCP
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyCloudTrailTampering",
    "Effect": "Deny",
    "Action": [
      "cloudtrail:StopLogging",
      "cloudtrail:DeleteTrail",
      "cloudtrail:UpdateTrail",
      "cloudtrail:CreateTrail"
    ],
    "Resource": "*"
  }]
}

POLICY_NAME="Deny-CloudTrail-Tamper"
POLICY_ID=$(aws organizations create-policy --type SERVICE_CONTROL_POLICY \
  --name "$POLICY_NAME" --description "Deny CloudTrail Stop/Delete/Update/Create" \
  --content file://scp-deny-cloudtrail-tamper.json \
  --query 'Policy.PolicySummary.Id' --output text)

ROOT_ID=$(aws organizations list-roots --query 'Roots[0].Id' --output text)
aws organizations attach-policy --policy-id "$POLICY_ID" --target-id "$ROOT_ID"

TRAIL_NAME=$(aws cloudtrail list-trails --query 'Trails[0].Name' --output text)
aws cloudtrail update-trail --name "$TRAIL_NAME" --is-multi-region-trail true

aws organizations detach-policy --policy-id "$POLICY_ID" --target-id "$ROOT_ID"
aws organizations delete-policy --policy-id "$POLICY_ID"
