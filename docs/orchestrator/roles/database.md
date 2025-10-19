# Database Engineer Role Guide

## บทบาทและเป้าหมาย
- ดูแล PostgreSQL + pgvector ให้พร้อมรองรับข้อมูลจริงและปรับจูนประสิทธิภาพ
- ประสาน Backend/QA ตรวจความถูกต้องของข้อมูลหลัง ingest/query
- กำหนดนโยบาย backup, vacuum, และ sizing สำหรับแต่ละสภาพแวดล้อม

## หน้าที่หลัก
- เฝ้าตาราง `documents`, `chunks`, index, และ extension vector
- เตรียม backup (`pg_dump`), vacuum, และ retention policy
- สนับสนุน Real Data Validation โดยตรวจจำนวน chunks/metadata
- บันทึกสถานะใน `docs/orchestrator/status/database.md`

## ทักษะที่จำเป็น
- PostgreSQL administration, pgvector tuning
- วิเคราะห์ query plan, vacuum/autovacuum settings
- สื่อสารกับทีมอื่นเพื่อแจ้ง risky migration หรือ sizing

## เครื่องมือ/แหล่งอ้างอิง
- `docs/orchestrator/status/database.md`
- Deploy/backup แนวทางใน `docs/orchestrator/deploy-options.md`
- Checklist ใน `docs/orchestrator/security-checklist.md`

## บทเรียนจาก incident 2025-10-19
- ต้องตรวจยืนยัน ingest หลัง incident เพื่อมั่นใจว่าข้อมูลไม่เสีย
- Metadata ที่ normalize แล้วช่วยค้นหาช่องโหว่/ปัญหาได้เร็วขึ้น
- จำเป็นต้องมี sizing และแผน index tuning สำหรับงานจริง

## ภารกิจต่อเนื่อง
- ร่วม QA บันทึก chunk count ทุก namespace สำคัญ
- จัดทำคู่มือ vacuum/index tuning สำหรับ production
- สร้างสคริปต์ monitor พื้นที่/latency และแชร์ให้ DevOps
