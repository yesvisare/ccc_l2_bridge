# M8.1 → M8.2 BRIDGE SCRIPT: FROM EVALUATION TO EXPERIMENTATION

**Bridge Type:** Within-Module (M8.1 → M8.2)  
**Module:** M8 - Testing & Quality Assurance  
**Duration:** 8-10 minutes  
**From:** M8.1 RAGAS Evaluation Framework  
**To:** M8.2 A/B Testing for RAG Improvements

---

## SECTION 1: RECAP M8.1 — WHAT YOU JUST MASTERED (2 minutes)

**NARRATION:**

"[VISUAL: RAGAS dashboard showing four metrics tracked over time]

You just finished M8.1: RAGAS Evaluation Framework. Let's recap what you now have in production:

✅ **Golden Test Set Created**  
You built a curated collection of 100+ test cases with ground truth answers and annotated relevant documents. This is your baseline for measuring quality systematically.

✅ **Four-Dimensional Quality Measurement**  
You're now tracking:
- **Faithfulness:** 0.82 (82% of answer content is grounded in retrieved docs)
- **Answer Relevance:** 0.78 (78% of answers directly address user questions)
- **Context Precision:** 0.75 (75% of retrieved documents are relevant)
- **Context Recall:** 0.68 (68% of all relevant documents are being retrieved)

✅ **Automated Evaluation Pipeline**  
You implemented LLM-as-a-judge using GPT-4 to score quality automatically. Your evaluation runs nightly, measuring all 100 test cases in 5-8 minutes for $2-3 per night.

✅ **Regression Detection System**  
You're tracking baselines over time. When metrics drop >5%, you get alerts BEFORE users complain about declining quality.

**This is massive progress. You've moved from 'gut feeling' evaluation to systematic, data-driven quality measurement.**

[PAUSE 2 seconds]

But now you have a new problem."

---

## SECTION 2: WHY NEXT — THE PROBLEM GAP (2 minutes)

**NARRATION:**

"[VISUAL: Split screen - Left: RAGAS scores showing improvement, Right: production metrics showing degradation]

You're a week into using RAGAS. You experiment with a new chunking strategy—instead of 512 tokens per chunk, you try 1024 tokens. You run RAGAS evaluation.

**The results:**
- Faithfulness: 0.82 → 0.87 (+5 percentage points) ✅
- Context Precision: 0.75 → 0.81 (+6 percentage points) ✅
- Answer Relevance: 0.78 → 0.82 (+4 percentage points) ✅

**All metrics improved! This is a clear win, right?**

You deploy the change to 100% of your users immediately.

**48 hours later:**

- User complaints up 30%
- Average query cost: $0.15 → $0.32 (doubled)
- P95 latency: 280ms → 620ms (2.2x slower)
- Support tickets about 'slow responses' tripled

**What went wrong?**

**The gap:** RAGAS told you answer QUALITY improved. But it said nothing about:
- **Cost impact:** Larger chunks = more tokens sent to LLM = higher cost
- **Latency impact:** Processing 1024-token chunks takes longer than 512-token chunks
- **User experience:** Technically better answers delivered too slowly = frustrated users

**Plus, an even bigger problem:** You deployed to 100% of users at once. No safety net. When problems emerged, ALL your users experienced them simultaneously.

[PAUSE]

**The lesson:** Having evaluation metrics doesn't tell you if changes are safe to deploy. You need a way to:

1. **Test improvements on a SUBSET of traffic** (not all users)
2. **Measure MULTIPLE dimensions** (quality + cost + latency + user satisfaction)
3. **Detect problems EARLY** (10% of traffic, not 100%)
4. **Rollback QUICKLY** (if something goes wrong)

**This is A/B testing for RAG systems. And that's what M8.2 teaches you to build."**

---

## SECTION 3: READINESS CHECKLIST — ARE YOU READY? (2 minutes)

**NARRATION:**

"[VISUAL: Readiness checklist with checkboxes]

Before we move to M8.2, let's verify you have the foundation:

### CHECKPOINT 1: RAGAS Evaluation Infrastructure Operational

☐ Golden test set with 100+ annotated test cases  
☐ Automated evaluation pipeline running nightly  
☐ Baseline metrics tracked over past 7+ days  
☐ LLM-as-a-judge integration with GPT-4 working

**Impact if missing:** Without RAGAS baselines, you can't measure if your A/B test variations are actually better. You need evaluation as the foundation for experimentation.

**Time to establish:** If your RAGAS setup is incomplete, spend 3-4 hours completing M8.1 before proceeding.

---

### CHECKPOINT 2: Production RAG System with Traffic Logging

☐ RAG system deployed and serving real user queries  
☐ Database storing query, context, response, and user_id per request  
☐ At least 100 queries per day (minimum for meaningful A/B tests)  
☐ Ability to modify retrieval/generation logic without full redeployment

**Impact if missing:** A/B testing requires splitting real user traffic between control and treatment groups. Without production traffic, you can't run experiments.

**Minimum traffic threshold:** <100 queries/day = A/B testing likely not statistically significant. Consider alternatives (sequential testing, manual comparison).

---

### CHECKPOINT 3: Cost and Latency Tracking Enabled

☐ Per-query cost tracking (token usage × pricing)  
☐ Per-query latency measurement (end-to-end response time)  
☐ Prometheus or similar metrics system collecting performance data  
☐ Dashboards showing cost/latency trends over time

**Impact if missing:** A/B tests measure quality (RAGAS), but you also need to measure cost and latency. An improvement in quality that doubles cost is not necessarily a win.

**Example:** Your A/B test shows faithfulness improved 8%. But per-query cost increased 50%. Is this acceptable? You need both metrics to decide.

---

### CHECKPOINT 4: Ability to Toggle RAG Configurations

☐ Retrieval parameters (num_docs, similarity_threshold) configurable via feature flags  
☐ Prompt templates versionable (can switch prompts per experiment)  
☐ Embedding model swappable (can test different embedding models)  
☐ Rollback mechanism (can revert to previous configuration quickly)

**Impact if missing:** A/B testing requires running two configurations simultaneously (control vs. treatment). If you can't toggle between configurations easily, you can't run experiments.

**Time to enable:** If your system is tightly coupled (hardcoded parameters), refactor to use config files or feature flags. Estimated time: 4-6 hours.

---

**Bottom line:** If you checked ALL 4 checkpoints, you're ready for M8.2. If you're missing 2+, pause here and complete the prerequisites."

---

## SECTION 4: PRACTATHON CHECKPOINT — 30-MIN EXERCISE (2 minutes)

**NARRATION:**

"[VISUAL: 30-minute timer and exercise components]

Before moving to M8.2, let's solidify your M8.1 foundation with a 30-minute PractaThon exercise.

### PRACTATHON CHALLENGE: BASELINE COMPARISON TEST

**Goal:** Measure how your current RAG configuration performs across ALL dimensions (quality + cost + latency), not just RAGAS scores.

**Learn (5 min):**  
Export your M8.1 RAGAS evaluation results. Review your four baseline metrics:
- Faithfulness: X.XX
- Answer Relevance: X.XX
- Context Precision: X.XX
- Context Recall: X.XX

**Build (10 min):**  
Add cost and latency tracking to your evaluation pipeline:

```python
def evaluate_with_all_metrics(query, response, contexts):
    # RAGAS scores (from M8.1)
    ragas_scores = evaluate_ragas(query, response, contexts)
    
    # Add cost tracking
    token_count = count_tokens(contexts + response)
    cost_per_query = token_count * PRICE_PER_TOKEN
    
    # Add latency tracking
    start_time = time.time()
    # ... RAG pipeline execution ...
    latency_ms = (time.time() - start_time) * 1000
    
    return {
        **ragas_scores,
        'cost': cost_per_query,
        'latency_ms': latency_ms
    }
```

**Apply (10 min):**  
Run this on your 100-item golden test set. Calculate:
- Average cost per query: $X.XX
- P95 latency: XXXms
- Cost per 1,000 queries: $XX.XX
- Monthly cost projection (assuming 3,000 queries/month): $XXX

**Ship (5 min):**  
Export comprehensive baseline:

```csv
metric,value
faithfulness,0.82
answer_relevance,0.78
context_precision,0.75
context_recall,0.68
avg_cost_per_query,0.15
p95_latency_ms,280
monthly_cost_projection,450
```

Commit to GitHub: `baseline_comprehensive.csv`

**Why this matters:** In M8.2, you'll compare TWO configurations (control vs. treatment) across ALL these dimensions. This baseline is your control group reference.

**Time investment:** 30 minutes  
**Deliverable:** CSV with 7+ metrics showing complete baseline performance

**If you skip this exercise:** You'll struggle in M8.2 when we ask you to compare experiments—you won't have a clear baseline to compare against."

---

## SECTION 5: CALL-FORWARD — PREVIEW M8.2 (2 minutes)

**NARRATION:**

"[VISUAL: A/B testing split diagram showing control (50%) vs. treatment (50%)]

In M8.2: A/B Testing for RAG Improvements, you're building the scientific experimentation framework that lets you validate changes BEFORE rolling them out to all users.

### KEY CAPABILITY 1: Traffic Splitting Logic

You'll implement:
- **Control group (50% of traffic):** Your current RAG configuration (baseline)
- **Treatment group (50% of traffic):** Your proposed improvement (experiment)
- **Random assignment:** Each user is randomly assigned to control or treatment
- **Consistent assignment:** Same user always gets same configuration (no flip-flopping)

**Why this matters:** You test improvements on HALF your traffic. If treatment performs worse, only 50% of users are affected—not 100%.

---

### KEY CAPABILITY 2: Multi-Dimensional Measurement

You'll track:
- **Quality:** RAGAS scores (faithfulness, relevance, precision, recall)
- **Cost:** Per-query token usage and cost
- **Latency:** P50, P95, P99 response times
- **User satisfaction:** Thumbs up/down feedback rates

**Decision framework:**

Treatment is a WIN if:
- Quality improves >5% AND cost increases <20% AND latency increases <15%

Treatment is a LOSS if:
- Quality improves <2% OR cost increases >30% OR latency increases >25%

Treatment is UNCERTAIN if:
- Mixed results (quality up, cost way up, latency OK)

**You learn to make trade-off decisions systematically.**

---

### KEY CAPABILITY 3: Statistical Significance Testing

You'll implement:
- **P-value calculations:** Is the difference between control and treatment statistically significant?
- **Confidence intervals:** What's the range of expected performance difference?
- **Minimum sample size:** How many queries needed before you can trust results?

**Example:**

After 500 queries (250 control, 250 treatment):
- Control faithfulness: 0.82 ± 0.04
- Treatment faithfulness: 0.87 ± 0.03
- P-value: 0.003 (statistically significant difference)

**Conclusion:** Treatment is reliably better. Safe to roll out to 100%.

---

### KEY CAPABILITY 4: Gradual Rollout Strategies

You'll learn:
- **Canary deployment:** 10% → 25% → 50% → 100% traffic rollout
- **Blue-green switch:** Run control and treatment in parallel, instant cutover
- **Rollback protocol:** How to quickly revert if problems emerge

**Reality check:** Not every improvement is worth deploying. Sometimes control wins.

**Example real scenario:**

- Treatment improved faithfulness by 4 percentage points
- But cost increased 60%
- Decision: Keep control configuration, research cheaper alternatives

**A/B testing prevents bad deployments.**

---

**By end of M8.2, you'll have:**

- Traffic splitting infrastructure (control vs. treatment groups)
- Multi-dimensional measurement (quality + cost + latency + satisfaction)
- Statistical significance testing (p-values, confidence intervals)
- Gradual rollout strategies (canary, blue-green, rollback)

**The outcome:** You can confidently experiment with RAG improvements, measure their impact scientifically, and deploy only the changes that make your system objectively better.

**Reality check from M8.2:** You'll also learn when A/B testing is overkill (hint: <500 queries/day makes statistical significance hard to achieve) and what simpler alternatives exist.

**Ready to build your experimentation framework? Let's go."**

---

**END OF BRIDGE SCRIPT**

**Total Duration:** 8-10 minutes

---

## PRODUCTION NOTES

**Visuals Needed:**
1. RAGAS dashboard showing M8.1 achievements (four metrics)
2. Split screen: RAGAS improving vs. production metrics degrading
3. A/B testing split diagram (50% control, 50% treatment)
4. Readiness checklist with checkboxes
5. 30-minute PractaThon timer and components
6. M8.2 preview: Traffic splitting logic
7. Multi-dimensional measurement chart (quality, cost, latency, satisfaction)
8. Statistical significance visualization (confidence intervals)
9. Gradual rollout diagram (10% → 25% → 50% → 100%)

**Pacing:**
- Section 1 (Recap): Quick, celebratory
- Section 2 (Why Next): Problem reveal—build tension
- Section 3 (Readiness): Practical, checkpoint-focused
- Section 4 (PractaThon): Action-oriented, time-bound
- Section 5 (Call-Forward): Exciting preview of experimentation

**Tone:** Acknowledging M8.1 success while revealing the next challenge (safe deployment requires experimentation, not just evaluation).

**Key Transition:** The shift from "evaluation tells you quality improved" to "but doesn't tell you if it's safe to deploy" should feel like a natural evolution—evaluation is necessary but insufficient for production changes.
