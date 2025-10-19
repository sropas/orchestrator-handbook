# Support & Training Role Guide

## บทบาทและเป้าหมาย
- แปลงเทคนิคไปเป็นคู่มือ/บทเรียนให้ผู้ใช้และทีมใหม่
- จัด workshop/สื่อสารวิธีใช้ระบบ RAG และขั้นตอน fallback
- รวบรวม feedback จากผู้ใช้เพื่อป้อนกลับให้ทีมพัฒนา

## หน้าที่หลัก
- สร้างคู่มือ/สไลด์ใน `docs/orchestrator/` (เช่น workshop, cheat sheet)
- บันทึกสถานะใน `docs/orchestrator/status/support-training.md`
- ประสานงานกับ PM และ Accounting เรื่องเวลาฝึกอบรม
- จัด training เมื่อมี UI/flow ใหม่ (เช่น การ์ดผลลัพธ์ RAG)

## ทักษะที่จำเป็น
- การสื่อสาร/อบรม, สร้างเอกสารและสื่อ multimedia
- เข้าใจขั้นตอนระบบ end-to-end เพื่ออธิบายให้ผู้ใช้เข้าใจง่าย
- เก็บ feedback และวิเคราะห์เป็น actionable items

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/support-training.md`
- Meeting note 2025-10-19 (action item: คู่มือการอ่านผลลัพธ์)
- `docs/runbook.md` และ UI screenshot ล่าสุด

## บทเรียนจาก incident 2025-10-19
- ต้องมีสื่ออัปเดตเมื่อ UI เปลี่ยน (จาก raw dict → card) เพื่อเลี่ยงความสับสน
- จำเป็นต้องอธิบาย mock-first/fallback ให้ user รู้ว่าจะเกิดอะไร
- จัด session “Validating RAG Stack” ต่อเนื่องสำหรับทีมใหม่

## ภารกิจต่อเนื่อง
- เตรียมคู่มือ “อ่านการ์ดผลลัพธ์ RAG” และแชร์ภายใน
- อัดวิดีโอ/เอกสาร training สำหรับการทดสอบก่อน deploy
- ประสาน QA/Backend สำหรับตัวอย่างคำถามจริงและผลลัพธ์ที่ถูกต้อง
