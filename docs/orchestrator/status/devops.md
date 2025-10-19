# DevOps Report — 2024-06-11

## งานที่ทำ
- แก้ Docker Compose ให้ใช้ `pgvector/pgvector:pg16`
- Build image สำเร็จ (`osadmin-rag_service`) และขึ้น container ทั้งสามได้ (postgres / rag_service / n8n)
- ตรวจ log `docker compose logs -f rag_service` ยืนยันไม่มี error หลัง startup

## ประเด็นสำคัญ
- สัปดาห์หน้าเตรียม push ขึ้น repo และให้ CI `.github/workflows/ci.yml` ทำงานจริง
- ต้องเพิ่มคำสั่งสำหรับ clean-up volumes ใน runbook (`docker compose down -v`) เมื่อ reset stack

## งานถัดไป
1. สร้าง script `scripts/dev_logs.sh` ดึง log service ตามชื่อ (เพื่อให้ทีมเรียกง่าย)
2. วางขั้นตอน backup volume `postgres_data` (tar / pg_dump) ใน runbook
3. เตรียม secrets (GitHub Actions) สำหรับ deploy script เมื่อ repo push ขึ้น remote
