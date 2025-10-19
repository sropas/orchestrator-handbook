# สถาปัตยกรรม/เทคโนโลยีตัวเลือก (A/B/C)

| ปัจจัย | ทางเลือก A — Docker Compose Monolith | ทางเลือก B — Microservices + K8s | ทางเลือก C — Managed Cloud + Hybrid |
| --- | --- | --- | --- |
| ภาพรวม | PostgreSQL + FastAPI + n8n อยู่ใน docker-compose เดียว, deploy ด้วย Docker Compose | แยก PostgreSQL, FastAPI, n8n เป็น service บน Kubernetes (Helm/Argo) | ใช้ Cloud SQL/Aurora + ECS/Fargate + cloud-managed n8n (หรือ VM) |
| ข้อดี | - เตรียมเร็ว (1-2 วัน)<br>- Dev/QA ใช้ stack เดียว<br>- ใช้ทรัพยากรน้อย | - ขยายขนาดง่าย<br>- Fault isolation ดี<br>- เหมาะกับ production scale | - Managed DB scale สูง<br>- ลดภาระบำรุงรักษา infra<br>- Integrate กับ Cloud IAM ได้ |
| ข้อเสีย | - Scale จำกัด<br>- ไม่มี auto healing<br>- มี single point of failure | - Setup ซับซ้อน, ต้องใช้ DevOps/K8s | - ค่าใช้จ่ายสูง, latency ขึ้นกับ cloud<br>- PDPA ต้องเช็ก data residency |
| ค่าใช้จ่าย | ต่ำ (VM เดียว 2 vCPU/8GB) | ปานกลาง-สูง (cluster + ops) | สูง (บริการ cloud หลายตัว) |
| ความเร็วพัฒนา | สูงสุด (พร้อมใช้เร็ว) | ปานกลาง (ต้องลง K8s infra) | ปานกลาง (ต้อง config cloud services) |
| ความเสี่ยง | - Backup/manual failover<br>- Scaling limitation | - Misconfig K8s<br>- ต้อง maintain Helm chart | - Vendor lock-in<br>- PDPA/data locality |

**คะแนน (1-5)**

| เกณฑ์ | น้ำหนัก | A | B | C | เหตุผล |
| --- | --- | --- | --- | --- | --- |
| Time-to-Market | 0.3 | **5** | 3 | 3 | A พร้อมใช้ทันที, B/C ต้องเตรียม infra |
| Cost | 0.2 | **5** | 3 | 2 | A ใช้ resource ต่ำสุด |
| Maintainability | 0.15 | 3 | **4** | 3 | B แยกบริการชัด, automation สูง |
| Security | 0.15 | 3 | **4** | 4 | B/C ใช้ features (RBAC, secrets, cloud KMS) ได้ดี |
| Scalability | 0.1 | 3 | **5** | 4 | B scale ดีกว่ามาก |
| Team Fit | 0.1 | **4** | 3 | 2 | ทีมยังไม่พร้อมดูแล cloud หลายบริการ |

**คะแนนรวม:**
- A: 4.1
- B: 3.9
- C: 3.2

**สรุป:** เริ่มที่ทางเลือก A (Docker Compose) สำหรับ dev/staging เพื่อให้ time-to-market สูงสุด จากนั้นเตรียม roadmap ไปทาง B เมื่อต้องการ scaling/hardening (เริ่มจัดทำ Helm chart, GitOps). ทางเลือก C เก็บไว้สำหรับกรณีต้องย้ายขึ้น cloud และต้องการ managed service เพื่อ compliance ระยะยาว.
