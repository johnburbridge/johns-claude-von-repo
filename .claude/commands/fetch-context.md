# Fetch Context Command

## Purpose
Retrieve and synthesize the Epic → Story → Subtask hierarchy from Linear using the Linear MCP integration for comprehensive context.

## Prerequisites
- Linear MCP server configured and authenticated in Claude Code
- Access to the Linear workspace
- Proper permissions for the requested issues

## Usage
```
fetch-context <item_identifier>
```

Where `item_identifier` can be:
- Linear issue key (e.g., `ENG-123`)
- Issue title or partial title
- Issue URL from Linear

## Linear MCP Tools Available

The Linear MCP server provides these tools:
- **Search Issues**: Find issues with flexible filtering
- **Get Sprint Issues**: Get current sprint issues for a team
- **Search Teams**: Find team information
- **Filter Sprint Issues**: Filter sprint issues by status
- **Create Issues**: Create new issues (for follow-ups)

## Fetch Context Process

### Step 1: Find the Target Issue

Use the Linear MCP `Search Issues` tool:
```
Tool: Search Issues
Parameters:
  query: "{{item_identifier}}"
  limit: 1
  
Returns: Issue details including ID, title, description, status, parent, labels
```

### Step 2: Determine Issue Type

Analyze the returned issue to determine its type:
```
IF issue.parent exists AND issue.parent.parent exists:
  → This is a Subtask
ELIF issue.parent exists:
  → This is a Story
ELIF issue.children exist OR issue.labels contains "Epic":
  → This is an Epic
ELSE:
  → Standalone issue
```

### Step 3: Traverse the Hierarchy

#### For a Subtask - Get Full Context:
```
1. Current Subtask (already fetched)

2. Parent Story:
   Tool: Search Issues
   Parameters:
     query: "{{subtask.parent.id}}"
     limit: 1

3. Parent Epic:
   Tool: Search Issues
   Parameters:
     query: "{{story.parent.id}}"
     limit: 1

4. Sibling Subtasks:
   Tool: Search Issues
   Parameters:
     query: "parent:{{story.id}}"
     limit: 20
```

#### For a Story - Get Context Up and Down:
```
1. Current Story (already fetched)

2. Parent Epic (if exists):
   Tool: Search Issues
   Parameters:
     query: "{{story.parent.id}}"
     limit: 1

3. Child Subtasks:
   Tool: Search Issues
   Parameters:
     query: "parent:{{story.id}}"
     status: ["Todo", "In Progress", "In Review"]
     limit: 50

4. Sibling Stories (same Epic):
   Tool: Search Issues
   Parameters:
     query: "parent:{{epic.id}}"
     limit: 20
```

#### For an Epic - Get All Children:
```
1. Current Epic (already fetched)

2. Child Stories:
   Tool: Search Issues
   Parameters:
     query: "parent:{{epic.id}}"
     limit: 50

3. All Subtasks (for each Story):
   For each story in stories:
     Tool: Search Issues
     Parameters:
       query: "parent:{{story.id}}"
       limit: 50
```

### Step 4: Get Sprint Context (Optional)

If the issue is part of current sprint:
```
Tool: Get Sprint Issues
Parameters:
  teamId: "{{issue.team.id}}"

Or filtered by status:
Tool: Filter Sprint Issues
Parameters:
  teamId: "{{issue.team.id}}"
  status: "In Progress"
```

## Context Assembly Format

### Assembled Context Structure
```markdown
# Context for: {{item.key}} - {{item.title}}

## 📍 Current Location in Hierarchy
{{epic.key}} Epic: {{epic.title}}
└── {{story.key}} Story: {{story.title}}
    └── {{subtask.key}} Subtask: {{subtask.title}} ← YOU ARE HERE

## 🎯 Epic Context
**Key**: {{epic.key}}
**Status**: {{epic.status}}
**Progress**: {{completed_stories}}/{{total_stories}} stories complete

### Epic Goals
{{epic.description}}

### Success Metrics
- {{parse metrics from epic.description}}

### Constraints
- Performance: {{extract from epic}}
- Security: {{extract from epic}}

## 📋 Story Context  
**Key**: {{story.key}}
**Status**: {{story.status}}
**Sprint**: {{story.cycle.name}}
**Points**: {{story.estimate}}
**Assignee**: {{story.assignee.name}}

### User Story
{{parse from story.description}}

### Acceptance Criteria
{{extract numbered AC from story.description}}

### Story Progress
- Subtasks: {{completed}}/{{total}}
- Blockers: {{story.blockingIssues}}

## 🔧 Current Subtask
**Key**: {{subtask.key}}
**Status**: {{subtask.status}}
**Assignee**: {{subtask.assignee.name}}

### Task Details
{{subtask.description}}

### Deliverables
{{extract from subtask.description}}

## 🔗 Related Items

### Sibling Subtasks (Same Story)
{{for each sibling subtask}}
- {{status_emoji}} {{key}}: {{title}}
{{end}}

### Dependencies
- Blocked by: {{list blocking issues}}
- Blocks: {{list blocked issues}}

## 📊 Sprint Status
**Current Sprint**: {{sprint.name}}
**Sprint Progress**: {{completed}}/{{total}} issues
**Your Sprint Items**: {{user's assigned issues in sprint}}

## 💡 Next Actions
Based on current state:
1. {{suggested next action}}
2. {{suggested follow-up}}
```

## Search Query Examples

### Find by Issue Key
```
Tool: Search Issues
Parameters:
  query: "ENG-123"
```

### Find by Title
```
Tool: Search Issues
Parameters:
  query: "implement search endpoint"
```

### Find Parent's Children
```
Tool: Search Issues
Parameters:
  query: "parent:ENG-100"
```

### Find by Multiple Criteria
```
Tool: Search Issues
Parameters:
  query: "search"
  team: "backend"
  status: ["In Progress", "In Review"]
  assignee: "current_user"
```

### Find Related Issues
```
Tool: Search Issues
Parameters:
  query: "label:search-feature status:Todo"
```

## Error Handling

### Issue Not Found
```markdown
❌ Could not find issue matching: {{identifier}}

Suggestions:
- Check the issue key is correct
- Try searching with partial title
- Verify you have access to this issue
```

### Incomplete Hierarchy
```markdown
⚠️ Partial context retrieved

Found:
✅ Story: {{story.key}}
✅ Subtasks: {{count}} items
❌ Epic: Not linked or not accessible

Note: This story may not be part of an Epic, or you may lack permissions.
```

### Permission Denied
```markdown
❌ Cannot access issue: {{issue.key}}

This could mean:
- Issue is in a private project
- Your Linear account lacks permissions
- Linear MCP needs re-authentication
```

## Usage Examples

### Example 1: Fetch Context for Subtask
```
Human: fetch-context ENG-123

Claude: I'll fetch the complete context for ENG-123 using Linear MCP.

[Uses Linear MCP Search Issues tool with query: "ENG-123"]

Found: ENG-123 is a Subtask. Let me gather the full hierarchy context.

[Searches for parent Story and Epic]

# Context for: ENG-123 - Create search API endpoint

## 📍 Current Location in Hierarchy  
ENG-100 Epic: Enhanced Search Capabilities
└── ENG-120 Story: Product Text Search
    └── ENG-123 Subtask: Create search API endpoint ← YOU ARE HERE

## 🎯 Epic Context
**Status**: In Progress (2/5 stories complete)
**Goal**: Reduce search abandonment by 30%

[... returns full formatted context ...]
```

### Example 2: Fetch Context for Story
```
Human: fetch-context "implement product search"

Claude: I'll search for issues matching "implement product search".

[Uses Linear MCP Search Issues tool]

Found: ENG-120 - Implement Product Search with Text Matching (Story)

[Fetches parent Epic and child Subtasks]

# Context for: ENG-120 - Implement Product Search with Text Matching

## 📍 Current Location in Hierarchy
ENG-100 Epic: Enhanced Search Capabilities
└── ENG-120 Story: Implement Product Search ← YOU ARE HERE
    ├── ENG-121 Create search API endpoint (In Progress)
    ├── ENG-122 Add search indexing (Todo)
    └── ENG-123 Write search tests (Done)

[... returns full formatted context ...]
```

### Example 3: Sprint Context
```
Human: Get context for current sprint items

Claude: I'll fetch the current sprint issues for your team.

[Uses Linear MCP Get Sprint Issues tool]

# Current Sprint: Sprint 23 - Search Implementation

## Your Assigned Issues
1. ENG-123 - Create search API endpoint (In Progress)
2. ENG-125 - Fix search performance (Todo)

## Team Sprint Overview
- Total Issues: 12
- Completed: 5
- In Progress: 4
- Todo: 3

[... returns sprint-focused context ...]
```

## Tips for Effective Context Fetching

### 1. Use Specific Queries
- Issue keys are fastest: `ENG-123`
- Partial titles work: `"search endpoint"`
- Labels help narrow: `label:backend`

### 2. Leverage Filters
Combine multiple filters for precision:
```
Tool: Search Issues
Parameters:
  query: "search"
  status: ["In Progress"]
  assignee: "me"
  team: "backend"
```

### 3. Check Sprint Context
For active work, also fetch sprint issues:
```
Tool: Get Sprint Issues
Parameters:
  teamId: "{{team.id}}"
```

### 4. Follow Dependencies
Always check blocking/blocked issues:
```
Tool: Search Issues
Parameters:
  query: "blocks:{{issue.id}}"
```

## Integration with TDD Workflow

### For Coordinator Agent
Use fetch-context at the start of each TDD cycle:
1. Fetch full Epic → Story → Subtask context
2. Identify acceptance criteria from Story
3. Check for blocking dependencies
4. Verify sprint alignment

### For QA Agent
Focus on Story-level context:
- Extract acceptance criteria
- Identify edge cases mentioned
- Check for test-related comments

### For Developer Agent
Focus on Subtask-level context:
- Current subtask requirements
- Technical constraints from Story
- Performance requirements from Epic

### For Architect Agent
Consider full hierarchy:
- Epic-level architectural constraints
- Cross-story impacts
- System-wide NFRs

### For Code Reviewer
Review against full context:
- Verify AC satisfaction
- Check Epic-level constraints
- Ensure sprint goals met

## Troubleshooting

### Linear MCP Not Responding
```
Check:
1. Linear MCP is configured in Claude Code settings
2. Authentication is valid (may need to re-auth)
3. Network connection is stable
```

### Slow Searches
```
Optimize by:
1. Using specific issue keys when known
2. Limiting search scope with filters
3. Fetching only necessary hierarchy levels
```

### Missing Data
```
Possible causes:
1. Permissions - check Linear workspace access
2. Incomplete hierarchy - not all issues have parents
3. Deleted issues - references may be stale
```

## Summary

The fetch-context command leverages Linear MCP to provide comprehensive, hierarchical context for any issue in your Linear workspace. It automatically traverses the Epic → Story → Subtask structure, assembles relevant information, and presents it in a format optimized for the TDD workflow agents.