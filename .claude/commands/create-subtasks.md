# Create Subtasks Command

## Purpose
Break down a Story into atomic, executable subtasks that can be completed in hours.

## Usage
```
create-subtasks <story_id> <story_title>
```

## Prompt Template

Create atomic Subtasks for Story {{story_id}} ("{{story_title}}") that a single engineer can finish in hours.

For each Subtask, output:

### Subtask Format

#### Title
[Verb-led, concrete action] (5-10 words)

#### Description
Specific implementation details including:
- Exact files/components to modify
- Methods/functions to create
- Configuration changes needed
- Data structures to implement

#### Deliverables
Concrete artifacts produced:
- [ ] Files created/modified
- [ ] Tests written
- [ ] Documentation updated
- [ ] Configuration changed
- [ ] Database migrations

#### Acceptance Checks
Objective verification criteria:
- [ ] Unit tests pass for [component]
- [ ] Integration test covers [scenario]
- [ ] Linting/formatting clean
- [ ] Type checking passes
- [ ] Manual test steps verified

#### Estimated Hours
[1-8 hours max]

#### Dependencies
- Depends on: [Subtask IDs]
- Blocks: [Subtask IDs]
- Parent Story: {{story_id}}

## Subtask Categories

### 1. Test Creation Subtasks
```
Title: Write failing tests for search API endpoint

Description:
- Create test file: tests/api/test_search.py
- Write tests for SearchRequest validation
- Write tests for SearchResponse format
- Add fixtures for test product data
- Include edge case tests (empty query, special chars)

Deliverables:
- [ ] tests/api/test_search.py created
- [ ] tests/fixtures/search_data.json created
- [ ] 10+ test cases covering all AC

Acceptance Checks:
- [ ] Tests run and fail (red state)
- [ ] Each AC has corresponding test
- [ ] Test names clearly describe scenarios
- [ ] No implementation code included

Estimated Hours: 3-4 hours
```

### 2. Implementation Subtasks
```
Title: Implement search endpoint request handling

Description:
- Create SearchController in src/controllers/search.py
- Add POST /api/v1/search route
- Implement request validation using SearchSchema
- Add error handling for invalid queries
- Configure rate limiting

Deliverables:
- [ ] src/controllers/search.py created
- [ ] src/schemas/search.py created
- [ ] Route registered in src/routes.py
- [ ] Rate limit config in src/config.py

Acceptance Checks:
- [ ] Endpoint responds to POST requests
- [ ] Invalid requests return 400
- [ ] Rate limiting enforced
- [ ] Request logging working

Estimated Hours: 4-5 hours
```

### 3. Integration Subtasks
```
Title: Connect search service to Elasticsearch

Description:
- Configure Elasticsearch client in src/services/elastic.py
- Create ProductIndexer class
- Implement index creation with mappings
- Add bulk indexing method
- Create search query builder

Deliverables:
- [ ] src/services/elastic.py created
- [ ] config/elasticsearch.yml created
- [ ] Index mappings defined
- [ ] Connection pool configured

Acceptance Checks:
- [ ] Connection to ES established
- [ ] Index created successfully
- [ ] Test data indexed
- [ ] Search queries return results

Estimated Hours: 5-6 hours
```

### 4. Database/Migration Subtasks
```
Title: Create database migration for search metadata

Description:
- Create migration: 001_add_search_metadata.sql
- Add search_logs table
- Add indexed_at column to products
- Create indexes for performance
- Add rollback script

Deliverables:
- [ ] migrations/001_add_search_metadata.sql
- [ ] migrations/rollback/001_rollback.sql
- [ ] Migration tested locally
- [ ] Performance indexes created

Acceptance Checks:
- [ ] Migration runs without errors
- [ ] Rollback tested successfully
- [ ] No data loss on migration
- [ ] Indexes improve query performance

Estimated Hours: 2-3 hours
```

### 5. Documentation Subtasks
```
Title: Document search API in OpenAPI spec

Description:
- Update docs/api/openapi.yaml
- Add SearchRequest schema
- Add SearchResponse schema
- Document error responses
- Add example requests/responses

Deliverables:
- [ ] OpenAPI spec updated
- [ ] Request/response examples added
- [ ] Error codes documented
- [ ] Generated docs reviewed

Acceptance Checks:
- [ ] Spec validates without errors
- [ ] Examples match implementation
- [ ] All parameters documented
- [ ] Swagger UI renders correctly

Estimated Hours: 2 hours
```

### 6. Configuration Subtasks
```
Title: Configure search feature flags and monitoring

Description:
- Add feature flag: enable_product_search
- Configure Datadog metrics
- Set up search latency alerts
- Add search analytics events
- Configure cache TTLs

Deliverables:
- [ ] Feature flag in config service
- [ ] Metrics dashboard created
- [ ] Alert rules configured
- [ ] Analytics events firing

Acceptance Checks:
- [ ] Feature flag toggles work
- [ ] Metrics visible in dashboard
- [ ] Alerts trigger on threshold
- [ ] Analytics data collected

Estimated Hours: 3 hours
```

## Rules for Subtask Creation

### DO:
- Keep each subtask under 8 hours
- Make subtasks independently testable
- Include specific file paths
- Define clear completion criteria
- Order subtasks logically
- Group related work together

### DON'T:
- Repeat Story context
- Create multi-day subtasks
- Mix unrelated changes
- Leave deliverables vague
- Include research in implementation
- Combine testing and coding

## Subtask Ordering Guidelines

1. **Test-First Approach**:
   - Test creation subtasks
   - Implementation subtasks
   - Integration subtasks
   - Documentation subtasks

2. **Infrastructure-First Approach**:
   - Database/migration subtasks
   - Configuration subtasks
   - Implementation subtasks
   - Test creation subtasks

3. **Risk-First Approach**:
   - Highest risk/unknown subtasks
   - Integration points
   - Core functionality
   - Polish/documentation

## Example Full Breakdown

For Story: "Implement Product Search with Text Matching"

### Subtasks Created:
1. Write failing tests for search API endpoint (3h)
2. Create search request/response schemas (2h)
3. Implement search endpoint request handling (4h)
4. Connect search service to Elasticsearch (5h)
5. Create product indexing pipeline (4h)
6. Add search query builder with text matching (3h)
7. Implement search results ranking (3h)
8. Add search caching layer (2h)
9. Create database migration for search metadata (2h)
10. Document search API in OpenAPI spec (2h)
11. Configure search feature flags and monitoring (3h)
12. Add search performance tests (2h)

Total: ~35 hours (1 sprint for 1 developer)