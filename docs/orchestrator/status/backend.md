# Backend Report — 2024-06-11

## งานที่ทำ
- ปรับโค้ดให้ใช้ `pydantic-settings` (แก้ BaseSettings) และแก้ฟิลด์ `metadata` → `metadata_json`
- สลับ postgres image ไป `pgvector/pgvector:pg16`
- ประทับ schema ผ่าน `/upload` UI พร้อมตรวจ insert ใน database

## ปัญหา/ความเสี่ยง
- Latency สูงเมื่อใช้ embedding provider ที่ต้องพึ่ง external API (ยังไม่มี key) → ทางแก้: ตั้งค่า `.env`
- ยังไม่มี unit tests ครอบคลุม ingest/query logic → ต้องเพิ่มก่อน merge ฟีเจอร์ใหม่

## งานถัดไป
1. ทำ Real Data Validation ตาม `docs/orchestrator/real-data-validation.md` (ingest namespace `prod_validation` ร่วมกับ QA/DB)
2. เขียน unit test `test_chunk_text` และ `test_upsert_generates_embeddings` (mock provider)
3. สร้าง endpoint `/metrics` (optional) เพื่อให้ monitoring ใน sprint ถัดไป

## หมายเหตุ Real Data Validation (เติมหลังทดสอบ)
- Namespace: `prod_validation`
- ไฟล์ที่ ingest: 
- Chunk count: 
- เวลาเริ่ม / จบ: 
- ปัญหาที่พบ: 

## Retrospective Notes (2025-10-19)
- Dependency mismatch (httpx/openai) ควรถูกจับได้ตั้งแต่ Dev → ต้องเพิ่ม test pipeline
- เมื่อคีย์จริงใช้ไม่ได้ ให้ switch เป็น mock provider ทันทีเพื่อยืนยัน flow
- ปรับโค้ดให้ตรวจจับ error ของ embedding provider พร้อมแนะนำขั้นตอนแก้ไขอัตโนมัติ (เช่น log ว่าให้ rebuild --no-cache)
