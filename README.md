# Bridge L3.M5.2 → L3.M5.3 Readiness Validation

## Purpose

This repository contains validation checks to ensure your **Data Pipelines** (M5.2) are production-ready before introducing **Data Quality** features (M5.3).

**Bridge Goal:** Validate that automated scheduling, parallel processing, monitoring, and minimum dataset requirements are met.

## What's Inside

- **Bridge_L3_M5_2_to_M5_3_Readiness.ipynb** - Validation notebook with 4 readiness checks
- **M5_2_to_M5_3_BRIDGE.md** - Source bridge specification document

## How to Run

### Prerequisites

```bash
# Install Jupyter
pip install jupyter notebook

# Optional: Install service SDKs if you want automated checks
pip install apache-airflow celery prometheus-client pinecone-client
```

### Execute Validation

```bash
# Launch notebook
jupyter notebook Bridge_L3_M5_2_to_M5_3_Readiness.ipynb

# Run all cells (Kernel → Restart & Run All)
# OR run cells sequentially to review each check
```

### Environment Variables (Optional)

For automated checks instead of manual verification:

```bash
export AIRFLOW_HOME=/path/to/airflow
export CELERY_BROKER_URL=redis://localhost:6379/0
export PROMETHEUS_URL=http://localhost:9090
export PINECONE_API_KEY=your-api-key
export PINECONE_ENVIRONMENT=us-west1-gcp
```

## Pass Criteria

All four checks must pass:

| Check | Requirement | Why Critical |
|-------|-------------|--------------|
| **1. Airflow DAGs** | Green runs for 3 days | Prevents debugging quality checks on broken pipelines |
| **2. Parallel Processing** | 4-8 active workers | Quality checks add 20% overhead; parallelization maintains performance |
| **3. Prometheus Metrics** | `documents_processed_total` visible in Grafana | Quality metrics build on pipeline metrics |
| **4. Dataset Size** | 1,000+ vectors in Pinecone | Meaningful quality analysis requires sufficient data |

### What Success Looks Like

- All checks print `✓` (pass) or provide manual verification instructions
- No blocking errors (missing services print `⚠️ Skipping` and continue)
- Manual verification steps completed via dashboards (Airflow UI, Flower, Grafana, Pinecone)

## Next Steps

Once all checks pass, proceed to:

**Module 5.3: Data Quality**
- Chunk Quality Scoring (>80% accuracy detecting corrupted text)
- Duplicate Detection (MinHash/LSH, <5% false positives)
- Data Drift Monitoring (statistical tests for distribution changes)

**Central Question:** "How do you know you're indexing good quality data?"

## Reference

- Bridge specification: [M5_2_to_M5_3_BRIDGE.md](M5_2_to_M5_3_BRIDGE.md)
- Previous module: L3.M5.2 Data Pipelines (Production Automation)
- Next module: L3.M5.3 Data Quality (Quality Scoring & Drift Detection)
