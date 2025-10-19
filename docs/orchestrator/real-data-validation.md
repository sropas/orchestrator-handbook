# Real Data Validation Plan

## Preconditions
- `.env` ถูกอัปเดตแล้วด้วย provider จริง (เช่น `EMBEDDING_PROVIDER=openai`, `OPENAI_API_KEY=...`) หรือโมเดลภายในที่อนุมัติแล้ว
- `docker compose up -d rag_service` รันและ `curl http://localhost:8000/health` ตอบกลับ `{"status":"ok", ...}`
- Postgres backup พร้อม:  
  ```bash
  docker compose exec postgres pg_dump -U "$POSTGRES_USER" "$POSTGRES_DB" > backups/ragdb-pre-realdata.sql
  ```
- security ปิดการเข้าถึงสาธารณะ (VPN/VLAN) และทีมได้รับอนุญาตให้ใช้ข้อมูลจริง

## ปริมาณงาน & Namespace
- สร้าง namespace `prod_validation` (หรือชื่อที่ PM กำหนด) สำหรับรอบทดสอบนี้
- เอกสารจริง 1–3 ชิ้น (PDF) ที่ผ่านการ masking/อนุมัติแล้ว เตรียมไว้ในเครื่อง dev

## ขั้นตอน (ดำเนินการตามเวลา)
| ลำดับ | ผู้รับผิดชอบ | รายละเอียด / คำสั่ง |
| --- | --- | --- |
| 1 | System Admin | เฝ้า `docker compose logs -f rag_service` และยืนยันว่าไม่มี error ก่อนเริ่ม |
| 2 | Backend + Database | รัน ingest ผ่าน CLI:<br>`tools/ingest_pdf_markitdown.py --pdf /secure/path/doc1.pdf --api http://localhost:8000 --namespace prod_validation --api_key "$API_KEY"`<br>หรือใช้สคริปต์ `scripts/run_real_validation.sh` (log จะอยู่ใน `logs/real-data-validation.log`)<br>บันทึกเวลาที่เริ่ม/จบ และจำนวน chunks |
| 3 | Security | ตรวจไฟล์/metadata ก่อน ingest ว่าไม่มี PII ที่ไม่ได้ mask; เมื่อ ingest เสร็จตรวจอีกครั้งด้วย query metadata เพื่อยืนยัน |
| 4 | QA/Test | ยิงคำถามตัวอย่าง 5 ข้อ (เช่น policy, process, FAQ) ผ่าน `/ask` หรือ `examples/curl_query.sh http://localhost:8000 prod_validation` และบันทึก latency + correctness ใน `docs/orchestrator/test-plan.md` |
| 5 | Backend | ถ้าตอบไม่ตรง ให้เก็บ request/response และพิจารณาปรับ chunk size, overlap หรือ metadata tagging |
| 6 | System Admin | หลังเสร็จรอบแรก บันทึกสถานะใน `docs/orchestrator/status/system-admin.md` และตรวจ volume ใช้งาน: `docker compose exec postgres psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -c "SELECT COUNT(*) FROM chunks WHERE namespace='prod_validation';"` |

## Post-Test
- QA สรุปผลใน `docs/orchestrator/status/qa.md` (เพิ่มหัวข้อ Real Data Validation)
- Security อัปเดต `docs/orchestrator/security-checklist.md` ว่าขั้นตอน PDPA ผ่านแล้ว/มีข้อสังเกต
- PM/SA สร้าง decision log entry (`docs/decisions/YYYY-MM-DD-real-data-validation.md`) ระบุผลการทดสอบและ action items ต่อไป
- หากต้อง rollback:  
  ```bash
  docker compose exec postgres psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -c "DELETE FROM chunks WHERE namespace='prod_validation';"
  docker compose exec postgres psql -U "$POSTGRES_USER" -d "$POSTGRES_DB" -c "DELETE FROM documents WHERE namespace='prod_validation';"
  # หรือ restore จาก backup ที่ทำไว้
  ```

## Checklist (ติ๊กโดย PM)
- [ ] Backup pre-run สร้างแล้ว
- [ ] Security approve เอกสาร
- [ ] Ingest เสร็จ (จำนวน chunks: ____ )
- [ ] QA บันทึกผล 5 คำถาม + latency
- [ ] System Admin ไม่มี error ใน log
- [ ] Decision log อัปเดตเรียบร้อย
