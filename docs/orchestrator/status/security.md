# Security & PDPA Report — 2024-06-11

## งานที่ทำ
- ตรวจ `docker-compose.yml` และ `.env.example` ให้ไม่มี secrets จริง; `.env` ไม่ถูก commit
- ยืนยัน upload จำกัด MIME type เป็น `application/pdf`

## ประเด็นสำคัญ
- ยังไม่มี TLS / reverse proxy → ต้องวางแผนสำหรับ production (Traefik/Nginx)
- ควรเพิ่ม auth บางส่วนสำหรับ `/ask`/`/upload` (API key หรือ basic auth) ก่อนเปิดให้ user กว้าง

## งานถัดไป
1. Approve ข้อมูลจริงก่อน Real Data Validation และบันทึกรายละเอียดไว้ใน `docs/orchestrator/real-data-validation.md`
2. อัปเดต checklist `docs/orchestrator/security-checklist.md` ช่องที่ทำแล้ว
3. ร่าง policy สำหรับ log retention และ incident response (เพิ่มใน security checklist)

## หมายเหตุ Real Data Validation (เติมหลังทดสอบ)
- เอกสารที่อนุมัติ: 
- การตรวจ metadata หลัง ingest: 
- Risk / Mitigation: 
