# Definition of Done (DoD)

1. โค้ดผ่าน lint (`ruff`) และ format (`black`) และ unit tests (pytest)
2. ไม่มี warning/error ใน `docker compose up` logs ระหว่าง smoke test
3. เอกสารที่เกี่ยวข้องถูกอัปเดต (README, runbook, decision log, security checklist)
4. มี test coverage สำหรับฟีเจอร์ใหม่ ≥85% หรือมีการทำ reasoning ว่าทดสอบไม่ได้
5. ไม่มี secrets หรือ credentials hard-code ในโค้ดหรือ commit
6. Checklist PDPA/OWASP ที่เกี่ยวข้องได้รับการอัปเดต/ตรวจสอบแล้ว
7. Reviewer อย่างน้อย 1 คนอนุมัติ และ CI pipeline ผ่านทุก job
8. Deploy (staging) สำเร็จหรือมีแผน deploy พร้อม rollback ชัดเจน
