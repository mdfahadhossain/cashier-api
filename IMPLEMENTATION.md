# IMPLEMENTATION.md

## 1. Overview

CashierAI is a scalable, containerized backend system built with **Python**, **FastAPI**, **LangChain**, **Qdrant**, and **PostgreSQL (SQLAlchemy)**.  
It is designed as a **modular monolith, ready for microservice extraction**, and supports **team‑level (multi-tenant) access** for families, companies, and institutions.  
It integrates **OpenAI models** for embeddings, voice processing, and LLM reasoning with **function calling / tool execution**.

This document describes the complete tech stack, architecture, and the final project folder structure.

---

# 2. Tech Stack

## Backend Framework

- FastAPI (ASGI)
- Uvicorn / Gunicorn
- Pydantic v2

## Database Layer

- PostgreSQL
- SQLAlchemy ORM
- Alembic migrations

## Vector Search / RAG

- Qdrant (dockerized)
- LangChain
- OpenAI embeddings

## AI / LLM

- OpenAI GPT-4.2 (reasoning)
- OpenAI GPT-4o / 4o-mini (fast tasks)
- Whisper v3 STT
- OpenAI + Google TTS
- Function calling (LLM tools)

## Deployment / Infra

- Docker + docker-compose
- NGINX reverse proxy
- VPS hosting (DO / Hetzner)
- Optional: Traefik, Prometheus, Grafana

## Security

- JWT Authentication
- HTTPS
- CORS
- Rate limiting

## Background Jobs & Caching

- Redis (fast, in-memory store)
- Celery / RQ workers (PDF generation, email, notifications, embeddings)
- Scheduled jobs (cron-style for recurring reports / insights)

---

# 3. High‑Level Architecture

```
Mobile App → API Gateway → Microservices (Auth, User, Transactions, Reports, AI Engine)
                                  → PostgreSQL
                                  → Qdrant Vector DB
                                  → OpenAI API
```

---

# 4. Folder Structure

/backend
│
├── app
│ ├── api
│ │ ├── v1
│ │ │ ├── routers # FastAPI routers (endpoints only)
│ │ │ │ ├── auth.py
│ │ │ │ ├── users.py
│ │ │ │ ├── teams.py
│ │ │ │ ├── transactions.py
│ │ │ │ ├── reports.py
│ │ │ │ ├── ai.py
│ │ │ │ └── billing.py
│ │ │ ├── controllers # Orchestration layer per domain
│ │ │ │ ├── auth_controller.py
│ │ │ │ ├── user_controller.py
│ │ │ │ ├── team_controller.py
│ │ │ │ ├── transaction_controller.py
│ │ │ │ ├── report_controller.py
│ │ │ │ ├── ai_controller.py
│ │ │ │ └── billing_controller.py
│ │ │ └── dependencies # Common Depends() helpers
│ │ └── errors # HTTP error mappers / handlers
│ │
│ ├── core # App-wide config / security / logging
│ │ ├── config.py
│ │ ├── security.py
│ │ ├── logging.py
│ │ └── rate_limit.py
│ │
│ ├── db
│ │ ├── session.py # SQLAlchemy session + engine
│ │ ├── base.py # Base metadata + registry
│ │ ├── migrations/ # Alembic
│ │ └── repositories # Repository layer per aggregate
│ │ ├── user_repo.py
│ │ ├── team_repo.py
│ │ ├── transaction_repo.py
│ │ ├── category_repo.py
│ │ ├── budget_repo.py
│ │ ├── goal_repo.py
│ │ ├── subscription_repo.py
│ │ └── report_job_repo.py
│ │
│ ├── models # SQLAlchemy models (database layer)
│ │ ├── user.py
│ │ ├── team.py
│ │ ├── membership.py
│ │ ├── role.py
│ │ ├── subscription_plan.py
│ │ ├── subscription.py
│ │ ├── account.py
│ │ ├── category.py
│ │ ├── transaction.py
│ │ ├── budget.py
│ │ ├── goal.py
│ │ ├── report_job.py
│ │ ├── ai_message.py
│ │ └── notification.py
│ │
│ ├── schemas # Pydantic models (request/response)
│ │ ├── auth_schemas.py
│ │ ├── user_schemas.py
│ │ ├── team_schemas.py
│ │ ├── transaction_schemas.py
│ │ ├── report_schemas.py
│ │ ├── ai_schemas.py
│ │ └── billing_schemas.py
│ │
│ ├── services # Domain services / “app services”
│ │ ├── auth
│ │ │ └── auth_service.py
│ │ ├── users
│ │ │ └── user_service.py
│ │ ├── teams
│ │ │ └── team_service.py
│ │ ├── transactions
│ │ │ └── transaction_service.py
│ │ ├── reports
│ │ │ └── report_service.py
│ │ ├── ai
│ │ │ └── ai_service.py
│ │ ├── billing
│ │ │ └── billing_service.py
│ │ └── notifications
│ │ └── notification_service.py
│ │
│ ├── workers # Background workers (Celery/RQ)
│ │ ├── pdf_worker.py # Monthly/yearly reports, exports
│ │ ├── insights_worker.py # Trend analysis, scheduled insights
│ │ └── notification_worker.py
│ │
│ ├── ai_engine
│ │ ├── embeddings # Embedding generation helpers
│ │ ├── llm # LLM client + prompt routing
│ │ ├── vectorstore # Qdrant adapters
│ │ ├── stt # Whisper integration
│ │ ├── tts # TTS integrations
│ │ └── tools # LangChain tools / function-calling
│ │
│ ├── utils
│ │ ├── time_utils.py
│ │ ├── pagination.py
│ │ └── file_storage.py # S3/Cloud storage abstraction
│ │
│ ├── main.py # FastAPI app entrypoint
│ └── app_init.py # Wiring (routers, middleware, DI)
│
├── docker
├── docker-compose.yml
├── requirements.txt
└── IMPLEMENTATION.md

---

# 5. Domain Services & Endpoints

Each domain has its own **router → controller → service → repository** chain, making it easy to split into microservices later.

- **Auth Service**
  - Responsibilities: phone login, OTP, JWT issuance/refresh, session checks.
  - Main endpoints (example): `POST /auth/login`, `POST /auth/verify-otp`, `POST /auth/refresh`.
- **User & Team Service**
  - Responsibilities: user profiles, teams, memberships, roles, subscription linkage.
  - Endpoints: `GET /me`, `GET /teams`, `POST /teams`, `POST /teams/{id}/members`.
- **Transactions Service**
  - Responsibilities: CRUD on expenses/income/loans/debts, filters, exports.
  - Endpoints: `GET /transactions`, `POST /transactions`, `PATCH /transactions/{id}`, `DELETE /transactions/{id}`, `POST /transactions/export`.
- **Reports & PDF Service**
  - Responsibilities: AI and non-AI summaries, PDF jobs, downloads.
  - Endpoints: `POST /reports/summary`, `POST /reports/pdf`, `GET /reports/jobs/{id}`.
- **AI Service**
  - Responsibilities: chat, natural-language queries, smart search, personality modes.
  - Endpoints: `POST /ai/chat`, `POST /ai/transactions/query`, `POST /ai/receipt/parse`.
- **Billing & Subscription Service**
  - Responsibilities: plans, payments integration, usage limits, plan checks.
  - Endpoints: `GET /billing/plans`, `POST /billing/subscribe`, `GET /billing/subscription`.
- **Notification Service**
  - Responsibilities: email/push for reminders, insights, anomalies.
  - Used mainly via workers and internal events.

# 6. Database Design (Team-Level / Multi-Tenant)

All user data is **scoped by team** (family, company, institution). Any table containing user financial data must have a `team_id` foreign key and appropriate composite indexes.

## 6.1 Core Entities

- **User**
  - Fields: `id`, `phone`, `email`, `name`, `status`, timestamps.
- **Team**
  - Represents a family, company, or group.
  - Fields: `id`, `name`, `type` (family/company/etc.), `owner_user_id`, timestamps.
- **TeamMembership**
  - Many-to-many between users and teams.
  - Fields: `id`, `team_id`, `user_id`, `role_id`, `invited_by`, `joined_at`.
- **Role**
  - Predefined roles: `admin`, `cashier` (read/write), `observer` (read-only).
  - Fields: `id`, `key`, `name`, `description`.
- **SubscriptionPlan**
  - System-level plans (Free, Premium, Enterprise).
  - Fields: `id`, `code`, `name`, `limits_json`, `price`, `billing_cycle`.
- **Subscription**
  - Team-level subscription state.
  - Fields: `id`, `team_id`, `plan_id`, `status`, `valid_from`, `valid_until`, `provider_customer_id`.

## 6.2 Finance & Tracking

- **Account**
  - Logical money containers (cash, card, bKash, etc.).
  - Fields: `id`, `team_id`, `name`, `type`, `currency`, `initial_balance`.
- **Category**
  - Expense/income categories (food, transport, salary).
  - Fields: `id`, `team_id`, `name`, `type` (expense/income), `parent_id`, `is_default`.
- **Transaction**
  - Core ledger entry; all are scoped to a team and optionally a user.
  - Fields: `id`, `team_id`, `user_id`, `account_id`, `category_id`, `type`, `amount`, `currency`, `occurred_at`, `notes`, `tags`, `source` (manual/ai/receipt).
- **Budget**
  - Team/category-level budgets (e.g., monthly food budget).
  - Fields: `id`, `team_id`, `category_id`, `period` (month/year), `amount`, `currency`.
- **Goal**
  - Savings / payoff goals.
  - Fields: `id`, `team_id`, `name`, `target_amount`, `target_date`, `current_amount`.
- **ReportJob**
  - Asynchronous report/PDF jobs.
  - Fields: `id`, `team_id`, `created_by`, `type`, `status`, `filters_json`, `file_url`, timestamps.
- **AiMessage**
  - Stores prompts and key AI responses for analytics and debugging.
  - Fields: `id`, `team_id`, `user_id`, `role`, `content`, `tokens`, `meta_json`.
- **Notification**
  - Records notifications sent to users (for idempotency + audit).
  - Fields: `id`, `team_id`, `user_id`, `channel`, `template_key`, `payload_json`, `sent_at`.

## 6.3 Team-Level Scaling Rules

- Every multi-tenant table includes `team_id` and is indexed with `(team_id, created_at)` or `(team_id, user_id, created_at)`.
- Controllers always resolve **current team** from JWT or header and pass `team_id` down to services/repositories.
- For future hard multi-tenancy, the schema allows partitioning/sharding by `team_id`.

# 7. Request Flow Examples

## 7.1 “Add Expense” Flow

1. Mobile app calls `POST /v1/transactions` with amount, category, date, and optional natural-language note.
2. Router (`transactions.py`) validates payload using Pydantic schema and injects `current_user` + `current_team`.
3. Controller (`transaction_controller.py`) calls `transaction_service.create_expense(...)`.
4. Service performs:
   - Permission check via `TeamMembership` and `Role`.
   - Category/account validation using repositories.
   - Creation of `Transaction` in the DB.
   - Optional event dispatch to workers (e.g., re-calc budgets, trigger alerts).

## 7.2 “Ask AI for Monthly Summary PDF”

1. Mobile app calls `POST /v1/reports/pdf` with a natural-language description or explicit period.
2. Router + controller normalize the request into a structured `ReportJob` and enqueue a worker task.
3. Worker:
   - Loads transactions for the team.
   - Calls AI engine for insights/comments.
   - Renders charts + PDF, uploads it (e.g., S3), updates `ReportJob.file_url`.
4. Client polls `GET /v1/reports/jobs/{id}` or is notified when the job is finished.

# 8. Scalability Notes

- Modular structure that can be split into true microservices by moving domain folders into separate repos.
- Async FastAPI + async SQLAlchemy for high I/O concurrency.
- Separate AI engine layer (LLM, embeddings, vectorstore) that can be scaled independently.
- Background workers for embeddings, scheduled insights, and PDF reports.
- Database partitioning options for large tables (e.g., `Transaction`, `AiMessage`, `ReportJob`).
- Qdrant sharding/replication for vector workloads.

---

# 9. Deployment Guide

1. Build containers:
   ```
   docker-compose up --build -d
   ```
2. Apply migrations:
   ```
   alembic upgrade head
   ```
3. Configure NGINX + HTTPS (Let’s Encrypt or managed cert).
4. Verify health endpoints (`/health`, `/metrics` if enabled).

---

# 10. Optional Enhancements

- Webhooks for real-time event streaming (budget breach, new reports).
- Message queue (Redis / RabbitMQ / Kafka) for higher-volume events.
- Fine‑tuned or custom LLMs based on anonymized usage patterns.
