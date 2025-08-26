# Week 2 — Day 6: Permission Boundaries (Delegated Admin)

## Goal
Allow a “delegator” to create roles, but force a permissions boundary so those roles stay limited.

## Evidence
```text
arn:aws:iam::457664479040:role/PB-WithBoundary
```

## Boundary policy ARN
```text
arn:aws:iam::457664479040:policy/PB-DelegatedReadOnly
```

### Caller identity (delegator session `pbdel`)

```bash
aws sts get-caller-identity --profile pbdel --output json

"UserId": "AROAWVDXNX5AH4SYZ2F5L:pbdel",
"Account": "457664479040",
"Arn": "arn:aws:sts::457664479040:assumed-role/PB-Delegator/pbdel"

## Show the expected failure (create role without a boundary)

```bash
aws iam create-role \
  --role-name PB-NoBoundary \
  --assume-role-policy-document file://policies/role-trust-ec2.json \
  --profile pbdel

An error occurred (AccessDenied) when calling the CreateRole operation: User: arn:aws:sts::457664479040:assumed-role/PB-Delegator/pbdel is not authorized to perform: iam:CreateRole on resource: arn:aws:iam::457664479040:role/PB-NoBoundary because no identity-based policy allows the iam:CreateRole action

## Show the success (create role with the boundary)

```bash
aws iam get-role --role-name PB-WithBoundary --profile pbdel \
  --query "Role.PermissionsBoundary" --output json

 "PermissionsBoundaryType": "Policy",
 "PermissionsBoundaryArn": "arn:aws:iam::457664479040:policy/BoundaryS3ReadOnly"
