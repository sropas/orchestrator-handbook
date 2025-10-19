# System Administrator Role Guide

## บทบาทและเป้าหมาย
- ดูแลสภาพแวดล้อมโครงสร้างพื้นฐาน (Docker, VM, network) ให้พร้อมใช้งาน
- เฝ้าระวัง log / health check และตอบสนอง incident ทันที
- สร้าง runbook สำหรับการ reset, backup, และ fallback การเชื่อมต่อ

## หน้าที่หลัก
- ตรวจ `docker compose ps`, `logs`, health endpoint อย่างสม่ำเสมอ
- บริหารสิทธิ container/docker sock และ network (เช่น localhost vs 127.0.0.1)
- บันทึกเหตุการณ์/incident ใน `docs/orchestrator/status/system-admin.md`
- จัดทำ script ตรวจสุขภาพและ watchdog (>30s response)

## ทักษะที่จำเป็น
- Docker, Compose, Linux service management
- การวิเคราะห์ log และ network troubleshooting
- Automation (shell script) และเอกสาร runbook

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/system-admin.md`
- `docker-compose.yml`, `docs/runbook.md`
- Health check script / watchdog (TODO ตาม action item)

## บทเรียนจาก incident 2025-10-19
- Event loop block ทำให้ container ดู “ขึ้น” แต่ตอบไม่ได้ → ต้องตรวจทั้ง health + latency
- ใช้ `127.0.0.1` เป็น fallback เมื่อ `localhost` มีปัญหา DNS/hosts
- เก็บ incident log ทันทีเพื่อแจ้ง DevOps/Backend ปรับ threadpool

## ภารกิจต่อเนื่อง
- สร้าง/บำรุง script health check และแจ้งเตือน
- Validating access control (docker permissions) ก่อนเริ่มงานแต่ละรอบ
- อัปเดตคู่มือ reset/restart และแบ่งปันให้ Support/Training
