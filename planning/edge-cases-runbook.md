# Edge Cases and Runbook

## Objective

Define operational handling for common and high-impact failure scenarios.

## Key Edge Cases

1. Configured media (post/reel) deleted.
2. Instagram account deleted/disconnected.
3. Account switched public -> private.
4. Account switched creator/business -> personal.
5. Token expired/revoked.
6. Permission scope removed.
7. Webhook endpoint unreachable or invalid.
8. Signature validation failures.
9. Duplicate webhook deliveries.
10. Out-of-order webhook events.
11. Comment deleted/edited after trigger.
12. Messaging-window closure errors.
13. Recipient ID resolution failures.
14. Graph API transient failures or rate limits.
15. AI timeout / parse failure / provider outage.
16. High-frequency spam-trigger comments from same user.
17. Worker crashes during in-flight jobs.

## Handling Pattern

1. Detect: explicit error code/status, health check drift, or event anomaly.
2. Classify: transient vs terminal.
3. Action:
   - transient: retry with backoff and jitter
   - terminal: pause affected rule/integration and notify user
4. Communicate: user-visible status in dashboard + recommended action.
5. Recover: reconnect, reauthorize, or unpause rule after validation.

## State Model

1. Integration states:
   - `ACTIVE`
   - `AT_RISK`
   - `REAUTH_REQUIRED`
   - `DISCONNECTED`
2. Rule states:
   - `ACTIVE`
   - `PAUSED`
   - `PAUSED_ORPHANED_MEDIA`
   - `ERROR`

## Retry Guidance

1. Retryable:
   - 429/5xx/transient network errors
2. Non-retryable:
   - policy window violations
   - missing permissions
   - invalid/unknown recipient
   - deleted resources

## Monitoring Signals

1. Webhook receipts per minute.
2. DM success/failure ratio by error code.
3. Skip reasons distribution.
4. Reauth-required account count.
5. Queue lag and DLQ growth.
