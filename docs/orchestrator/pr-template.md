# Pull Request Template

## Summary
- [ ] อธิบายการเปลี่ยนแปลงหลัก (What / Why)
- [ ] ลิงก์ไปยัง issues หรือ requirement tickets

## Validation
- [ ] `python -m pytest`
- [ ] `python -m ruff check service tools`
- [ ] `python -m black service tools`
- [ ] `docker compose up -d --build` (ถ้ามีการแก้ infra)
- [ ] Test cases หรือ screenshots ประกอบ (UI / API / n8n)

## Security / PDPA Impact
- [ ] มีข้อมูล PII ถูกเพิ่ม/แก้ไขหรือไม่
- [ ] ต้องอัปเดตเอกสาร PDPA/OWASP หรือไม่

## Checklist
- [ ] อัปเดต docs/runbook หรือ decision log (ถ้ามีผลกระทบ)
- [ ] เพิ่ม/อัปเดต tests coverage
- [ ] Definition of Done checklist ผ่านทุกข้อ
