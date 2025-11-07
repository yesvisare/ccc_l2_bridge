# Bridge L3.M7.1 → L3.M7.2 Readiness Validation

## Purpose

This notebook validates your readiness to transition from **M7.1 (Distributed Tracing)** to **M7.2 (Application Performance Monitoring)**.

It performs **bridge validation only** — checking that your M7.1 distributed tracing setup is properly configured before moving to code-level APM with Datadog.

## What Gets Validated

### ✓ M7.1 Recap
- OpenTelemetry instrumentation working end-to-end
- Jaeger visualization deployed
- 10% sampling strategy configured
- Production observability in place

### ✓ Four Readiness Checks

1. **Jaeger Showing Traces** — UI accessible at http://localhost:16686 with ≥10 traces
2. **Sampling Configured** — 10% sampling rate (TraceIdRatioBased(0.10)) to avoid cost overruns
3. **Custom Span Attributes** — llm.model, prompt_tokens, completion_tokens appearing in traces
4. **Dependencies Installed** — opentelemetry and ddtrace packages available

### ✓ M7.2 Call-Forward
- What M7.2 will add: function-level profiling, memory leak detection, query optimization
- The gap: traces show WHAT is slow (openai.generate = 3.5s) but not WHY
- Next capability: answer "Which FUNCTION inside that span is the bottleneck?"

## How to Run

### Prerequisites
- Python 3.8+
- Jupyter Notebook or JupyterLab

### Installation
```bash
pip install jupyter requests
```

### Execution
```bash
jupyter notebook Bridge_L3_M7_1_to_M7_2_Readiness.ipynb
```

Run each cell sequentially. Each readiness check will either:
- ✓ Pass automatically (e.g., dependency imports)
- ⚠️ Require manual verification (e.g., checking Jaeger UI)
- ⚠️ Skip if service unavailable (e.g., Jaeger not running)

## Pass Criteria

**You are ready for M7.2 if:**
1. Jaeger UI is accessible (manual check: open http://localhost:16686)
2. Sampling configuration is set to 10% (manual check: verify tracer.py)
3. Custom span attributes appear in traces (manual check: inspect Jaeger span)
4. Dependencies import successfully (automated check passes)

**Note:** Some checks require manual verification in Jaeger UI. Automated checks validate accessibility and imports only.

## Next Module

**M7.2: Application Performance Monitoring with Datadog APM**

You'll learn to:
- Profile function-level performance inside spans
- Detect memory leaks with heap monitoring
- Optimize database queries with query analysis
- Run APM with one command: `ddtrace-run python main.py`

**Driving question:** "Traces show `openai.generate` took 3.5 seconds. Which FUNCTION inside that span is the bottleneck?"

## Files

- `Bridge_L3_M7_1_to_M7_2_Readiness.ipynb` — Main validation notebook (6 sections)
- `M7_1_to_M7_2_BRIDGE.md` — Bridge specification (source document)
- `README.md` — This file

## Duration

**Estimated time:** 8-10 minutes
- Section 1 (Recap): 2 min
- Sections 2-5 (Checks): 5 min
- Section 6 (Call-Forward): 2 min

## Notes

- This is a **bridge validation** only — no new codebases or production deployments
- Checks use tiny stubs (≤5 lines output) and "# Expected:" comments
- If services/keys are absent, checks print "⚠️ Skipping (no keys/service)" and continue
- Manual verification steps are clearly marked with ⚠️ warnings

## Support

For issues or questions about M7.1 or M7.2, refer to the main course materials or contact your instructor.
