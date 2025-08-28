# Week 3 — Day 2: Real-time Alerts (EventBridge → SNS)

## Goal
Get immediate email alerts for risky IAM activity and failed console logins, without needing a CloudWatch log group.

## What I set up
- **SNS Topic:** `sec-alarms` (email subscription confirmed)
- **EventBridge Rule #1:** `ev-ConsoleLogin-Fail`  
  Matches CloudTrail events: `source=aws.signin`, detail-type `AWS Console Sign In via CloudTrail`, `ConsoleLogin=Failure`.
- **EventBridge Rule #2:** `ev-IAM-Change`  
  Matches IAM API changes (create/delete user/role, attach/detach policy, access keys, trust policy updates, etc.)
- **Targets:** both rules publish to the SNS topic (topic policy allows `events.amazonaws.com` with `aws:SourceArn` = rule ARN).

## Evidence
```text
Topic:     arn:aws:sns:<REGION>:<ACCOUNT_ID>:sec-alarms
Rule #1:   arn:aws:events:<REGION>:<ACCOUNT_ID>:rule/ev-ConsoleLogin-Fail
Rule #2:   arn:aws:events:<REGION>:<ACCOUNT_ID>:rule/ev-IAM-Change
Targets:   aws events list-targets-by-rule --rule ev-ConsoleLogin-Fail
           aws events list-targets-by-rule --rule ev-IAM-Change
