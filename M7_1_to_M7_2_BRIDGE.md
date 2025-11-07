# Module 7: Distributed Tracing & Advanced Observability
## Bridge M7.1→M7.2: Distributed Tracing Complete → Performance Monitoring
**Duration:** 8-10 minutes (Within-Module Bridge)  
**Format:** Transition from M7.1 (Distributed Tracing) to M7.2 (Application Performance Monitoring)  
**NO QUIZ:** Proceeds directly to M7.2  

---

## [00:00-02:00] 1) RECAP - WHAT YOU JUST ACCOMPLISHED

**Duration:** 2:00 minutes  
**Purpose:** Celebrate M7.1 completion

[SLIDE 1: Title - "M7.1 Complete: Request-Level Visibility Achieved"]

"Welcome back! You just completed M7.1: Distributed Tracing with OpenTelemetry. Let's recap your four major achievements.

[SLIDE 2: Four Achievements]

**Achievement 1: OpenTelemetry Instrumentation Working End-to-End**

✓ You installed OpenTelemetry SDK and configured the tracer provider with OTLP exporter
- BatchSpanProcessor exporting every 5 seconds (async, low overhead)
- Jaeger backend running at localhost:16686
- Auto-instrumented FastAPI with FastAPIInstrumentor
- Manual spans added to RAG pipeline functions (retrieve, rerank, generate)

[Pause 2 seconds]

✓ You can now answer: 'Where did 4.2 seconds go in THIS specific request?'
- Jaeger waterfall view shows exact timing: Retrieval (180ms) → Reranking (420ms) → OpenAI (3,500ms) → Cache (15ms)
- You identified OpenAI as the bottleneck (83% of total time)
- No more guessing which component is slow

**Achievement 2: Trace Visualization in Jaeger UI**

✓ You deployed Jaeger and viewed your first traces
- Parent span (POST /query) with child spans for each operation
- Span attributes showing llm.model, user_id, cost_usd
- Filtering capability: 'Show me all traces where duration >2s'

✓ You debugged that mystery 4.2-second query
- Found exact bottleneck: GPT-4 generation took 3.5 seconds
- Discovered next action: Switch to GPT-3.5 for simple queries (cost optimization coming in M2)

**Achievement 3: Sampling Strategy Configured**

✓ You implemented 10% head-based sampling
- Reduced overhead from 20ms per request → 2ms (90% reduction)
- Still captures enough traces for debugging (1,000 traces/day at 10K requests/day)
- Balances cost (120MB/month storage) vs coverage

✓ You understand sampling trade-offs
- 100% sampling: complete coverage but expensive ($$$)
- 10% sampling: good coverage, reasonable cost ($$)
- 1% sampling: minimal cost but might miss rare issues ($)

**Achievement 4: Production Observability Deployed**

✓ Your RAG system now has X-ray vision
- Every request gets a trace_id (tracking number)
- Context propagates through all services (FastAPI → Pinecone → OpenAI)
- Traces retained for 30 days (enough to debug recurring issues)

✓ You can answer questions metrics couldn't:
- "Why was request abc123 slow?" → Check trace in Jaeger
- "Which service is the bottleneck?" → See waterfall view
- "Are slow queries correlated with specific users?" → Filter by user_id attribute

Congratulations—you have request-level observability!"

[END - 02:00]

---

## [02:00-04:00] 2) WHY NEXT - THE GAP DISTRIBUTED TRACING CAN'T FILL

**Duration:** 2:00 minutes  
**Purpose:** Create urgency for APM

[SLIDE 3: The New Problem - "Traces Show WHAT is Slow, Not WHY"]

"Your distributed tracing shows OpenAI took 3.5 seconds. Great! You found the bottleneck. But now you need to ask: WHY is OpenAI slow?

[SLIDE 4: Scenario - "The 3.5-Second OpenAI Span"]

**What you see in Jaeger:**
```
Span: openai.generate
Duration: 3,500ms
Attributes: model=gpt-4, prompt_tokens=1850, completion_tokens=420
```

**Questions you STILL can't answer:**
- Is 3.5s normal for GPT-4 with 1850 tokens? Or is this request abnormal?
- Are you sending redundant data in the prompt? (wasting tokens)
- Is the slow response from OpenAI's API or your code serializing the prompt?
- Is there a memory leak slowly degrading performance over time?
- Which FUNCTION inside your generate_response() is the bottleneck?

[Pause 2 seconds]

[SLIDE 5: The Limitation - "Span Granularity"]

**Distributed tracing operates at span-level (API calls, services):**
- You see: openai.generate took 3.5 seconds
- You DON'T see: Which of the 47 function calls inside generate_response() is slow

**Code-level visibility requires different tools.**

[SLIDE 6: Cost-Driven Quantification]

**Without code-level profiling:**
- You know OpenAI is slow (3.5s)
- You don't know if you can optimize it (maybe it's normal for this prompt size)
- You waste time optimizing the wrong things (Pinecone retrieval taking 180ms, which is already fast)

**Example wasted effort:**
- Week 1: Optimize Pinecone retrieval from 180ms → 120ms (60ms saved, 2% improvement)
- Week 2: Discover OpenAI prompt has 800 redundant tokens (could save 1.2s, 34% improvement)
- Result: Optimized wrong thing first, wasted week

**Time-driven quantification:**
- Debugging without profiling: 4-8 hours of manual instrumentation per issue
- Debugging with profiling: 30 minutes (automatic function-level breakdown)
- Savings: 3-7 hours per debugging session

[SLIDE 7: What You Need - Application Performance Monitoring]

**APM adds:**
- **Code-level profiling:** See which functions are slow (line 247 in embeddings.py)
- **Memory profiling:** Detect memory leaks before they crash production
- **Database query analysis:** Find slow SQL queries doing full table scans
- **Automatic instrumentation:** No manual span creation, captures everything

**M7.2 solves:** 'I know WHICH service is slow. Now show me WHICH FUNCTION inside that service is the bottleneck.'"

[END - 04:00]

---

## [03-06:00] 3) READINESS CHECKLIST

**Duration:** 2:00 minutes

[SLIDE 8: Pre-Flight Checklist]

"Before starting M7.2, verify you have the foundation.

### Checkpoint 1: Jaeger Showing Traces Successfully

☐ **Verify:** Open Jaeger UI at http://localhost:16686
   - Search for traces from last hour
   - Should see ≥10 traces (if you tested M7.1 properly)
   - Waterfall view should show parent + child spans

☐ **Impact:** M7.2 integrates APM with existing traces. If Jaeger isn't working, APM traces won't correlate. Fix now = save 2 hours troubleshooting.

### Checkpoint 2: Sampling Configured Correctly

☐ **Verify:** Check tracer.py configuration
   ```python
   # Should see something like:
   sampler = TraceIdRatioBased(0.10)  # 10% sampling
   ```
   - Confirm sampling rate is 0.10 (10%)
   - Check Jaeger: should see ~10% of requests traced

☐ **Impact:** APM will use same sampling. If set to 100%, you'll overwhelm APM backend and incur high costs. Fix now = avoid unexpected bills.

### Checkpoint 3: Custom Span Attributes Working

☐ **Verify:** In Jaeger, view a trace
   - Click on openai.generate span
   - Check attributes section shows: llm.model, llm.prompt_tokens, llm.completion_tokens
   
☐ **Impact:** M7.2 adds MORE custom attributes (function names, memory usage). If basic attributes aren't working, advanced ones won't either. Fix now = save 1-2 hours debugging.

### Checkpoint 4: OpenTelemetry Dependencies Installed

☐ **Verify:** Run test:
   ```bash
   python -c "import opentelemetry; from ddtrace import tracer; print('Ready')"
   ```
   Should print 'Ready' (no errors)

☐ **Impact:** M7.2 uses both OpenTelemetry + Datadog SDK. Missing dependencies = import errors. Install now = save 30 min troubleshooting."

[END - 06:00]

---

## [06:00-08:00] 4) PRACTATHON CHECKPOINT

**Duration:** 2:00 minutes

[SLIDE 9: 30-Minute Exercise - "Profile Your Slowest Span"]

"Complete this 30-minute checkpoint before M7.2.

[SLIDE 10: Learn/Build/Apply/Ship]

**Learn (5 min):** Analyze Jaeger Traces
- Open Jaeger UI
- Find the slowest trace from last 24 hours (sort by duration)
- Identify which span took longest
- Document: Which operation is slowest? By how much?

**Build (10 min):** Add Python Profiler to Slow Operation
```python
# Install py-spy
pip install py-spy --break-system-packages

# Profile the slow operation
import subprocess
subprocess.run(['py-spy', 'record', '-o', 'profile.svg', '--', 'python', 'your_slow_function.py'])
```
Run your slow operation, generate profile.svg

**Apply (10 min):** Analyze Profile Results
- Open profile.svg in browser
- Identify which function consumes most CPU time
- Document findings: Is it CPU-bound? I/O-bound? Inefficient algorithm?

**Ship (5 min):** Document Bottleneck
- Create bottleneck_analysis.md in your repo
- Include: Slowest span name, duration, root cause hypothesis
- Commit to GitHub
- This becomes your M7.2 starting point

**Success criteria:** You have a specific function to optimize in M7.2."

[END - 08:00]

---

## [08:00-10:00] 5) CALL-FORWARD - M7.2 PREVIEW

**Duration:** 2:00 minutes

[SLIDE 11: M7.2 Preview - "Application Performance Monitoring with Datadog APM"]

"Next up: M7.2 - Application Performance Monitoring.

[SLIDE 12: Three Capabilities You'll Gain]

**Capability 1: Function-Level Performance Breakdown**
- See which function inside openai.generate span is slow
- Automatic profiling: no manual instrumentation needed
- Example: Discover serialize_prompt() takes 800ms (can optimize serialization)

**Capability 2: Memory Leak Detection**
- Monitor heap growth over time
- Identify objects not being garbage collected
- Example: Find that document_cache dict grows unbounded (add eviction policy)

**Capability 3: Database Query Optimization**
- See exact SQL queries being executed
- Identify N+1 query problems
- Example: Discover 50 individual SELECT queries (should be 1 JOIN)

[SLIDE 13: The Tool - Datadog APM]

M7.2 integrates Datadog APM:
- One-line setup: ddtrace-run python main.py
- Automatic instrumentation: FastAPI, Redis, OpenAI, Pinecone
- Correlates with your existing OpenTelemetry traces
- Function-level flame graphs showing CPU time

[SLIDE 14: M7.2 Driving Question]

**'Traces show openai.generate took 3.5 seconds. Which FUNCTION inside that span is the bottleneck?'**

M7.2 answers this with code-level profiling.

Ready to dive deeper? See you in M7.2.

[END - Total: 10 min 00 sec]

---

## PRODUCTION NOTES

**Recording Setup:**
- Within-module bridge (8-10 min)
- Conversational, building on M7.1 success
- Shift from "request-level" to "code-level" visibility
- Use waterfall visuals showing span boundaries vs function calls

**Slide Deck:**
- Total slides: 14
- Naming: M7-1-to-7-2-Bridge-01.png through M7-1-to-7-2-Bridge-14.png
- Key visuals: Jaeger waterfall with highlighted slow span, function-level breakdown preview

**References:**
- Source: M7.1 Augmented
- Next: M7.2 Augmented - Application Performance Monitoring
