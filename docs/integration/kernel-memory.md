# Using KERNEL + MEMORY Together

**How to store your cognitive structures in persistent memory**

---

## The Problem

**KERNEL** gives you structure (5-floor templates).
**MEMORY** gives you storage (knowledge graph).

**The gap:** KERNEL structures live in separate files. How do you track them over time? How do you connect them?

**The integration:** Store KERNEL structures as nodes in MEMORY graph.

---

## Quick Example

### Before Integration

**Using KERNEL alone:**
```
Floor_01_Foundation/
  project_alpha.md
  project_beta.md

Floor_02_Systems/  
  alpha_patterns.md
  beta_patterns.md
```

**Issues:**
- No way to link project_alpha → alpha_patterns
- No history tracking
- Hard to query across projects

### After Integration

**Using KERNEL + MEMORY:**
```python
from aurelion_memory_lite import LibrarySystem

memory = LibrarySystem()

# Store Floor 01 content
memory.add_document("project_alpha_foundation", {
    "type": "kernel_floor_01",
    "project": "alpha",
    "content": "..." 
})

# Store Floor 02 content  
mem.add_node("project_alpha_patterns", {
    "type": "kernel_floor_02",
    "project": "alpha",
    "content": "..."
})

# Link floors
mem.add_edge("project_alpha_foundation", "project_alpha_patterns", "evolves_into")
```

**Benefits:**
- ✅ Connections explicit
- ✅ Can query all Floor 01 foundations
- ✅ Track evolution over time

---

## Integration Patterns

### Pattern 1: Store Each Floor as Node

```python
floors = ["foundation", "systems", "networks", "action", "vision"]

for floor in floors:
    mem.add_node(f"{project}_{floor}", {
        "floor": floor,
        "project": project,
        "content": read_file(f"Floor_{floor}/project.md")
    })
```

### Pattern 2: Link Floors Vertically

```python
# Foundation → Systems → Networks → Action → Vision
mem.add_edge("alpha_foundation", "alpha_systems", "builds_on")
mem.add_edge("alpha_systems", "alpha_networks", "builds_on")
mem.add_edge("alpha_networks", "alpha_action", "builds_on")
mem.add_edge("alpha_action", "alpha_vision", "builds_on")
```

### Pattern 3: Query by Floor

```python
# Get all Vision-level content across projects
visions = mem.query_nodes(type="kernel_floor_05")

# Get full stack for one project
project_stack = mem.query_subgraph(starts_with="project_alpha")
```

---

## Workflow Example

### Starting a New Project

```python
# 1. Create KERNEL structure (markdown files)
# Use KERNEL templates in Floors 01-05

# 2. After a week of work, persist to MEMORY
from aurelion_memory_lite import LibrarySystem
from pathlib import Path

memory = LibrarySystem()

# Read your Floor 01 file
content = Path("Floor_01_Foundation/new_project.md").read_text()

# Store in memory
mem.add_node("new_project_foundation", {
    "type": "kernel_floor_01",
    "project": "new_project",
    "date_created": "2026-02-17",
    "content": content
})

# 3. Repeat for other floors as you fill them out
```

### Tracking Project Evolution

```python
# Month 1: Foundation
mem.add_node("project_v1_foundation", {...})

# Month 3: Updated foundation with new insights
mem.add_node("project_v2_foundation", {...})

# Link versions
mem.add_edge("project_v1_foundation", "project_v2_foundation", "updated_to")

# Query evolution
history = mem.get_path("project_v1_foundation", "project_v2_foundation")
```

### Cross-Project Insights

```python
# Find all Floor 03 (Networks) content
networks = mem.query_nodes(type="kernel_floor_03")

# Find patterns across projects
for node in networks:
    print(f"Project: {node['project']}")
    print(f"Networks: {node['content']}")
    # Identify common patterns
```

---

## Real-World Use Cases

### Use Case 1: Academic Research

**Problem:** Tracking research questions (Floor 01) → methodology (Floor 02) → findings (Floor 03) → papers (Floor 04) → career impact (Floor 05)

**Solution:**
```python
mem.add_node("research_question_A", {"type": "floor_01", ...})
mem.add_node("methodology_for_A", {"type": "floor_02", ...})
mem.add_edge("research_question_A", "methodology_for_A", "answered_by")

# Later: Query all methodologies that answered similar questions
```

### Use Case 2: Career Planning

**Problem:** Career goals (Floor 05) connected to specific skills (Floor 02) and daily actions (Floor 01)

**Solution:**
```python
mem.add_node("career_vision_2026", {"type": "floor_05", "goal": "Senior Engineer"})
mem.add_node("skill_system_design", {"type": "floor_02", "area": "technical"})
mem.add_node("daily_coding", {"type": "floor_01", "habit": "2hr/day"})

mem.add_edge("daily_coding", "skill_system_design", "builds")
mem.add_edge("skill_system_design", "career_vision_2026", "enables")

# Query: What daily actions lead to my vision?
path = mem.get_path("daily_coding", "career_vision_2026")
```

### Use Case 3: Project Management

**Problem:** Multiple projects, each with 5-floor structure, need to see connections

**Solution:**
```python
# Project A and B both need floor_02_database_design
mem.add_node("project_A_systems", {"depends_on": "database_design"})
mem.add_node("project_B_systems", {"depends_on": "database_design"})
mem.add_node("shared_database_design", {"type": "reusable_system"})

mem.add_edge("project_A_systems", "shared_database_design", "uses")
mem.add_edge("project_B_systems", "shared_database_design", "uses")

# Query: Which projects depend on this system?
dependents = mem.query_incoming_edges("shared_database_design")
```

---

## Technical Setup

### File Structure

```
my-aurelion-workspace/
├── aurelion-kernel-lite/  (cloned from GitHub)
│   ├── Floor_01_Foundation/
│   ├── Floor_02_Systems/
│   └── ...
├── aurelion-memory-lite/  (cloned from GitHub)
│   ├── aurelion_memory_lite/  ← Python package
│   └── data/
└── my_integration/
    └── sync_kernel_to_memory.py  ← Your integration script
```

### Integration Script Template

```python
# sync_kernel_to_memory.py

from pathlib import Path
import sys
sys.path.append("../aurelion-memory-lite")

from aurelion_memory_lite import LibrarySystem
import json
from datetime import datetime

# Initialize memory system
memory = LibrarySystem(
    graph_path="../aurelion-memory-lite/data/knowledge_graph.json"
)

def sync_floor_to_memory(floor_path, floor_number, project_name):
    """Read KERNEL markdown files and store in MEMORY"""
    
    for md_file in Path(floor_path).glob("*.md"):
        content = md_file.read_text()
        
        node_id = f"{project_name}_floor_{floor_number}_{md_file.stem}"
        
        mem.add_node(node_id, {
            "type": f"kernel_floor_{floor_number:02d}",
            "project": project_name,
            "file": str(md_file),
            "content": content,
            "last_updated": datetime.now().isoformat()
        })
        
        print(f"Synced: {node_id}")

# Usage
if __name__ == "__main__":
    kernel_root = Path("../kernel/aurelion-kernel-lite")
    
    sync_floor_to_memory(kernel_root / "Floor_01_Foundation", 1, "my_project")
    sync_floor_to_memory(kernel_root / "Floor_02_Systems", 2, "my_project")
    # ... repeat for floors 3-5
    
    mem.save()
    print("Sync complete!")
```

---

## Best Practices

### 1. Sync Regularly
Run integration script weekly or after major updates.

### 2. Use Timestamps
Track when each floor was created/updated.

### 3. Version History
Don't overwrite nodes - create new versions and link them.

### 4. Explicit Connections
Make edge relationships specific: "builds_on", "evolves_into", "depends_on"

### 5. Query Often
Use MEMORY queries to discover patterns across projects.

---

## What You Gain

**Without integration:**
- Separate markdown files
- Manual tracking of connections
- Hard to see patterns across projects

**With integration:**
- Persistent structured knowledge
- Queryable history
- Cross-project insights
- Evolution tracking
- Reusable patterns

---

## Next Steps

1. Set up both KERNEL and MEMORY modules
2. Use KERNEL templates for a project (1-2 weeks)
3. When you have substance, start syncing to MEMORY
4. Build queries to discover patterns
5. Let insights emerge from the graph

---

[← Back to Integration Guides](../README.md#-integration-guides)
