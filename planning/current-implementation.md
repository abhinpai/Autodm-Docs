# Current Implementation Snapshot

## Purpose

Capture the current state of the automation engine before larger product and architecture changes.

## What Works Today

1. Manifest-driven campaign engine (no content fallbacks from `.env`).
2. Webhook verification and signature validation.
3. Comment keyword matching per media rule.
4. AI fallback intent matching for non-keyword comments.
5. Per-media campaign behavior for:
   - initial DM template
   - optional public comment reply
   - follow-up CTA behavior
6. Follow-up delivery modes:
   - `same_message`
   - `on_user_reply`
7. Follower-gated follow-up (`onlyIfNotFollowing`).
8. Idle mode when no active media rules are enabled.
9. Explicit skip logs for AI/non-matching paths.

## AI Capabilities Added

1. Structured intent classification from configured intent list.
2. Robust JSON parsing for LLM output (including noisy response handling).
3. Per-media AI mode:
   - `precision`
   - `balanced`
   - `recall`
4. Recall-mode lexical fallback to catch short/typo asks (e.g., `link`, `itineary plz`).

## Known Gaps (Production)

1. Webhook request path still does processing inline before response.
2. Idempotency and conversation-window stores are in-memory only.
3. No distributed queue yet for retries/backpressure.
4. No full tenant/billing enforcement model yet.
5. No admin frontend for campaign management yet.

## Immediate Operational Guidance

1. Keep `DRY_RUN=false` only in controlled environments.
2. Use `AI_MODEL=llama3.2:1b` and tuned timeout for local Ollama development.
3. Keep manifest `ai.mode="recall"` for typo-heavy lead magnet comments.
