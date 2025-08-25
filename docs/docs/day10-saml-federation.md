# Day 10 – SAML Federation (IdP → AWS)

## Goal
Enable SAML sign-in to AWS via an external IdP and prove it with STS identity.

## Steps (we’ll fill as we go)
- Create IAM SAML provider
- Create IAM role that trusts the SAML provider
- Map role in the IdP and test sign-in
- Capture CLI evidence

## Evidence (CLI)
### STS identity after SAML
```text
<paste aws sts get-caller-identity output here>
