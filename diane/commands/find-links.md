---
description: Find semantic connections and suggest wikilinks for any note
---

# Find Links

Find semantically related notes in your vault and suggest wikilinks to create connections.

## Configuration

**⚠️ IMPORTANT**: This plugin reads configuration from `diane/.claude-plugin/config.json`. If configuration is not found or `vault_path` is empty, prompt the user to run `/diane:setup` first.

### Load Configuration

**Before starting**, read the plugin configuration file at `diane/.claude-plugin/config.json` to get:

- `vault_path` - Absolute path to the user's Obsidian vault
- `folders` - Folder names for clippings, fleeting, ideas, literature, permanent, project notes
- `naming.wikilink_format` - Display name format (e.g., `[[Ritual Interface Bridge]]`)

**Never hardcode paths.** Always construct full paths using: `${vault_path}/${folder_name}`

## Usage

This command helps you discover connections between notes that might not be immediately obvious. It's useful when:
- Writing a new permanent note and want to see what it connects to
- Reviewing an existing note and strengthening its connections
- Exploring conceptual clusters in your vault
- Finding orphaned notes that should be linked

## Workflow

### Step 1: Identify Target Note

Ask the user which note they want to find links for:

```
Find links for which note?

[1] Current note (if you're viewing one in Obsidian)
[2] Specify a note path
[3] Paste note content here

Your choice:
```

### Step 2: Read Note Content

Read the full content of the target note to understand:
- Main concepts and ideas
- Current arguments or themes
- Existing links (to avoid suggesting duplicates)
- Key terminology and vocabulary
- Overall topic and context

### Step 3: Search Vault Semantically

Search the entire vault for related notes, focusing on:

1. **Conceptual relationships** - Notes discussing related ideas, even with different terminology
2. **Thematic connections** - Notes in the same domain or subject area
3. **Complementary ideas** - Notes that extend, challenge, or build upon the target note's concepts
4. **Bridge notes** - Notes that connect the target to other clusters
5. **Source material** - Literature notes that provide foundation or evidence

**Search Priority (use configured folder paths):**
- Permanent notes (from `folders.permanent`) - Highest priority, most valuable connections
- Ideas notes (from `folders.ideas`) - High priority, sprawling multi-concept notes with many links
- Literature notes (from `folders.literature`) - Good for evidence and references
- Project notes (from `folders.project`) - Connect ideas to active work
- Fleeting notes (from `folders.fleeting`) - Lower priority, may reveal developing thoughts
- Clippings (from `folders.clippings`) - Lowest priority, imported content (often work of others)

### Step 4: Rank and Present Suggestions

Organize suggestions by confidence level and type:

```markdown
# Link Suggestions for: [Note Title]

## High Confidence Connections

**[[Ritual as Interface]]** (Permanent)
- Conceptual overlap: Both discuss ritual as structured protocol
- Connection: Core concept directly builds on this foundation
- Suggested placement: Links section

**[[Cyber Religion Essay]]** (Project)
- Thematic connection: Applies this concept to active project
- Connection: Practical application of theoretical framework
- Suggested placement: Why it matters section

## Medium Confidence Connections

**[[Interface Theory Johnson]]** (Literature)
- Source material: Provides theoretical grounding
- Connection: Evidence for interface metaphor
- Suggested placement: Evidence / References section

**[[Digital Sacred Practices]]** (Permanent)
- Related domain: Explores similar territory from different angle
- Connection: Complementary perspective
- Suggested placement: Links section

## Potential Connections (Lower confidence)

**[[Symbolic Systems]]** (Permanent)
- Tangential relationship: Related but not central
- Connection: Broader context

**[[Human Computer Interaction]]** (Literature)
- Domain connection: Related field
- Connection: Background context

## Orphaned Notes (might be relevant)

**[[Gestural Interfaces]]** (Fleeting)
- Possible connection: Physical ritual gestures as interface
- Status: Underdeveloped idea, may need review

---

Would you like to:
[A] Copy all high confidence links
[B] Copy specific links (enter numbers)
[C] See detailed explanations
[D] Search for specific concepts
[Q] Quit
```

### Step 5: Provide Link Text

Based on user selection, provide the wikilink text in the correct format:

```markdown
Copy these links:

[[Ritual as Interface]] [[Cyber Religion Essay]] [[Interface Theory Johnson]] [[Digital Sacred Practices]]

Or with context:

## Links
[[Ritual as Interface]] - Core concept foundation
[[Cyber Religion Essay]] - Active project application
[[Interface Theory Johnson]] - Theoretical grounding
[[Digital Sacred Practices]] - Complementary perspective
```

## Advanced Features

### Bidirectional Link Suggestions

When suggesting links, also mention if the target notes should link back:

```
Bidirectional link opportunity:

[[Cyber Religion Essay]] should link to this note.
Suggested location: "Key Ideas" section
Reason: This note develops a core concept for that project
```

### Cluster Visualization

Show how the target note fits into conceptual clusters:

```
This note is part of the "Interface Theory" cluster:

Central hub: [[Interface Theory Johnson]]
├─ [[Ritual as Interface]] ← Your note
├─ [[Digital Sacred Practices]]
├─ [[Gestural Interfaces]]
└─ [[API as Metaphor]]

Consider linking to other cluster members for stronger graph structure.
```

### Missing Bridge Notes

Identify gaps where a bridge note would strengthen connections:

```
Gap detected:

You have notes on:
- Ritual practices
- Interface design
- Cyber-religion

But no note explicitly connecting ritual and interface theory.

Suggested bridge note: "Ritual as Interface Protocol"
This would strengthen connections between these clusters.
```

## Best Practices

1. **Semantic Understanding:** Look beyond keywords to find conceptual relationships
2. **Quality Over Quantity:** Better to suggest 5 strong links than 20 weak ones
3. **Explain Connections:** Help the user understand why links are relevant
4. **Respect Atomicity:** Don't force connections that would dilute note focus
5. **Build Graph Structure:** Prioritize links that strengthen overall vault architecture

## Example Usage

**User:** "Find links for my note about ritual interfaces"

**You read:** `${vault_path}/${folders.permanent}/ritual-as-interface-bridge.md`

**You search vault and respond:**

```markdown
# Link Suggestions for: Ritual as Interface Bridge

## High Confidence Connections (4)

**[[Ritual Theory Anthropology]]** (Literature)
Conceptual foundation - provides academic grounding for ritual analysis

**[[Cyber Religion Sacred Algorithms]]** (Permanent)
Direct application - explores algorithmic rituals in digital contexts

**[[Interface Design Patterns]]** (Literature)
Parallel framework - interface patterns mirror ritual structures

**[[Human Computer Ritual]]** (Permanent)
Core overlap - directly examines ritualistic aspects of computing

## Medium Confidence Connections (3)

**[[Symbolic Interaction]]** (Permanent)
**[[Sacred Geometry Digital]]** (Fleeting)
**[[Protocol as Performance]]** (Permanent)

---

Copy high confidence links?
```

**User:** "Yes, copy all high confidence"

**You respond:**

```markdown
Here are your links in display name format:

[[Ritual Theory Anthropology]] [[Cyber Religion Sacred Algorithms]] [[Interface Design Patterns]] [[Human Computer Ritual]]

Paste these into your note's Links section.

Also suggested: [[Cyber Religion Sacred Algorithms]] should link back to this note in its "Key Ideas" section.
```

---

Begin by asking which note the user wants to find links for.
