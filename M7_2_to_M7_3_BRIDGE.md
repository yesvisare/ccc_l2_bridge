# Module 7: Distributed Tracing & Advanced Observability
## Bridge M7.2→M7.3: APM Complete → Business Metrics
**Duration:** 8-10 minutes (Within-Module Bridge)  
**Format:** Transition from M7.2 (APM) to M7.3 (Custom Business Metrics)  
**NO QUIZ:** Proceeds directly to M7.3  

---

## [00:00-02:00] 1) RECAP

[SLIDE 1: "M7.2 Complete: Code-Level Bottlenecks Identified"]

"You just completed M7.2: Application Performance Monitoring with Datadog APM.

[SLIDE 2: Four Achievements]

**Achievement 1: Datadog APM Integrated**
✓ Installed ddtrace, configured API keys
✓ Continuous profiling at 5% sampling (<3% CPU overhead)
✓ Automatic instrumentation: FastAPI, Redis, OpenAI, Pinecone

**Achievement 2: Function-Level Bottlenecks Found**
✓ Flame graphs showing CPU time per function
✓ Identified serialize_prompt() taking 800ms (should be 50ms)
✓ Discovered O(n²) loop in chunk_filter at line 187

**Achievement 3: Memory Profiling Enabled**
✓ Heap growth tracked over time
✓ Found document_cache dict growing unbounded (memory leak)
✓ Implemented LRU cache eviction policy

**Achievement 4: Database Query Optimization**
✓ APM showing actual SQL queries with timing
✓ Discovered 50 individual SELECT queries (N+1 problem)
✓ Optimized to single JOIN query (10x speedup)"

[END - 02:00]

---

## [02:00-04:00] 2) WHY NEXT

[SLIDE 3: "The Business Metrics Blindspot"]

"Your technical observability is complete:
- Metrics: P95 latency, cache hits, error rates
- Tracing: Request-level timing breakdowns
- APM: Function-level bottlenecks

But there's a critical gap: BUSINESS metrics.

[SLIDE 4: The Problem]

Your CEO asks:
- 'Are users satisfied with answers?'  
  → You show P95 latency 850ms (technical metric)
- 'Which features drive retention?'  
  → You show cache hit rates (technical metric)
- 'What's cost per active user?'  
  → You show total Pinecone cost (not per-user)

**Gap:** Technical metrics don't answer business questions

[SLIDE 5: What's Missing]

Business metrics you're NOT tracking:
- Query satisfaction scores (thumbs up/down)
- Answer accuracy drift over time
- Cost per user cohort
- Feature adoption rates
- Revenue-generating vs support queries

M7.3 fills this gap with RAG-specific custom metrics."

[END - 04:00]

---

## [04:00-06:00] 3) READINESS CHECKLIST

### Checkpoint 1: Datadog APM Showing Data
☐ Verify: Datadog UI shows flame graphs from last hour
☐ Impact: M7.3 adds custom metrics to same Datadog dashboard

### Checkpoint 2: Prometheus Still Collecting Metrics
☐ Verify: Grafana shows metrics updating
☐ Impact: M7.3 creates custom Prometheus metrics

### Checkpoint 3: User Feedback Mechanism Exists
☐ Verify: Your RAG API can capture user ratings (thumbs up/down)
☐ Impact: M7.3 tracks satisfaction as custom metric

### Checkpoint 4: Cost Data Available
☐ Verify: You know OpenAI, Pinecone costs
☐ Impact: M7.3 creates cost-per-query metric

[END - 06:00]

---

## [06:00-08:00] 4) PRACTATHON CHECKPOINT

"30-minute exercise: Create Your First Business Metric

**Learn (5 min):** Identify 3 business questions your current metrics CAN'T answer
**Build (10 min):** Add thumbs-up/thumbs-down to /query endpoint
**Apply (10 min):** Create Prometheus counter for satisfaction_score
**Ship (5 min):** Document 3 business metrics you'll track in M7.3"

[END - 08:00]

---

## [08:00-10:00] 5) CALL-FORWARD

[SLIDE 6: "M7.3: Custom Business Metrics"]

"Next up: M7.3 - Custom Business Metrics

**Capability 1:** RAG-Specific Quality Metrics
- Track answer accuracy, satisfaction scores
- Monitor accuracy drift over time
- Alert when quality degrades

**Capability 2:** Cohort Analysis
- Cost per user segment (free vs paid)
- Feature usage by cohort
- Retention by query type

**Capability 3:** Executive KPI Dashboards
- Translate technical metrics to business value
- Revenue per query, cost per active user
- Product metrics that executives understand

M7.3 Driving Question: 'User satisfaction dropped from 4.2 to 3.8 this week. Which queries are causing dissatisfaction?'

Ready? See you in M7.3.

[END - Total: 10 min]
