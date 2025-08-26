# Week 2 — Review: Roles, Federation & Guardrails

## What I built this week
- **Delegated Admin with Permissions Boundaries**  
  Evidence: [`docs/day6-permission-boundaries.md`](../docs/day6-permission-boundaries.md)
- **GitHub Actions OIDC → AWS (no long-lived keys)**  
  Evidence: [`docs/day4-oidc-federation.md`](../docs/day4-oidc-federation.md)
- **Cross-account role assumption**  
  Evidence: [`docs/day9-cross-account.md`](../docs/day9-cross-account.md)  <!-- if you used this file -->
- (Earlier pieces in this repo you may reference as context)  
  [`day8-iam-roles.md`](../docs/day8-iam-roles.md), [`day3-policy-sim.md`](../docs/day3-policy-sim.md), etc.

## Key takeaways (my notes)
- Permissions boundaries **limit the *maximum* a role can do**, regardless of inline/attached policies.
- GitHub OIDC trust uses conditions like `token.actions.githubusercontent.com:aud` and `...:sub` to pin repo/branch.
- Guardrails belong in **SCPs & org config**; least privilege belongs in **IAM policies**.
- When troubleshooting AssumeRole: check **trust policy**, **caller identity**, **conditions**, and **region/profile**.

## Evidence snapshots
- **Boundary policy ARN**:  
  `arn:aws:iam::<ACCOUNT_ID>:policy/PB-DelegatedReadOnly`
- **Delegator session ARN** (pbdel):  
  `arn:aws:sts::<ACCOUNT_ID>:assumed-role/PB-Delegator/pbdel`
- **Permissions boundary on created role**: `PB-WithBoundary` → `PB-DelegatedReadOnly`

## Self-check
- [ ] I can explain “permissions boundary” vs. “managed/inline” policy.  
- [ ] I can read an OIDC trust policy and verify `aud` + `sub` match repo/branch.  
- [ ] I can predict why an `AssumeRole` fails by looking at the **error + trust conditions**.  
- [ ] I can show the boundary attached to a role via `aws iam get-role --query Role.PermissionsBoundary`.

## Optional cleanup (safe to keep if you’ll reuse)
```bash
# variables (edit ACCOUNT_ID)
ACCOUNT_ID=<your-account-id>
BOUNDARY_ARN=arn:aws:iam::$ACCOUNT_ID:policy/PB-DelegatedReadOnly

# delete test roles (ignore errors if not found)
aws iam delete-role --role-name PB-NoBoundary      2>/dev/null || true
aws iam delete-role --role-name PB-WithBoundary    2>/dev/null || true
aws iam delete-role --role-name PB-Delegator       2>/dev/null || true

# delete boundary policy (only after no roles reference it)
aws iam delete-policy --policy-arn "$BOUNDARY_ARN" 2>/dev/null || true
