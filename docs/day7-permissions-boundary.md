# Day 7 — Permissions Boundary

**Goal:** Prove that a permissions boundary limits the *maximum* permissions of a principal, even when their identity policies allow more.

We’ll:
- Create a boundary that only allows **S3 read** on one bucket.
- Create a “power” user with **AmazonS3FullAccess**.
- Attach the boundary to that user.
- Simulate actions to show: **PutObject = denied**, **GetObject = allowed**.

## Evidence (CLI)

### Environment
Paste your env values here.

### Simulations
Paste the 2 tables here:
- PutObject (expect **denied**)
- GetObject (expect **allowed**)

