# Bridge L3.M7.2 → L3.M7.3 Readiness Validation

## Purpose

This repository contains a **minimal validation notebook** to verify readiness for advancing from:
- **Module 7.2 (APM Complete)** → **Module 7.3 (Business Metrics)**

The notebook validates that M7.2 deliverables are functional before introducing custom business metrics and cohort analysis in M7.3.

## Contents

- `Bridge_L3_M7_2_to_M7_3_Readiness.ipynb` - Main validation notebook
- `M7_2_to_M7_3_BRIDGE.md` - Source bridge specification

## How to Run

### Prerequisites
- Python 3.8+
- Jupyter Notebook or JupyterLab

### Optional Services (checks will skip if unavailable)
- Datadog API keys (`DD_API_KEY`, `DD_APP_KEY`)
- Prometheus endpoint
- Grafana dashboard
- RAG API with feedback endpoint

### Execution

```bash
# Install Jupyter if needed
pip install jupyter

# Launch notebook
jupyter notebook Bridge_L3_M7_2_to_M7_3_Readiness.ipynb

# Run all cells
# Expected runtime: <2 minutes
```

## Pass Criteria

All 4 readiness checks must pass (or show documented workarounds):

1. **Datadog Flame Graphs** - APM profiling data visible for last 1 hour
2. **Grafana/Prometheus Metrics** - Metrics actively updating in last 5 minutes
3. **User Feedback Mechanism** - POST /api/feedback endpoint functional
4. **Cost Data Documentation** - OpenAI and Pinecone usage tracked

### Interpreting Results

- ✓ **Check passes**: Service configured and verified
- ⚠️ **Skipping**: Service/keys unavailable (acceptable for demo environments)
- ❌ **Manual verification required**: Follow notebook instructions

## What M7.2 Shipped

1. Datadog APM integration with continuous profiling
2. Function-level bottleneck analysis via flame graphs
3. Memory profiling with heap growth tracking
4. Database query optimization (N+1 detection)

## What M7.3 Will Introduce

1. **RAG-Specific Quality Metrics** - Answer accuracy, satisfaction scoring, degradation alerts
2. **Cohort Analysis** - Cost segmentation, feature usage patterns, retention by query type
3. **Executive Dashboards** - Business KPIs (revenue per query, cost per active user)

### M7.3 Driving Question

> Satisfaction score dropped from 4.2 → 3.8 in the past week.
> Which query types are problematic? Which user cohorts are affected? What's the cost impact?

## Next Steps

Once all checks pass:
1. Review M7.3 module objectives
2. Set up business metrics infrastructure
3. Begin cohort analysis implementation

## Repository Structure

```
ccc_l2_bridge/
├── Bridge_L3_M7_2_to_M7_3_Readiness.ipynb  # Main validation notebook
├── M7_2_to_M7_3_BRIDGE.md                  # Bridge specification
└── README.md                               # This file
```

## Notes

- This is a **bridge validation only** - no new features implemented here
- Notebook contains stubs and placeholders for demonstration
- Actual M7.3 implementation happens in the next module
- If services are unavailable, checks will gracefully skip with warnings
