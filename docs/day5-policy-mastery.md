
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

## 5) Embed Evidence (paste after screenshots exist)
```markdown
## Evidence

**Alice — read-only**
![Alice Get allowed](../screenshots/day5-alice-get-allowed.png)
![Alice Put denied](../screenshots/day5-alice-put-denied.png)
![Alice List allowed](../screenshots/day5-alice-list-allowed.png)

**Ben — writer**
![Ben Put allowed](../screenshots/day5-ben-put-allowed.png)

**Nino — MFA guardrail**
![Nino Put denied (no MFA)](../screenshots/day5-nino-put-denied-no-mfa.png)
![Nino Put allowed (with MFA)](../screenshots/day5-nino-put-allowed-with-mfa.png)
```

---

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
