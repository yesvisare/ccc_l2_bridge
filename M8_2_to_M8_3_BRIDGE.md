# M8.2 → M8.3 BRIDGE SCRIPT: FROM EXPERIMENTATION TO AUTOMATION

**Bridge Type:** Within-Module (M8.2 → M8.3)  
**Module:** M8 - Testing & Quality Assurance  
**Duration:** 8-10 minutes  
**From:** M8.2 A/B Testing for RAG Improvements  
**To:** M8.3 Regression Testing & CI/CD

---

## SECTION 1: RECAP M8.2 — WHAT YOU JUST MASTERED (2 minutes)

**NARRATION:**

"[VISUAL: A/B test dashboard showing control vs. treatment comparison]

You just completed M8.2: A/B Testing for RAG Improvements. Let's recap what you now have:

✅ **Traffic Splitting Infrastructure**  
You implemented consistent user assignment using hash-based routing. User A always gets control. User B always gets treatment. No flip-flopping between configurations.

✅ **Multi-Dimensional Measurement**  
You're tracking 7+ metrics simultaneously:
- Quality: Faithfulness, Answer Relevance, Context Precision, Context Recall
- Performance: P95 latency, error rate
- Economics: Cost per query
- User satisfaction: Thumbs up/down ratio

✅ **Statistical Significance Testing**  
You ran an experiment comparing chunk sizes (512 vs. 1024 tokens). After 1,000 queries (500 per group), you calculated p-values and determined:
- Faithfulness improved +6 percentage points (p=0.02, statistically significant)
- Cost increased +35% (acceptable trade-off)
- Latency increased +18% (within guardrails)

**Decision:** Deploy treatment via gradual rollout (10% → 25% → 50% → 100%)

✅ **Gradual Rollout Strategy**  
You learned to minimize blast radius: test on 10% (canary), validate, then expand incrementally. If problems emerge, rollback instantly via feature flags.

**This is massive progress. You can now experiment safely, measure impact scientifically, and deploy improvements confidently.**

[PAUSE 2 seconds]

But there's a hidden problem."

---

## SECTION 2: WHY NEXT — THE PROBLEM GAP (2 minutes)

**NARRATION:**

"[VISUAL: Timeline showing multiple team members making changes]

You're three weeks into using A/B testing. It's working great. You've run 2 successful experiments, deployed both improvements. Your RAG quality is measurably better.

Then this happens:

**Monday:** Engineer A pushes a 'small refactor' of the chunking logic. 'Just cleaning up code,' they say. No A/B test—it's not a feature change, just refactoring.

**Tuesday:** Engineer B updates the embedding model from `text-embedding-3-small` to `text-embedding-3-large`. 'Better embeddings, better results!' No A/B test—they checked 5 queries manually, all looked good.

**Wednesday:** You update prompt templates to be more concise. You run a quick manual check on 10 queries. Looks fine. You deploy.

**Thursday morning:** User complaints flood in. Answer quality dropped from 4.2/5 to 2.9/5. Your RAGAS metrics:
- Faithfulness: 0.87 → 0.61 (tanked)
- Answer Relevance: 0.84 → 0.68 (degraded)

**What went wrong?**

[VISUAL: Three changes compounding diagram]

**The compounding effect:** Each change looked innocent in isolation. But together, they broke the system:

1. Refactored chunking → Introduced subtle boundary bug (missed overlap between chunks)
2. New embedding model → Different semantic space, retrieval precision dropped 15pp
3. Concise prompts → Worked great with old embeddings, terrible with new ones

**No single change was tested against the COMBINED effect of the others.**

By the time you caught it, **10,000 bad answers** were already sent to users.

[PAUSE]

**The gap:** A/B testing validates changes ONE AT A TIME. But in production, engineers make changes constantly:
- 5-10 commits per day
- Multiple team members
- Small tweaks that seem 'safe'

**You can't manually A/B test every commit.** It's too slow. You need automated regression testing that runs on EVERY code change BEFORE it reaches production.

**This is CI/CD for RAG quality. That's M8.3."**

---

## SECTION 3: READINESS CHECKLIST — ARE YOU READY? (2 minutes)

**NARRATION:**

"[VISUAL: Readiness checklist]

Before M8.3, verify you have the foundation:

### CHECKPOINT 1: Golden Test Set with Historical Baseline

☐ 100+ test cases with ground truth answers  
☐ Baseline RAGAS metrics established:
  - Faithfulness: 0.XX
  - Answer Relevance: 0.XX
  - Context Precision: 0.XX
  - Context Recall: 0.XX
☐ Test cases cover common queries + edge cases + historical failures

**Impact if missing:** Without a golden test set, you can't detect regressions automatically. You need known-good baselines to compare new code against.

**Time to establish:** If incomplete, revisit M8.1 (3-4 hours to build robust golden set).

---

### CHECKPOINT 2: Version Control with Git

☐ Code in GitHub/GitLab repository  
☐ Pull request workflow (no direct commits to main branch)  
☐ Branch protection rules enabled  
☐ Comfortable with git commands (commit, branch, pull request)

**Impact if missing:** CI/CD automation runs on every PR. Without version control and PR workflow, you can't automate quality checks.

**Time to set up:** 30-60 minutes if starting from scratch.

---

### CHECKPOINT 3: Containerized RAG System

☐ Dockerfile for RAG application exists  
☐ Docker image builds successfully (`docker build`)  
☐ Tests can run inside container  
☐ All dependencies specified in requirements.txt or package.json

**Impact if missing:** GitHub Actions (CI/CD platform) runs tests in Docker containers. If your system isn't containerized, tests won't run in CI.

**From Level 1 M3:** You should already have Docker setup. If not, budget 2-3 hours to containerize.

---

### CHECKPOINT 4: Fast Test Execution (<5 minutes)

☐ Golden test set runs in <5 minutes  
☐ Can mock expensive calls (OpenAI, Pinecone) for speed  
☐ Tests don't require full database setup  
☐ Can run tests locally before pushing to CI

**Impact if missing:** If tests take 20+ minutes, CI/CD pipeline is too slow. Engineers will skip it or find workarounds.

**Target:** <5 minutes for regression test suite. Use sampling (20% of golden set) or mocking to achieve this.

**Optimization strategies:**
- Mock OpenAI calls (use cached responses)
- Mock vector DB calls (use in-memory similarity search)
- Run only P0 tests in CI (comprehensive tests nightly)

---

**Bottom line:** If you have all 4 checkpoints, you're ready for M8.3. If missing 2+, pause and complete prerequisites.

**Critical success factor:** Test execution speed. If CI takes >10 minutes, no one will wait for it. Invest time making tests fast."

---

## SECTION 4: PRACTATHON CHECKPOINT — 30-MIN EXERCISE (2 minutes)

**NARRATION:**

"[VISUAL: 30-minute timer]

Before M8.3, solidify your M8.2 foundation with a PractaThon challenge.

### PRACTATHON: A/B TEST DECISION ANALYSIS

**Goal:** Make a systematic deploy/iterate/reject decision based on your M8.2 experiment results.

**Learn (5 min):**  
Review your A/B test results:
- Control metrics (baseline)
- Treatment metrics (experiment)
- Statistical significance (p-values)
- Effect sizes (percentage point improvements)

**Build (10 min):**  
Create trade-off analysis framework:

```python
def evaluate_tradeoff(control, treatment, guardrails):
    # Primary metric improvement
    quality_gain = treatment.faithfulness - control.faithfulness
    
    # Guardrail changes
    cost_increase = (treatment.cost - control.cost) / control.cost
    latency_increase = (treatment.latency - control.latency) / control.latency
    
    # Decision logic
    if quality_gain > 0.05 and cost_increase < 0.20 and latency_increase < 0.15:
        return "DEPLOY"
    elif cost_increase > 0.40 or latency_increase > 0.30:
        return "REJECT"
    else:
        return "ITERATE"  # Mixed results, needs refinement
```

**Apply (10 min):**  
Analyze three scenarios:

**Scenario 1:**
- Faithfulness: +8pp
- Cost: +15%
- Latency: +10%
- Decision: ?

**Scenario 2:**
- Faithfulness: +3pp
- Cost: +55%
- Latency: +5%
- Decision: ?

**Scenario 3:**
- Faithfulness: +12pp
- Cost: +25%
- Latency: +40%
- Decision: ?

**Ship (5 min):**  
Document decision framework in README:

```markdown
## A/B Test Decision Criteria

### DEPLOY if:
- Primary metric (faithfulness) improves >5pp
- Cost increases <20%
- Latency increases <15%
- p-value <0.05 (statistically significant)

### REJECT if:
- Cost increases >40%
- Latency increases >30%
- Error rate increases >2x
- (Even if quality improves)

### ITERATE if:
- Quality gain <5pp (effect too small)
- Mixed results (quality up, but cost/latency borderline)
- Not statistically significant (p>0.05)
```

**Why this matters:** In M8.3, you'll encode these decision rules into automated CI checks. If you haven't thought through your thresholds explicitly, automation will use arbitrary defaults.

**Deliverable:** `tradeoff_analysis.md` with your decision framework committed to GitHub."

---

## SECTION 5: CALL-FORWARD — PREVIEW M8.3 (2 minutes)

**NARRATION:**

"[VISUAL: GitHub Actions CI/CD pipeline diagram]

In M8.3: Regression Testing & CI/CD, you're automating the quality checks so no code reaches production without validation.

### KEY CAPABILITY 1: GitHub Actions Workflow

You'll build a CI pipeline that runs on EVERY pull request:

```yaml
name: RAG Quality Check
on: [pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - Checkout code
      - Build Docker container
      - Run golden test set (100 test cases)
      - Calculate RAGAS metrics
      - Compare to baseline
      - Block PR if quality degrades >5%
```

**Execution time:** <5 minutes per PR  
**Automated:** Engineers don't need to remember to run tests—CI does it automatically

---

### KEY CAPABILITY 2: Regression Detection with Baselines

You'll implement automatic quality comparison:

```python
def detect_regression(current_metrics, baseline_metrics):
    faithfulness_drop = baseline.faithfulness - current.faithfulness
    
    if faithfulness_drop > 0.05:  # 5pp regression
        return "BLOCK_PR: Faithfulness degraded by {faithfulness_drop:.2%}"
    
    if current.error_rate > baseline.error_rate * 2:
        return "BLOCK_PR: Error rate doubled"
    
    return "PASS"
```

**Result:** PRs that degrade quality are automatically blocked before merge.

**Example caught regression:**

PR #42: "Refactor chunking logic"  
CI Result: ❌ BLOCKED  
Reason: Faithfulness dropped from 0.87 to 0.79 (-8pp)  
Action: Engineer fixes bug, repushes, CI passes, PR merged

**Prevented:** 10,000 bad answers being sent to production users.

---

### KEY CAPABILITY 3: Model & Prompt Versioning with DVC

You'll implement version control for:
- Embedding models
- LLM models  
- Prompt templates
- Golden test sets

**Why this matters:** When something breaks, you can instantly rollback to last-known-good configuration.

**Example:**

```bash
# Current production (v2.3)
embedding_model: text-embedding-3-large
faithfulness: 0.87

# Deploy v2.4 (new embedding)
embedding_model: text-embedding-ada-002
faithfulness: 0.62  # REGRESSION!

# Instant rollback
dvc checkout v2.3
# Back to text-embedding-3-large, faithfulness restored to 0.87
```

**Rollback time:** 2-3 minutes (vs. hours of debugging)

---

### KEY CAPABILITY 4: Cost & Latency Regression Detection

You'll track non-quality regressions too:

```python
def check_guardrails(current, baseline):
    cost_increase = (current.cost - baseline.cost) / baseline.cost
    latency_increase = (current.latency - baseline.latency) / baseline.latency
    
    if cost_increase > 0.30:  # 30% cost increase
        return "WARNING: Cost increased by {cost_increase:.0%}"
    
    if latency_increase > 0.25:  # 25% latency increase
        return "WARNING: Latency increased by {latency_increase:.0%}"
    
    return "PASS"
```

**Result:** CI catches not just quality regressions, but also cost explosions and latency degradations.

---

**By end of M8.3, you'll have:**

- GitHub Actions pipeline running on every PR
- Automated RAGAS evaluation comparing against baseline
- Quality gates blocking PRs that degrade performance >5%
- Model/prompt versioning with instant rollback capability
- Fast test execution (<5 minutes) via mocking and sampling

**The outcome:** **Zero regressions reach production.** Every commit is validated automatically. Engineers get feedback in 5 minutes, not 3 days after users complain.

**Reality check from M8.3:** You'll also learn when CI/CD is overkill (teams with <2 engineers, <1 deploy/week) and what manual alternatives are simpler.

**Ready to automate your quality checks? Let's go."**

---

**END OF BRIDGE SCRIPT**

**Total Duration:** 8-10 minutes

---

## PRODUCTION NOTES

**Visuals Needed:**
1. A/B test dashboard showing M8.2 achievements
2. Timeline of multiple engineers making changes
3. Compounding effect diagram (3 changes breaking system)
4. GitHub Actions CI/CD pipeline flowchart
5. Readiness checklist with checkboxes
6. 30-minute PractaThon timer
7. Regression detection logic (baseline vs. current comparison)
8. DVC version control diagram (v2.3 → v2.4 rollback)
9. Quality gates visualization (PR blocked if metrics degrade)

**Pacing:**
- Section 1 (Recap): Quick, celebratory
- Section 2 (Why Next): Tension-building (compounding failures)
- Section 3 (Readiness): Practical checkpoints
- Section 4 (PractaThon): Decision framework focus
- Section 5 (Call-Forward): Automation excitement

**Tone:** Acknowledging experimentation success while revealing the next challenge (manual testing doesn't scale with team velocity—need automation).

**Key Transition:** The shift from "you can test changes manually" to "but with 5-10 commits/day, manual testing is too slow" creates natural need for automated CI/CD.
