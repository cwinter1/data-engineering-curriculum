# Data Engineering Curriculum

A hands-on, project-driven curriculum for engineers who want to work in data — built from real production experience in Israeli fintech and eCommerce, not from textbooks.

This is not a course you can buy. It was assembled over years of building pipelines in banking, regulatory environments, and high-volume eCommerce — and refined through the process of teaching it. Every module is grounded in problems that actually occur at work.

---

## Who It's For

- Software engineers transitioning into data engineering
- Analysts who write SQL and want to move upstream into pipeline work
- Junior DEs who passed the interview but feel shaky on production fundamentals

Not for: people who want certificates. This is for people who want to build things that work.

---

## How It's Taught

**No slides. No multiple choice.**

Each module follows the same pattern:

1. **The problem first** — you see a broken or missing pipeline, a failing query, a production incident. You understand *why* this matters before you touch any code.
2. **Guided build** — you build a working solution from scratch, with the instructor walking through decisions in real time, not just showing the answer.
3. **Break it deliberately** — after it works, you introduce a failure: a schema change, a late partition, a Kafka consumer falling behind. You fix it yourself.
4. **Capstone application** — each module's techniques feed into a running capstone project. Nothing is isolated.

Learning is cumulative. Week 6 pipelines use the SQL from Week 2 and the orchestration from Week 4. If you skip a module, you feel it.

---

## Curriculum

| Module | Topics | Builds toward |
|--------|--------|--------------|
| 1 — Python for Data | File I/O, data structures, type hints, error handling, CLI design | Capstone ingestion layer |
| 2 — SQL That Scales | Window functions, CTEs, query plans, index strategy, materialized views | Capstone transformation layer |
| 3 — Pipeline Design | ETL vs ELT, idempotency, incremental loads, backfill patterns, late data | Capstone pipeline contract |
| 4 — Cloud Storage | S3, BigQuery, Athena — reading, writing, partitioning, cost awareness | Capstone storage layer |
| 5 — Orchestration | Airflow DAG design, retries, sensors, task dependencies, SLA alerts | Capstone scheduler |
| 6 — Stream Processing | Kafka concepts, consumer groups, offset management, at-least-once delivery | Capstone real-time feed |
| 7 — Testing & Quality | Unit tests, integration tests, data quality contracts, dbt tests as reference | Capstone QA layer |
| 8 — Security for Data Engineers | Secrets management, least-privilege IAM, encryption at rest and in transit, audit logging, PII handling in pipelines | Capstone hardening pass |
| 9 — Production Patterns | Logging, alerting, schema evolution, runbooks, incident response | Capstone production readiness |

---

## Capstone Projects

Each cohort builds one of two capstones end-to-end, from raw ingestion to a live dashboard:

**Capstone A — Fintech Transaction Pipeline**
Ingest, validate, and aggregate a stream of payment events into a reporting layer. Includes regulatory-style audit trail, PII masking, and late-arrival handling. Mirrors the architecture used in Israeli banking environments.

**Capstone B — eCommerce Analytics Platform**
Multi-source ingestion (orders, inventory, ad spend), incremental daily loads, a dimensional model, and a self-service query layer. Based on patterns from a 50-store Amazon operation.

Both capstones go through the full security hardening module before sign-off.

---

## Cybersecurity Module (Module 8)

Data engineers move sensitive data. Most don't think about what happens when something goes wrong.

This module covers:

- **Secrets management** — no credentials in code or git history; Vault, AWS Secrets Manager, env injection patterns
- **IAM least privilege** — scoping S3 bucket policies, BigQuery dataset permissions, service account design
- **Encryption** — at-rest (S3 SSE, BigQuery CMEK) and in-transit (TLS, client cert validation)
- **PII in pipelines** — tokenisation, masking, pseudonymisation; when to apply each; GDPR implications for Israeli companies with EU customers
- **Audit logging** — what to log, where to ship it, how to detect anomalies
- **Incident response for data** — what to do when a pipeline leaks data, writes to the wrong table, or exposes a credential

This is not a security certification track. It's the minimum a production data engineer needs to not get their company breached.

---

## Stack

Python 3.10+, PostgreSQL, BigQuery, AWS (S3 / Athena / Glue), Apache Airflow, Apache Kafka, Spark (Module 6), HashiCorp Vault (Module 8)

---

## What You Leave With

- A working capstone pipeline deployed to a real cloud environment
- The ability to read and write production-grade Python without hand-holding
- Enough security awareness to not be the person who commits an API key
- A mental model of how data moves through a real organization — not a demo environment

---

*Built from production experience. Not available for purchase. Taught in cohorts.*
