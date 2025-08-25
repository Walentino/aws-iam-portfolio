# Day 9 — Cross-account access with AssumeRole

## Goal
Use a role in Account A to assume a role in Account B and prove what we can/can’t do.

## Evidence

### 1) Assumed-role identity (STS)
> `"UserId": "AROAWVDXNX5ANU3H7WRRS:day9demo",
    "Account": "457664479040",
    "Arn": "arn:aws:sts::457664479040:assumed-role/xacct-s3-readonly/day9demo"`

```json

