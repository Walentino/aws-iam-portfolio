# Week 5 — Day 2: SCP — deny non-approved regions

## Goal
Prevent actions in regions outside my approved list using an Organization Service Control Policy.

## SCP content
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyNotInApprovedRegions",
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": { "aws:RequestedRegion": ["us-east-1","us-west-2"] },
      "Null": { "aws:RequestedRegion": "false" }
    }
  }]
}
