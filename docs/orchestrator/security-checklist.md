# Security & PDPA Checklist

## OWASP Top 10 (โฟกัส)
- [ ] A01: Injection — ใช้ parametrized queries ผ่าน SQLAlchemy, sanitize metadata
- [ ] A02: Broken Auth — เปิดใช้งาน API key/JWT, สามารถเพิ่ม OAuth2 ในอนาคต
- [ ] A03: Sensitive Data Exposure — รันใน VPN, เปิด TLS ผ่าน reverse proxy, ปิดไม่ให้ส่ง plain text log ที่มี PII
- [ ] A04: XML External Entities — ไม่รองรับ XML, ป้องกันไม่ให้ผู้ใช้โหลดไฟล์นามสกุลอื่น
- [ ] A05: Broken Access Control — แยก role สำหรับ ingest/query, RBAC ใน n8n
- [ ] A06: Security Misconfiguration — ใช้ Infrastructure as Code, กำหนดค่าด้วย `.env`, disable default creds
- [ ] A07: XSS — sanitize/escape ข้อมูลใน template, ใช้ CSP header หากไปหน้า frontend SPA
- [ ] A08: Insecure Deserialization — ไม่ deserialize objects ที่ไม่ trust ใน API
- [ ] A09: Components w/ Known Vulnerabilities — รัน `pip-audit`, `npm audit`, watch CVE trend
- [ ] A10: Insufficient Logging & Monitoring — เก็บ access logs, audit events, ส่ง alert ผ่าน Prometheus/Loki/ELK

## PDPA Considerations
- [ ] Data Minimization — Ingest เฉพาะข้อมูลที่จำเป็นสำหรับ QA
- [ ] Consent & Purpose — บันทึกกระบวนการและเอกสารว่าข้อมูลถูกนำมาใช้เพื่ออะไร
- [ ] Subject Access — เตรียมช่องทางให้ลบ/ถอนข้อมูลได้ตาม namespace
- [ ] Retention — กำหนดการเก็บข้อมูล (เช่น 180 วัน) และลบอัตโนมัติผ่าน cron job
- [ ] Data Classification — ติด tag (sensitivity) ใน metadata เพื่อตรวจสอบระดับความลับ
- [ ] Access Control — ใช้ API key/Token ต่อ user group, log ทุก request ใน Postgres + n8n
- [ ] Incident Response — จัดทำ playbook เมื่อข้อมูลรั่วไหล, แจ้งผู้มีอำนาจตามกฎหมาย

## Secrets Handling
- [ ] ใช้ `.env` ใน dev แต่สำหรับ prod ให้ใช้ Vault/Secret Manager
- [ ] จำกัดสิทธิ์ไฟล์ `.env` (`chmod 600`)
- [ ] ใช้ Docker secrets หรือ K8s secrets ใน production
- [ ] Rotate API keys ทุก 90 วัน, ตรวจสอบ commit ไม่ให้ push secrets ขึ้น Git

## Logging & Monitoring
- [ ] เชื่อมต่อ Prometheus/Grafana เพื่อตรวจ health และ latency
- [ ] จัดเก็บ log ใน Loki/ELK, กำหนด retention policy
- [ ] สร้าง alert rule สำหรับ error rate > X% หรือ query latency > 2s
