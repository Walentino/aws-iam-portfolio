# Week 3 — Day 3: Access Analyzer (Review & Archive Findings)

## Goal
List current Access Analyzer findings and archive a sample finding.

## Evidence – commands & output

```bash
# Set region
REGION=us-east-1

# (Optional) View analyzers to confirm the one you’ll use
aws accessanalyzer list-analyzers \
  --region "$REGION" \
  --query 'analyzers[].{name:name,arn:arn,status:status}' \
  --output table

# Use your existing analyzer by NAME (change if needed)
AA_NAME="mino-account-analyzer"

# Resolve its ARN (or paste the ARN directly if you have it)
AA_ARN=$(aws accessanalyzer list-analyzers \
  --region "$REGION" \
  --query "analyzers[?name=='$AA_NAME'].arn | [0]" \
  --output text)

echo "Analyzer ARN: $AA_ARN"

aws accessanalyzer list-findings \
  --analyzer-arn "$AA_ARN" \
  --max-results 10 \
  --region "$REGION"

FIRST_ID=$(aws accessanalyzer list-findings \
  --analyzer-arn "$AA_ARN" \
  --query 'findings[0].id' \
  --output text \
  --region "$REGION")

if [ -n "$FIRST_ID" ] && [ "$FIRST_ID" != "None" ]; then
  aws accessanalyzer archive-findings \
    --analyzer-arn "$AA_ARN" \
    --ids "$FIRST_ID" \
    --region "$REGION"
fi
