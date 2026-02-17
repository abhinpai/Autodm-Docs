# India Cost Model (Rough Estimates)

## Disclaimer

These are directional estimates for planning and should be validated with AWS and Supabase calculators before committing budget.

## Assumptions

1. Region focus: India (`ap-south-1`) where possible.
2. Supabase used for DB + Auth.
3. AWS used for API ingress, queue, workers, caching, and observability.
4. Ollama used for cost-controlled AI fallback.

## Monthly Budget Bands (Rough)

1. MVP / early customers:
   - INR 15,000 to 45,000
2. Beta / growing usage:
   - INR 45,000 to 1,20,000
3. Scale / high automation volume:
   - INR 1,20,000+

## Container-First Cost Shape (Supabase + AWS)

Directional ranges for India-focused planning:

1. Lean single-node style:
   - INR 9,500 to 15,000
   - Typical setup: Supabase + one container node running app/worker/redis
2. Better split:
   - INR 13,000 to 24,000
   - Typical setup: app node + dedicated ollama node + Supabase
3. Managed-heavy:
   - INR 27,000 to 81,000+
   - Typical setup: API gateway + queue + managed cache + worker fleet + Supabase

These are planning bands, not fixed quotes.

## Main Cost Drivers

1. Worker uptime and CPU/memory sizing.
2. Redis usage and key churn.
3. CloudWatch log volume.
4. WAF and API request volume.
5. AI inference path:
   - local small models: lower cost, lower quality ceiling
   - hosted premium models: higher quality, potentially high spend
6. Degree of self-management vs managed services.

## Cost Control Levers

1. Keyword-first routing to reduce AI calls.
2. Queue-based batch handling for non-urgent jobs.
3. Tiered retention for logs and analytics events.
4. Strict per-tenant soft/hard usage limits.
5. Feature flags for expensive capabilities.
6. Separate ollama workloads from webhook path to prevent over-provisioning core API nodes.

## Revenue Model Considerations

1. Pricing tiers by contacts/messages/automation complexity.
2. Add-on pricing for AI-heavy usage.
3. Team/seat and agency workspace pricing options.
