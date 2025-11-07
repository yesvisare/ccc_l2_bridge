# Bridge L3.M8.3 → L3.M8.4 Validation Notebook

## Purpose

This validation notebook verifies readiness to transition from **M8.3 (Regression Testing & CI/CD)** to **M8.4 (Human-in-the-Loop Evaluation)**.

**Duration:** 8-10 minutes

**Key Question:** When do automated metrics fail to capture user experience?

## What This Repository Contains

- `Bridge_L3_M8_3_to_M8_4_Readiness.ipynb` - Main validation notebook
- `automation_gaps.md` - Template for PractaThon exercise
- `README.md` - This file

## How to Run

### Prerequisites
```bash
python >= 3.8
jupyter notebook
```

### Steps
1. **Clone or open this repository**
   ```bash
   cd /path/to/ccc_l2_bridge
   ```

2. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook Bridge_L3_M8_3_to_M8_4_Readiness.ipynb
   ```

3. **Run all cells sequentially**
   - Each cell contains either:
     - Markdown explanations
     - Validation checks (with "Expected:" comments)
     - Stub code that prints status or "⚠️ Skipping (no X)" if resources unavailable

## Pass Criteria

### All 4 Readiness Checks Must Be Understood:

1. **Production RAG with Tracking**
   - [ ] Understand need for >100 queries/week
   - [ ] Know where query logs are stored
   - [ ] Have feedback collection mechanism

2. **RAGAS Baseline Established**
   - [ ] Identify divergence between RAGAS (0.92) and satisfaction (67%)
   - [ ] Understand when human labeling is unnecessary (correlation > 0.90)

3. **Budget Awareness**
   - [ ] Know cost: $25-100 (crowdsourced) or $250-750 (expert) per 100 queries
   - [ ] Have budget approval for chosen tier

4. **Embrace Ambiguity**
   - [ ] Accept inter-annotator agreement is 70-85%, not 100%
   - [ ] Understand subjective nature of human evaluation

### PractaThon Exercise (Optional, 30 min)
- Complete `automation_gaps.md` with real cases from your system
- Categorize failures: Tone, Structure, Context, Interpretation

## Expected Outputs

If infrastructure exists:
- Query tracking status summary
- RAGAS vs. satisfaction metrics
- Budget estimates
- Inter-annotator agreement simulation

If infrastructure is missing:
- Each cell prints "⚠️ Skipping (no X)" and continues
- Focus shifts to understanding concepts vs. validation

## Next Module

**M8.4: Human-in-the-Loop Evaluation**

Learn to:
- Collect detailed feedback (Accuracy, Clarity, Helpfulness on 1-5 scales)
- Implement active learning to prioritize high-value queries
- Integrate Label Studio for annotation workflows
- Create closed-loop improvements using human labels

**Key Insight from Bridge:**
> "You can have 0.92 faithfulness and still ship unhelpful answers—because automated evaluation doesn't capture user experience."

## Troubleshooting

**Q: Cells show "⚠️ Skipping (no X)"**
A: This is expected if you don't have production infrastructure yet. Focus on understanding the concepts.

**Q: I don't have 100 queries/week**
A: The notebook is educational. You can adjust thresholds in code cells to match your scale.

**Q: How do I know if I need M8.4?**
A: If RAGAS scores are high (>0.85) but user satisfaction is low (<0.75), human evaluation can identify blind spots.

## License

Educational material for CCC Level 2 Bridge Program.
