# Week 5 — Day 1: Organizations + Identity Center (SSO) groundwork

## Goal
Create an AWS Organization, enable IAM Identity Center, and assign a least-privilege permission set.

## Evidence – commands & outputs
```bash
# Org exists + all features?
aws organizations describe-organization \
  --query 'Organization.{Id:Id,FeatureSet:FeatureSet,MasterAccountId:MasterAccountId}' --output table

# Identity Center instance
aws sso-admin list-instances \
  --query 'Instances[].{Arn:InstanceArn,IdentityStoreId:IdentityStoreId}' --output table

# Permission sets present
INSTANCE_ARN=$(aws sso-admin list-instances --query 'Instances[0].InstanceArn' --output text)
aws sso-admin list-permission-sets --instance-arn "$INSTANCE_ARN"
