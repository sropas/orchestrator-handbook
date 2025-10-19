# Database/Infra DB Report — 2024-06-11

## งานที่ทำ
- ตรวจสอบ schema `documents`, `chunks` หลัง migration (metadata → metadata_json)
- PGVector extension พร้อมใช้งานบน image ใหม่ `pgvector/pgvector:pg16`
- Index ตรวจสอบแล้ว: `idx_chunks_namespace`, `idx_chunks_metadata_json`, `idx_chunks_embedding`

## ประเด็นสำคัญ
- ต้องกำหนดค่าพารามิเตอร์ IVFFLAT (lists/probes) ให้เหมาะกับขนาดข้อมูลจริง (ปัจจุบัน default)
- Backup strategy ยังไม่ได้ระบุใน runbook (ต้องเพิ่มคำสั่ง `pg_dump` + retention policy)

## งานถัดไป
1. สนับสนุน Real Data Validation (ดู `docs/orchestrator/real-data-validation.md`) พร้อม monitor จำนวน chunks/metadata คลาดเคลื่อนหรือไม่
2. ทำเอกสาร vacuum/auto-analyze policy ใน `docs/orchestrator/security-checklist.md`
3. คำนวณ sizing (disk, memory) สำหรับ production; บันทึกใน decision log

## หมายเหตุ Real Data Validation (เติมหลังทดสอบ)
- ผลการตรวจสอบ `prod_validation` (จำนวน chunks / metadata): 
- ข้อเสนอแนะด้าน index หรือ vacuum: 
