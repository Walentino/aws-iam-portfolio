# Day 6 — Policy Drills (MFA & Read-only)

## Scope
- User: `alice-readonly`
- Action: `s3:GetObject`
- Object: `arn:aws:s3:::dummy-nino-policy-mastery/tests/test.txt`

## Evidence (CLI)

### Alice — MFA=false
<!-- paste the CLI table here -->

### Alice — MFA=true
<!-- paste the CLI table here -->

## Notes (Why?)
- MFA=false → explicitDeny by `DenyIfNoMFA`.
- MFA=true  → allowed by read-only policy on object ARNs.

### Alice — MFA=false
```text
-----------------------------------------------------------------------
|                       SimulatePrincipalPolicy                       |
+----------+----------------------------------------------------------+
|  Action  |  s3:GetObject                                            |
|  Decision|  explicitDeny                                            |
|  Matched |  DenyIfNoMFA                                             |
|  Resource|  arn:aws:s3:::dummy-nino-policy-mastery/tests/test.txt   |
+----------+----------------------------------------------------------+
```

### Alice — MFA=true
```text
-----------------------------------------------------------------------
|                       SimulatePrincipalPolicy                       |
+----------+----------------------------------------------------------+
|  Action  |  s3:GetObject                                            |
|  Decision|  allowed                                                 |
|  Matched |  ReadOnlyAccess                                          |
|  Resource|  arn:aws:s3:::dummy-nino-policy-mastery/tests/test.txt   |
+----------+----------------------------------------------------------+
```
