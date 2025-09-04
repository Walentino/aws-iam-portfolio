# Week 5 — Day 7: Monitoring wrap-up & self-assessment

## Goal
Confirm monitoring is healthy, capture evidence, and note improvements for next week.

## Evidence

### CloudWatch alarms (state)
```bash
aws cloudwatch describe-alarms \
  --query 'MetricAlarms[].{Name:AlarmName,State:StateValue,Updated:StateUpdatedTimestamp}' \
  --output table

aws events list-rules \
  --query 'Rules[].{Name:Name,State:State,EventBusName:EventBusName}' \
  --output table

aws events list-targets-by-rule --rule "<RULE_NAME>" \
  --query 'Targets[].{Id:Id,Arn:Arn}' --output table

aws events list-rules \
  --query 'Rules[].{Name:Name,State:State,EventBusName:EventBusName}' \
  --output table

aws events list-targets-by-rule --rule "<RULE_NAME>" \
  --query 'Targets[].{Id:Id,Arn:Arn}' --output table

aws cloudtrail describe-trails --include-shadow-trails \
  --query 'trailList[].{Name:Name,HomeRegion:HomeRegion,IsMultiRegion:IsMultiRegion,LogFileValidation:LogFileValidationEnabled}' \
  --output table

aws cloudtrail get-trail-status --name "<TRAIL_NAME>" \
  --query '{LatestDeliveryTime:LatestDeliveryTime,IsLogging:IsLogging}' \
  --output table

DET=$(aws guardduty list-detectors --query 'DetectorIds[0]' --output text)
aws guardduty get-detector --detector-id "$DET" \
  --query '{Status:Status,FindingPublishingFrequency:FindingPublishingFrequency}' \
  --output table
