# แผนการทดสอบ (Test Pyramid)

## Test Pyramid
- **Unit Tests (ฐาน):**
  - ครอบคลุม `service/app/rag.py`, `utils.py`, `embeddings.py` (mock external APIs)
  - ตัวอย่าง: `tests/test_rag_query.py` ตรวจการสร้าง MatchMetadata, `tests/test_chunk_text.py`
- **Integration Tests:**
  - Spin docker-compose (Postgres + API) → ใช้ pytest fixtures เพื่อยิง `/upsert`, `/query`
  - ตัวอย่าง: `tests/integration/test_upsert_and_query.py` สร้าง namespace ทดสอบ, ตรวจ metadata
- **E2E / Smoke Tests:**
  - ใช้ `examples/curl_query.sh` + Selenium หรือ Playwright ตรวจหน้า `/ask`, `/upload`
  - Smoke checklist รันทุก deployment

## ตัวอย่าง Test (Unit)
```python
# tests/test_chunk_text.py
from service.app.utils import chunk_text

def test_chunk_text_overlap():
    text = "abc" * 500
    chunks = chunk_text(text, chunk_size=100, chunk_overlap=20)
    assert len(chunks) > 1
    assert all(len(c) <= 100 for c in chunks)
```

## ตัวอย่าง Test (Integration)
```python
# tests/integration/test_rag_flow.py
import uuid
import requests

API = "http://localhost:8000"
API_KEY = "changeme"

NAMESPACE = f"test_{uuid.uuid4().hex[:8]}"

payload = {
    "namespace": NAMESPACE,
    "items": [
        {
            "id": str(uuid.uuid4()),
            "text": "ข้อมูลเรื่อง PDPA ปี 2567",
            "metadata": {"source": "policy.pdf", "page": 1}
        }
    ]
}

headers = {"Content-Type": "application/json", "X-API-Key": API_KEY}

resp = requests.post(f"{API}/upsert", json=payload, headers=headers)
resp.raise_for_status()

query = {
    "namespace": NAMESPACE,
    "query": "PDPA",
    "top_k": 1,
    "use_rerank": False
}
match = requests.post(f"{API}/query", json=query, headers=headers).json()
assert match["matches"]
```

## Smoke Checklist
- [ ] `/health` คืน `status=ok`
- [ ] `/docs` เปิดได้
- [ ] `/ask` สามารถถามและได้ผลลัพธ์ ≥1 ชิ้น
- [ ] `/upload` อัปโหลด PDF ได้และไม่เกิด error ใน log
- [ ] n8n UI (http://localhost:5678) เปิดได้และ login ได้
- [ ] Database `chunks` มีข้อมูล ใหม่หลัง ingest
