# Averis SDOC · Autonomous Trade Document Audit Platform
> Developed for **Averis x GDG Monash Hackathon 2026**
> **Evaluation Benchmark:** Perfect Composite Score `1.0000 (100%)`

---

## 1. Executive Summary
Cross-referencing maritime trade documentation—specifically unstructured Shipping Instructions (SI) and carrier Bills of Lading (BL)—has traditionally required intensive manual cross-checking, introducing operational latency and compliance risk.

**Averis SDOC** is an enterprise-grade automated reconciliation engine pairing deterministic heuristics with multi-modal ingestion to audit 7 mandatory trade fields in under 1.3 seconds per document while eliminating false clearances.

---

## 2. Technical Evaluation Benchmark
Evaluated against the official benchmark suite via `score_cli.py`:

| Stage | Metric Evaluated | Target | Achieved Score | Status |
| :--- | :--- | :--- | :--- | :--- |
| **Stage 1** | Intent Stream Classification | > 0.9500 | **1.0000 (100%)** | Verified |
| **Stage 2** | Discrepancy Isolation Recall | > 0.9500 | **1.0000 (46/46)** | Verified |
| **Stage 3** | Reliability Exception Catch | > 0.9500 | **1.0000 (20/20)** | Verified |
| **Overall** | **Composite Benchmark Score** | **> 0.9500** | **1.0000** | **PERFECT** |

---

## 3. End-to-End Architecture
1. **Multi-Modal Ingestion Pipeline:** Parses MIME email payloads, scans, and attachments (PDF/DOCX).
2. **Deterministic Entity Normalization:**
   - Standardizes logistics metrics (e.g. Metric Tons to Kilograms `MT -> KG`).
   - Validates UN/LOCODE port identifiers (`MYPKG`, `IDJKT`, `MYPGU`).
   - Fuzzy entity matching (`Jaro-Winkler > 0.92`) across Shipper, Consignee, and Notify Party.
3. **7-Field Discrepancy Matrix:** Cross-checks `shipper`, `consignee`, `notify_party`, `port_of_loading`, `port_of_discharge`, `container_count`, and `gross_weight_kg`.
4. **Cloud Persistence & Executive UI:** Synchronizes 520 immutable audit rows into a managed Supabase PostgreSQL instance exposed through a responsive, modern SaaS dashboard.

---

## 4. Live Links & Deliverables
- **Live Production URL:** [https://averis-sdoc.vercel.app](https://averis-sdoc.vercel.app)
- **Official Evaluation Payload:** `submission.json` (Direct Root Directory)
- **Backend Architecture:** Supabase Managed PostgreSQL (`ap-southeast-1` Singapore)

---

## 5. Local Reproduction Quickstart
```bash
# Clone repository
git clone [https://github.com/Rfarihin-dev/averis-sdoc.git](https://github.com/Rfarihin-dev/averis-sdoc.git)
cd averis-sdoc

# Install dependencies
pip install -r requirements.txt

# Run benchmark evaluation
python score_cli.py submission.json --ground-truth ground_truth.json
```