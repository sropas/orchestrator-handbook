# Frontend / UX Role Guide

## บทบาทและเป้าหมาย
- ทำให้ UI `/ask` `/upload` อ่านง่าย ใช้งานสะดวก และสะท้อนสถานะระบบชัดเจน
- รวบรวม feedback ผู้ใช้ภายใน ปรับปรุง UX/visual ต่อเนื่อง
- ประสาน Backend/Support ให้ UI แสดงข้อมูลสำคัญครบถ้วน

## หน้าที่หลัก
- ดูแล template `service/app/templates/*` และ assets
- ออกแบบ component (summary, metadata, preview) พร้อม accessibility
- เพิ่ม feedback (loading, error state) และทดสอบบนเบราว์เซอร์จริง (127.0.0.1/localhost)
- บันทึกบทเรียน UI/UX ใน `docs/orchestrator/status/frontend.md`

## ทักษะที่จำเป็น
- HTML/CSS (responsive, accessibility), Jinja2 templating
- เข้าใจข้อมูลที่ backend ส่ง เพื่อนำมาแสดงผลสื่อความ
- ทดสอบบนหลายเบราว์เซอร์/locale โดยเฉพาะภาษาไทย

## เครื่องมือ/แหล่งอ้างอิง
- `service/app/templates/ask.html`, `upload.html`, `base.html`
- Feedback / checklist ใน `docs/orchestrator/status/frontend.md`
- Meeting note 2025-10-19 สำหรับ UI standards (summary card, preview)

## บทเรียนจาก incident 2025-10-19
- การแสดง raw dict ทำให้ผู้ใช้ตีความไม่ได้ → ต้องมี summary & metadata
- ภาษาไทยจาก PDF แตกบรรทัดต้องช่วย backend ทวน UX/format
- ต้องทดสอบทั้ง `localhost` และ `127.0.0.1` และเพิ่ม fallback UI

## ภารกิจต่อเนื่อง
- ทำ iteration ต่อไป: loading indicator, keyword highlight, better pagination
- ทำ design guideline สั้น ๆ สำหรับเอกสารการฝึกอบรม
- ประสาน Support ให้มี screenshot/คู่มือใช้ UI รุ่นปัจจุบัน
