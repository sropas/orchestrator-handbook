# Version Control & Git Flow

## Initial Setup
```bash
git init
cp .env.example .env   # อย่าคอมมิตไฟล์ .env
cat <<'GITIGNORE' >> .gitignore
.env
.venv/
__pycache__/
GITIGNORE

git add .
git commit -m "chore: bootstrap n8n-local stack"
git remote add origin git@github.com:your-org/n8n-local.git
git push -u origin main
```

## Branching Strategy
- ใช้ `main` สำหรับ production-ready code
- สร้าง `develop` เพื่อรวบรวมงานก่อนปล่อย production
- ฟีเจอร์ใหม่ใช้ branch `feature/<short-topic>`; bugfix เร่งด่วนใช้ `hotfix/<issue>`
- Merge → `develop` ผ่าน PR, เมื่อพร้อม release ให้ PR จาก `develop` → `main` และ tag เวอร์ชัน (`v1.0.0`)

## PR Flow
1. สร้าง branch: `git checkout -b feature/rag-query-filters`
2. ทำงาน, commit ด้วย Conventional Commit
3. Push: `git push origin feature/rag-query-filters`
4. เปิด PR ใช้ template `docs/orchestrator/pr-template.md`
5. ต้องผ่าน CI (lint/test/build) ก่อน merge
6. Reviewer อย่างน้อย 1 คน (PM/Lead) อนุมัติ

## Tagging & Releases
- เมื่อ deploy production สำเร็จ: `git tag -a v1.0.0 -m "Initial release"`
- Push tag: `git push origin v1.0.0`
- ใช้ GitHub Releases เพื่อแนบ changelog/artefacts (docker images, n8n flows)

## GitHub Integration Commands
```bash
# สร้าง PAT แล้ว login
gh auth login
# สร้าง repo ใหม่
gh repo create your-org/n8n-local --private --source=. --remote=origin
# ตรวจสอบ CI
gh workflow list
```
