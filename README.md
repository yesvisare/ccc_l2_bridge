# Bridge L3.M8.2 → L3.M8.3 Readiness Validation

**From EXPERIMENTATION TO AUTOMATION**

## Purpose

This bridge validates your readiness to transition from **manual A/B testing (M8.2)** to **automated CI/CD pipelines with regression detection (M8.3)**.

The notebook performs lightweight checks to ensure you have the foundational infrastructure needed for automation, without requiring you to build new codebases.

## What's Included

### Notebook: `Bridge_L3_M8_2_to_M8_3_Readiness.ipynb`

**Section 1: M8.2 Recap**
- Summary of what you mastered in M8.2 (traffic splitting, multi-dimensional measurement, statistical significance, gradual rollout)
- The problem gap: why automation matters (production failure scenario)

**Section 2-5: Four Readiness Checks**
1. **Golden Test Set with Historical Baseline** — Foundation for regression detection
2. **Version Control with Git** — Enabling automated quality checks
3. **Containerized RAG System** — CI/CD test execution environment
4. **Fast Test Execution (<5 minutes)** — Preventing CI/CD bottlenecks

Each check includes:
- Lightweight validation code (checks for files, services, tools)
- Clear "Expected" outputs showing what success looks like
- ⚠️ warnings when infrastructure is missing (graceful degradation)

**Section 6: Call-Forward to M8.3**
- GitHub Actions automated pipeline
- Regression detection with baselines
- Model & prompt versioning with DVC
- Cost & latency regression detection

**Section 7: Summary**
- Passing criteria
- Next steps

## How to Run

### Prerequisites
- Python 3.8+
- Jupyter Notebook or JupyterLab

### Installation
```bash
# Install Jupyter if needed
pip install jupyter

# Navigate to the bridge directory
cd /path/to/ccc_l2_bridge

# Launch Jupyter
jupyter notebook Bridge_L3_M8_2_to_M8_3_Readiness.ipynb
```

### Execution
1. Open the notebook
2. Run all cells: **Cell → Run All**
3. Review the output for each readiness check
4. Note any ⚠️ warnings (these indicate missing infrastructure)

## Passing Criteria

You are ready for M8.3 if:

- ✅ **At least 2 of 4 readiness checks** show existing infrastructure
- ✅ You understand **why automation matters** (problem gap section)
- ✅ You have a **clear vision of M8.3 capabilities** (call-forward section)

**Note**: It's OK if some checks show ⚠️ warnings. M8.3 will guide you through building missing components.

## Expected Outputs

### If Infrastructure Exists
```
✓ Found golden test set: golden_test_set.json
  Test cases: 125
✓ Git repository detected
✓ Remote configured: git@github.com:org/rag-system.git
✓ Found Dockerfile: Dockerfile
✓ Docker available: Docker version 24.0.5
✓ Test files detected
✓ Mocking library available (unittest.mock)
```

### If Infrastructure is Missing
```
⚠️ Skipping (no golden test set found)
# Expected: golden_test_set.json with 100+ test cases
⚠️ Skipping (no Dockerfile found)
# Expected: Dockerfile with test execution stage
⚠️ Skipping (no test files found)
# Expected: Test suite completes in <5 minutes
```

Both scenarios are acceptable! The notebook identifies what you have vs. what M8.3 will introduce.

## What This Bridge Does NOT Do

- ❌ Does not create new codebases or infrastructure
- ❌ Does not run actual RAGAS evaluations (no API keys needed)
- ❌ Does not execute long-running tests
- ❌ Does not require Docker containers to be built

This is a **validation-only** notebook. M8.3 will guide implementation.

## Next Module

**M8.3: From Experimentation to Automation**

Learn to implement:
- GitHub Actions workflows for automated testing
- RAGAS baseline comparison and regression blocking
- Model/prompt versioning with DVC
- Cost and latency guardian rails

**Module Link**: [M8.3 Documentation](#) *(add link when available)*

## Troubleshooting

**Issue**: Notebook cells fail with import errors

**Solution**: Install missing dependencies
```bash
pip install jupyter pathlib
```

**Issue**: Git checks fail

**Solution**: Ensure you're running the notebook from a Git repository
```bash
git init  # if not already a repo
git remote add origin <your-repo-url>
```

**Issue**: All checks show ⚠️ warnings

**Solution**: This is expected if you don't have existing RAG infrastructure. Proceed to M8.3 to build it.

## Files

- `Bridge_L3_M8_2_to_M8_3_Readiness.ipynb` — Main validation notebook
- `README.md` — This file
- `M8_2_to_M8_3_BRIDGE.md` — Source bridge specification

## License

Part of the CCC Level 2 Bridge curriculum.

---

**Ready?** Run the notebook and proceed to M8.3!
