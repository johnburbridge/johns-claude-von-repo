# Check Overlap Command

## Purpose
Review Epic, Stories, and Subtasks for duplication and rewrite children to reference parents efficiently.

## Usage
```
check-overlap <epic_id>
```

## Prompt Template

Review Epic {{epic_id}}, its Stories, and Subtasks for duplication.

Perform the following analysis:

## 1. Duplication Detection

### Check for Repeated Content
Scan all levels for:
- [ ] Identical sentences across Epic/Story/Subtask
- [ ] Redundant acceptance criteria
- [ ] Duplicated scope definitions
- [ ] Repeated technical requirements
- [ ] Overlapping deliverables

### Identify Overlap Patterns
- **Context Duplication**: Same background info at multiple levels
- **Requirement Duplication**: AC repeated in different forms
- **Scope Duplication**: Same boundaries defined repeatedly
- **Technical Duplication**: Implementation details at wrong level

## 2. Overlap Analysis Report

### Epic Level Duplication
```markdown
Found in Epic description:
- [Duplicated content]
- Also appears in: Story {{id}}, Subtask {{id}}
- Recommendation: Keep in Epic only, reference from children
```

### Story Level Duplication
```markdown
Found in Story {{story_id}}:
- [Duplicated content from Epic]
- Should reference: Epic {{epic_id}} instead
- Suggested rewrite: "As defined in Epic {{epic_id}}..."
```

### Subtask Level Duplication
```markdown
Found in Subtask {{subtask_id}}:
- [Duplicated content from Story]
- Should reference: Story {{story_id}}
- Suggested rewrite: "Per Story {{story_id}} requirements..."
```

## 3. Rewrite Templates

### Epic (Keep Context Here)
Original:
```
The system must support real-time search with sub-second response times,
handle 1000 requests per second, and maintain 99.9% availability.
```

Revised (no change - this is the source):
```
The system must support real-time search with sub-second response times,
handle 1000 requests per second, and maintain 99.9% availability.
```

### Story (Reference Epic)
Original:
```
This story implements search functionality. The system must support 
real-time search with sub-second response times, handle 1000 requests 
per second, and maintain 99.9% availability.
```

Revised:
```
This story implements search functionality per Epic {{epic_id}} 
performance requirements.
```

### Subtask (Reference Story)
Original:
```
Implement the search endpoint that returns results in sub-second time
as required by the Epic. The endpoint must handle 1000 requests per 
second with 99.9% availability.
```

Revised:
```
Implement search endpoint meeting Story {{story_id}} specifications.
```

## 4. Hierarchy Best Practices

### Epic Should Contain
- Overall goals and success metrics
- System-wide constraints (performance, security, compliance)
- High-level scope boundaries
- Cross-story dependencies
- Definition of done for entire epic

### Story Should Contain
- Reference to Epic ID (not repeat Epic content)
- Story-specific acceptance criteria
- Unique technical requirements for this story
- Story-level validation signals
- Dependencies on other stories

### Subtask Should Contain
- Reference to Story ID (not repeat Story content)
- Specific files/components to change
- Concrete deliverables
- Hour estimates
- Dependencies on other subtasks

## 5. Common Anti-Patterns to Fix

### Anti-Pattern: Cascading Requirements
```
Epic: "Search must be fast"
Story: "Search must be fast with sub-second response"
Subtask: "Implement fast search with sub-second response"
```

**Fix:**
```
Epic: "Search response time < 1 second for 95th percentile"
Story: "Implement search per Epic {{id}} performance requirements"
Subtask: "Optimize query for Story {{id}} performance targets"
```

### Anti-Pattern: Repeated Acceptance Criteria
```
Epic AC: "Users can search by product name"
Story AC: "Users can search by product name"
Subtask: "Allow users to search by product name"
```

**Fix:**
```
Epic: "Search capabilities include product name matching"
Story AC: "Product name search returns relevant results"
Subtask: "Implement name matching in search service"
```

### Anti-Pattern: Technical Details at Wrong Level
```
Epic: "Use Elasticsearch with specific mappings..."
Story: "Configure Elasticsearch with mappings..."
Subtask: "Set up Elasticsearch mappings..."
```

**Fix:**
```
Epic: "Full-text search capability required"
Story: "Implement full-text search using chosen technology"
Subtask: "Configure Elasticsearch mappings per design doc"
```

## 6. Validation Checklist

After removing overlaps, verify:

### Content Distribution
- [ ] Each requirement stated exactly once
- [ ] Requirements at appropriate level
- [ ] No redundant descriptions
- [ ] Clear parent-child references

### Readability
- [ ] Can understand story without reading epic details
- [ ] Can execute subtask without reading story details
- [ ] References are clear and traceable
- [ ] No information loss from de-duplication

### Maintainability
- [ ] Changes to epic don't require story updates
- [ ] Changes to story don't require subtask updates
- [ ] Single source of truth for each requirement
- [ ] Clear ownership at each level

## 7. Example Output

### Overlap Analysis Summary
```markdown
## Duplication Found: 12 instances

### Critical Overlaps (4)
1. Performance requirements repeated in 3 stories
2. Security constraints duplicated in 8 subtasks
3. API specifications in both Epic and Stories
4. Test coverage targets at all levels

### Minor Overlaps (8)
- Definition of done elements
- Technology choices
- Non-functional requirements
- Data model descriptions

### Recommended Actions
1. Move all NFRs to Epic level only
2. Keep API specs in Stories, reference from Subtasks
3. Centralize test coverage requirements in Epic
4. Remove all Epic descriptions from Stories

### Estimated Cleanup Impact
- Text reduction: ~40% across all items
- Clarity improvement: High
- Maintenance burden: Reduced
- Reference additions needed: 15
```

## 8. Revised Items Format

Return the cleaned-up versions in this format:

```markdown
## Epic {{epic_id}} (Revised)
[Cleaned epic text with no duplication]

## Story {{story_id}} (Revised)
References: Epic {{epic_id}}
[Cleaned story text with epic references]

## Subtask {{subtask_id}} (Revised)
References: Story {{story_id}}
[Cleaned subtask text with story references]
```