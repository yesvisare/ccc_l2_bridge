# Bridge L3.M6.4 → L3.M7.1 Readiness Validation

## Purpose

This repository contains a **minimal validation notebook** to verify that Module 6.4 (Security Complete) deliverables are operational before beginning Module 7.1 (Observability with Distributed Tracing).

This is a **bridge validation only** — no new codebases, only checks and verification of existing M6.4 infrastructure.

## Contents

- `Bridge_L3_M6_4_to_M7_1_Readiness.ipynb` - Readiness validation notebook
- `M6_4_Bridge_to_M7_1.md` - Bridge specification document

## How to Run

### Prerequisites

Ensure the following services from M6.4 are running:
- Elasticsearch (port 9200)
- Kibana (port 5601)
- Grafana (port 3000)
- Prometheus (port 9090)

### Execute the Notebook

```bash
jupyter notebook Bridge_L3_M6_4_to_M7_1_Readiness.ipynb
```

Run each section sequentially. Uncomment the function calls at the end of each code cell to execute the checks.

### What Gets Validated

The notebook performs 4 readiness checks:

1. **ELK Stack Operational**
   - Kibana accessible at localhost:5601
   - Index `audit-logs-*` exists with ≥100 events in last 24 hours

2. **Structured Logging**
   - Application logs contain `request_id` fields
   - Correlation works between logs and audit events

3. **Prometheus Metrics**
   - Grafana accessible at localhost:3000
   - Dashboard shows P50 (300-600ms), P95 (700-1,200ms)
   - Error rates < 1%

4. **M6 Completion**
   - Git repository contains M6.1-M6.4 commits
   - Module functionality verified (PII detection, Vault, RBAC, audit events)

## Pass Criteria

**All 4 checks must pass** before proceeding to M7.1.

If a service is unavailable, the check will print:
```
⚠️ Skipping (no keys/service)
```

This is acceptable for validation purposes, but services should be running in production environments.

## Next Module

Once all checks pass, proceed to **Module 7.1: Distributed Tracing with OpenTelemetry**

Module 7.1 will deliver:
- Request-level tracing through retrieval → reranking → generation pipeline
- Jaeger UI for trace visualization
- Bottleneck identification with millisecond precision
- Service dependency mapping and cascading failure detection

## Module 7 Roadmap (155 minutes total)

- **M7.1** (42 min): OpenTelemetry instrumentation
- **M7.2** (38 min): Unified observability (metrics → logs → traces)
- **M7.3** (35 min): Performance profiling (CPU/memory hotspots)
- **M7.4** (40 min): Trace-based SLI monitoring and anomaly detection

## Reference

See `M6_4_Bridge_to_M7_1.md` for the complete bridge specification.
