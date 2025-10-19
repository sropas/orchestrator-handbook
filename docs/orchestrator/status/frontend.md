# Frontend/UI Report — 2024-06-11

## งานที่ทำ
- ตรวจสอบหน้า `/ask` และ `/upload` เปิดใช้งานหลัง stack ขึ้น
- Confirm ว่าฟอร์มส่งค่า namespace/chunk size ได้ถูกต้อง และฝั่ง backend รับข้อมูลครบ

## ข้อสังเกต
- หน้า `/upload` ไม่มี progress indicator → ผู้ใช้ไม่รู้ว่ากำลังประมวลผลไฟล์อยู่
- ไม่มี favicon → เบราว์เซอร์ log 404 เมื่อเปิดหน้า web

## งานถัดไป
1. เพิ่ม loading spinner เมื่อกดปุ่ม “อัปโหลดและฝังความรู้” (ใช้ JS fetch + disable button)
2. เพิ่ม favicon (วางไฟล์ใน `service/app/static/favicon.ico` และแก้ template)
3. รวบรวม feedback เพื่อนำไปออกแบบ UI ใหม่ (เช่น React/Vite) ใน sprint ถัดไป
