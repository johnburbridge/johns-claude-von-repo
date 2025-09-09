# Create Epic Command

## Purpose
Generate a well-structured Linear Epic with minimal overlap and clear scope boundaries.

## Usage
```
create-epic <project_name> <epic_description>
```

## Prompt Template

You are drafting a Linear Epic for {{project_name}}.

Based on: {{epic_description}}

Output the following fields:

### Title
Concise, outcome-oriented title (5-10 words)

### Summary
Brief description (≤5 sentences) explaining:
- What we're building
- Why it matters
- Who benefits
- Expected timeline

### Outcomes & Success Metrics
List 3-5 measurable outcomes:
- Specific metrics with targets
- User impact measurements
- Business value indicators
- Technical improvements

### Scope Definition

#### In Scope
- Core functionality to be delivered
- User journeys covered
- Systems/services affected
- Key integrations

#### Out of Scope
- Explicitly excluded features
- Deferred enhancements
- Separate epic dependencies
- Future iterations

### Non-Functional Requirements
- **Performance**: Response times, throughput, scalability targets
- **Security**: Authentication, authorization, data protection needs  
- **Reliability**: Uptime SLOs, error budgets, recovery objectives
- **Observability**: Metrics, logs, traces, alerts required
- **Compliance**: Regulatory, legal, policy requirements

### Dependencies & Risks

#### Dependencies
- External services or APIs
- Other teams' deliverables
- Infrastructure changes
- Data migrations

#### Risks
- Technical uncertainties
- Resource constraints
- Timeline challenges
- Integration complexities

### Definition of Done
Epic-level completion criteria:
- [ ] All child stories completed
- [ ] Integration testing passed
- [ ] Performance benchmarks met
- [ ] Security review completed
- [ ] Documentation updated
- [ ] Monitoring in place
- [ ] Feature flags configured
- [ ] Rollback plan tested

### Proposed Stories
List 3-6 high-level stories (one line each, no implementation details):
- Story 1: [User-facing outcome]
- Story 2: [User-facing outcome]
- Story 3: [User-facing outcome]
- Story 4: [Technical enabler if needed]
- Story 5: [Technical enabler if needed]

### Links to Documentation
- Architecture: `@docs/architecture.md`
- PRD: `@docs/prd.md`
- Technical Design: `@docs/design/epic-{{epic_key}}.md`
- Related ADRs: [List relevant ADR numbers]

## Rules for Epic Creation

1. **Do NOT include**:
   - Implementation steps or technical details
   - Team assignments or individual ownership
   - Specific technical solutions
   - Story-level acceptance criteria

2. **Keep context that children can inherit**:
   - High-level constraints
   - Shared assumptions
   - Cross-cutting concerns
   - Global requirements

3. **Prefer references over repetition**:
   - Link to existing docs
   - Reference other epics by ID
   - Use consistent terminology

## Example Output

### Title
Enhanced Search Capabilities for Product Catalog

### Summary
Implementing advanced search functionality to help users find products faster and more accurately. This includes full-text search, filters, faceted navigation, and search suggestions. The enhancement aims to reduce search abandonment by 30% and improve conversion rates. Expected delivery in Q2.

### Outcomes & Success Metrics
- Search result relevance score > 85%
- Average search completion time < 2 seconds
- Search abandonment rate reduced by 30%
- Conversion from search improved by 15%
- Zero-result searches reduced by 50%

### Scope Definition

#### In Scope
- Full-text product search
- Multi-faceted filtering
- Search suggestions/autocomplete
- Search results ranking
- Search analytics dashboard

#### Out of Scope
- Personalized recommendations
- Visual/image search
- Voice search
- International language support
- Third-party marketplace search

[Continue with remaining sections...]