# QA / Test Engineer Role Guide

## บทบาทและเป้าหมาย
- ยืนยันว่าระบบ RAG ทำงานตามความคาดหวังทั้งเชิงฟังก์ชันและ UX
- สร้าง test data/เคสที่ใกล้เคียงผู้ใช้จริง พร้อมครอบ parameter หลากหลาย
- ทำรายงานผลทดสอบและบันทึกลง test plan รองรับ auditing

## หน้าที่หลัก
- เตรียม smoke test (health/ask/upload) และบันทึกผลใน `docs/orchestrator/test-plan.md`
- ออกแบบ unit/integration tests (pytest) ครอบ ingest/query/mocking
- ทำ Real Data Validation ร่วมกับ Backend/DB/Security
- รายงานสถานะใน `docs/orchestrator/status/qa.md`

## ทักษะที่จำเป็น
- การออกแบบ test case เชิง scenario และ property-based (parameter variant)
- pytest, mock, และการตรวจ log/metrics
- ความเข้าใจ RAG pipeline เพื่อจำลอง input/output ได้เหมือนผู้ใช้

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/test-plan.md`
- `docs/orchestrator/status/qa.md`
- Meeting note 2025-10-19 (บทเรียนเรื่องการเทส UX)

## บทเรียนจาก incident 2025-10-19
- ตรวจเฉพาะ HTTP 200 ไม่พอ ต้องดู UX/format ด้วย
- จำเป็นต้องสร้าง test data ภาษาไทยและ parameter หลากหลาย
- ต้องจดผลทดสอบทุกครั้งเพื่อให้ PM/Accounting ใช้ติดตาม effort

## ภารกิจต่อเนื่อง
- อัปเดต test plan ด้วยเคสจริงและเวลาทดสอบ
- สร้าง automation coverage สำหรับ normalize text และ threadpool flow
- ประสาน Support เพื่อทำ training “ทดสอบเหมือนผู้ใช้จริง”
