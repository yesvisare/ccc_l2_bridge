# Bridge L3.M6.1 → L3.M6.2 Readiness Validation

**Bridge:** PII Detection & Redaction → Secrets Management
**Duration:** 8-10 minutes
**Type:** Within-Module Bridge

## Purpose

This repository contains a minimal validation notebook to verify M6.1 (PII Detection & Redaction) completeness before advancing to M6.2 (Secrets Management & Rotation).

**Important:** This is a bridge validation tool, not a full implementation. It checks for readiness artifacts and provides call-forward context for the next module.

## Contents

- `Bridge_L3_M6_1_to_M6_2_Readiness.ipynb` — Interactive validation notebook with 6 sections
- `README.md` — This file

## How to Run

### Prerequisites

- Python 3.8+
- Jupyter Notebook or JupyterLab
- M6.1 PII Detection & Redaction module (optional for full validation)

### Installation

```bash
# Install Jupyter if not already installed
pip install jupyter

# Launch the notebook
jupyter notebook Bridge_L3_M6_1_to_M6_2_Readiness.ipynb
```

### Running the Validation

1. Open `Bridge_L3_M6_1_to_M6_2_Readiness.ipynb`
2. Execute cells sequentially from top to bottom
3. Review each readiness check:
   - **Check #1:** PII Detection Active
   - **Check #2:** Redaction Strategy Tested
   - **Check #3:** Log Masking Verified
   - **Check #4:** GDPR Deletion Function Tested

## Pass Criteria

**All 4 readiness checks should either:**
- ✓ Pass with confirmation of functionality, OR
- ⚠️ Show "Skipping (no keys/service)" if components are not yet implemented

**Notebook Structure:**
1. **Section 1:** Recap — What M6.1 shipped
2. **Sections 2-5:** Four readiness checks
3. **Section 6:** Call-forward to M6.2 preview

## Expected Outputs

Each check produces minimal output (≤5 lines):

```
✓ PII detection script found
# Expected: python process_document.py test_hr_policy.pdf
# Expected: [PII DETECTED] Found X entities
```

Or:

```
⚠️ Skipping (no process_document.py found)
```

## What This Notebook Does NOT Do

- Does not create new codebases or implementations
- Does not install dependencies or configure services
- Does not perform actual PII detection or redaction
- Only validates presence of M6.1 artifacts and provides M6.2 context

## Readiness Check Details

### Check #1: PII Detection Active
- **Test:** Verify PII detection system exists
- **Impact:** Prevents GDPR fines (€50K minimum per violation)
- **Pass:** `process_document.py` exists or skip

### Check #2: Redaction Strategy Tested
- **Test:** Confirm redaction strategy documented
- **Impact:** Prevents 3-6 hours re-indexing
- **Pass:** `pii_stats` shows documents scanned or skip

### Check #3: Log Masking Verified
- **Test:** Validate PII redaction in logs
- **Impact:** HIPAA violation fines range $50K-1.5M
- **Pass:** Log masking module exists or skip

### Check #4: GDPR Deletion Function Tested
- **Test:** Verify user data deletion functionality
- **Impact:** €20M or 4% annual revenue fine for non-compliance
- **Pass:** GDPR deletion module exists or skip

## Next Module

After completing this validation, proceed to:

**L3.M6.2: Secrets Management & Rotation**

M6.2 will introduce:
1. HashiCorp Vault Integration
2. Automated Key Rotation (90-day cycle)
3. Pre-Commit Secret Scanning (detect-secrets hooks)

## Critical Question

> "Your OpenAI key is in production code—how long until it leaks?
> And when it does, how fast can you rotate without downtime?"

M6.2 provides the answer.

## Source

Bridge script reference: [M6_Bridge_6-1_to_6-2.md](https://github.com/yesvisare/ccc_l2_bridge/blob/main/M6_Bridge_6-1_to_6-2.md)

## License

Educational use only. Part of the CCC (Cloud Computing Curriculum) Level 3 module series.
