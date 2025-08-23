
# Day 5 — Policy Mastery (Simulator-Only)

**Goal:** Build intuition by *proving* IAM evaluation across three demo users and capture clean evidence. Users: `alice-readonly`, `ben-s3writer`, `nino-dev-demo`.

---

## What to screenshot (filenames)
Put images in `screenshots/` with these names:
- `day5-alice-get-allowed.png`
- `day5-alice-put-denied.png`
- `day5-alice-list-allowed.png`
- `day5-ben-put-allowed.png`
- `day5-nino-put-denied-no-mfa.png`
- `day5-nino-put-allowed-with-mfa.png`
(_Optional:_ `day5-ben-put-denied-no-mfa.png` if your global MFA deny affects Ben.)

> In each result row click the ▶ to show **Matching statements** and include that in the screenshot when helpful.

---

## 1) Alice — Read-only S3
**Selected user:** `alice-readonly` → Service **Amazon S3**
1. **GetObject** → expect **Allowed** (read)
2. **ListBucket** → expect **Allowed** (list)
3. **PutObject** → expect **Denied** (no write)

Record which statement matched (likely from `ReadOnlyAccess`).

---

## 2) Ben — S3 Writer
**Selected user:** `ben-s3writer`
1. **PutObject** → expect **Allowed**
2. (Optional) **GetObject** → record outcome (depends on your writer policy)
3. (Optional) If `DenyIfNoMFA` is attached to Ben, set `aws:multifactorauthpresent=false` in **Global settings** and re-run PutObject → expect **Denied**.

---

## 3) Nino — MFA Guardrail
**Selected user:** `nino-dev-demo`
1. Global: `aws:multifactorauthpresent=false` → **PutObject** → **Denied**
2. Global: `aws:multifactorauthpresent=true` → **PutObject** → **Allowed**

_Note:_ This demonstrates that **explicit Deny wins** and how `NotAction` + `BoolIfExists` in `DenyIfNoMFA` works.

---

## 4) Summary Table (fill while testing)

| User           | Action     | MFA Present | Result | Controlling Statement/Policy |
|----------------|------------|-------------|--------|------------------------------|
| alice-readonly | GetObject  | n/a         |        |                              |
| alice-readonly | ListBucket | n/a         |        |                              |
| alice-readonly | PutObject  | n/a         |        |                              |
| ben-s3writer   | PutObject  | n/a         |        |                              |
| ben-s3writer   | GetObject  | n/a         |        |                              |
| ben-s3writer   | PutObject  | false       |        |                              |
| nino-dev-demo  | PutObject  | false       |        |                              |
| nino-dev-demo  | PutObject  | true        |        |                              |

---

## Evidence (CLI)


### Alice — read-only

**GetObject (MFA=false)**  

<img width="1746" height="460" alt="Screenshot 2025-08-22 at 11 42 50 PM" src="https://github.com/user-attachments/assets/77481e13-5d4e-4579-87ba-359712110c70" />

```

**GetObject (MFA=true)**

![GetObject (MFA=true)](https://github.com/user-attachments/assets/ff1bd3f3-6f7f-4b70-a51f-909a4155a0a3)


```
Why: With MFA=true, ReadOnlyAccess allows read.

PutObject (MFA=true)

<img width="3390" height="922" alt="Screenshot 2025-08-22 at 11 44 00 PM" src="https://github.com/user-attachments/assets/417f4a9a-6f9b-43b9-859e-2f84fb63a34b" />

```
Why: ReadOnly has no Put permission → ImplicitDeny.

## 6) Commit Flow
```bash
git switch -c feat/day5-policy-mastery
mkdir -p docs screenshots
# move this file into docs/ then:
git add docs/day5-policy-mastery.md
git commit -m "Week1/Day5: Policy Mastery doc (simulator plan)"
git push -u origin feat/day5-policy-mastery

# after you add images:
git add screenshots/day5-*.png
git commit -m "Day5: add simulator screenshots"
git push
# open PR to main on GitHub
```
