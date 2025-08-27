# Week 3 — Day 1: CloudTrail (IAM-only Org/Single-Account Trail)

## Goal
Enable CloudTrail for **only IAM events**, writing to an S3 bucket, and capture a quick proof that it’s working.

---

## Trail + bucket (values)
- **Trail name**: `$(echo $TRAIL_NAME)`
- **Bucket**: `$(echo $BUCKET)`
- **Region**: `$(echo $REGION)`
- **Account**: `$(aws sts get-caller-identity --query Account --output text)`

> If you prefer, replace the placeholders above with the final values you used.

---

## Event selector (IAM-only)
```json
{
  "AdvancedEventSelectors": [
    {
      "Name": "Only IAM management events",
      "FieldSelectors": [
        { "Field": "eventCategory", "Equals": ["Management"] },
        { "Field": "eventSource",  "Equals": ["iam.amazonaws.com"] }
      ]
    }
  ]
}
