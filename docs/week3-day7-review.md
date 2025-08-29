# Week 3 — Review & Wrap-up (Security, Monitoring & Auditing)

## What I built this week
- **Day 1 — CloudTrail (IAM events)**
  - Org trail to S3 (multi-region, global events).
  - (Note) CW Logs linking skipped for org trails; used account trail/EventBridge where needed.
- **Day 2 — AWS Config (planned)**
  - Recorder/Rules plan captured; parked due to shell/CLI quirks. JSON-based setup noted.
- **Day 3 — Access Analyzer**
  - Reused existing analyzer; listed findings; archived one for evidence.
- **Day 4 — Policy Generation from CloudTrail**
  - Generated least-priv policy for `gha-oidc-demo`, trimmed, validated (Policy Simulator).
- **Day 5 — Real-time Alerts**
  - EventBridge rules → SNS email for sensitive IAM writes & CloudTrail tampering.
- **Day 6 — Access Reviews (Unused Access)**
  - Created unused-access analyzer; identified principals/permissions to prune.

## Links to evidence
- Day 1: `docs/day1-cloudtrail.md` (IAM-only selectors + lookup proof)
- Day 3: `docs/day3-access-analyzer.md`
- Day 4: `docs/week3-day4-policy-generation.md`
- Day 5: `docs/week3-day5-monitoring-alerts.md`
- Day 6: `docs/week3-day6-access-reviews.md`

## Key takeaways (my notes)
- **Org trail vs. account trail:** org → S3 archival; account trail can link to **CloudWatch Logs** for real-time metrics.
- **Event-driven alerts beat polling:** EventBridge + SNS gives immediate visibility for sensitive changes.
- **Least privilege is iterative:** generate → trim wildcards → validate → monitor → refine.
- **Access Analyzer:** two modes matter this week—**external access findings** and **unused access**.

## “What I’d improve next”
- Finish AWS Config setup via JSON (avoid shell quoting issues).
- Add a ConsoleLogin failure rule (signin.amazonaws.com) to alerts.
- Track alert volume; route to email today, Slack/SNS/Lambda tomorrow.

## Evidence snapshot (fill in)
- Trail name: `<org-iam-trail>` (Region: `us-east-1`)
- Analyzer ARN: `<your analyzer arn>`
- Final least-priv policy ARN: `arn:aws:iam::<ACCOUNT_ID>:policy/gha-oidc-demo-leastpriv-v1`
- SNS topic: `arn:aws:sns:<REGION>:<ACCOUNT_ID>:iam-alerts`
