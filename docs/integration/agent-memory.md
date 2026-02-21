# Using AGENT + MEMORY Together

**How AI collaboration protocols combine with persistent memory to create a stateful, integrity-checked thinking partner**

---

## The Combination

**AGENT** defines how the AI should behave (6 integrity triggers, session protocols, challenge patterns).
**MEMORY** stores and retrieves your knowledge graph across sessions.

**The gap without integration:** AGENT protocols reset every session. MEMORY stores knowledge but has no behavioral layer.

**The integration:** AGENT triggers apply to Memory queries — so the AI doesn't just retrieve stored knowledge, it validates it, flags staleness, and challenges assumptions built on outdated data.

---

## Quick Example

### Without Integration

**AGENT alone:**
- Integrity triggers fire based only on what's in the current session
- No access to prior decisions, past context, or documented commitments
- Session closes — all behavioral context resets

**MEMORY alone:**
- Retrieves documents on request
- Has no way to flag when data is stale or when two documents conflict
- Returns whatever is stored without challenging its accuracy

### With Integration

**AGENT + MEMORY:**
- Memory retrieves a document → AGENT Trigger 1 checks the timestamp and flags if it's outdated
- Two Memory documents conflict → AGENT surfaces the contradiction before the user acts on it
- Session opens → Memory loads prior context → AGENT applies session open protocol to that context automatically
- Session closes → AGENT generates handoff note → Memory stores it as the active Floor 4 session file

---

## Step-by-Step Integration Workflow

### Step 1 — Configure the Session Open Protocol with Memory

At the start of every session, AGENT's session open protocol runs against Memory:

```
AGENT Session Open (Memory-augmented):

1. Current task:
   → Memory: load active Floor 4 handoff note
   → Surface: "Last session: [what was done]. Open items: [list]."

2. Active context:
   → Memory: load top 3 Floor 5 goals
   → Surface: "Your current strategic priorities are: [list]."

3. Decision authority:
   → User confirms: what can the AI decide vs. what needs approval today?

4. Hard constraints:
   → Memory: load any Floor 2 SOPs relevant to today's task
   → Apply constraints from documented standards, not just stated ones
```

This means the AI walks into every session already knowing where you left off — without you having to re-explain.

### Step 2 — Apply AGENT Trigger 1 to All Memory Retrievals

Trigger 1 (Data Integrity) should fire automatically whenever Memory returns a document:

```
Memory returns: Floor_01_Foundation/career-master.md
AGENT checks:
  - Last updated: [date]
  - If > 90 days old: "Data integrity flag — this Career Master was last updated
    [X days] ago. Skills inventory may be stale. Proceed with current data or
    update first?"
  - If < 90 days: proceed normally
```

**Why this matters:** Stored knowledge has a shelf life. Making career decisions on a year-old skills inventory is a silent data integrity failure.

### Step 3 — Apply AGENT Trigger 5 to Memory Graph Traversal

When Memory traverses the knowledge graph, AGENT Trigger 5 (Blind Spot) should surface relationships the user hasn't noticed:

```
User asks: "What do I know about Project Alpha?"
Memory returns: Floor_04_Action/project-alpha.md
AGENT checks graph relationships:
  → project-alpha DEPENDS_ON alpha-vendor-contract
  → alpha-vendor-contract last updated 6 months ago
  → AGENT surfaces: "One thing worth noting — Project Alpha depends on the
    vendor contract in Floor 2, which hasn't been reviewed since [date].
    Worth checking before proceeding?"
```

### Step 4 — Use AGENT Trigger 1 for CONTRADICTS Relationships

MEMORY tracks `CONTRADICTS` relationships between documents. AGENT should surface these before the user acts:

```python
# Memory detects a contradiction
graph.add_relationship(
    "Floor_05_Vision/goal-senior-de.md",
    "Floor_04_Action/project-alpha.md",
    relationship_type="CONTRADICTS",
    note="Goal requires system design ownership; current project is analysis-only"
)
```

```
AGENT surfaces: "Data integrity flag — your Floor 5 goal (Senior DE by Q4 2026)
has a CONTRADICTS relationship with your current active project (Project Alpha —
analysis scope only). This tactic may not close the system design gap you need.
How would you like to proceed?"
```

### Step 5 — AGENT Session Close Writes to Memory

Every session close produces a handoff note that goes directly into Memory:

```
AGENT Session Close → writes to Memory:

File: Floor_04_Action/session-handoff-2026-02-21.md

## Session Handoff — February 21, 2026

### What was done
- Reviewed promotion gap analysis
- Updated stakeholder map for Director of DE
- Drafted 90-day sprint plan

### What remains
- Need manager buy-in for pipeline migration ownership
- Skip-level meeting not yet scheduled

### First action next session
Schedule skip-level meeting with Director of DE before Feb 28.

### State of play
Sprint plan is drafted and stored in Floor_04_Action/sprint-q1-2026.md.
Stakeholder map updated in Floor_03_Networks/director-de.md.
```

Next session, Memory loads this automatically during step 1.

---

## The Full Loop

```
Session opens
    ↓
Memory loads prior context (Floor 4 handoff + Floor 5 goals)
    ↓
AGENT applies session open protocol to loaded context
    ↓
[Work happens]
    ↓
AGENT Trigger 1 fires on any retrieved Memory document (staleness check)
    ↓
AGENT Trigger 5 fires on graph traversal (blind spot detection)
    ↓
AGENT Trigger 1 fires on CONTRADICTS relationships (conflict surfacing)
    ↓
Session closes
    ↓
AGENT generates handoff note
    ↓
Memory stores handoff note in Floor 4
    ↓
[Next session begins — loop repeats]
```

---

## MCP Server: Full Automation

With the AURELION Memory MCP server running, this entire loop is automated inside Claude Desktop or VS Code Copilot Chat:

```json
{
  "mcpServers": {
    "aurelion-memory": {
      "command": "python",
      "args": ["-m", "aurelion_memory_mcp"],
      "env": {
        "AURELION_MEMORY_PATH": "/path/to/your/memory-store"
      }
    }
  }
}
```

With MCP active:
- `memory_session` tool loads the prior session context automatically at open
- `memory_search` tool triggers AGENT integrity checks on every retrieval
- `memory_write` tool stores AGENT session close output without manual file management

Install: `pip install aurelion-memory-lite`

---

## When to Use This Integration

| Situation | What to do |
|-----------|-----------|
| Starting any work session | Let Memory load prior context, let AGENT challenge it |
| Retrieving stored facts to make a decision | Apply Trigger 1 — check staleness before acting |
| Working across multiple projects with shared dependencies | Let Memory graph traversal + Trigger 5 surface cross-project blind spots |
| Long-running projects (months of context) | AGENT session close → Memory write ensures nothing is lost between windows |
| Conflicting stored information | Let CONTRADICTS relationships + Trigger 1 surface conflicts before they cause errors |

---

## Integration with the Full Ecosystem

- **Add KERNEL** — Memory stores KERNEL floor documents; AGENT protocols apply to their retrieval and validation.
- **Add ADVISOR** — AGENT integrity triggers apply during Advisor career planning sessions; Memory stores all Advisor output for cross-session continuity.

Full ecosystem: https://github.com/chase-key/aurelion-hub
