# Deployment Playbook (3 ทางเลือก)

## 1. Docker Compose (Dev/Staging)
```bash
cp .env.example .env
# แก้ค่าตาม environment
sudo docker compose up -d --build
sudo docker compose logs -f rag_service
```
- ใช้สำหรับ dev/staging, รองรับ quick rollback (`docker compose down && docker compose up -d`)
- สำรองข้อมูล Postgres ด้วย `docker exec -t n8n_local_postgres pg_dump -U raguser ragdb > backup.sql`

## 2. SSH + Systemd Service (VM Production เริ่มต้น)
1. สร้าง image: `docker build -t registry.local/n8n-local-rag:latest ./service`
2. Push ไป registry ภายใน
3. SSH เข้า VM แล้วสร้าง unit:
```bash
sudo tee /etc/systemd/system/rag.service <<'UNIT'
[Unit]
Description=n8n-local RAG API
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker run --name rag_api --env-file /opt/n8n-local/.env \
  --network rag_net -p 8000:8000 registry.local/n8n-local-rag:latest
ExecStop=/usr/bin/docker rm -f rag_api

[Install]
WantedBy=multi-user.target
UNIT
sudo systemctl daemon-reload
sudo systemctl enable --now rag.service
```
4. จัดการ Postgres แยก หรือใช้ managed service แล้วตั้งค่า `DATABASE_URL`

## 3. Kubernetes (Production Scale)
```bash
# สร้าง namespace
kubectl create namespace n8n-local
kubectl apply -n n8n-local -f infra/k8s/secret-sample.yaml
kubectl apply -n n8n-local -f infra/k8s/postgres.yaml
kubectl apply -n n8n-local -f infra/k8s/rag-service.yaml
```
-ใช้ Ingress Controller (Traefik/NGINX) map `/` → rag-service, `/n8n` → n8n deployment (แยก chart)
- เพิ่ม HorizontalPodAutoscaler เพื่อ scale `rag-service`
- ใช้ `kubectl rollout status deployment/rag-service` เพื่อตรวจสุขภาพและ `kubectl rollout undo` หากพบปัญหา

## เพิ่มเติม
- Backup/Restore: จัด cron job `pg_dump`, เก็บไฟล์ใน object storage
- Monitoring: ติดตั้ง Prometheus Operator + ServiceMonitor สำหรับ API และ Postgres exporter
- Secrets: ใน production ให้ใช้ External Secrets หรือ Vault แทนการเก็บในไฟล์
