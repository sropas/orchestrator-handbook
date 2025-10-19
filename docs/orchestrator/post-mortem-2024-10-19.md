# Post-Mortem: Real Data Validation Round (2025-10-19)

## ผู้เข้าร่วม
- Backend Engineer
- Database Engineer
- QA/Test Engineer
- DevOps Engineer
- Security Manager
- System Administrator
- Project Manager / System Analyst
- Accounting Manager
- Support & Training (ST) Manager

## สรุปเหตุการณ์
- ระหว่างอัปโหลด PDF จริงครั้งแรก (`/upload`), ระบบตอบกลับ 500 เพราะ dependency ไม่ตรง (`httpx` กับ `openai`) และเครือข่ายออกนอกไม่ได้ → หน้า UI ค้าง
- ทีมใช้เวลาแก้ไขหลายรอบจากการขาดการสื่อสารเรื่องค่า `.env`, network และการ rebuild image → ทำให้การทดสอบจริงล่าช้า

## ปัจจัยที่ทำให้เกิดเหตุการณ์
### ใครทำพลาดอะไรบ้าง (ตรงไปตรงมา)
- **Backend:** ไม่ตรวจสอบ dependency compatibility (httpx vs openai) และไม่ได้ใส่ try/except ที่ละเอียดพอ ทำให้ error message ดูยาก
- **DevOps:** ไม่ rebuild image ใหม่พร้อม `httpx` locked version ในทันที ส่งผลให้ dependency เก่ายังแคชอยู่
- **QA:** ไม่ทวนผลลัพธ์ `/upload` หลังแก้ไขแต่ละรอบ ทำให้หน้า UI ค้างโดยไม่แจ้งทีมทันที
- **System Admin:** ไม่ได้จัดการอนุญาต docker socket ให้ทุก terminal ทำให้บางคนรันคำสั่งล้มเหลว ซ้ำซ้อน
- **Security:** ไม่แจ้งให้ Backend ทดสอบด้วย provider mock ก่อนอนุมัติข้อมูลจริง → เกิดความเสี่ยงเสียเวลาบน production key
- **PM:** ไม่เก็บรายละเอียดขั้นตอน/ข้อผิดพลาดแบบ real-time ส่งผลให้การแก้ไขด่านหน้าไม่เห็นภาพรวมรวดเร็ว

## สิ่งที่ควรทำให้ดีขึ้นในการทดสอบครั้งถัดไป
1. **Backend** – เพิ่ม health-check ว่า embedding provider ใช้งานได้ (เช่น endpoint `/embedding/status`) ก่อนเปิดให้ upload จริง; ใส่ retry + error logging ที่อ่านง่าย
2. **DevOps** – บังคับใช้ `docker compose build --no-cache` ใน runbook เมื่อแก้ requirements; เพิ่ม preflight script ตรวจ dependency; บันทึกการ rebuild ทุกครั้ง
3. **QA** – ทำ checklist หลังแก้ไข: รัน `/upload` + `/query` แล้วบันทึกผลทันที; ตั้ง alert เมื่อ UI ค้างเกิน 20 วินาที
4. **System Admin** – ตั้ง script `scripts/tail_services.sh` ให้เปิด log พร้อมกัน และตรวจสิทธิ์ docker socket ให้ทุกผู้ใช้ก่อนเริ่ม
5. **Security** – บังคับใช้ขั้นตอน “ทดสอบด้วย mock provider” ทุกครั้งก่อนเปิดคีย์จริง, อัปเดต PDPA checklist ในวันเดียวกัน
6. **PM/SA** – ตั้ง daily sync สั้น ๆ เพื่อเก็บ status + blockers, และเขียน decision log ระหว่างเกิดปัญหา
7. **Accounting Manager** – ทวนเวลา dev ใช้ไปกับ troubleshooting เพื่อคำนวน cost; ต้องมีช่องบันทึกเวลาทุกสัปดาห์
8. **Support & Training Manager** – สรุปบทเรียนเพื่อใช้ฝึกทีม (เช่น วิธีตรวจ log, วิธีปรับ `.env` ไป mock → real provider)

## Action Items (มอบหมายพร้อม Due Date)
- [ ] Backend: เพิ่ม endpoint `/embedding/status` + detailed logging (Due: 2025-10-22)
- [ ] DevOps: เพิ่ม `build-no-cache` step ใน runbook + script (Due: 2025-10-21)
- [ ] QA: ปรับ test plan ให้มี section “UI responsiveness” (Due: 2025-10-20)
- [ ] System Admin: สร้าง script tail log + จัดสิทธิ์ docker (Due: 2025-10-20)
- [ ] Security: อัปเดต PDPA checklist + mock-first policy (Due: 2025-10-21)
- [ ] PM: สรุปบทเรียนและเผยแพร่ใน meeting หลังสุดสัปดาห์ (Due: 2025-10-20)
- [ ] Accounting Manager: รวบรวมเวลาที่ใช้แก้ปัญหาและประเมินค่าใช้จ่าย (Due: 2025-10-22)
- [ ] Support & Training: จัด session สั้น ๆ “How to validate RAG stack” (Due: 2025-10-24)

## การติดตามผล (PM จะอัปเดต)
- ทุกข้อ Action ต้องเข้า backlog/issue tracker พร้อมผู้รับผิดชอบ
- รายงานความคืบหน้าในการประชุมถัดไป (ใช้ `docs/meeting-template.md`)
