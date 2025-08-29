# Day 4 – Policy Generation from CloudTrail → Least-Privilege → Validation

## Goal
Generate a least-privilege policy from actual CloudTrail activity, attach it to the principal, and validate with the Policy Simulator.

## Steps (console)
1. IAM → **Access Analyzer** → **Policy generation** → **Create from CloudTrail**.
2. Select **Principal** (user/role), pick **Time range** (last 7–14 days), select **Trail**, click **Generate**.
3. Review generated JSON; replace wildcards with resource ARNs where possible; remove unused actions.
4. Save as **customer managed policy** (e.g., `LeastPriv-<principal>-v1`) or attach inline to the principal.
5. Validate in **Policy Simulator**: choose the principal, add actions/resources, and confirm **Allowed**/**Denied** is expected.

## Evidence
### Principal


### Generated policy (final)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    { "Effect": "Allow", "Action": ["<SERVICE>:<Action1>", "<SERVICE>:<Action2>"], "Resource": ["<ARN1>", "<ARN2>"] }
  ]
}

aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::<ACCOUNT_ID>:role/<YOUR_ROLE_NAME> \
  --action-names "<SERVICE>:<Action1>" "<SERVICE>:<Action2>" \
  --resource-arns "<ARN1>" "<ARN2>" \
  --region <REGION>


---

