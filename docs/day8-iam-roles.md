# Day 8 — IAM Roles (EC2 + AssumeRole)

## Goal
Prove an EC2 instance can access S3 via an instance profile, then practice AssumeRole.

## Steps (we’ll fill these in as we go)
- Create EC2 role + instance profile
- Attach instance profile to EC2 and verify S3 access
- Create a second role and test sts:AssumeRole
- Capture CLI evidence

## Evidence

### On-instance identity (STS)

```json
"UserId": "AROAWVDXNX5APFZD2DIYI:i-02947efaa53fdd5ac",
    "Account": "457664479040",
    "Arn": "arn:aws:sts::457664479040:assumed-role/ec2-s3-readonly/i-02947efaa53fdd5ac"
```
### Instance metadata (role name)

```text
ROLE_NAME=ec2-s3-readonly
```

### Role ec2-s3-readonly — GetObject
```text
-----------------------------------------------------------------------
|                       SimulatePrincipalPolicy                       |
+----------+----------------------------------------------------------+
|  Action  |  s3:GetObject                                            |
|  Decision|  allowed                                                 |
|  Matched |  AmazonS3ReadOnlyAccess                                  |
|  Resource|  arn:aws:s3:::dummy-nino-policy-mastery/tests/test.txt   |
+----------+----------------------------------------------------------+
```

### Role ec2-s3-readonly — PutObject
```text
-----------------------------------------------------------------------
|                       SimulatePrincipalPolicy                       |
+----------+----------------------------------------------------------+
|  Action  |  s3:PutObject                                            |
|  Decision|  implicitDeny                                            |
|  Matched |                                                          |
|  Resource|  arn:aws:s3:::dummy-nino-policy-mastery/tests/test.txt   |
+----------+----------------------------------------------------------+
```
