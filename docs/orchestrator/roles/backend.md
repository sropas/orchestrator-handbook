# Backend Engineer Role Guide

## บทบาทและเป้าหมาย
- พัฒนาและดูแล FastAPI RAG service ให้เสถียร ปลอดภัย และทดสอบได้
- จัดการ ingestion/query pipelines รวมถึง embedding provider และ database layer
- ประสานงานกับ QA/DB เพื่อประกันคุณภาพข้อมูลและประสิทธิภาพ

## หน้าที่หลัก
- ดูแลโค้ด `service/app/*`, schema, ORM, และ integration กับ pgvector
- จัดทำ unit/integration tests (เช่น `test_chunk_text`, `test_rag_query`)
- บริหารคีย์/การตั้งค่า embedding provider (mock vs external)
- ตรวจสอบและอัปเดตรันบุ๊ก/decision log ด้าน backend ที่เกี่ยวข้อง

## ทักษะที่จำเป็น
- Python (FastAPI, SQLAlchemy), Pydantic, async/sync patterns
- ความรู้ฐานข้อมูล PostgreSQL + pgvector
- การออกแบบ API, error handling, logging

## เครื่องมือ/แหล่งอ้างอิง
- `service/app/` modules, `Dockerfile`, `requirements.txt`
- `docs/orchestrator/status/backend.md`
- `docs/orchestrator/test-plan.md` (ร่วมกับ QA)

## บทเรียนจาก incident 2025-10-19
- งาน synchronous (embedding) ใน handler async ต้องโอนลง `run_in_threadpool`
- Normalize ข้อความภาษาไทยก่อนเก็บ/ตอบเพื่อให้อ่านได้จริง
- ต้องมี mock provider พร้อมใช้งานเมื่อ external API ล่ม

## ภารกิจต่อเนื่อง
- เขียน/อัปเดต unit test สำหรับ `normalize_extracted_text` และ query flows
- ร่วมกับ QA สร้าง test data จริง และ snapshot UI/response
- สื่อสารกับ DevOps เรื่อง metrics/health endpoint และ mock-first policy
