# CI/CD Strategy

## CI Pipeline (GitHub Actions)
- Workflow: `.github/workflows/ci.yml`
- Jobs:
  1. **lint-test** — install dependencies, run `ruff`, `black --check`, `pytest`
  2. **docker-build** — build FastAPI service image for verification
- Future extensions:
  - Upload coverage report (`coverage.xml`)
  - Push container image to GHCR (`docker/build-push-action`)
  - Add n8n workflow validation (export to JSON + lint)

## CD Options
1. **Manual Deploy (Docker Compose)**
   - Triggered after CI success; `docker compose pull && docker compose up -d --build`
   - Save logs in `logs/compose-$(date).txt`
2. **GitHub Actions → SSH Deploy**
   ```yaml
   - name: Deploy to VM
     uses: appleboy/ssh-action@v1.0.3
     with:
       host: ${{ secrets.VM_HOST }}
       username: ${{ secrets.VM_USER }}
       key: ${{ secrets.VM_SSH_KEY }}
       script: |
         cd /opt/n8n-local
         git pull
         docker compose pull
         docker compose up -d --build
   ```
3. **GitOps (ArgoCD/Flux)**
   - Push k8s manifests (`infra/k8s`) to `deploy` branch
   - ArgoCD syncs changes automatically to cluster

## Release Gates
- CI green (lint-test + docker build)
- Smoke checklist ผ่าน (`docs/orchestrator/test-plan.md`)
- Security checklist items ที่มีผล impact ✓
- Approval จาก PM + Securityก่อน merge → main

## Observability Hook
- Integrate checks post-deploy: call `/health`, `SELECT COUNT(*) FROM chunks` อัปเดตใน pipeline
- เป้าหมาย: เปิดใช้ Prometheus + Alertmanager ภายใน sprint 3
