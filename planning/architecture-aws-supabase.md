# AWS + Supabase Target Architecture

## Goal

Use Supabase for database/auth while running automation pipeline and infra controls on AWS.

## Recommended Architecture

1. Frontend:
   - React + Vite dashboard
   - Hosted on AWS Amplify Hosting (or S3 + CloudFront)
2. Auth + DB:
   - Supabase Auth for dashboard users
   - Supabase Postgres with RLS for tenant isolation
3. Ingress:
   - API Gateway (HTTP API) + Lambda for webhook verification/fast ACK
4. Async pipeline:
   - SQS queue (+ DLQ)
   - ECS worker service for campaign processing and Graph API calls
5. Stateful controls:
   - Redis (ElastiCache/Valkey) for dedupe, cooldown, per-user rate limits, messaging windows
6. Observability:
   - CloudWatch logs/metrics/alarms
   - Optional Sentry for exception tracking
7. Security:
   - WAF in front of API endpoint
   - Secrets Manager for API keys and tokens

## Cost-Optimized Variant (Container-First)

For early-stage cost control, run selected components as containers on fewer nodes:

1. Backend API + worker on one compute node.
2. Redis on same node for MVP only (or managed redis when reliability needs increase).
3. Ollama on dedicated node/container to isolate inference load.

This variant lowers baseline spend but increases operational complexity and failure blast radius.

## Why This Split

1. Supabase gives fast iteration for multi-tenant relational data and auth.
2. AWS gives robust queueing, scaling, edge protection, and worker orchestration.
3. Clear separation between product data plane (Supabase) and high-throughput execution plane (AWS).

## Suggested Data Ownership

1. Supabase:
   - tenants/workspaces
   - connected instagram accounts metadata
   - campaigns/rules/intents
   - user roles
   - audit/activity records
2. Redis:
   - ephemeral keys (idempotency, window state, cooldown)
3. S3 (optional):
   - exports, ingestion traces, reporting snapshots

## Ollama Deployment

1. Run as a dedicated containerized service.
2. Keep model scope narrow for classification workloads.
3. Route only needed requests to LLM (keyword-first, then AI fallback).
4. Add timeout and circuit-breaker behavior to avoid webhook delay impact.
5. Prefer a separate container host from webhook/API path for consistent latencies.

## Non-Goals in MVP

1. Full cross-channel support (WhatsApp, Messenger, etc.)
2. Complex workflow builder UI
3. Advanced team billing automation
