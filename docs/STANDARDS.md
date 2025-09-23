# Organizational Standards & Opinions

This framework embodies strong opinions about how software development should be organized. These standards ensure consistency, quality, and maintainability across projects.

## 🗂️ Documentation Organization Standards

**Hierarchical Information Architecture**
- **Durable Docs**: High-level, stable information in repo (`README.md`, `docs/architecture.md`, `docs/prd.md`, `CONTRIBUTING.md`)
- **Task-Oriented Docs**: Specific work details in Linear (Epic → Story → Subtask)
- **Progressive Context**: Information structured from coarse to fine granularity
- **Anti-Duplication**: Child items reference parents rather than repeating content

**Documentation Philosophy**
- **Requirements-Driven**: PRD and Architecture docs are foundational
- **Design-First**: Technical designs precede implementation
- **Decision Recording**: ADRs for all architectural choices
- **Template-Based**: Standardized formats via interview-driven generation
- **Living Documentation**: Updated throughout development, not created once

## 🤖 Agent Specialization Standards

**Role Segregation**
- **Coordinator**: Orchestrates but never writes code
- **QA**: Writes tests from acceptance criteria only
- **Developer**: Implements minimal code to pass tests only
- **Architect**: Refactors while maintaining green tests only
- **Code Reviewer**: Reviews against standards and AC only
- **Context Engineer**: Curates relevant documentation progressively

**Phase Gate Enforcement**
- Red → Green → Refactor → Review → Validate → Merge
- Each phase has specific artifacts and success criteria
- No phase skipping allowed
- Automatic rollback on gate failures

## 🎯 Issue Tracking Organization

**Hierarchical Work Breakdown**
```
Epic (Multi-sprint, broad outcome)
└── Story (Single-sprint, testable deliverable)
    └── Subtask (Hours-based, atomic implementation)
```

**Context Management Standards**
- **Epic**: Outcome, constraints, success metrics, dependencies
- **Story**: User story, AC, interfaces, edge cases, validation signals
- **Subtask**: Deliverables, file paths, test names, acceptance checks
- **Anti-Overlap**: Each level adds specificity without duplication

**Linear Integration Requirements**
- All work must be tracked in Linear
- Status updates at each phase transition
- Team and project IDs configured in CLAUDE.md
- AC-to-test mapping maintained

## 🏗️ Code Organization Standards

**Language-Agnostic Structure**
```
src/
├── [feature]/
│   ├── [feature].service.[ext]    # Business logic
│   ├── [feature].controller.[ext] # HTTP handlers
│   ├── [feature].model.[ext]      # Data models
│   ├── [feature].test.[ext]       # Tests
│   └── index.[ext]                # Module exports
```

**Technology Adaptation**
- Automatic detection of language, framework, test runner
- Configuration generation based on detected stack
- Language-specific best practices enforcement
- Framework-specific command integration

## 🧪 Testing Organization Standards

**TDD Cycle Enforcement**
- **Red Phase**: Failing tests from AC (QA agent only)
- **Green Phase**: Minimal implementation (Developer agent only)
- **Refactor Phase**: Design improvement (Architect agent only)

**Test Quality Requirements**
- **Coverage**: 80% minimum, 100% critical paths
- **Traceability**: Each test maps to specific AC
- **Structure**: Arrange-Act-Assert or Given-When-Then
- **Independence**: No test interdependencies
- **Naming**: Descriptive scenario-based names

**Test Organization**
```
tests/
├── unit/           # Fast, isolated tests
├── integration/    # Component interaction tests
├── e2e/           # End-to-end tests
└── fixtures/      # Test data and mocks
```

## 🔄 Git & Version Control Standards

**Branch Strategy**
```
main
├── feature/[issue-key]-[description]
├── bugfix/[issue-key]-[description]
├── hotfix/[issue-key]-[description]
└── release/[version]
```

**Commit Standards**
```
type(scope): subject

body (optional)

footer (optional)
```

**PR Requirements**
- One Story/Subtask per PR maximum
- All CI checks must pass
- Required approvals obtained
- Linear issue linked and updated
- Definition of Done satisfied

## 🎯 Development Standards

**Definition of Done (Language-Specific)**
- All tests passing
- Code reviewed and approved
- No security vulnerabilities
- Documentation updated
- Performance benchmarks met
- Architecture compliance verified

**Code Quality Gates**
- Lint and type checking pass
- Coverage thresholds met
- Security scans pass
- Performance within SLAs
- Accessibility standards met

## 🗣️ Communication Standards

**Agent-to-Agent Communication**
- **Normal Flow**: Silent handoffs via committed artifacts
- **Exceptions**: Comments posted to Linear with structured format
- **Escalation**: Coordinator notified of blockers
- **Audit Trail**: All decisions documented

**Linear Comment Format**
```
[AGENT] Discovery/Decision:
- Type: [Technical Debt|Security|Performance|Architecture]
- Summary: [Brief description]
- Details: [Relevant information]
- Impact: [Who needs to know]
```

**Status Communication**
- Phase transitions logged in Linear
- Blockers reported immediately
- Success criteria met before handoff
- Human approval required for architecture changes

## 🔧 Architecture Standards

**Change Control**
- ADRs required for cross-cutting changes
- Human approval for architectural decisions
- Backward compatibility maintained
- Migration strategies documented

**Design Principles**
- **SOLID**: Single responsibility, Open-closed, etc.
- **DRY**: Don't Repeat Yourself (but avoid premature abstraction)
- **YAGNI**: You Aren't Gonna Need It
- **KISS**: Keep It Simple, Stupid

## 📊 Quality Assurance Standards

**Edge Case Requirements**
- Null/empty value handling
- Boundary conditions
- Error scenarios
- Race conditions
- Performance limits
- Security edge cases

**Review Criteria**
- Correctness against AC
- Security vulnerability assessment
- Performance impact analysis
- Accessibility compliance
- Maintainability score
- Technical debt identification

## 🔄 End-of-Cycle Standards

**Hygiene Requirements**
- Technical debt logged as Linear issues
- Documentation audit completed
- Architecture diagrams updated
- ADR follow-ups addressed
- Knowledge transfer documented

**Release Standards**
- Semantic versioning enforced
- Release notes comprehensive
- Rollback procedures tested
- Monitoring alerts configured
- Stakeholder communication complete

---

These standards ensure that every project maintains high quality, consistency, and maintainability while leveraging AI assistance effectively. The framework adapts to your technology stack while maintaining these core organizational principles.