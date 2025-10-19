# PM / System Analyst Role Guide

## บทบาทและเป้าหมาย
- กำหนดวิสัยทัศน์/ขอบเขตโครงการและจัดลำดับ sprint
- ประสานงานทุกบทบาทให้ตรงกับ WBS และ runbook
- ทำ decision log, รายงานต่อผู้มีส่วนได้ส่วนเสีย และเตรียมเอกสาร briefing

## หน้าที่หลัก
- ติดตามความคืบหน้า sprint รายวัน ใช้ `docs/meeting-template.md`
- รวบรวมผล Real Data Validation (namespace, เวลาทดสอบ, approval security)
- อัปเดต `docs/orchestrator/status/*.md` และ `docs/decisions/`
- จัดประชุมเร่งด่วนเมื่อเกิด incident และสรุป post-mortem

## ทักษะที่จำเป็น
- การจัดการโครงการเชิง Agile, WBS, backlog grooming
- ความเข้าใจ RAG stack ระดับ high-level (API, DB, n8n flows)
- การสื่อสาร / facilitator สำหรับ multi-role workshop

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/pm_sa.md`
- Decision log และ `docs/orchestrator/meeting-template.md`
- `docs/orchestrator/wbs-plan.md`, `docs/runbook.md`

## บทเรียนจาก incident 2025-10-19
- ต้องมี incident log ทันทีเพื่อ briefing ทีมอื่น
- ตั้ง recurring sync 15 นาทีเพื่อลดการวิ่งไล่ถาม
- จัด checkpoint เรื่อง mock provider / health check ให้ทุก sprint

## ภารกิจต่อเนื่อง
- บันทึกคะแนนประเมินทีมและแจ้ง Accounting
- ตรวจว่าทุกบทบาทอัปเดต status file หลังงานใหญ่
- รักษา repository นี้ให้พร้อม reuse ในโครงการถัดไป
