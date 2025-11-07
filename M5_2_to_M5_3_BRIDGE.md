# BRIDGE SCRIPT — M5.2 → M5.3: Data Pipelines to Data Quality
**Track:** CCC Level 2 - Module 5: Production Data Management  
**Duration:** 8-10 minutes  
**Bridge Type:** Within-Module  
**Generated:** November 2, 2025

---

## [00:00-01:30] 1) RECAP

[SLIDE 1: Title - "Bridge: M5.2 Data Pipelines → M5.3 Data Quality"]

Huge progress. Let's review what you just built.

[SLIDE 2: M5.2 Achievements]

In M5.2: Data Pipelines & Orchestration, you accomplished:

✓ **Automated scheduling with Airflow** — DAGs run daily at 2 AM with zero manual intervention, no more forgetting to trigger updates

✓ **Parallel processing** — 5,000 documents process in 8 minutes instead of 40 (5x speedup) using 4-8 Celery workers

✓ **Graceful error handling** — Single document failures don't crash pipeline, automatic retries with exponential backoff, Slack alerts on critical failures

✓ **Production monitoring** — Prometheus metrics tracking success rate, P95 latency, error types with Grafana dashboards

[Pause 3 seconds]

Your data pipeline is now production-grade. Automated, fast, resilient, and observable.

---

## [01:30-03:30] 2) WHY NEXT

[SLIDE 3: The quality blindspot]

Your pipeline runs perfectly. Processes 5,000 documents in 8 minutes. Zero failures. Dashboard shows 100% success rate. Beautiful.

But here's what's actually happening:

[SLIDE 4: Quality-driven quantification]

**Scenario:** Your pipeline successfully processed 5,000 documents last night.

But "successfully processed" doesn't mean "indexed good quality data."

**Hidden problems:**
- **Document #847:** Corrupted PDF, extracted garbage text ("��h�R`~") → Indexed as legitimate content
- **Documents #1,204-1,299:** 96 duplicate policies (copy-paste errors) → Wasting 96x embedding costs, diluting search results
- **Documents #3,401-3,500:** Last month's versions accidentally included → Your index now has conflicting information (30-day refund vs. 60-day refund)

**Your monitoring says:** ✅ 100% success rate, 8 minutes duration, 0 failures

**Reality:** 2% garbage data, 1.9% duplicates, 2% version conflicts = **5.9% of your index is polluted**

[SLIDE 4.5: Reality Check - The degradation timeline]

**Week 1:** 5.9% bad data → Barely noticeable (users see mostly good results)

**Week 4:** Bad data compounds → 23% of index is duplicates/garbage/stale

**Week 8:** User complaints start → "Why does search return the same document 5 times?" "Why do I see contradictory policies?"

**Week 12:** Trust collapses → Users stop using your RAG system

**Total cost:**
- **Quality degradation:** Answer accuracy drops from 4.2/5 to 2.8/5
- **Wasted embedding costs:** $200/month on duplicate content
- **User churn:** 40% of users stop trusting the system

[Pause 5 seconds]

[SLIDE 5: Warning]

**Warning:** Your pipeline can run perfectly (100% uptime, fast processing, zero errors) while slowly poisoning your index with bad data. System health ≠ data quality.

---

## [03:30-05:30] 3) READINESS CHECKLIST

[SLIDE 6: Checklist]

Before M5.3: Data Quality & Validation:

☐ **Airflow DAGs running on schedule successfully**
   → Check: Airflow UI shows green runs for last 3 days
   → Impact: Saves 3 hours debugging quality checks on broken pipeline

☐ **Parallel processing working (4-8 workers)**
   → Check: Flower dashboard shows all workers active during runs
   → Impact: Quality checks add 20% overhead—parallelization keeps total time acceptable

☐ **Prometheus metrics collecting pipeline data**
   → Check: Grafana dashboard shows documents_processed_total metric
   → Impact: Quality metrics build on top of pipeline metrics foundation

☐ **At least 1,000 documents indexed successfully**
   → Check: Pinecone stats show 1K+ vectors
   → Impact: Quality analysis requires meaningful dataset size

[SLIDE 7: Warning]

**Warning:** Quality checks are computationally expensive. If your pipeline barely fits within time window now, adding quality validation may push you over. Optimize base pipeline first.

---

## [05:30-07:30] 4) PRACTATHON CHECKPOINT

[SLIDE 8: PractaThon Framework]

**Challenge:** Manually audit 100 random documents from your index

**Learn (5-10 min):**
- Query Pinecone for 100 random vectors
- Download their source documents
- Manually check for obvious quality issues (corrupted text, duplicates, outdated content)

**Build (10-15 min):**
- Create quality_audit.py script that:
  - Samples N random documents from Pinecone
  - Retrieves source text
  - Flags suspicious patterns (non-UTF8 chars, very short chunks, exact duplicates)

**Apply (10-15 min):**
- Run audit on your 1,000+ document corpus
- Count how many issues you find (estimate % of bad data)
- Calculate cost: bad_data_% × embedding_cost × chunk_count

**Ship (5 min):**
- Document findings in quality_audit_report.md
- Include: total docs audited, issues found, estimated cost of bad data
- Commit to GitHub

[SLIDE 9: Expected output]

Your report should look like:
```
Quality Audit Report - 2025-11-02

Sampled: 100 random documents from 5,000 total
Issues found:
- Corrupted text: 3 documents (3%)
- Exact duplicates: 5 documents (5%)
- Suspiciously short: 2 documents (2%)
- Total bad data: 10% estimate

Estimated annual cost of bad data:
- Wasted embeddings: $240/year
- Degraded search quality: User churn risk
```

This quantifies the problem before we solve it.

---

## [07:30-08:30] 5) CALL-FORWARD

[SLIDE 10: M5.3 Data Quality & Validation preview]

In M5.3: Data Quality & Validation, you'll add:

**1. Chunk quality scoring with >80% accuracy**
   → Detect corrupted text, nonsense content, extraction errors automatically

**2. Duplicate detection using MinHash/LSH**
   → Find near-duplicates across millions of chunks with <5% false positive rate

**3. Data drift monitoring with statistical tests**
   → Alert when document distributions change meaningfully (new topics, format changes)

[SLIDE 11: The question for M5.3]

**"Your pipeline runs automatically and fast—but how do you know it's indexing good quality data?"**

[Pause 3 seconds]

See you then.

[END - Total: 8 min 30 sec]

---

## PRODUCTION NOTES

**Slides:** 11 total  
**Source:** M5.2 + M5.3 Augmented scripts  
**Next:** M5.3 Concept

---

## NO QUIZ

Bridge videos proceed directly to M5.3 Concept.

---

**Template Version:** 2.1.3  
**Quality:** Production-ready