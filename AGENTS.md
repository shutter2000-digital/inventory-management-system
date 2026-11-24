# AGENTS.md — Inventory Management System

## Project context
Company-approved internal inventory digitalization project for Shutter 2000 Plus (Pvt) Ltd. Planned stack: React + TypeScript + Vite, ASP.NET Core Web API (.NET 10), EF Core and PostgreSQL. The frontend and backend remain in this single product repository.

## Working agreement
- Read README.md, the relevant requirements/decisions and existing code before editing. Treat proposed features and unverified assumptions as such.
- Keep changes focused on the assigned issue. Do not silently change scope, architecture, interfaces, or dependencies; record consequential decisions as ADRs and seek team review.
- Use descriptive branches (feat/, fix/, docs/, test/, chore/), small commits and reviewed pull requests. Do not push directly to protected branches, deploy, publish, or change repository settings without authorization.
- Match established conventions. Prefer simple, maintainable implementations over speculative abstractions; do not introduce microservices or infrastructure without a demonstrated need.
- Add or update relevant tests and documentation. Run the checks available in the repository; report commands, actual outcomes and anything not run. Never claim unverified success.
- Never commit credentials, production environment files, real customer/supplier data, unapproved company assets, raw interview transcripts or confidential business records. Use synthetic fixtures and .env.example with placeholders.
- Keep the public portfolio and company handover distinct; publication, licensing and production access require approval. Do not infer that a project is open source.
- Do not implement cross-repository integrations without a documented agreement; each deliverable must remain independently adoptable.

## Architecture and boundaries
- Keep the IMS a modular monolith, with a feature-oriented React SPA and a REST API. Domain rules belong in Domain/Application, HTTP concerns in Api, persistence and integrations in Infrastructure. Domain must not depend on EF Core or HTTP. Avoid circular project dependencies.
- Use typed API contracts/OpenAPI, predictable error responses, server-side validation, pagination for large collections and server-enforced authorization. Prefer existing framework identity and session mechanisms over homemade security.
- Organize frontend features around inventory workflows; use TypeScript strict mode, accessible components and an explicit server-state strategy. Do not use client state as the authoritative stock balance.

## Inventory rules and scope
- First delivery: material catalogue and units, receipts/issues/returns/adjustments, stock balances and history, authorized staff access, low-stock thresholds, job-linked usage, opening-balance import and basic reporting. Confirm detailed requirements before implementation; reservations, advanced costing, customizable reports and offline mode depend on scope validation.
- Distinguish on-hand stock from available stock and reserved stock if reservation is approved. Do not deduct stock on quotation or order creation. An issue is a physical stock movement; a return restores stock. Avoid double deductions and double release of reservations.
- Keep stock changes transactional with auditable movement records and appropriate concurrency protection. Enforce non-negative balances unless an explicitly approved exception is designed. Validate item IDs, units, quantities and job references. Use decimal quantities where needed; avoid floating-point arithmetic for inventory/costing. Correct mistakes through explicit adjustments/reversals, not silent history rewrites.
- Restrict stock editing to authorized roles; factory manager and management permissions require confirmation. Protect audit records. Treat file imports as untrusted; validate rows, provide error feedback, and prevent duplicate processing.
- Do not assume a full ERP, mobile application, accounting integration or customer portal is required. No connection to the public website or CRM by default.

## Tests and development commands
After initialization, document exact runtime versions, lockfile, startup instructions and environment variables in README.md. Intended commands (run only after corresponding manifests/scripts exist):
- `cd frontend && pnpm install --frozen-lockfile && pnpm lint && pnpm test && pnpm build`
- `dotnet build backend` and `dotnet test backend` (use the actual solution path once created).
- `docker compose -f infra/compose.yaml up -d` once local service configuration exists.
- Add domain unit tests for movements and validation, PostgreSQL integration tests for atomicity/concurrency, authorization/import tests and E2E tests for the primary stock workflow. Use isolated test databases; never test against production.

## Definition of done
Acceptance criteria addressed, appropriate checks performed or explicitly marked not run, relevant documentation updated, and limitations disclosed. A human reviewer decides whether the change is ready to merge.
