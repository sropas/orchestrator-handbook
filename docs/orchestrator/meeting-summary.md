# ทีม Orchestrator: การประชุมเปิดโครงการ n8n-local

- **Project:** n8n-local — Self-hosted RAG stack + n8n automation
- **Date:** 2024-06-11
- **Attendees:**
  - Project Manager (PM)
  - System Analyst (SA)
  - Senior Backend (Python/PHP/JS)
  - Senior Frontend (CSS/JS)
  - Senior Database Engineer
  - DevOps Engineer
  - QA/Test Engineer
  - Document & Paper Owner
  - Infra Manager (VM/Linux/Docker/K8s)
  - System Administrator (Monitoring/Operations)
  - Security Manager (OWASP/PDPA)
  - Accounting Manager (Budget/Cost Tracking)
  - Support & Training Manager (ST Manager)

## 1. Requirement & Context Summary
- **Core Goal:** ให้ธุรกิจใช้งาน RAG (PDF ingest + hybrid search) และรัน workflow ด้วย n8n ภายในองค์กร โดยไม่ใช้บริการ SaaS ภายนอก
- **Scope:**
  - Self-hosted stack: PostgreSQL + pgvector, FastAPI, web UI (ask/upload), n8n
  - รองรับ PDF, URL, future JSON ingest
  - Query service พร้อม citations, metadata filters, optional rerank
  - UI พร้อมสอบถามและอัปโหลดฝั่งผู้ใช้ภายใน
- **Traffic & Scale (สมมติฐาน):** 50 concurrent users, 10K docs, ~20GB embeddings, latency <1.5s ต่อคำถาม
- **PDPA & Security:** ข้อมูลภายใน, ต้องรันใน VPN/VLAN ปิด, audit log, retention 180 วัน, exportable reports
- **Assumptions:** มี Docker, Kubernetes option ในอนาคต, มี team Ops ดูแล backup & monitoring

## 2. ความเสี่ยงหลัก
- Latency จาก external embedding providers → มี fallback sentence-transformers
- ขนาดฐานข้อมูลโตเร็ว ต้องจัดการ IVFFLAT และ autovacuum
- PDPA: ต้อง mask ข้อมูล PII ก่อน ingest, กำหนด role-based access
- n8n flows อาจเข้าถึงข้อมูลจำนวนมาก ต้องมี RBAC และ audit
- ความพร้อมทีม: ต้องอบรม RAG และ pgvector พื้นฐาน

## 3. ข้อกำหนดเชิงเทคนิค
- Postgres 16+, pgvector 0.5+, extension vector + pgcrypto
- FastAPI backend + SQLAlchemy, async-ready, CORS configurable
- Storage: metadata JSONB, vector index IVFFLAT, fallback GIN
- CI/CD: GitHub Actions, lint/test build, deploy via Docker/K8s
- Infra: รองรับ Docker Compose สำหรับ dev/staging, Terraform + Ansible optional สำหรับ prod

## 4. ปฏิทินการทำงาน (สัปดาห์แรก)
- วัน 1: Setup infra dev, docker compose, seed DB
- วัน 2: Ingest pipeline, PDF upload UI, API tests
- วัน 3: Query API + citations, integrate n8n (basic flows)
- วัน 4: Security hardening, PDPA checklist, monitoring hooks
- วัน 5: E2E tests, smoke plan, deploy to staging, retrospective

## 5. Action Items
- PM: สร้าง backlog ใน GitHub Projects, mapping WBS
- SA: ร่าง system context diagram + ERD (docs/architecture/)
- Backend: Implement API skeleton (done), test harness
- Frontend: สร้าง landing/ask/upload UI (ต่อยอดจาก template)
- Database: Tune pgvector parameters, vacuum strategy
- DevOps: เตรียม CI/CD workflows, artifact registry
- QA: กำหนด test scenarios และ automation plan
- Docs Owner: รวบรวม meeting notes, DoD, PR template
- Infra Manager: วาง plan deploy (Docker Compose → K8s)
- Security: ทำ OWASP/PDPA checklist และ access policy (API key/JWT)
