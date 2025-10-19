# System Administrator Report — 2024-06-11

## สถานะระบบ
- ตรวจสอบ container: `n8n_local_postgres`, `n8n_local_rag_service`, `n8n_local_n8n` กำลังทำงาน
- Log ล่าสุดของ `rag_service` แสดงว่ารับคำขอ `/ask` / `/upload` แล้ว ไม่มี error ใน startup

## Alert: Upload ค้าง
- อาการ: หน้า `/upload` รอผลนาน (ไฟล์ไม่บันทึก) อาจเกิดจากการ generate embedding ช้า หรือฝั่ง API ไม่ได้ auth key
- ตรวจสอบเบื้องต้น: ไม่มี OPENAI_API_KEY ทำให้ fallback หรือเกิด timeout กว่า 30s

## แผนแก้ไข
1. สร้างไฟล์ `scripts/check_upload.sh` เพื่อตรวจ queue/worker ของ FastAPI (ดู log แบบ tail)
2. เพิ่ม health log เมื่อเริ่มอัปโหลด/จบกระบวนการ (backend จะเพิ่ม logging)
3. ขอให้ Backend จัดเตรียม mock embedding provider (`EMBEDDING_PROVIDER=mock`) ใน `.env` เพื่อใช้ขณะไม่เชื่อมภายนอก

## Monitoring/Actions
- ใช้คำสั่ง `docker compose logs -f rag_service` ระหว่างอัปโหลดเพื่อดูว่าเกิด exception หรือไม่
- เก็บ metrics พื้นฐาน (pending รอ integrate Prometheus)

## หมายเหตุ Real Data Validation (เติมหลังทดสอบ)
- เวลาเริ่ม monitoring: 
- พบ error/warning: 
- ข้อเสนอแนะ: 
