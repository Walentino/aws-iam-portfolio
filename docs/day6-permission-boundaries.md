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
