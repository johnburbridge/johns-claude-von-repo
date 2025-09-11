# Create Story Command

## Purpose
Generate a Linear Story that references its Epic efficiently without duplication.

## Usage
```
create-story <epic_id> <epic_title> <story_description>
```

## Prompt Template

Draft a Linear Story for Epic {{epic_id}} ("{{epic_title}}").

Based on: {{story_description}}

Output the following fields:

### Title
Single-sprint, testable outcome (5-10 words)

### User Story
As a [persona]
I want [capability]
So that [benefit/value]

### Rationale
Brief explanation (2-3 sentences) of:
- Why this story exists under the Epic
- How it contributes to Epic goals
- Its priority/sequence reasoning

### Acceptance Criteria
Numbered list of verifiable criteria that will become tests:
1. **Given** [context] **When** [action] **Then** [outcome]
2. **Given** [context] **When** [action] **Then** [outcome]
3. [Additional criteria as needed]
4. [Edge case handling]
5. [Error scenarios]

Each criterion should be:
- Testable via automation
- Independent of other criteria
- Clear pass/fail determination
- User or system observable

### Technical Scope

#### Interfaces/Contracts
- API endpoints affected
- Data models touched
- Events published/consumed
- External service calls

#### Data Considerations
- Schema changes needed
- Migration requirements
- Data validation rules
- Volume/performance impacts

#### Edge Cases
- Boundary conditions
- Concurrent operations
- Network failures
- Invalid inputs
- Resource limits

### Dependencies
- Parent Epic: {{epic_id}}
- Blocked by: [Story IDs if any]
- Blocks: [Story IDs if any]
- External dependencies: [Services, APIs]

### Validation Signals
What QA/CI should verify:
- Unit test coverage > X%
- Integration tests for [components]
- Performance benchmarks
- Security scans clean
- Accessibility checks pass

### Out of Scope
Explicitly not part of this story:
- [Feature/functionality]
- [Related but separate work]
- [Future enhancements]

### Definition of Done
Story-level completion checklist:
- [ ] All AC have passing tests
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] No critical bugs
- [ ] Performance targets met
- [ ] Security review passed
- [ ] Feature flag configured
- [ ] Monitoring added

### Subtask Breakdown Hints
Suggested subtask categories (not detailed steps):
- Test creation tasks
- Implementation tasks
- Integration tasks
- Documentation tasks
- Deployment tasks

### Links
- Epic: {{epic_id}}
- Technical Design: `@docs/design/story-{{story_key}}.md`
- Related Stories: [IDs]
- Relevant Docs: [Links]

## Rules for Story Creation

1. **Reference the Epic, don't repeat it**:
   - Link to Epic for context
   - Don't copy Epic's description
   - Focus on this story's unique aspects

2. **Keep scope achievable in one sprint**:
   - Single PR worth of work
   - Clear boundaries
   - Independent deployment possible

3. **Leave execution details to Subtasks**:
   - Don't specify implementation
   - Focus on outcomes
   - Let subtasks define "how"

4. **Acceptance Criteria best practices**:
   - Write from user perspective
   - Avoid technical implementation details
   - Each should map to specific tests
   - Include negative cases

## Example Output

### Title
Implement Product Search with Text Matching

### User Story
As a customer
I want to search for products by name and description
So that I can quickly find items I'm looking for

### Rationale
This story implements the core text search functionality for the Enhanced Search Epic. It's the foundational capability that other search features (filters, facets) will build upon. Prioritized first to establish the search infrastructure.

### Acceptance Criteria
1. **Given** a search query **When** user submits search **Then** results containing the query terms are returned
2. **Given** partial product name **When** searching **Then** products with matching partial names appear
3. **Given** search with no matches **When** results display **Then** "No results found" message appears with suggestions
4. **Given** special characters in query **When** searching **Then** characters are properly escaped and search executes safely
5. **Given** search query > 100 chars **When** submitted **Then** query is truncated with warning message

### Technical Scope

#### Interfaces/Contracts
- POST /api/v1/search endpoint
- SearchRequest/SearchResponse DTOs
- ProductSearchService interface
- SearchIndexer for data sync

#### Data Considerations
- Elasticsearch index for products
- Denormalized product data
- Index refresh strategy
- 100GB estimated index size

[Continue with remaining sections...]