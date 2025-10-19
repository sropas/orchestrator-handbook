# Session Runbook

Follow these steps every time you return to the project to ensure continuity and shared context across the team.

## 1. Kickoff (10 minutes)
- Review the latest entry in `docs/decisions/`.
- Scan open issues or the current sprint board; flag blockers for the daily sync.
- Open the meeting agenda template (`docs/meeting-template.md`) and fill in date, attendees, and key topics.

## 2. Environment Setup (5 minutes)
```bash
git pull origin main
./scripts/setup_dev.sh
DOCKER_BUILDKIT=1 docker compose build rag_service  # caches pip wheels
```
- Activate the virtual environment (`source .venv/bin/activate`) if prompted by the setup script.
- Confirm dependencies installed cleanly; capture new packages in `requirements.txt`.

## 3. Daily Sync & Planning (15 minutes)
- Each role shares status, new requirements, risks, PDPA or security considerations, and traffic/scale assumptions.
- Identify work items and assign owners; update issues accordingly.

## 4. Development Loop
1. Create or switch to the relevant feature branch (`git checkout -b feature/<short-topic>`).
2. Implement changes and keep notes of decisions/problems.
3. Run quality gates often:
   ```bash
   python3 -m pytest
   python3 -m ruff check .
   python3 -m black .
   ```
4. Update `docs/decisions/<YYYY-MM-DD>-<slug>.md` when a design choice is finalized.

## 5. Wrap-Up (10 minutes)
- Stage and commit work using Conventional Commit prefixes (`feat:`, `fix:`, `chore:`, etc.).
- Push the branch and open or update the pull request.
- Add a short hand-off summary at the top of this file under **Next Session Notes**.

## Next Session Notes
- ประสาน Backend + Database + QA ตั้ง dataset ทดสอบร่วม (namespace `qa_shared`) และรัน ingest/query เพื่อตรวจ latency + correctness
- QA บันทึกผลทดสอบร่วมใน `docs/orchestrator/test-plan.md` และแจ้งผลกลับในช่องทีมก่อนสิ้นวัน
- Backend จัดทำ script seed ข้อมูล (`scripts/seed_sample_data.py`) ส่งให้ QA ก่อนเริ่มเทสต์
