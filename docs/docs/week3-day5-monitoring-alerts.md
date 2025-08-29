# Week 3 — Day 5: Monitoring & Alerting (EventBridge → SNS)

## Goal
Alert on sensitive IAM changes and CloudTrail tampering without relying on log groups.

## Architecture
EventBridge rules (AWS API Call via CloudTrail) → SNS topic `iam-alerts` (email)

## Rules
- **iam-sensitive-writes** (source `aws.iam`, detail.eventSource `iam.amazonaws.com`)
  - Actions: CreateUser, CreateRole, AttachRolePolicy, CreateAccessKey, … (curated list)
- **cloudtrail-changes** (source `aws.cloudtrail`, detail.eventSource `cloudtrail.amazonaws.com`)
  - Actions: StopLogging, DeleteTrail, UpdateTrail, CreateTrail

## Commands used
```bash
REGION=us-east-1
EMAIL=<your email>
# create SNS + allow EventBridge + put rules + set targets
<…commands from Day 5 guide…>
