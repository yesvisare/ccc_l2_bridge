# Module 6: Enterprise Security & Compliance
## Bridge: M6.2 Secrets Management → M6.3 RBAC & Access Control
**Duration:** 8-10 minutes  
**Type:** Within-Module Bridge  
**Purpose:** Celebrate M6.2 completion, create urgency for M6.3

---

## [00:00-01:30] 1) RECAP: WHAT WAS ACCOMPLISHED

**Duration:** 1:30 minutes  
**Purpose:** Acknowledge completion, celebrate progress

[SLIDE 1: Title card - "Bridge: M6.2 Secrets Management → M6.3 RBAC & Access Control"]

"You just solved one of the most dangerous security vulnerabilities in production systems. Let's review what you built.

[SLIDE 2: M6.2 Achievements Checklist]

Here's what you accomplished in M6.2: Secrets Management & Rotation:

[Point to each item]

✓ **HashiCorp Vault deployed and integrated** — You set up Vault server (locally or in cloud), configured environment-specific secret paths (dev/staging/prod with complete isolation), and built the VaultClient wrapper with connection pooling and error handling. Your application now fetches secrets dynamically instead of loading from .env files.

✓ **Zero-downtime secret rotation implemented** — You tested rotating OpenAI and Pinecone keys while your RAG system was serving live requests. With 5-minute grace periods, both old and new keys remained valid during transition. Result: Zero failed requests during rotation. You can now rotate credentials in production without any service disruption.

✓ **Secret leaks prevented at the source** — You installed pre-commit hooks with detect-secrets that scan every commit before it reaches GitHub. You ran truffleHog on your git history and found (and rotated) 2-3 leaked credentials from months ago. Future secret leaks are now impossible—commits with secrets are automatically blocked.

✓ **Complete audit trail established** — Every secret access is now logged with identity, timestamp, action, and source IP. When auditors ask 'Who accessed production database password?', you can show them exact logs. SOC 2 and ISO 27001 audit requirements: met.

[Pause 3 seconds]

Your secrets are no longer sitting in .env files waiting to leak. They're centralized, automatically rotated, and fully audited."

[END - 01:30]

---

## [01:30-03:30] 2) WHY NEXT: THE ACCESS CONTROL GAP

**Duration:** 2:00 minutes  
**Purpose:** Create urgency for RBAC

[SLIDE 3: The Hidden Vulnerability]

"But there's a critical security gap we haven't addressed yet.

You've protected your API keys and credentials. But what about your USERS' access to data?

[SLIDE 4: Current State - Everyone Sees Everything]

Right now, your RAG system has authentication but no authorization:

```
Authentication (✅ Working):
- User presents API key
- System verifies key is valid
- Request proceeds

Authorization (❌ Missing):
- User has ANY valid key → sees ALL documents
- No role checking (admin vs. user vs. intern)
- No document-level permissions
```

[Point to diagram showing access problem]

**What this means in practice:**

[SLIDE 5: Real Scenarios - The Access Problem]

**Scenario 1: Confidential documents leaked internally**
- Legal team uploads board meeting minutes marked "confidential"
- Marketing intern queries: "What's our M&A strategy?"
- RAG system retrieves board minutes: "We're acquiring Company X for $50M"
- Intern now knows confidential acquisition details

**Scenario 2: HR data exposed across departments**
- HR uploads employee performance reviews
- Engineer queries: "How is John performing?"
- RAG system returns: "John received poor performance rating, on PIP"
- Engineer gossips, John finds out, HR violation occurs

**Scenario 3: Financial data accessible to all**
- Finance uploads quarterly revenue forecasts
- Sales rep queries: "What's our Q4 target?"
- RAG system reveals: "We're $5M behind Q4 projections"
- Sales rep posts on LinkedIn: "Company struggling financially"
- Stock price drops 15%

[Pause 4 seconds]

**Validation-Driven Quantification:**

[SLIDE 6: Cost of Missing Authorization]

Every department without document-level access control creates risk:

**HR Documents:**
- Employee records, performance reviews, salary data
- Exposure risk: EEOC violations, discrimination lawsuits
- Cost per violation: $50K-500K in settlements
- Compliance requirement: Must prove who accessed employee data

**Legal Documents:**
- Board minutes, M&A discussions, IP filings
- Exposure risk: Insider trading, competitive disadvantage
- Cost per leak: $1M-10M in lost deal value or SEC fines
- Compliance requirement: Attorney-client privilege must be maintained

**Financial Documents:**
- Revenue forecasts, profitability data, customer contracts
- Exposure risk: Public disclosure before earnings, competitor intelligence
- Cost per leak: Market cap loss (10-30% stock drop) + SEC inquiry
- Compliance requirement: Sarbanes-Oxley requires access controls on financial data

**Engineering Documents:**
- System architecture, security vulnerabilities, deployment keys
- Exposure risk: External attackers gain internal knowledge
- Cost per leak: Data breach ($4.5M average per IBM study) + reputation damage
- Compliance requirement: SOC 2 requires least privilege access

[SLIDE 7: Reality Check]

**Reality Check:** Authentication tells you WHO someone is. Authorization tells you WHAT they can access. You have the first, but not the second.

[Pause 3 seconds]

**Without role-based access control, your RAG system is a compliance violation waiting to happen.**

Current state:
- 50 employees across 5 departments
- Everyone with API key sees everything
- No way to restrict documents by role or department
- Audit fails: "No evidence of least privilege access control"

M6.3 solves this."

[END - 03:30]

---

## [03:30-05:30] 3) READINESS CHECKLIST

**Duration:** 2:00 minutes  
**Purpose:** Verify M6.2 completion before proceeding

[SLIDE 8: Checklist - 4 Critical Items]

"Before moving to M6.3: RBAC & Access Control, verify these four things:

[Read each item with clear pauses]

☐ **Vault integration working with zero .env dependencies**
   → Check: Run `python -c "from vault_client import VaultClient; v = VaultClient(); print(v.get_rag_secrets())"` and verify it returns secrets without reading .env
   → Impact: RBAC system will also use Vault for storing role definitions. If Vault isn't working, M6.3 becomes much harder. Fixing now saves 2-3 hours of debugging.

☐ **Secret rotation tested with zero downtime**
   → Check: Your notes from M6.2 should show: "Rotated OpenAI key, 0 failed requests during 5-minute grace period"
   → Impact: RBAC policies will also need rotation (when user leaves company, revoke access). Understanding rotation from M6.2 prepares you for policy rotation in M6.3. Testing now saves confusion later.

☐ **Pre-commit hooks installed and tested**
   → Check: Run `git add .env` (if .env exists) and verify pre-commit hook BLOCKS the commit with "Detected secrets"
   → Impact: In M6.3, you'll be adding user-role mappings to your codebase. Pre-commit hooks ensure you never accidentally commit "user:admin:true" mappings with real usernames. Prevention saves credential rotation.

☐ **Git history scanned and leaked secrets rotated**
   → Check: Your SECRETS_AUDIT.md from M6.1 checkpoint should list: "Total findings: X, Real secrets: Y, All rotated: ✓"
   → Impact: RBAC depends on secrets (API keys map to users, users map to roles). If old secrets are leaked and not rotated, attackers can bypass RBAC by using leaked admin keys. Rotating now eliminates this attack vector.

[SLIDE 9: Warning]

**Warning:** If Vault isn't fully integrated, M6.3 becomes significantly harder. RBAC policies need centralized storage—we'll use Vault for this. If you're still using .env files for ANY secrets, fix that before M6.3. Otherwise you'll have a hybrid system (some secrets in Vault, some in files) which is the worst of both worlds."

[END - 05:30]

---

## [05:30-07:30] 4) PRACTATHON CHECKPOINT

**Duration:** 2:00 minutes  
**Purpose:** Transition exercise bridging secrets management to access control

[SLIDE 10: PractaThon Framework - Learn/Build/Apply/Ship]

"Your checkpoint exercise before M6.3: RBAC & Access Control.

This 30-minute exercise helps you map your current users to future roles, preparing for M6.3's implementation.

**Learn (5 min):**
- List all current users of your RAG system (anyone with an API key)
- For each user, document their department and responsibilities
- Example:
  - John (Engineering, Senior Dev) — needs access to tech docs
  - Sarah (HR, Manager) — needs access to HR policies
  - Mike (Legal, Associate) — needs access to legal docs only
- Identify 3-5 logical role groupings based on access patterns

**Build (10 min):**
- Create `rbac_design.md` documenting your role hierarchy
- Define roles with clear permissions:
  ```
  Admin: Full access to all documents, can manage users
  HR_Manager: Access to HR docs + general company policies
  Engineer: Access to technical docs + general policies
  Legal: Access to legal docs only
  Viewer: Read-only access to public docs only
  ```
- For each current user, assign to one role
- Identify edge cases: "What if engineer needs to see HR policy for parental leave?"

**Apply (10 min):**
- Map your existing Pinecone metadata to RBAC requirements
- Review documents currently indexed: What metadata exists?
  - `source`: File path
  - `chunk_index`: Position in document
  - `page_number`: Page reference
  - **Missing:** `department`, `confidentiality_level`, `allowed_roles`
- Design metadata schema for access control:
  ```json
  {
    "chunk_text": "...",
    "metadata": {
      "source": "hr_policy_parental_leave.pdf",
      "department": "hr",
      "confidentiality": "internal",
      "allowed_roles": ["admin", "hr_manager", "hr_staff"],
      "pii_checked": true
    }
  }
  ```
- Calculate re-indexing scope: How many documents need metadata updates?

**Ship (5 min):**
- Create `RBAC_AUDIT.md` documenting:
  - Total users: X
  - Role hierarchy: [List roles with descriptions]
  - User→Role mappings: [Table showing each user's assigned role]
  - Metadata gaps: [List documents missing access control metadata]
  - Re-indexing plan: "Need to update metadata for Y documents"
- Commit this audit to your repo

[SLIDE 11: Expected Output Example]

Your `RBAC_AUDIT.md` should look like this:

```markdown
# RBAC Design Audit - M6.2 Checkpoint

**Date:** 2025-11-02
**Prepared by:** [Your Name]

## Current Users
| User | Department | Current Access | Proposed Role |
|------|------------|----------------|---------------|
| John | Engineering | Full | engineer |
| Sarah | HR | Full | hr_manager |
| Mike | Legal | Full | legal_staff |
| Alex | Sales | Full | viewer |

**Total: 4 users, all currently have full access**

## Role Hierarchy Design
1. **admin** — Full system access, user management
2. **hr_manager** — HR docs + general policies, can manage HR users
3. **engineer** — Technical docs + general policies
4. **legal_staff** — Legal docs only
5. **viewer** — Public docs only (marketing materials, press releases)

## Required Metadata Changes
**Current documents: 250**
**Documents with department tags: 0**
**Documents needing metadata updates: 250**

**Proposed metadata schema:**
- `department`: hr | engineering | legal | sales | general
- `confidentiality`: public | internal | confidential | restricted
- `allowed_roles`: [list of role IDs that can access]

## Re-indexing Plan
1. Extract all documents from Pinecone (250 docs)
2. Manual classification: HR=50, Legal=30, Engineering=120, General=50
3. Add metadata tags per classification
4. Re-index with updated metadata
**Estimated time: 3-4 hours**

## Edge Cases Identified
1. Engineer needs parental leave policy (HR doc) → Add to general access
2. Legal needs architecture docs for patent filing → Create temporary override
3. Sales needs competitor analysis (confidential) → Create sales_manager role
```

This design becomes your implementation blueprint for M6.3."

[END - 07:30]

---

## [07:30-08:30] 5) CALL-FORWARD: NEXT FOCUS

**Duration:** 1:00 minute  
**Purpose:** Preview M6.3 RBAC

[SLIDE 12: M6.3 Preview - "RBAC & Multi-Level Access Control"]

"Tomorrow, in M6.3: RBAC & Multi-Level Access Control, you'll add:

**1. Role-based access control with Casbin policy engine**
   → Implement admin, manager, staff, and viewer roles with hierarchical permissions. Use Casbin to define "admin inherits all permissions" and "viewer can only read public docs." Your application will check permissions before every query with <100ms overhead.

**2. Document-level access filtering using Pinecone metadata**
   → Tag every document with `department` and `allowed_roles` metadata. When users query, automatically filter: 'Only show documents where allowed_roles includes user's role.' Pinecone handles filtering natively—no post-processing needed.

**3. Real-time permission enforcement with audit logging**
   → Every query gets logged: user ID, role, documents accessed, timestamp. Track permission denial attempts: "User Alex (viewer role) tried to access Legal/BoardMinutes.pdf at 2:15 PM—blocked by RBAC." Compliance auditors love this.

[SLIDE 13: The Question for M6.3]

**"How do you prevent interns from seeing executive-only documents without breaking your existing RAG pipeline or degrading query performance?"**

[Pause 3 seconds]

Complete your RBAC design checkpoint, then we'll implement the permission system that makes your RAG system enterprise-ready.

See you then.

[END - Total: 08 min 30 sec]

---

## PRODUCTION NOTES

**Recording Setup:**
- Use teleprompter for script
- Pause cues = add 2-5 sec silence in edit
- Emphasize real scenarios (board minutes leak, HR violation, financial leak)
- Source: Extracted from M6.2 Augmented accomplishments + M6.3 Augmented introduction

**Slide Deck Requirements:**
- Total slides: 13
- Naming: M6-Bridge-6-2-to-6-3-01.png through M6-Bridge-6-2-to-6-3-13.png
- Key visuals needed:
  - Achievement checklist with checkmarks (Slide 2)
  - Authentication vs Authorization diagram (Slide 4)
  - Real scenario consequences (Slide 5)
  - Cost breakdown per document type (Slide 6)
  - RBAC design example (Slide 11)
- Use red warning colors for security scenarios
- Use green checkmarks for accomplishments

**Visual Assets:**
- m6_2_achievements.png (4 checkmarks with descriptions)
- auth_vs_authz_diagram.png (showing the gap)
- access_scenarios.png (3 real failure scenarios)
- rbac_design_template.png (markdown example)

**References:**
- Source: M6.2 Augmented: Secrets Management & Rotation
- Next: M6.3 Augmented: RBAC & Multi-Level Access Control
- Related: Level 1 M3.3 (API Development & Security)

**Editing Notes:**
- Build tension during scenario section (emphasize consequences)
- Use moderate pacing for cost quantification (let numbers sink in)
- Friendly, supportive tone for RBAC design checkpoint
- Emphasize "this is the missing piece" narrative
