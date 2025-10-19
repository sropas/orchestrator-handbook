# QA/Test Report — 2024-06-11

## งานที่ทำ
- Smoke test manual: `/health`, `/ask`, `/upload` with sample.pdf (ขนาด ~100KB) → ผ่านครบ
- ตรวจ log หลังอัปโหลด ไม่มี error ขณะสร้าง embeddings (ใช้ mock provider)

## ประเด็นสำคัญ
- ต้องบันทึกผล smoke test ใน `docs/orchestrator/test-plan.md` (เช่นเวลาที่ใช้, namespace)
- ยังกำหนดไม่ได้ว่าชุด pytest integration จะรันใน CI เพราะยังไม่มี containerized testing pipeline

## งานถัดไป
1. ทำ Real Data Validation ร่วมกับ Backend/DB (ดู `docs/orchestrator/real-data-validation.md`) และบันทึก latency/result 5 คำถาม
2. เพิ่ม section “Smoke Test Results” ใน `docs/orchestrator/test-plan.md`
3. สร้างไฟล์ `tests/test_chunk_text.py`, `tests/test_rag_query.py` สำหรับ unit test base

## หมายเหตุ Real Data Validation (เติมหลังทดสอบ)
- Namespace: `prod_validation`
- คำถามที่ใช้:
  1. 
  2. 
  3. 
  4. 
  5. 
- Latency โดยเฉลี่ย: 
- ข้อสังเกต/Issue:
