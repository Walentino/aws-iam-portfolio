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
