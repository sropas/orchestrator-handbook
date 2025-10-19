# Security / PDPA Role Guide

## บทบาทและเป้าหมาย
- ปกป้องข้อมูลและรับรองการปฏิบัติตาม PDPA/TLS/Access control
- ติดตามการอนุมัติใช้ข้อมูลจริงและบริการภายนอก
- สื่อสารสถานะความปลอดภัยกับทีมอื่นอย่างสม่ำเสมอ

## หน้าที่หลัก
- ดูแล `docs/orchestrator/security-checklist.md` และ approval log
- ตรวจ `.env`, secrets, และจำกัด MIME/upload
- วางนโยบาย mock-first, TLS, auth สำหรับแต่ละสภาพแวดล้อม
- อัปเดต `docs/orchestrator/status/security.md` หลังแต่ละรอบการทดสอบ

## ทักษะที่จำเป็น
- Security baseline สำหรับเว็บ/API, PDPA compliance
- ความรู้การจัดการ secrets, TLS, network segmentation
- การสื่อสารข้อกำหนด/ข้อยกเว้นกับทีม dev/ops

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/security.md`
- `docs/orchestrator/security-checklist.md`, `docs/orchestrator/real-data-validation.md`
- Meeting note 2025-10-19 (mock-first policy, PDPA approval)

## บทเรียนจาก incident 2025-10-19
- ต้องประกาศสถานะ mock-first/PDPA ทันทีระหว่าง incident ไม่ปล่อยให้ “เงียบ”
- การบันทึก approval log ทำให้ตรวจสอบภายหลังง่าย
- ต้องทำคู่มือ response สำหรับ provider external ล่ม

## ภารกิจต่อเนื่อง
- ออกเอกสาร mock-first policy และแจ้งทุกบทบาท
- กำหนด timeline เปิด TLS/auth และอัปเดต decision log
- ตรวจสอบ log retention / incident response plan อย่างสม่ำเสมอ
