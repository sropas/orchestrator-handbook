# Work Breakdown Structure & ไทม์ไลน์ย่อ (นาน 4 สัปดาห์)

| สัปดาห์ | บทบาทหลัก | Tasks | Deliverables |
| --- | --- | --- | --- |
| Week 1 | PM, SA, Backend, Database | - เก็บ requirements<br>- ร่าง ERD + API schema<br>- ตั้งค่า docker-compose, init.sql, FastAPI skeleton<br>- Runbook, meeting template | - docs/orchestrator/meeting-summary.md<br>- sql/init.sql พร้อม extension<br>- service/app/main.py skeleton |
| Week 2 | Backend, Frontend, DevOps | - พัฒนา `/upsert`, `/query`, ingest script<br>- UI ask/upload (Jinja)<br>- docs runbook & CI template<br>- ตั้งค่า `.env.example` | - tools/ingest_pdf_markitdown.py<br>- service/app/rag.py<br>- .env.example, README.md |
| Week 3 | QA, Security, Infra | - Test plan (unit/integration/smoke)<br>- Security checklist (OWASP/PDPA)<br>- Draft CI/CD YAML (lint/test/build)<br>- Prepare deploy commands (Docker, Ansible, K8s) | - docs/orchestrator/test-plan.md<br>- .github/workflows/ci.yml (draft)<br>- docs/orchestrator/security-checklist.md |
| Week 4 | PM, DevOps, Infra, Docs | - Dry-run staging deploy<br>- Smoke tests, bugfix<br>- Document DoD + PR template<br>- Retrospective + backlog grooming | - ci-cd pipeline ready<br>- docs/orchestrator/pr-template.md<br>- docs/orchestrator/dod.md |

## Task Assignments (รายบทบาท)
- **Project Manager:** manage backlog, stakeholder updates, ensure PDPA compliance sign-off
- **System Analyst:** maintain ERD, data flow diagrams, update runbook assumptions
- **Backend Engineer:** RAG pipeline, API endpoints, unit tests, performance tuning
- **Frontend Engineer:** enhance ask/upload UI, future React/Vite migration plan
- **Database Engineer:** pgvector tuning, vacuum/autovacuum policy, backup scripts
- **DevOps Engineer:** docker-compose maintenance, GitHub Actions, deploy scripts
- **QA/Test:** test pyramid design, build pytest suite, smoke checklist execution
- **Docs Owner:** maintain docs in `docs/`, gather meeting outcomes, update README
- **Infra Manager:** define VM sizing, load balancer setup, optional K8s manifests
- **Security Manager:** OWASP Top 10 checklist, secrets management, audit logging plan

## Key Milestones
- **M1 (Day 5):** Dev stack up (docker compose + basic ingest/query)
- **M2 (Day 10):** Basic UI + n8n flow integrated
- **M3 (Day 15):** CI pipeline green + security checklist drafted
- **M4 (Day 20):** Smoke tests pass, ready for staging deploy
- **M5 (Day 25):** Staging sign-off, prepping prod handover
