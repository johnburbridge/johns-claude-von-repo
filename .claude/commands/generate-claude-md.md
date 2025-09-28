# Generate CLAUDE.md Command

## Purpose
Generate a tailored CLAUDE.md file based on discovered project information and documentation.

## Usage
```
generate-claude-md
```

This command should be run after:
1. Project documentation exists (PRD, Architecture)
2. Technology stack has been detected or specified
3. Linear configuration is known (optional)

## Input Requirements

The generator needs access to:
- `docs/prd.md` - Product Requirements Document
- `docs/architecture.md` - Architecture Document
- Detected technology stack information
- Linear configuration (if applicable)

## Generation Process

### Step 1: Extract Information

#### From PRD
```javascript
const prdInfo = {
  projectName: // Extract from PRD title
  projectDescription: // Extract from executive summary
  targetUsers: // Extract from target users section
  coreFeatures: // Extract from features section
  successMetrics: // Extract from KPIs
}
```

#### From Architecture
```javascript
const archInfo = {
  language: // Extract from technology stack
  framework: // Extract from technology stack
  database: // Extract from technology stack
  testFramework: // Extract from testing section
  buildSystem: // Extract from build tools
  deploymentTarget: // Extract from infrastructure
  cicdPlatform: // Extract from CI/CD section
}
```

#### From File System Detection
```javascript
const detected = {
  language: detectLanguage(), // Based on config files
  testFramework: detectTestFramework(),
  buildTools: detectBuildTools(),
  linters: detectLinters(),
  packageManager: detectPackageManager()
}
```

### Step 2: Generate CLAUDE.md Content

```markdown
# CLAUDE.md - {{projectName}} Configuration

Generated on: {{currentDate}}
Generated from: Project documentation and automated detection

## Project Overview

**Name**: {{projectName}}
**Description**: {{projectDescription}}
**Target Users**: {{targetUsers}}

## Technology Stack

Detected and configured technologies:
- **Language**: {{language}}
- **Framework**: {{framework || 'None specified'}}
- **Database**: {{database || 'None specified'}}
- **Testing**: {{testFramework}}
- **Build System**: {{buildSystem}}
- **Package Manager**: {{packageManager}}

## Documentation References

Core project documentation:
- **Product Requirements**: @docs/prd.md
- **Architecture**: @docs/architecture.md
- **Development Guide**: @CONTRIBUTING.md
- **TDD Workflow Guide**: @ABOUT.md

Design documents:
- **Epic Designs**: @docs/design/epics/
- **Story Designs**: @docs/design/stories/
- **Architecture Decisions**: @docs/adr/

## TDD Workflow Configuration

### Specialized Agents
Configure Claude Code to use these specialized agents:
- **Coordinator**: @.claude/agents/coordinator.md
- **QA**: @.claude/agents/qa.md
- **Developer**: @.claude/agents/developer.md
- **Architect**: @.claude/agents/architect.md
- **Code Reviewer**: @.claude/agents/code-reviewer.md

### Workflow Commands
- **Initialize Project**: @.claude/commands/initialize-project.md
- **Generate CLAUDE.md**: @.claude/commands/generate-claude-md.md
- **Create Epic**: @.claude/commands/create-epic.md
- **Create Story**: @.claude/commands/create-story.md
- **Create Subtasks**: @.claude/commands/create-subtasks.md
- **Check Overlap**: @.claude/commands/check-overlap.md
- **Fetch Context**: @.claude/commands/fetch-context.md

{{if linearConfigured}}
## Linear Integration

### Configuration
- **Team**: {{linearTeam}} (`{{linearTeamId}}`)
- **Project**: {{linearProject}} (`{{linearProjectId}}`)

### Issue Hierarchy
- **Epic**: Multi-sprint feature or capability
- **Story**: Single-sprint user-facing functionality
- **Subtask**: Hours-based technical implementation

### Working with Linear
1. Use `fetch-context <issue-id>` to get full hierarchy
2. Check parent and children relationships
3. Update status as work progresses
{{/if}}

## Development Environment

### Commands
Based on your {{language}} project using {{packageManager}}:

#### Development
```bash
# Install dependencies
{{installCommand}}

# Start development server
{{devCommand}}

# Run tests
{{testCommand}}

# Run tests with coverage
{{coverageCommand}}

# Build for production
{{buildCommand}}
```

#### Code Quality
```bash
{{if linter}}
# Lint code
{{lintCommand}}
{{/if}}

{{if typeChecker}}
# Type check
{{typeCheckCommand}}
{{/if}}

{{if formatter}}
# Format code
{{formatCommand}}
{{/if}}
```

### Testing Strategy
Framework: **{{testFramework}}**

Test structure:
```
tests/
├── unit/        # Unit tests
├── integration/ # Integration tests
└── e2e/        # End-to-end tests (if applicable)
```

Coverage requirements:
- Minimum: {{coverageThreshold || '80%'}}
- Critical paths: 100%

## Project Structure

Current project structure:
```
{{projectRoot}}/
├── src/           # Source code
├── tests/         # Test files
├── docs/          # Documentation
│   ├── architecture.md
│   ├── prd.md
│   ├── design/
│   └── adr/
├── .claude/       # Claude configuration
│   ├── agents/
│   └── commands/
{{additionalStructure}}
```

## Definition of Done

### For {{language}} Development

A Story/Subtask is "Done" when:

#### Code Quality
- [ ] All acceptance criteria satisfied
- [ ] Code reviewed and approved
- [ ] Follows {{language}} best practices
- [ ] No linting errors
{{if typeChecker}}- [ ] Type checking passes{{/if}}

#### Testing
- [ ] All tests passing
- [ ] Coverage ≥ {{coverageThreshold}}%
- [ ] Edge cases tested
- [ ] Integration tests passing

#### Documentation
- [ ] Code documented
- [ ] API docs updated (if applicable)
- [ ] README updated (if needed)

#### Security
- [ ] No security vulnerabilities
- [ ] Dependencies up to date
- [ ] Secrets in environment variables

#### Process
- [ ] PR approved
- [ ] Linear issue updated
- [ ] CI/CD pipeline passes

## Security Considerations

Based on architecture document:
{{securityRequirements}}

## Performance Requirements

Based on architecture document:
{{performanceRequirements}}

## Branch Strategy

```
main (or master)
├── develop (if using git-flow)
├── feature/{{issue-key}}-description
├── bugfix/{{issue-key}}-description
└── release/{{version}}
```

## Commit Convention

Format:
```
{{commitFormat || 'type(scope): description'}}

{{commitBody || 'Optional body explaining why'}}

Story: {{linearStoryId}}
```

Types: feat, fix, docs, style, refactor, test, chore

## Quick Reference

### Common Tasks
```bash
# Create feature branch
git checkout -b feature/{{issue-key}}-{{description}}

# Run development
{{devCommand}}

# Run tests
{{testCommand}}

# Check code quality
{{lintCommand}}
{{typeCheckCommand}}

# Build
{{buildCommand}}

# Commit
git commit -m "feat: implement feature X"
```

### TDD Cycle
1. `fetch-context {{story-id}}` - Get requirements
2. Write failing tests (Red)
3. Implement minimal code (Green)
4. Refactor (Refactor)
5. Review and validate
6. Update Linear status

## Environment Variables

Required variables (from architecture):
```bash
{{envVariables}}
```

Development setup:
```bash
cp .env.example .env
# Edit .env with your values
```

## Getting Help

- **TDD Workflow**: See @ABOUT.md
- **Architecture Details**: See @docs/architecture.md
- **Product Requirements**: See @docs/prd.md
- **Development Guide**: See @CONTRIBUTING.md

## Notes for AI Assistants

When working on this project:

1. **Always start with context**: Use `fetch-context` to understand the task
2. **Follow TDD**: Write tests first, then implement
3. **Use appropriate agent**: Each phase has a specialized agent
4. **Check Definition of Done**: Before marking complete
5. **Update documentation**: As you make changes
6. **Commit frequently**: Small, focused commits

Technology-specific considerations for {{language}}:
{{languageSpecificNotes}}

---

*This file was auto-generated from project documentation.*
*Regenerate with: `generate-claude-md`*
*Last generated: {{currentDate}}*
```

## Generation Rules

### Language-Specific Defaults

#### JavaScript/TypeScript
```javascript
{
  installCommand: "npm install",
  devCommand: "npm run dev",
  testCommand: "npm test",
  coverageCommand: "npm run test:coverage",
  buildCommand: "npm run build",
  lintCommand: "npm run lint",
  typeCheckCommand: "npm run typecheck",
  formatCommand: "npm run format"
}
```

#### Python
```javascript
{
  installCommand: "pip install -r requirements.txt",
  devCommand: "python manage.py runserver", // or "python app.py"
  testCommand: "pytest",
  coverageCommand: "pytest --cov",
  buildCommand: "python -m build",
  lintCommand: "flake8",
  typeCheckCommand: "mypy",
  formatCommand: "black ."
}
```

#### Go
```javascript
{
  installCommand: "go mod download",
  devCommand: "go run .",
  testCommand: "go test ./...",
  coverageCommand: "go test -cover ./...",
  buildCommand: "go build",
  lintCommand: "golangci-lint run",
  typeCheckCommand: "go vet ./...",
  formatCommand: "go fmt ./..."
}
```

#### Java
```javascript
{
  installCommand: "mvn install", // or "gradle build"
  devCommand: "mvn spring-boot:run",
  testCommand: "mvn test",
  coverageCommand: "mvn test jacoco:report",
  buildCommand: "mvn package",
  lintCommand: "mvn checkstyle:check",
  formatCommand: "mvn formatter:format"
}
```

### Smart Defaults

If certain information is not available:
- Use sensible defaults based on language
- Add placeholders with clear instructions
- Include comments about what needs configuration

### Validation

Before generating, validate:
1. Required documents exist (PRD, Architecture)
2. Language is detected or specified
3. File structure makes sense
4. No conflicting information

### Error Handling

If generation fails:
```markdown
❌ Cannot generate CLAUDE.md

Missing required information:
- [ ] Product Requirements Document (docs/prd.md)
- [ ] Architecture Document (docs/architecture.md)
- [ ] Detected language/framework

Please run `initialize-project` first to create missing documentation.
```

## Example Usage

```
Human: generate-claude-md

Claude: I'll generate a customized CLAUDE.md based on your project documentation and detected configuration.

📊 Analyzing project...
- ✅ Found PRD: "E-Commerce Search Platform"
- ✅ Found Architecture: Microservices, Node.js, PostgreSQL
- ✅ Detected: TypeScript, Jest, npm
- ✅ Linear configured: Team "Backend", Project "Search"

📝 Generating CLAUDE.md...

✅ CLAUDE.md generated successfully!

Key configurations:
- Language: TypeScript
- Framework: Express.js
- Testing: Jest
- Database: PostgreSQL
- CI/CD: GitHub Actions

The file includes:
- Project-specific commands
- Actual test/build/lint commands
- Your Definition of Done
- Linear integration details
- TDD workflow configuration

You can regenerate this file anytime by running `generate-claude-md`.
```

## Integration with Initialize-Project

This generator is called automatically by `initialize-project` after:
1. Documentation is created/found
2. Technology stack is detected
3. Linear is configured (optional)

It can also be run standalone to regenerate CLAUDE.md when:
- Documentation is updated
- Technology stack changes
- New tools are added
- Linear configuration changes

## Additional Files Generated

After generating CLAUDE.md, also create:

### .gitignore
Based on the detected/specified technology stack:
```bash
# Generate .gitignore with patterns for:
- Detected language (from architecture doc)
- Detected framework
- Detected build tools
- Common patterns (OS, IDE, .env files)
```

The .gitignore should be comprehensive and include:
- Language-specific build artifacts
- Dependency directories
- IDE and editor files
- Environment and secret files
- OS-specific files
- Test coverage reports
- Log files