# Week 3 — Day 6: Access Reviews (Unused Permissions)

## Goal
Identify and remove unused IAM permissions to move toward least privilege.

## Steps
1. IAM → Access Analyzer → **Unused access** → **Create analyzer**
   - Scope: Current account
   - Name: `unused-access`
   - Window: Last 90 days
2. Review findings:
   - **Principals not used** (roles/users unused in the window)
   - **Permissions not used** (actions never invoked)
3. Remediate:
   - Trim policy JSON to remove unused actions (or split detachable policies)
   - Disable/delete truly unused roles
4. Suppress intentional exceptions using **Archive** or an **archive rule**.

## Commands (optional)
```bash
REGION=us-east-1
AA6_NAME=unused-access
aws accessanalyzer create-analyzer --analyzer-name "$AA6_NAME" --type ACCOUNT_UNUSED_ACCESS --region "$REGION" || true
AA6_ARN=$(aws accessanalyzer list-analyzers --region "$REGION" --query "analyzers[?type=='ACCOUNT_UNUSED_ACCESS'].arn | [0]" --output text)
aws accessanalyzer list-findings --analyzer-arn "$AA6_ARN" --max-results 50 --region "$REGION" --output table
