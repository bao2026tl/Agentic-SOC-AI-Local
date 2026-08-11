# Agentic-SOC-AI-Local

**Một nền tảng Security Operations Center (SOC) cục bộ (local-first) tích hợp AI, với khả năng phát hiện theo thời gian thực (real-time detection), phân loại thông minh (intelligent triage) và tự động hóa phản hồi có sự tham gia của con người (human-in-the-loop).**

---

### Bối cảnh (Background)

Các môi trường SOC truyền thống đang phải đối mặt với một số thách thức nghiêm trọng:

1. **Alert Fatigue (Quá tải cảnh báo)**: Các analyst (chuyên gia phân tích) ngập chìm trong hàng ngàn alert (cảnh báo) kém chất lượng mỗi ngày, làm giảm hiệu quả phản hồi.
2. **Detection Latency (Độ trễ phát hiện)**: Việc triage (phân loại) thủ công tốn thời gian, làm chậm trễ quá trình incident response (phản ứng sự cố) hàng giờ liền.
3. **Phụ thuộc vào chuyên môn**: Các analyst Tier-1 thường phải làm những công việc lặp đi lặp lại dựa trên rule thay vì tập trung vào nghiệp vụ forensics (điều tra số) mang lại giá trị cao.
4. **Sự phân mảnh công cụ (Fragmented Tooling)**: Việc tích hợp chéo giữa các hệ thống SIEM, EDR, quản lý playbook và ticketing thường phức tạp và dễ phát sinh lỗi.
5. **Phụ thuộc vào Cloud**: Các giải pháp SOC truyền thống yêu cầu phải có kết nối cloud, gây ra những rủi ro về độ trễ và tuân thủ (compliance).

### Sự đổi mới (The Innovation)

**Agentic-SOC-AI-Local** mang đến một **kiến trúc agentic, ưu tiên chạy cục bộ (local-first)** với các đặc tính:

- Hoạt động hoàn toàn on-premise (không phụ thuộc vào cloud).
- Sử dụng **deterministic rules (luật xác định) + ML (Machine Learning) + RAG** (Retrieval-Augmented Generation) nhằm mang lại độ chính xác cao trong phát hiện (high-fidelity detection).
- Tự động hóa quá trình triage, enrichment (làm giàu dữ liệu) và đề xuất phản hồi chỉ trong vài giây.
- Áp dụng các cổng phê duyệt **HITL (Human-In-The-Loop)** trước khi thực thi các hành động mang tính can thiệp/gián đoạn.
- Cung cấp audit logs (nhật ký kiểm toán) không thể thay đổi (immutable) phục vụ cho compliance và forensics.
- Tích hợp mượt mà với hạ tầng sẵn có (OPNsense, Splunk, Suricata, Windows Event logs).

---

## Vấn đề mà nó giải quyết

| Vấn đề (Problem) | Giải pháp của Agentic-SOC (Agentic-SOC Solution) |
|---------|---------------------|
| **Quá tải cảnh báo (Alert Overload)** | Bộ lọc dựa trên ML + correlation (tương quan) giúp giảm hơn 70% nhiễu. |
| **Triage chậm trễ (Slow Triage)** | Các agent chạy song song (normalize→enrich→triage→rag) xử lý trong chưa tới 2s. |
| **Phản hồi không nhất quán (Inconsistent Response)** | Các policy mang tính xác định (deterministic) đảm bảo security posture (trạng thái bảo mật) nhất quán. |
| **Thiếu Audit Trail (Nhật ký truy vết)** | Mọi hành động đều được lưu vết immutable (người thực hiện, thời gian, quyết định, kết quả). |
| **Analyst Burnout (Quá tải nhân sự)** | Tự động hóa các bước triage lặp đi lặp lại; analyst chỉ cần tập trung vào các case có rủi ro cao. |
| **Phân mảnh công cụ (Tool Fragmentation)** | Gom chung về một giao diện quản lý (single pane of glass) + tích hợp webhook (n8n, webhooks). |
| **False Positives (Cảnh báo giả)** | Sự kết hợp giữa enrichment + RAG playbooks làm giảm đáng kể tỷ lệ FP. |

---

## Mục tiêu cốt lõi (Core Purpose)

### Mục tiêu chính (Primary Objectives)

1. **Ingestion log theo thời gian thực (Real-time Log Ingestion)**: Chuẩn hóa (normalize) dữ liệu telemetry từ đa nguồn (firewall, web, Windows, custom).
2. **Phát hiện thông minh (Intelligent Detection)**: Kết hợp dựa trên signature (Suricata) + ML (các model tùy chỉnh) + Knowledge-based (RAG).
3. **Triage tự động (Automated Triage)**: Đánh giá mức độ nghiêm trọng (severity), độ tin cậy (confidence), MITRE ATT&CK và loại hình tấn công (attack type) một cách song song.
4. **Cổng phê duyệt của con người (Human Approval Gate)**: Không có hành động gián đoạn nào được thực thi nếu chưa qua kiểm duyệt của analyst.
5. **Phản hồi khép kín (Closed-Loop Response)**: Thực thi các hành động đã được duyệt (block IP, isolate host, notify) với đầy đủ audit trail.
6. **Bảo tồn kiến thức (Knowledge Preservation)**: RAG agent sẽ truy xuất các playbooks nội bộ để bổ sung ngữ cảnh (enrich context) cho analyst.

### Các tính năng chính (Key Features)

✅ **Ingest từ đa nguồn (Multi-Source Ingest)**: REST API, Splunk HEC, Syslog UDP.

✅ **Chuẩn hóa Event (Event Normalization)**: Apache, Suricata, Windows, JSON-generic.

✅ **Tương quan (Correlation)**: Gom nhóm (grouping) dựa trên fingerprint trong khung thời gian 5 phút.

✅ **Làm giàu dữ liệu (Enrichment)**: Asset inventory (kho tài sản), tra cứu IOC, IP context (private/global/reserved).

✅ **Phát hiện bằng ML (ML Detection)**: Được train trên các bộ dữ liệu event đã được gán nhãn (A11 seed, DataSense/CIC compatible).

✅ **Cơ sở tri thức RAG (RAG Knowledge Base)**: Tìm kiếm full-text trên các file playbook định dạng Markdown.

✅ **Hỗ trợ Local LLM**: Tích hợp tùy chọn với Ollama để phân tích chuyên sâu hơn.

✅ **Đề xuất phản hồi (Response Proposals)**: Block IP, isolate host (cô lập máy chủ), gửi alert (kèm các cổng phê duyệt).

✅ **Hub Tự động hóa (Automation Hub)**: Tích hợp n8n webhook dành cho các luồng SOAR workflow bên ngoài.

✅ **Dashboard Real-time**: Cập nhật live dựa trên công nghệ SSE, không phụ thuộc vào CDN.

✅ **Audit Log không thể thay đổi (Immutable Audit Log)**: Mọi event, phê duyệt và action đều được lưu lại.

---

## Kiến trúc & Luồng dữ liệu

### Sơ đồ luồng End-to-End

```text
┌──────────────────────────────────────────────────────────────────────┐
│ 1. NGUỒN TELEMETRY (Ingestion Đa giao thức)                          │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ OPNsense / pfSense (Syslog UDP 514/5514)                        │
│  ├─ Apache Access Logs (REST API / Syslog)                          │
│  ├─ Suricata EVE (REST API / HEC)                                   │
│  ├─ Windows Security Events (Syslog / REST)                         │
│  └─ Custom JSON (REST / HEC)                                        │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 2. INGESTION & NORMALIZATION (app/parsers.py)                        │
├──────────────────────────────────────────────────────────────────────┤
│  Chuyển đổi log thô thành schema đồng nhất:                          │
│  ├─ event_type (suricata.*, web.*, windows.*, opnsense.*)           │
│  ├─ timestamp, src_ip, dst_ip, dst_port                             │
│  ├─ title, description, category                                    │
│  └─ raw (log gốc dùng để audit)                                     │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 3. CORRELATION (app/pipeline.py:_ensure_incident)                    │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ Gom nhóm theo Fingerprint (khung correlation 5 phút)            │
│  ├─ Tăng event_count nếu trùng fingerprint                          │
│  ├─ Cập nhật severity/confidence bằng max()                         │
│  └─ Giữ lại raw events làm bằng chứng (evidence chain)              │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 4. ENRICHMENT PHASE (app/agents/enrichment.py)                       │
├──────────────────────────────────────────────────────────────────────┤
│  Tra cứu song song (non-blocking):                                   │
│  ├─ Asset Inventory: IP → tên tài sản, loại, người sở hữu           │
│  ├─ IOC Matching: src_ip, dst_ip → các indicator độc hại đã biết    │
│  ├─ IP Context: version, private/global, loopback, multicast        │
│  └─ Lab Source Detection: flag nếu xuất phát từ bài test bảo mật    │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 5. DETECTION AGENTS (Chuỗi Quyết định Song song)                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌─────────────────────┐    ┌──────────────────┐    ┌────────────┐  │
│  │ Triage Agent        │    │ ML Detector      │    │ RAG Agent  │  │
│  │ (Dựa trên Rules)    │    │ (ML model)       │    │ (Playbook) │  │
│  ├─────────────────────┤    ├──────────────────┤    ├────────────┤  │
│  │ Severity:           │    │ Enabled: true/   │    │ Search KB: │  │
│  │ ├─ LOW              │    │ false            │    │ ├─ Event   │  │
│  │ ├─ MEDIUM           │    │ Status: ok/      │    │ ├─ Title   │  │
│  │ ├─ HIGH             │    │ missing          │    │ ├─ Message │  │
│  │ └─ CRITICAL         │    │ Attack type:     │    │ └─ Returns │  │
│  │ Confidence: 0.45-0.95    │ ├─ network_scan  │    │ top-3 docs │  │
│  │ MITRE ATT&CK: [ ]        │ ├─ http_flood_   │    │ với điểm   │  │
│  │ Reasons: [ ]             │ │  dos           │    │ relevance  │  │
│  │                          │ ├─ web_attack    │    │            │  │
│  │                          │ └─ brute_force   │    │            │  │
│  │                          │ Confidence:      │    │            │  │
│  │                          │ 0.0-1.0          │    │            │  │
│  └─────────────────────┘    └──────────────────┘    └────────────┘  │
│                                                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │ Optional: Local LLM Agent (app/agents/llm.py)              │    │
│  │ ├─ Gọi Ollama tại {OLLAMA_URL}:{11434}                    │    │
│  │ ├─ Input: normalized_event + triage + enrichment + RAG    │    │
│  │ ├─ Output: tóm tắt, đánh giá, độ tin cậy, bằng chứng      │    │
│  │ └─ Timeout: 45s (graceful fallback nếu không khả dụng)    │    │
│  └─────────────────────────────────────────────────────────────┘    │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 6. LƯU TRỮ & TRẠNG THÁI (SQLite/Postgres qua SQLAlchemy ORM)         │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ Alert (fingerprint, severity, confidence, event_count, status)  │
│  ├─ SecurityEvent (raw events đứng sau mỗi alert)                   │
│  ├─ Incident (auto-escalation các case HIGH/CRITICAL)               │
│  ├─ ResponseAction (block_ip, isolate_host, notify)                 │
│  └─ AuditEvent (log immutable: actor, action, outcome, detail)      │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 7. ĐỀ XUẤT PHẢN HỒI (app/agents/response.py)                         │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ notify_soc (rủi ro thấp, tự động thực thi - auto-execute)       │
│  ├─ block_ip (rủi ro cao, yêu cầu phê duyệt)                        │
│  └─ isolate_host (rủi ro rất cao, yêu cầu phê duyệt)                │
│                                                                      │
│  Kiểm tra an toàn (Safety checks):                                   │
│  ├─ Không block các private IPs                                     │
│  ├─ Không block các IP loopback, multicast, unspecified             │
│  └─ Cần mức severity CRITICAL để có thể thực thi isolate_host       │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 8. CỔNG PHÊ DUYỆT HUMAN-IN-THE-LOOP (app/main.py)                    │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ Các hành động Pending hiển thị trong Dashboard ở tab "Response" │
│  ├─ Analyst review: severity, confidence, evidence                  │
│  ├─ Analyst quyết định: APPROVE (Duyệt) hoặc REJECT (Từ chối)       │
│  ├─ Audit log: tên analyst, lý do, timestamp                        │
│  └─ Mọi hành động là immutable (không thể thay đổi sau khi quyết định)│
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 9. THỰC THI ACTION (app/integrations/response_executor.py)           │
├──────────────────────────────────────────────────────────────────────┤
│  Mode: RESPONSE_MODE (từ .env)                                      │
│                                                                      │
│  ├─ dry_run (DEFAULT - chỉ ghi log, không thay đổi thực tế)        │
│  │  ├─ Mô phỏng block_ip, isolate_host                             │
│  │  └─ An toàn cho các bài lab và test                             │
│  │                                                                  │
│  ├─ webhook (gửi tới SOAR bên ngoài - n8n)                         │
│  │  ├─ POST tới {RESPONSE_WEBHOOK_URL}                             │
│  │  └─ n8n xử lý các integration bên ngoài                         │
│  │                                                                  │
│  └─ opnsense (tích hợp API tường lửa trực tiếp)                    │
│     ├─ Yêu cầu OPNSENSE_URL, KEY, SECRET                           │
│     ├─ Thêm IP vào alias SOC_BLOCKLIST                             │
│     └─ Rule của firewall sẽ tự động chặn traffic                   │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 10. NOTIFICATIONS & TỰ ĐỘNG HÓA (Optional: n8n + Mailpit)            │
├──────────────────────────────────────────────────────────────────────┤
│  ├─ A11 gửi webhook tới n8n khi alert HIGH/CRITICAL được tạo       │
│  ├─ n8n workflow xử lý: filter, transform, route                    │
│  ├─ n8n gửi email tới Mailpit (dev) hoặc real SMTP (prod)           │
│  └─ Audit: n8n lưu lại log chạy về A11 (/automation/audit)          │
└────────────────────────┬─────────────────────────────────────────────┘
                         ↓
┌──────────────────────────────────────────────────────────────────────┐
│ 11. DASHBOARD & ANALYTICS (app/static/)                              │
├──────────────────────────────────────────────────────────────────────┤
│  Real-time WebSocket (SSE):                                         │
│  ├─ /api/v1/stream (Server-Sent Events)                             │
│  ├─ Live updates: alerts, incidents, actions, audit                 │
│  └─ Chế độ Offline mode: dashboard tiếp tục với dữ liệu lưu trữ     │
│                                                                      │
│  Các Tabs:                                                           │
│  ├─ Overview: KPIs, phân bổ severity, timeline                      │
│  ├─ Alert Queue: filterable theo severity/status                    │
│  ├─ Incidents: Chi tiết các case HIGH/CRITICAL                      │
│  ├─ Response: Giao diện phê duyệt                                   │
│  └─ Audit Trail: Log vận hành immutable                             │
└──────────────────────────────────────────────────────────────────────┘
```

### Tóm tắt luồng dữ liệu

**Đầu vào (Input)** → Chuẩn hóa (Normalize) → Tương quan (Correlate) → Làm giàu (Enrich) → Phát hiện (Rules+ML+RAG) → Đề xuất (Propose) → **Phê duyệt (Approve)** → Thực thi (Execute) → **Dashboard/Audit**

---

## Cấu trúc thư mục

```text
agentic-soc-ai-local/
├── README.md                          # File này
├── HUONG_DAN_DEMO_TIENG_VIET.md       # Hướng dẫn demo tiếng Việt
├── pyproject.toml                     # Metadata của project Python
├── requirements.txt                   # Dependency dùng cho Production
├── requirements-dev.txt               # Dependency dùng cho Development
├── docker-compose.yml                 # File điều phối Container
├── Dockerfile                         # Build container cho API
├── .env.example                       # Template cho file môi trường (Environment)
│
├── app/                               # Package chính của ứng dụng
│   ├── __init__.py
│   ├── main.py                        # Entry point của FastAPI app (chứa endpoints)
│   ├── config.py                      # Settings (được load từ file .env)
│   ├── database.py                    # Cấu hình SQLAlchemy ORM
│   ├── models.py                      # Các model Database (Alert, Incident, Action, AuditEvent)
│   ├── schemas.py                     # Các schema Pydantic cho request/response
│   ├── auth.py                        # Token-based auth (Bearer token, API key)
│   ├── event_bus.py                   # Event streaming cho live dashboard
│   ├── parsers.py                     # Chuẩn hóa log từ đa nguồn
│   ├── pipeline.py                    # SOCPipeline orchestrator (luồng logic chính)
│   │
│   ├── agents/                        # Các module ra quyết định Agentic
│   │   ├── __init__.py
│   │   ├── triage.py                  # Gán Severity/confidence/MITRE
│   │   ├── enrichment.py              # Tra cứu IOC, asset, IP context
│   │   ├── rag.py                     # RAG knowledge base (tìm kiếm playbook)
│   │   ├── llm.py                     # Local LLM agent (tích hợp Ollama)
│   │   ├── ml_detector.py             # Phân loại tấn công bằng ML (JSON model)
│   │   ├── response.py                # Đề xuất các Response action (block_ip, isolate_host)
│   │   └── report.py                  # Generate báo cáo incident (Markdown)
│   │
│   ├── integrations/                  # Các adapter kết nối hệ thống bên ngoài
│   │   ├── __init__.py
│   │   ├── syslog.py                  # UDP syslog protocol handler
│   │   ├── response_executor.py       # Execute actions (dry_run, webhook, opnsense)
│   │   └── splunk_poller.py           # Optional: Tích hợp với Splunk search
│   │
│   └── static/                        # Web dashboard (frontend)
│       ├── index.html                 # Main page (modal auth, tabs)
│       ├── app.js                     # Frontend logic (API calls, SSE stream)
│       └── styles.css                 # Dashboard styling
│
├── data/                              # Thư mục chứa Runtime data
│   ├── assets.json                    # Asset inventory (tên, IPs, loại)
│   ├── iocs.json                      # IOC database (IPs/hashes độc hại)
│   └── soc.db                         # SQLite database (tạo lúc runtime)
│
├── knowledge/                         # Thư viện playbook RAG dạng Markdown
│   ├── brute_force_response.md
│   ├── http_flood_dos_response.md
│   ├── ml_dataset_detection_agent.md
│   ├── n8n_automation_workflow.md
│   ├── opnsense_containment_response.md
│   ├── soc_operating_policy.md
│   ├── suricata_network_scan_response.md
│   ├── web_attack_response.md
│   └── windows_endpoint_response.md
│
├── models/                            # Chứa các ML model
│   └── attack_classifier.json         # Model Naive Bayes đã train (định dạng JSON, lightweight)
│
├── datasets/                          # Dữ liệu Training
│   ├── a11_seed_labeled_events.jsonl  # Demo labeled events (100+ mẫu)
│   └── README.md
│
├── scripts/                           # Các utility script
│   ├── start_local.ps1                # Launcher cho môi trường local dev
│   ├── train_attack_classifier.py     # Script train model ML
│   ├── send_demo_events.py            # Gửi các test event
│   ├── ship_apache_access.py          # Stream Apache logs
│   └── test_n8n_webhooks.sh           # Tester cho n8n webhook
│
├── tests/                             # Unit & integration tests
│   ├── test_api.py                    # Endpoint tests
│   ├── test_ml_detector.py            # ML model tests
│   ├── test_parsers.py                # Log normalization tests
│   ├── test_rag.py                    # Knowledge base tests
│   └── test_syslog_queue.py           # Syslog ingestion tests
│
├── n8n/                               # Cấu hình tự động hóa n8n
│   ├── README.md
│   └── workflows/
│       └── a11_soc_local_automation.json  # SOAR workflow
│
├── report_assets/                     # Report templates & diagrams
│   └── diagrams/
│
└── work/                              # Các script Development & documentation
    ├── draw_end_to_end_soc_flow.py
    ├── draw_final_topic_diagrams.py
    ├── draw_local_first_realtime_flow.py
    ├── finalize_report_format.py
    ├── update_report_*.py
    └── ...
```

### Mục đích của các Thư mục chính (Key Directory Purposes)

| Thư mục (Directory) | Mục đích (Purpose) |
|-----------|---------|
| `app/` | Logic cốt lõi của ứng dụng (FastAPI, agents, parsers). |
| `app/agents/` | Các module ra quyết định (triage, enrichment, ML, RAG, response). |
| `app/integrations/` | External connectors (syslog, OPNsense, Splunk, response executor). |
| `app/static/` | Frontend dashboard (HTML/CSS/JS). |
| `data/` | Cấu hình Runtime (assets, IOCs, SQLite DB). |
| `knowledge/` | Thư viện RAG playbook (Markdown dùng cho search full-text). |
| `models/` | ML models đã pre-train (định dạng JSON không yêu cầu dependency). |
| `datasets/` | Dữ liệu dùng để retrain model. |
| `scripts/` | Các tiện ích vận hành (training, demo events, dev server). |
| `tests/` | Automated test suite (chạy bằng pytest). |
| `n8n/` | Các definition của SOAR workflow. |

---

## Bắt đầu nhanh (Quick Start)

### Điều kiện tiên quyết (Prerequisites)

- **Docker & Docker Compose** (hoặc Python 3.11+ nếu build local dev).
- **Tối thiểu 2GB+ RAM**, **1GB ổ cứng (disk)**.
- **Các Port cần mở**: 8000 (A11 SOC), 5678 (n8n), 8025 (Mailpit), 5514 (syslog).

### Tùy chọn 1: Docker (Được khuyến nghị)

```bash
# 1. Clone source code và điều hướng
git clone https://github.com/your-org/agentic-soc-ai-local.git
cd agentic-soc-ai-local

# 2. Tạo file .env từ template
cp .env.example .env

# Chỉnh sửa .env (thay đổi SOC_API_KEY, SOC_ADMIN_TOKEN để bảo mật)
nano .env
# hoặc dùng
code .env

# 3. Build và khởi chạy các service
docker compose up -d --build

# 4. Kiểm tra các container đang chạy
docker compose ps

# 5. Mở dashboard
# Trình duyệt: http://127.0.0.1:8000
# Token (lấy từ .env): <SOC_ADMIN_TOKEN>

# 6. Chạy thử demo scenario
# Nhấn nút "Run lab scenario" trên giao diện dashboard
```

### Tùy chọn 2: Local Development

```bash
# 1. Clone source code
git clone https://github.com/your-org/agentic-soc-ai-local.git
cd agentic-soc-ai-local

# 2. Tạo virtual environment
python -m venv venv
source venv/bin/activate  # hoặc venv\Scripts\Activate.ps1 trên Windows

# 3. Cài đặt các dependencies
pip install -r requirements-dev.txt

# 4. Copy và edit file .env
cp .env.example .env
nano .env

# 5. Khởi động server
./scripts/start_local.ps1  # PowerShell
# hoặc
python -m uvicorn app.main:app --reload

# 6. Truy cập dashboard
# Trình duyệt: http://127.0.0.1:8000
```

### Tùy chọn 3: Chạy Docker cùng các service tùy chọn (n8n + Mailpit)

```bash
# Bật profile automation (SOAR, email notifications)
docker compose --profile automation up -d --build

# Các dịch vụ:
# - A11 SOC Dashboard: http://127.0.0.1:8000
# - n8n Automation: http://127.0.0.1:5678
# - Mailpit (dùng test email): http://127.0.0.1:8025

# Tắt các service automation (giữ lại core A11)
docker compose --profile automation down
```

---

## Các API cốt lõi (Core APIs)

### Các Endpoint REST (Yêu cầu xác thực bằng Bearer Token)

#### Ingestion
```bash
# Gửi log event (generic JSON)
POST /api/v1/ingest
Headers: X-API-Key: <SOC_API_KEY>
Body: { "source": "apache", "event": "<raw_log_line>" }

# Tương thích với Splunk HEC
POST /services/collector/event
Headers: Authorization: Splunk <SOC_API_KEY>
Body: { "sourcetype": "suricata:eve", "event": {...} }

# Ingestion dữ liệu text thô (Raw text)
POST /services/collector/raw?source=apache
Headers: Authorization: Splunk <SOC_API_KEY>
Body: <raw_text_log_lines>
```

#### Alerts
```bash
# List các alerts (có filter)
GET /api/v1/alerts?severity=high&limit=100
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Get chi tiết của một alert
GET /api/v1/alerts/{alert_id}
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Get raw events của một alert
GET /api/v1/alerts/{alert_id}/events
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Update status của một alert
PATCH /api/v1/alerts/{alert_id}/status
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
Body: { "status": "investigating", "analyst": "analyst_name" }
```

#### Incidents
```bash
# List incidents
GET /api/v1/incidents?limit=100
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Lấy báo cáo incident (định dạng Markdown)
GET /api/v1/incidents/{incident_id}/report
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
```

#### Các hành động phản hồi (Response Actions)
```bash
# List các action đang pending
GET /api/v1/actions?status=pending
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Approve/reject một action
POST /api/v1/actions/{action_id}/decision
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
Body: {
  "decision": "approve",  # hoặc "reject"
  "analyst": "analyst_name",
  "reason": "Legitimate threat, blocking now"
}
```

#### Cơ sở tri thức (Knowledge Base - RAG)
```bash
# List các playbooks khả dụng
GET /api/v1/knowledge
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Search trong playbooks
POST /api/v1/knowledge/search
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
Body: { "query": "http flood", "limit": 3 }
```

#### System Status
```bash
# Health check (không cần auth)
GET /health

# Thông tin Runtime (yêu cầu auth)
GET /api/v1/runtime
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Số liệu thống kê (Stats)
GET /api/v1/stats
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>

# Audit log
GET /api/v1/audit?limit=100
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
```

#### Demo
```bash
# Tạo lab scenario events
POST /api/v1/demo/generate
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
Response: { "generated": 3, "alert_ids": [...] }
```

### WebSocket / Server-Sent Events

```bash
# Lấy stream ticket (yêu cầu auth)
POST /api/v1/stream-ticket
Headers: Authorization: Bearer <SOC_ADMIN_TOKEN>
Response: { "ticket": "...", "expires_in": 28800 }

# Connect tới live stream (SSE)
GET /api/v1/stream?ticket=<ticket>
# Nhận các dữ liệu: alert, incident, action, audit events theo real-time
```

---

## Demo & Kiểm thử (Demo & Testing)

### Chạy Lab Scenario

```bash
# 1. Khởi động các docker services (nếu chưa chạy)
docker compose up -d --build

# 2. Mở dashboard
# http://127.0.0.1:8000
# Token: <SOC_ADMIN_TOKEN>

# 3. Nhấn nút "Run lab scenario"
# Sẽ sinh ra 3 demo alerts:
# - Apache 404 dirb scan (web.suspicious_request)
# - Nmap NSE probe (suricata alert)
# - Windows audit log cleared (windows.security_event)

# 4. Xem các alert xuất hiện trong "Alert queue"
# 5. Kiểm tra phần "Incidents" đối với các case HIGH/CRITICAL
# 6. Review các "Response" actions và tiến hành approve/reject
# 7. Kiểm tra "Audit trail" để xem log immutable
```

### Chạy các Test Tự động (Automated Tests)

```bash
# Tất cả các bài test
pytest

# Test một module cụ thể
pytest tests/test_parsers.py -v
pytest tests/test_ml_detector.py -v
pytest tests/test_rag.py -v

# Chạy có coverage
pytest --cov=app tests/
```

### Đào tạo ML Model tùy chỉnh (Training Custom ML Model)

```bash
# Train từ bộ dataset có sẵn (bundled)
python scripts/train_attack_classifier.py   --input datasets/a11_seed_labeled_events.jsonl   --output models/attack_classifier.json

# Train với external dataset (CIC/DataSense)
python scripts/train_attack_classifier.py   --input datasets/a11_seed_labeled_events.jsonl   --csv /path/to/external_dataset.csv   --sample-per-class 5000   --output models/attack_classifier.json

# Xác minh (Verify) model
python -c "from app.agents.ml_detector import MLDetectionAgent; m = MLDetectionAgent('models/attack_classifier.json'); print(m.stats())"
```

### Gửi các Test Event

```bash
# Thông qua REST API
curl -X POST http://127.0.0.1:8000/api/v1/ingest   -H "X-API-Key: <SOC_API_KEY>"   -H "Content-Type: application/json"   -d '{
    "source": "apache",
    "event": "203.0.113.66 - - [28/Jul/2026:15:31:11 +0000] "GET /.env HTTP/1.1" 404 512 "-" "dirb/2.22""
  }'

# Thông qua Splunk HEC
curl -X POST http://127.0.0.1:8000/services/collector/event   -H "Authorization: Splunk <SOC_API_KEY>"   -H "Content-Type: application/json"   -d '{
    "sourcetype": "suricata:eve",
    "event": {
      "event_type": "alert",
      "src_ip": "93.184.216.34",
      "dest_ip": "192.168.1.100",
      "dest_port": 80,
      "alert": {
        "severity": 2,
        "signature": "ET SCAN Nmap",
        "category": "Attempted Information Leak"
      }
    }
  }'

# Thông qua Syslog (từ external host)
echo "test alert from external" | nc -u -w0 <your_host_ip> 514
```

### Các lệnh phổ biến của Docker Compose

```bash
# Xem logs
docker compose logs -f api          # Live API logs
docker compose logs -f n8n          # Live n8n logs
docker compose logs --tail 100 api  # 100 dòng log gần nhất

# Inspect (Kiểm tra) các container
docker compose ps                   # Xem Status
docker compose exec api bash        # Bật Shell vào trong container
docker compose exec postgres psql -U soc -d soc  # Kết nối tới Database

# Clean up (Dọn dẹp)
docker compose down                 # Stop container (giữ lại volumes)
docker compose down -v              # Stop và xóa luôn volumes (reset DB)
docker compose restart              # Restart các containers
docker compose up -d --build        # Build lại và khởi chạy
```

---

## Lưu ý về Bảo mật (Security Considerations)

### Cấu hình Mặc định (Chỉ dành cho Development)

File `.env.example` chứa sẵn các thông tin đăng nhập mặc định:
- `SOC_API_KEY=change-me-ingest-key`
- `SOC_ADMIN_TOKEN=change-me-admin-token`

⚠️ **KHÔNG BAO GIỜ sử dụng cấu hình này trên môi trường production.**

### Danh sách kiểm tra trước khi Triển khai (Pre-Deployment Checklist)

- [ ] Đổi `SOC_API_KEY` sang một chuỗi random mạnh (strong random string).
- [ ] Đổi `SOC_ADMIN_TOKEN` sang một chuỗi random mạnh.
- [ ] Đổi `POSTGRES_PASSWORD` sang một mật khẩu mạnh.
- [ ] Đổi `N8N_ENCRYPTION_KEY` sang một khóa random mạnh.
- [ ] Cài đặt `SOC_HTTP_BIND=127.0.0.1` (giới hạn chỉ nhận localhost) hoặc sử dụng reverse proxy.
- [ ] Kích hoạt TLS/HTTPS trên môi trường production (qua reverse proxy: nginx, Traefik).
- [ ] Giới hạn quyền truy cập syslog chỉ trong các mạng tin cậy (trusted networks).
- [ ] Khởi chạy container với cờ `no-new-privileges: true`.
- [ ] Sử dụng user non-root bên trong file Dockerfile.
- [ ] Bật các policy của SELinux hoặc AppArmor.
- [ ] Thường xuyên backup database (file `data/soc.db` hoặc Postgres dumps).
- [ ] Giám sát log để phát hiện các hoạt động đáng ngờ.
- [ ] Áp dụng các rules tường lửa (sử dụng port allowlists).

### Mô hình Mối đe dọa (Threat Model)

| Mối đe dọa (Threat) | Giảm thiểu (Mitigation) |
|--------|-----------|
| **Truy cập API trái phép** | Bearer token auth, rate limiting (triển khai qua reverse proxy). |
| **Syslog Injection** | Validate/sanitize toàn bộ log input, chỉ chạy syslog trên trusted network. |
| **Data Exfiltration (Đánh cắp dữ liệu)** | Mã hóa dữ liệu at rest (LUKS, Bitlocker), sử dụng TLS khi in transit. |
| **Các hành động độc hại (Malicious Actions)** | Cổng phê duyệt HITL ngăn chặn auto-execution, audit log là immutable. |
| **Lateral Movement (Di chuyển ngang)** | Chạy trong Docker kết hợp network policies, hạn chế tối đa truy cập bên ngoài. |
| **Từ chối dịch vụ (Denial of Service)** | Sử dụng Queue limits (syslog_queue_maxsize) và correlation windowing. |

### Tuân thủ (Compliance)

- ✅ Có Audit trail immutable (phù hợp với HIPAA, SOX, PCI-DSS).
- ✅ Có Data retention policies (chính sách lưu giữ, cấu hình ở CORRELATION_WINDOW_SECONDS, DB cleanup).
- ✅ Credentials được mã hóa (dùng secrets manager trên production: HashiCorp Vault, AWS Secrets Manager).
- ✅ HITL phê duyệt đối với các hành động nhạy cảm (phù hợp GDPR, SOC 2).
- ✅ Có thể chạy on-premise (đảm bảo data locality theo chuẩn GDPR, CCPA).

---

## Cấu hình Nâng cao (Advanced Configuration)

### Bật Local LLM (Ollama)

```bash
# 1. Update file .env
OLLAMA_ENABLED=true
OLLAMA_MODEL=qwen2.5:7b
OLLAMA_TIMEOUT_SECONDS=45

# 2. Khởi động cùng với profile AI
docker compose --profile ai up -d --build

# 3. Pull model (cho lần đầu tiên)
docker exec a11-agentic-soc-ollama-1 ollama pull qwen2.5:7b
```

### Cấu hình OPNsense Response

```bash
# 1. Tạo một firewall alias có tên là SOC_BLOCKLIST bên trong OPNsense
# 2. Tạo rule: "block any to/from SOC_BLOCKLIST"
# 3. Update file .env
OPNSENSE_URL=https://192.168.1.1
OPNSENSE_KEY=<api_key>
OPNSENSE_SECRET=<api_secret>
OPNSENSE_ALIAS=SOC_BLOCKLIST
RESPONSE_MODE=opnsense

# 4. Khởi động lại (Restart)
docker compose down
docker compose up -d --build

# Bây giờ các action "block_ip" đã được phê duyệt sẽ tự động update firewall alias
```

### Bật Splunk Search Poller

```bash
# 1. Update file .env
SPLUNK_URL=https://your-splunk.example.com
SPLUNK_TOKEN=<bearer_token>
SPLUNK_SEARCH=search index=main earliest=-90s | head 500
SPLUNK_POLL_INTERVAL_SECONDS=60

# 2. Khởi động cùng với splunk profile
docker compose --profile splunk up -d

# Splunk poller sẽ poll mỗi 60s và ingest kết quả trả về vào A11
```

### Database: Đổi từ SQLite → PostgreSQL

```bash
# Update file .env
DATABASE_URL=postgresql+psycopg://soc:password@postgres:5432/soc

# Thêm Postgres vào trong docker-compose.yml (hoặc dùng external DB)
# Restart với DB_URL mới

# Các bảng (Tables) sẽ tự động được tạo ở lần chạy đầu tiên
```

### Tùy chỉnh Playbook (RAG)

```bash
# 1. Tạo file Markdown bên trong knowledge/
# my_new_playbook.md:
# # Custom Attack Response
# 
# Tags: brute-force, auth
# MITRE ATT&CK: T1110, T1087
# 
# ## Response
# 1. Isolate endpoint
# 2. Reset credentials
# ...

# 2. Restart hoặc reload knowledge base
docker compose restart api

# 3. RAG agent sẽ có khả năng search trực tiếp trên file này một cách tự động
```

### Tùy chỉnh Event Source (Generic JSON)

```bash
# Gửi thông qua REST API:
curl -X POST http://127.0.0.1:8000/api/v1/ingest   -H "X-API-Key: <SOC_API_KEY>"   -H "Content-Type: application/json"   -d '{
    "source": "custom_iot",
    "event": {
      "timestamp": "2026-08-11T12:00:00Z",
      "device_id": "sensor-001",
      "alert": "temperature_spike",
      "value": 95.5
    }
  }'

# Parser sẽ chuẩn hóa về dạng SOCPipeline schema
```

---

## Khắc phục Sự cố (Troubleshooting)

| Vấn đề (Issue) | Cách giải quyết (Solution) |
|-------|----------|
| **Container không chịu khởi động** | Check cú pháp của file `.env`, xác minh tính khả dụng của port, check logs: `docker compose logs api`. |
| **Lỗi kết nối tới Database** | Reset lại DB: `docker compose down -v`, check cấu hình `DATABASE_URL` bên trong file `.env`. |
| **Không thấy xuất hiện Alerts** | Verify lại ingestion: gửi một event test, check status của syslog queue trên dashboard. |
| **Lỗi xác thực Token** | Tiến hành regenerate token trong file `.env`, restart lại các container. |
| **N8N webhook không được kích hoạt** | Check biến RESPONSE_MODE=webhook, verify cấu hình N8N webhook URL, xem log n8n. |
| **Mailpit không nhận được emails** | Check biến `NOTIFICATION_WEBHOOK_URL`, restart n8n, inspect kĩ lại luồng n8n workflow. |
| **Sự cố liên quan tới hiệu suất (Performance)** | Tăng tham số `syslog_worker_count`, điều chỉnh tham số `correlation_window_seconds`, scale (mở rộng) Database. |
| **Không load được ML model** | Chạy train model: `python scripts/train_attack_classifier.py`, check lại model path trong file config. |
