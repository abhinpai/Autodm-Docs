# Product Roadmap (90 Days)

## Phase 1: Foundation (Weeks 1-3)

1. Implement queue-first processing (fast webhook ACK + worker).
2. Move dedupe/window state to Redis.
3. Add retry/backoff policies and DLQ handling.
4. Add structured logging and baseline dashboards.

## Phase 2: Control and Safety (Weeks 4-6)

1. Multi-tenant schema in Supabase with RLS.
2. Campaign/rule lifecycle states and health checks.
3. Anti-spam controls:
   - per-user cooldown
   - tenant send quotas
4. Self-serve reconnect/reauth flows.

## Phase 3: Product Surface (Weeks 7-9)

1. React + Vite admin app for campaign management.
2. Rule simulator UI (test comment -> expected action path).
3. Analytics panels:
   - trigger count
   - reply rate
   - DM delivery success
   - conversion proxy events

## Phase 4: Commercial Readiness (Weeks 10-12)

1. Pricing plans and usage metering.
2. Billing integration and overage policies.
3. Team permissions and workspace management.
4. Compliance workflow hardening and support playbooks.

## Success Criteria

1. High webhook reliability under load.
2. Low duplicate sends and predictable retry behavior.
3. Clear tenant-level observability and cost controls.
4. End-to-end campaign lifecycle managed from dashboard.
