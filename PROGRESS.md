# Progress & Learning Log

This file tracks milestones and concepts learned while building DataForge. See `roadmap.md` for the full level-by-level plan.

## Status

- **Current level:** Level 1 
- **Pace:** sustainable, ~12-15h/week
- **Started:** 2026-09-23

## Before DataForge: prior practice

Completed a Task Manager API (FastAPI, TDD, SOLID refactor) in a separate repo, under an earlier roadmap that was replaced by DataForge. Concepts practiced there:

- TDD: Red-Green-Refactor cycle, writing tests before implementation
- SOLID: identifying and fixing a Dependency Inversion violation (extracted a `TaskRepository`) and a Single Responsibility violation (split schemas from repository logic)
- FastAPI + Pydantic: path parameters vs. query parameters, `response_model`, status codes, request/response model separation (`TaskCreate` vs. `Task`)
- Pagination: limit/offset, and why Python list slicing is safer than manual indexing
- Python mechanics: `self` in instance methods, `**kwargs` (gathering vs. spreading), list slicing
- Git discipline: Conventional Commits, one commit per logical change
- ADRs: context → options → decision → consequences, written independently
- Documentation: README with architecture diagram, setup instructions, key decisions
- Self-checks: a Feynman explanation (caught and corrected a factual error about storage), a reimplementation-from-memory exercise, and a 10-question quiz (7 solid, 2 partial, 1 real gap — all closed)

## Concepts Learned Log

| Date | Level | Concept | Notes |
|---|---|---|---|

*(Add a row here whenever a new concept is genuinely understood — not just "touched.")*

## Level Checklist

- [x] **Level 0** — Cimientos (Clean Architecture skeleton, Docker Compose + Postgres, basic CI, first ADR)
- [ ] **Level 1** — Ingesta Crítica (IoT ingestion API, SOLID, TDD, idempotency, PostgreSQL)
- [ ] **Level 2** — Laberinto Asíncrono (Kafka/RabbitMQ, DI, Saga, CQRS, DLQ)
- [ ] **Level 3** — Leviatán de Datos (columnar storage, orchestration, observability)
- [ ] **Level 4** — Capa de IA (RAG over own docs, function calling into warehouse)
- [ ] **Boss Final** — full system, CI/CD, consolidated docs, real deploy
