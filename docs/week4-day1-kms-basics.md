# Week 4 — Day 1: KMS Keys, Key Policies & Grants

## Goal
Create a customer managed KMS key, set a tight key policy, test encrypt/decrypt with one allowed principal, and deny everyone else.

## Evidence (to fill)
- Key ARN:
- Key policy JSON:
- Encrypt/Decrypt: allowed principal ✅ / denied principal ❌

### KMS: Symmetric CMK smoke-test (Week 4 Day 1)

**Key metadata**
```bash
aws kms describe-key --region "$REGION" --key-id "$KEY_ID" \
  --query 'KeyMetadata.{KeyId:KeyId,Enabled:Enabled,Arn:Arn}'

aws kms get-key-policy --region "$REGION" --key-id "$KEY_ID" \
  --policy-name default --query Policy --output text \
| jq -r '.Statement[].Sid'

printf 'hello-iam' > /tmp/plain.txt
CIPH_B64=$(aws kms encrypt --region "$REGION" --key-id "$KEY_ID" \
  --plaintext fileb:///tmp/plain.txt --query CiphertextBlob --output text)
echo "$CIPH_B64" | base64 -D > /tmp/blob.bin
aws kms decrypt --region "$REGION" --ciphertext-blob fileb:///tmp/blob.bin \
  --query Plaintext --output text | base64 -D; echo
# -> hello-iam
