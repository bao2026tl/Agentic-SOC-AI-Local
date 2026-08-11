# Agentic-SOC-AI-Local

`Agentic-SOC-AI-Local` là nền tảng SOC automation **local-first, real-time** để nhận telemetry bảo mật, phân tích/triage, tạo incident & report, và đề xuất response có cơ chế phê duyệt trước khi thực thi.

## 1) Vì sao dự án này ra đời? Dự án giải quyết vấn đề gì?

Trong môi trường lab, SME hoặc team SOC nhỏ, các khó khăn thường gặp là:

- Log phân tán từ nhiều nguồn (Apache, Suricata, Windows, syslog, Splunk HEC).
- Quy trình triage và điều tra tốn thời gian, phụ thuộc thao tác thủ công.
- Khó chuẩn hóa playbook xử lý sự cố và lưu bằng chứng/audit xuyên suốt.
- Lo ngại gửi dữ liệu nhạy cảm ra dịch vụ cloud khi thử nghiệm AI automation.

`Agentic-SOC-AI-Local` được tạo ra để giải quyết các điểm nghẽn trên theo hướng chạy cục bộ: ingest đa nguồn, chuẩn hóa + tương quan, enrichment, tra cứu knowledge/playbook (RAG), sinh incident/report, và response có approval gate.

## 2) Mục đích chính của phần mềm

Mục tiêu chính của `Agentic-SOC-AI-Local` là cung cấp một pipeline SOC end-to-end phục vụ vận hành/thử nghiệm:

- Thu thập sự kiện qua REST ingest, Splunk-compatible HEC và syslog UDP.
- Chuẩn hóa dữ liệu và gom nhóm sự kiện liên quan thành alert.
- Triage theo mức độ `Low/Medium/High/Critical`, enrich IOC/asset/IP context.
- Tra cứu playbook nội bộ trong `knowledge/` (có thể bổ sung Ollama local).
- Tạo incident + report Markdown cho alert nghiêm trọng.
- Sinh response proposal (notify/block/isolate) nhưng chỉ thực thi khi analyst duyệt.
- Ghi audit đầy đủ và phát sự kiện thời gian thực qua SSE dashboard.

> API entrypoint: `app.main:app` (FastAPI/Uvicorn).

## 3) Cấu trúc repository

Các thư mục/top-level chính trong `Agentic-SOC-AI-Local`:

- `app/`: mã nguồn backend FastAPI và SOC pipeline (`main.py`, `pipeline.py`, `parsers.py`, `agents/`, `integrations/`, `static/`).
- `data/`: dữ liệu vận hành cục bộ (ví dụ `assets.json`, `iocs.json`).
- `knowledge/`: playbook/knowledge base cho RAG nội bộ.
- `models/`: model đã train (ví dụ `attack_classifier.json`).
- `datasets/`: seed dataset và dữ liệu phục vụ huấn luyện.
- `n8n/`: workflow automation cục bộ (ví dụ webhook/notification flow).
- `scripts/`: script hỗ trợ chạy local, train model, gửi demo events, test webhook.
- `tests/`: test cho API, parser, RAG, ML detector, syslog queue, workflow.
- `docs/`: tài liệu vận hành/triển khai (ví dụ Ubuntu local run).
- `report_assets/`: tài sản/scripts phục vụ report/thesis artefacts.
- `work/`: thư mục làm việc/phát sinh trong quá trình xử lý.

Các file cấu hình quan trọng:

- `docker-compose.yml`: stack local (api, postgres, optional n8n/mailpit/ollama/splunk-poller).
- `Dockerfile`: image cho API service.
- `requirements.txt`, `requirements-dev.txt`, `pyproject.toml`: dependency và cấu hình test.

## 4) Luồng dữ liệu chính (ingestion → alerting/incident/report/response)

```text
Nguồn log/sự kiện
(Apache/Suricata/Windows/Syslog/Splunk HEC)
            │
            ▼
Ingest layer
(REST /api/v1/ingest, /services/collector/*, syslog UDP)
            │
            ▼
Normalize + Correlate
(parser theo nguồn, gom event theo fingerprint/correlation window)
            │
            ▼
Triage + Enrichment
(severity/confidence/MITRE, IOC-asset-IP context)
            │
            ▼
RAG Playbook Lookup (+ Ollama local tùy chọn)
            │
            ▼
Alert / Incident / Markdown Report
            │
            ▼
Response Proposal (notify/block/isolate)
            │
            ▼
Approval Gate của analyst
            │
            ├─ Reject  → ghi audit
            └─ Approve → thực thi theo RESPONSE_MODE (dry_run/webhook/opnsense)
                          + ghi audit + phát SSE realtime
```

## 5) Quick start (onboarding nhanh)

### Cách 1: Chạy bằng Docker Compose

1. Khởi động stack mặc định:

```bash
docker compose up -d --build
```

2. Nếu cần đổi secret mặc định, thiết lập biến môi trường trước khi chạy (ít nhất `SOC_API_KEY`, `SOC_ADMIN_TOKEN`, `POSTGRES_PASSWORD`).

3. Truy cập dashboard: `http://127.0.0.1:8000`

4. Kiểm tra health:

```bash
curl http://127.0.0.1:8000/health
```

Mặc định hệ thống dùng `RESPONSE_MODE=dry_run` để an toàn khi lab.

### Cách 2: Chạy local để phát triển

```powershell
python -m pip install -r requirements-dev.txt
.\scripts\start_local.ps1
```

Script `start_local.ps1` sẽ chạy `uvicorn app.main:app --reload` và dùng token dev mặc định nếu bạn chưa set biến môi trường.

## 6) API vận hành chính

- `POST /api/v1/ingest`: ingest một hoặc nhiều event (header `X-API-Key`).
- `POST /services/collector/event`, `POST /services/collector/raw`: endpoint tương thích Splunk HEC.
- `GET /api/v1/alerts`, `GET /api/v1/incidents`, `GET /api/v1/actions`, `GET /api/v1/audit`: màn hình vận hành SOC.
- `POST /api/v1/actions/{action_id}/decision`: approve/reject response action.
- `POST /api/v1/stream-ticket` + `GET /api/v1/stream`: realtime updates qua SSE.

OpenAPI: `http://127.0.0.1:8000/docs`

## 7) Tích hợp tùy chọn và phạm vi

- **n8n + Mailpit** (profile `automation`): orchestration/notification local.
- **Ollama** (profile `ai`): phân tích bổ sung local LLM, không thay thế approval/safety logic.
- **splunk-poller** (profile `splunk`): poll sự kiện từ Splunk API.

`Agentic-SOC-AI-Local` tập trung cho lab/pilot nội bộ theo hướng local-first SOC automation; không nhằm thay thế đầy đủ SIEM/EDR thương mại trong production quy mô lớn.
