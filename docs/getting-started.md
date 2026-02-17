# Getting Started with AURELION

**First time here? Not sure which module to choose? This guide will help.**

---

## The AURELION Philosophy

AURELION is not a single product you install. It's a **toolkit** - a collection of independent modules you can use separately or combine based on your needs.

**Think of it like LEGO blocks:** Each module is complete on its own, but they connect in powerful ways.

---

## Quick Decision Tree

### 1. What's your immediate need?

**"I need to organize complex information"**
→ [KERNEL-Lite](#start-with-kernel)

**"I want to remember things better"**
→ [MEMORY-Lite](#start-with-memory)

**"I'm planning my career or a big project"**
→ [ADVISOR-Lite](#start-with-advisor)

**"I work with AI assistants frequently"**
→ [AGENT-Lite](#start-with-agent)

**"I'm building a simulation or game world"**
→ [NEXUS-Lite](#start-with-nexus)

**"I want it all / not sure yet"**
→ [Start with KERNEL](#start-with-kernel) (most foundational)

---

## Start with KERNEL

**Best for:** Anyone who needs to organize thinking across multiple levels of abstraction.

### Install
```bash
git clone https://github.com/chase-key/aurelion-kernel-lite.git
cd aurelion-kernel-lite
```

### Quick Start
1. Open `Floor_01_Foundation/` - Start here
2. Use templates to structure your first project/goal
3. Progress upward through floors as complexity grows

### Next Steps
- Add **MEMORY** to persist your cognitive structures
- Add **ADVISOR** for strategic planning templates

---

## Start with MEMORY

**Best for:** Anyone who wants to build a personal knowledge graph that grows with them.

### Install
```bash
git clone https://github.com/chase-key/aurelion-memory-lite.git
cd aurelion-memory-lite
pip install -r requirements.txt
```

### Quick Start
```python
from aurelion_memory_lite import LibrarySystem

memory = LibrarySystem()
memory.add_document("concept_name", {"type": "idea", "content": "..."})
```

### Next Steps
- Add **KERNEL** to structure what you remember
- Add **AGENT** to query memory with AI assistance

---

## Start with ADVISOR

**Best for:** Career planning, strategic decision-making, project planning.

### Install
```bash
git clone https://github.com/chase-key/aurelion-advisor-lite.git
cd aurelion-advisor-lite
```

### Quick Start
1. Browse `templates/` for planning frameworks
2. Study `examples/alex_thompson/` case study
3. Apply templates to your situation

### Next Steps
- Add **KERNEL** to structure your plans
- Add **AGENT** for AI-assisted planning
- Add **MEMORY** to track decisions over time

---

## Start with AGENT

**Best for:** Anyone working with AI assistants (ChatGPT, Claude, Copilot).

### Install
```bash
git clone https://github.com/chase-key/aurelion-agent-lite.git
cd aurelion-agent-lite
```

### Quick Start
1. Read `prompts/00_INDEX.md` for prompt categories
2. Use `protocols/strategic_advisor_protocol.md` for deep work
3. Apply `protocols/session_management.md` for long projects

### Next Steps
- Add **MEMORY** to persist AI conversations
- Add **ADVISOR** for strategic AI partnerships

---

## Start with NEXUS

**Best for:** Game masters, writers, simulation builders, world-builders.

### Install
```bash
git clone https://github.com/chase-key/aurelion-nexus-lite.git
cd aurelion-nexus-lite
pip install -r requirements.txt
```

### Quick Start
```python
from aurelion_s import World, NPC, Location

world = World()
npc = NPC(name="Character", personality="...")
location = Location(name="Place", description="...")
```

### Next Steps
- Add **MEMORY** to track world state
- Add **AGENT** to generate content with AI

---

## "I Want to Build CK's System"

CK uses all 5 modules together. Here's the recommended order:

### Phase 1: Foundation (Week 1)
1. **KERNEL** - Organize your first project with 5-floor structure
2. **MEMORY** - Start tracking knowledge as you work

### Phase 2: Intelligence (Week 2-3)
3. **AGENT** - Improve AI collaboration workflows
4. **ADVISOR** - Apply planning templates to a real goal

### Phase 3: Extension (When needed)
5. **NEXUS** - Add simulation when you have a specific use case

**Timeline:** 2-4 weeks to full integration (going at your own pace)

---

## Common Combinations

### "Personal Knowledge Management"
- KERNEL (structure) + MEMORY (storage) + AGENT (AI queries)

### "Career Planning"
- ADVISOR (templates) + KERNEL (organize) + MEMORY (track)

### "AI-Assisted Research"
- AGENT (prompts) + MEMORY (store findings) + KERNEL (structure insights)

### "Game Master / Writer"
- NEXUS (simulation) + MEMORY (world state) + AGENT (content generation)

### "Full Cognitive System" (CK's setup)
- All 5 modules interconnected

---

## System Requirements

### All Modules
- Any OS (Windows, macOS, Linux)
- Text editor or IDE

### MEMORY, NEXUS
- Python 3.8+
- ~10MB disk space per module

### KERNEL, ADVISOR, AGENT
- Just text files (markdown)
- No special dependencies

---

## Getting Help

**Module-specific issues:**
→ Open issue in that module's GitHub repo

**Integration questions:**
→ Open discussion in [aurelion-hub](https://github.com/chase-key/aurelion-hub)

**General questions:**
→ See [integration guides](integration/) or open a discussion

---

## What NOT to Expect

❌ **Not a complete app** - These are building blocks, not finished products

❌ **Not plug-and-play** - You'll need to adapt templates to your needs

❌ **Not AI-powered out of box** - Lite tier requires you to bring your own AI (Copilot, ChatGPT, etc.)

✅ **What to expect:** Flexible, powerful tools that grow with you

---

## Next Steps

1. **Pick one module** based on your immediate need
2. **Clone and explore** for 1-2 days
3. **Add a second module** when you see the integration opportunity
4. **Build your system** over weeks/months

**Remember CK's journey:** He didn't start with everything. Start small, grow organically.

---

[← Back to Hub](../README.md) | [Integration Guides →](integration/)

