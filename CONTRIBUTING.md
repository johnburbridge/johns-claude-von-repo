# Contributing Guide

> **Note**: This is a template Contributing Guide. Run `initialize-project` to generate a customized version based on your project's technology stack.

## Table of Contents
1. [Getting Started](#getting-started)
2. [Development Setup](#development-setup)
3. [Development Workflow](#development-workflow)
4. [Coding Standards](#coding-standards)
5. [Testing Guidelines](#testing-guidelines)
6. [Documentation Standards](#documentation-standards)
7. [Pull Request Process](#pull-request-process)
8. [Release Process](#release-process)

## Getting Started

### Prerequisites
Before contributing to this project, ensure you have:
- [ ] [Required tool 1] installed
- [ ] [Required tool 2] installed
- [ ] Access to required services (Linear, GitHub, etc.)
- [ ] Reviewed the [Architecture Document](docs/architecture.md)
- [ ] Reviewed the [Product Requirements](docs/prd.md)

### First Time Setup
```bash
# Clone the repository
git clone [repository-url]
cd [project-name]

# Install dependencies
[package-manager install command]

# Copy environment template
cp .env.example .env

# Configure environment variables
# Edit .env with your values

# Run initial setup
[setup command if applicable]

# Verify setup
[test command]
```

## Development Setup

### Environment Setup

#### Required Tools
| Tool | Version | Purpose | Installation |
|------|---------|---------|--------------|
| [Language] | [version] | Primary language | [install command] |
| [Package Manager] | [version] | Dependency management | [install command] |
| [Database] | [version] | Data storage | [install command] |
| [Other tools] | [version] | [Purpose] | [install command] |

#### Environment Variables
```bash
# Required environment variables
DATABASE_URL=        # Database connection string
API_KEY=            # External API key
ENVIRONMENT=        # development|staging|production

# Optional environment variables
DEBUG=              # Enable debug logging
LOG_LEVEL=          # Log verbosity
```

### IDE Setup

#### Recommended Extensions
- **[Extension 1]**: [Purpose]
- **[Extension 2]**: [Purpose]
- **[Extension 3]**: [Purpose]

#### Configuration Files
The project includes configuration for:
- `.editorconfig` - Editor settings
- `.prettierrc` - Code formatting (if applicable)
- `.eslintrc` - Linting rules (if applicable)
- `[language-specific configs]`

## Development Workflow

### TDD (Test-Driven Development)

This project follows the TDD workflow. See [ABOUT.md](ABOUT.md) for details.

1. **Red Phase**: Write failing tests first
2. **Green Phase**: Implement minimal code to pass tests
3. **Refactor Phase**: Improve code while keeping tests green
4. **Review Phase**: Code review and validation

### Branch Strategy

```
main (or master)
├── develop (optional)
├── feature/[issue-key]-[description]
├── bugfix/[issue-key]-[description]
├── hotfix/[issue-key]-[description]
└── release/[version]
```

#### Branch Naming
- **Feature**: `feature/ENG-123-add-search`
- **Bugfix**: `bugfix/ENG-456-fix-login`
- **Hotfix**: `hotfix/ENG-789-critical-fix`
- **Release**: `release/v1.2.0`

### Commit Messages

#### Format
```
type(scope): subject

body (optional)

footer (optional)
```

#### Types
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation only
- **style**: Formatting, missing semicolons, etc.
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **test**: Adding missing tests
- **chore**: Changes to build process or auxiliary tools

#### Examples
```bash
# Good commit messages
git commit -m "feat(search): add fuzzy search capability"
git commit -m "fix(auth): resolve token expiration issue"
git commit -m "docs(api): update endpoint documentation"

# With body and footer
git commit -m "feat(search): add fuzzy search capability

Implements Levenshtein distance algorithm for fuzzy matching.
This allows users to find results even with typos.

Story: ENG-123"
```

### Linear Integration

All work should be tracked in Linear:

1. **Before starting work**: 
   - Ensure Linear issue exists
   - Issue is assigned to you
   - Status is "In Progress"

2. **During development**:
   - Reference issue in branch name
   - Reference issue in commit messages
   - Update issue status as needed

3. **After completion**:
   - Link PR to Linear issue
   - Update issue status to "In Review"
   - Move to "Done" after merge

## Coding Standards

### General Principles
- **Readability**: Code should be self-documenting
- **Simplicity**: Prefer simple solutions over clever ones
- **Consistency**: Follow existing patterns in the codebase
- **SOLID Principles**: Single responsibility, Open-closed, etc.
- **DRY**: Don't Repeat Yourself (but don't over-abstract)

### Coding Standards (Language-Agnostic)

#### Universal Principles
```
// Use meaningful variable names
userEmail = "user@example.com"  // Good
e = "user@example.com"          // Bad

// Use descriptive function names
FUNCTION calculateTotalPrice(items)  // Good
FUNCTION calc(i)                    // Bad

// Document complex logic
FUNCTION processPayment(amount, method):
  """Process payment with specified method.

  Parameters:
    amount: Payment amount to process
    method: Payment method to use

  Returns:
    Payment confirmation or error
  """
  // Implementation
END_FUNCTION
```

#### Code Organization Patterns
```
// Service/Business Logic Pattern
CLASS UserService:
  PRIVATE repository

  METHOD getUser(userId):
    IF userId is empty:
      THROW InvalidArgumentError
    END_IF
    RETURN repository.findById(userId)
  END_METHOD
END_CLASS

// Clear separation of concerns
MODULE feature:
  - service layer (business logic)
  - controller layer (request handling)
  - model layer (data structures)
  - test layer (verification)
END_MODULE
```

**Note**: The framework adapts these patterns to your specific language's conventions and best practices during initialization.

### File Organization

```
src/
├── [feature]/
│   ├── [feature].service.[ext]    # Business logic
│   ├── [feature].controller.[ext] # HTTP handlers
│   ├── [feature].model.[ext]      # Data models
│   ├── [feature].test.[ext]       # Tests
│   └── index.[ext]                # Module exports
```

### Error Handling

#### Principles
- Handle errors explicitly
- Provide meaningful error messages
- Log errors appropriately
- Don't suppress errors silently

#### Examples (Pseudocode)
```
// Error handling pattern
TRY:
  result = riskyOperation()
  RETURN result
CATCH SpecificError as error:
  LOG_ERROR("Operation failed", error, context)
  THROW ApplicationError("Operation failed", error)
END_TRY

// Alternative pattern with multiple error types
TRY:
  result = performOperation()
CATCH ValidationError:
  HANDLE_VALIDATION_ERROR()
CATCH NetworkError:
  RETRY_OR_FAIL()
CATCH GENERIC_ERROR:
  LOG_AND_THROW()
END_TRY
```

## Testing Guidelines

### Test Structure

```
tests/
├── unit/           # Fast, isolated tests
├── integration/    # Component interaction tests
├── e2e/           # End-to-end tests
└── fixtures/      # Test data and mocks
```

### Writing Tests

#### Test Naming
```
// Descriptive test names (pseudocode)
TEST_SUITE: "UserService"
  TEST_GROUP: "createUser"
    TEST: "should create a user with valid data"
    TEST: "should throw error when email is invalid"
    TEST: "should prevent duplicate emails"
  END_GROUP
END_SUITE
```

#### Test Coverage
- **Minimum Coverage**: 80%
- **Critical Paths**: 100%
- **New Code**: Must include tests

#### Test Principles
- **Arrange, Act, Assert**: Clear test structure
- **One Assertion Per Test**: Keep tests focused
- **Test Behavior, Not Implementation**: Focus on outcomes
- **Use Test Doubles Sparingly**: Prefer real implementations

### Running Tests

```bash
# Run all tests
[test command]

# Run unit tests only
[unit test command]

# Run with coverage
[coverage command]

# Run specific test file
[specific test command]

# Watch mode (if available)
[watch command]
```

## Documentation Standards

### Code Documentation

#### Functions/Methods
```
/**
 * Calculate the total price including tax
 *
 * Parameters:
 *   subtotal - Price before tax
 *   taxRate - Tax rate as decimal (0.1 for 10%)
 *
 * Returns:
 *   Total price including tax
 *
 * Throws:
 *   Error if subtotal is negative
 */
FUNCTION calculateTotal(subtotal, taxRate):
  IF subtotal < 0:
    THROW Error("Subtotal cannot be negative")
  END_IF
  RETURN subtotal * (1 + taxRate)
END_FUNCTION
```

#### Classes/Modules
```
/**
 * User management service.
 *
 * This module handles all user-related operations including
 * creation, authentication, and profile management.
 */
CLASS UserService:
  """Service for managing user operations."""

  METHOD createUser(email, password) -> User:
    """
    Create a new user account.

    Parameters:
      email: User's email address
      password: Plain text password (will be hashed)

    Returns:
      Created user object

    Throws:
      DuplicateEmailError: If email already exists
      ValidationError: If email or password is invalid
    """
    // Implementation
  END_METHOD
END_CLASS
```

### API Documentation

All APIs should be documented using:
- **REST**: OpenAPI/Swagger specification
- **GraphQL**: Schema with descriptions
- **gRPC**: Proto file comments

### Architecture Decision Records (ADRs)

Significant architectural decisions should be documented:
```markdown
# ADR-001: Use PostgreSQL for primary database

## Status
Accepted

## Context
We need a reliable, scalable database for our application.

## Decision
We will use PostgreSQL as our primary database.

## Consequences
- Positive: Strong consistency, mature ecosystem
- Negative: Requires more operational expertise than NoSQL
```

## Pull Request Process

### Before Creating a PR

- [ ] All tests pass locally
- [ ] Code follows style guidelines
- [ ] Documentation is updated
- [ ] Linear issue is referenced
- [ ] Self-review completed

### PR Template

```markdown
## Summary
Brief description of changes

## Linear Issue
[Link to Linear issue]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Refactoring
- [ ] Documentation

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No new warnings
```

### Review Process

#### For Authors
1. Create PR with descriptive title
2. Fill out PR template
3. Request review from appropriate team members
4. Address feedback promptly
5. Keep PR up to date with base branch

#### For Reviewers
1. Review within 24 hours
2. Check against acceptance criteria
3. Verify tests are adequate
4. Ensure code quality standards
5. Provide constructive feedback

### Merge Requirements

- [ ] All CI checks pass
- [ ] Required approvals received
- [ ] No unresolved comments
- [ ] Branch is up to date
- [ ] Linear issue updated

## Release Process

### Versioning

We follow [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backwards compatible)
- **PATCH**: Bug fixes

### Release Checklist

- [ ] All tests pass
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Version bumped
- [ ] Release notes prepared
- [ ] Stakeholders notified

### Deployment

```bash
# Tag the release
git tag -a v1.2.3 -m "Release version 1.2.3"
git push origin v1.2.3

# Deploy to staging
[deploy staging command]

# Run smoke tests
[smoke test command]

# Deploy to production
[deploy production command]

# Verify deployment
[verification command]
```

## Getting Help

### Resources
- **Documentation**: [Link to docs]
- **Architecture**: [docs/architecture.md](docs/architecture.md)
- **Product Requirements**: [docs/prd.md](docs/prd.md)
- **TDD Workflow**: [ABOUT.md](ABOUT.md)

### Communication Channels
- **Slack**: #[channel-name]
- **Email**: [team-email]
- **Linear**: [Project link]

### Reporting Issues
1. Check existing issues in Linear
2. Create new issue with:
   - Clear title
   - Reproduction steps
   - Expected vs actual behavior
   - Environment details

## Code of Conduct

### Our Standards
- Be respectful and inclusive
- Accept constructive criticism
- Focus on what's best for the project
- Show empathy towards others

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or insulting comments
- Publishing private information
- Other unprofessional conduct

---

*Thank you for contributing to this project! Your efforts help make it better for everyone.*