# DevOps Role Guide

## บทบาทและเป้าหมาย
- ดูแล CI/CD, container build, และ infrastructure automation ให้ทีมใช้ได้สะดวก
- เฝ้าแผน backup, cleanup, และ rollback ของ Docker/Postgres
- ประสาน System Admin/Backend เพื่อให้ health check และ monitoring ทำงาน

## หน้าที่หลัก
- ดูแล `docker-compose.yml`, image build (`osadmin-rag_service`)
- จัด script สำหรับ log/cleanup (`docker compose logs`, `down -v`, backup volume)
- เตรียม/ดูแล workflow CI (`.github/workflows/ci.yml`) และ secrets
- อัปเดต `docs/orchestrator/status/devops.md`, runbook, deploy options

## ทักษะที่จำเป็น
- Docker, Compose, CI (GitHub Actions), shell scripting
- พื้นฐาน monitoring/log aggregation
- ประสานงาน incident และสื่อสารกับทุกบทบาท

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/devops.md`
- Deploy playbook `docs/orchestrator/deploy-options.md`
- Runbook / health check scripts (TODO)

## บทเรียนจาก incident 2025-10-19
- ต้องจับ latency/timeout เร็ว (health watchdog >30s)
- Rebuild ด้วย `--no-cache` เมื่อ dependency เปลี่ยน หรืองานล่ม
- ต้องเตรียม mock-first + fallback network (127.0.0.1) ร่วมกับ System Admin

## ภารกิจต่อเนื่อง
- สร้าง script health-check/report สำหรับ host และ container
- Integrate monitoring (Prometheus/Grafana) และแจ้งเตือน
- เตรียมแผน push ไป repo + CI rollout พร้อมกฎ security
