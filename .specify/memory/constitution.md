<!--
═══════════════════════════════════════════════════════════════════════════
CONSTITUTION SYNC IMPACT REPORT
───────────────────────────────────────────────────────────────────────────
Version: INITIAL → 1.0.0 (MINOR - Initial constitution ratification)

Modified Principles: N/A (Initial version)
Added Sections:
  - Core Principles (5 principles defined)
  - Security Requirements
  - Development Workflow
  - Governance

Removed Sections: N/A

Templates Status:
  ✅ spec-template.md - Verified: Constitution Check gates align with principles
  ✅ plan-template.md - Verified: Constitution Check section compatible
  ✅ tasks-template.md - Verified: Task categorization aligns with principles

Follow-up TODOs: None

Rationale for version 1.0.0:
  - Initial ratification of project constitution
  - All principles defined and actionable
  - All placeholder tokens replaced with concrete values
═══════════════════════════════════════════════════════════════════════════
-->

# RSS Feed Reader Constitution

## Core Principles

### I. Security-First Development

Security MUST be considered at every development stage. All components MUST follow secure coding practices:

- Input validation MUST be performed on all user-supplied data (URLs, text fields)
- External data (RSS/Atom feeds) MUST be sanitized before display to prevent XSS attacks
- API endpoints MUST validate request data and return appropriate error codes
- HTTP clients MUST use HTTPS for feed fetching operations
- Dependencies MUST be regularly reviewed for known vulnerabilities
- Sensitive configuration (API keys, connection strings) MUST NOT be committed to version control

**Rationale**: As an internet-connected application parsing external XML content, security vulnerabilities could expose users to malicious feeds, XSS attacks, or data breaches. Security-first development prevents these risks from becoming production incidents.

### II. Maintainability Through Simplicity

Code MUST prioritize readability and maintainability over clever optimizations:

- Use clear, descriptive names for classes, methods, and variables
- Keep methods focused on a single responsibility (SRP)
- Avoid premature optimization - profile before optimizing
- Follow YAGNI (You Aren't Gonna Need It) - implement only what is required for the current phase
- Use standard .NET patterns and conventions (no custom frameworks unless justified)
- Document complex business logic and architectural decisions in code comments

**Rationale**: This is an MVP-driven project with planned incremental expansion (MVP → Extended-MVP → Post-MVP). Simple, maintainable code enables rapid iteration and feature additions without technical debt accumulation.

### III. Test-First for Core Functionality

Tests MUST be written before implementing core functionality (RED → GREEN → REFACTOR):

- User acceptance scenarios from specs MUST have corresponding integration tests
- API contract changes MUST have contract tests that fail before implementation
- Critical business logic (feed parsing, subscription management) MUST have unit tests
- Tests MUST fail initially, then pass after correct implementation
- Refactoring MUST NOT break existing tests

**Rationale**: TDD ensures that requirements are correctly understood before implementation and provides regression protection as the application evolves through its MVP phases. Given the iterative development approach, tests serve as living documentation of intended behavior.

### IV. Separation of Concerns (Backend/Frontend)

Backend and frontend responsibilities MUST be clearly separated:

- **Backend** (ASP.NET Core Web API) handles: data management, business logic, external integrations (feed fetching), validation, error handling
- **Frontend** (Blazor WebAssembly) handles: UI rendering, user interaction, state management, presentation logic
- Communication MUST occur exclusively through well-defined HTTP API contracts (JSON)
- Frontend MUST NOT contain business logic that belongs in the backend
- Backend MUST NOT generate UI markup (except API documentation)

**Rationale**: Clear separation enables independent development, testing, and deployment of frontend and backend. It supports future scalability (multiple frontends, API reuse) and aligns with the project's architectural choice of Web API + Blazor.

### V. Incremental Development with Phase Gates

Features MUST be developed incrementally following defined project phases:

- **MVP Phase**: Subscription management only (no feed fetching, no persistence)
- **Extended-MVP Phase**: Add feed fetching and display (manual refresh, basic error handling)
- **Post-MVP Phase**: Add persistence, background polling, advanced features

Each phase MUST be:

- **Independently deployable**: MVP can be deployed without Extended-MVP features
- **Testable**: Clear acceptance criteria for phase completion
- **Documented**: Phase boundaries and feature scope defined in specs

Do NOT implement features from future phases during current phase development.

**Rationale**: The project explicitly follows an MVP-first approach (documented in ProjectGoals.md) to demonstrate subscription management quickly before adding complexity. Phase gates prevent scope creep and ensure rapid delivery of working increments.

## Security Requirements

All code MUST adhere to OWASP secure coding guidelines relevant to web applications:

- **XSS Prevention**: Sanitize all HTML content from RSS feeds using an allowlist approach (e.g., HtmlSanitizer library)
- **CORS Configuration**: Backend MUST configure CORS policies explicitly (no wildcard origins in production)
- **XML External Entity (XXE) Prevention**: XML parsers MUST disable external entity resolution
- **Dependency Management**: Use `dotnet list package --vulnerable` regularly; update vulnerable packages immediately
- **Error Handling**: Error messages MUST NOT expose sensitive implementation details (stack traces, file paths, connection strings)
- **HTTPS**: Production deployments MUST use HTTPS; HTTP is acceptable only for local development

## Development Workflow

All feature development MUST follow this workflow:

1. **Specification Phase**: Create feature spec using `/speckit.spec` (User stories, acceptance scenarios, functional requirements)
2. **Planning Phase**: Generate implementation plan using `/speckit.plan` (Technical context, constitution check, structure decision)
3. **Task Breakdown**: Generate task list using `/speckit.tasks` (Organized by user story, with phase gates)
4. **Implementation Phase**: Execute tasks following TDD for core functionality
5. **Validation Phase**: Verify all acceptance scenarios pass; update documentation if needed

**Code Review Requirements**:

- All changes MUST be submitted via pull requests
- PRs MUST include: description of changes, reference to spec/tasks, test results
- Reviewers MUST verify constitution compliance (security, maintainability, phase alignment)
- Breaking API changes MUST be documented and version-bumped appropriately

**Quality Gates**:

- Code MUST compile without warnings
- Tests MUST pass (no failing tests allowed in main branch)
- No known security vulnerabilities in dependencies
- Constitution principles MUST be followed (reviewer validates)

## Governance

This constitution supersedes all other development practices and guidelines. When conflicts arise between this constitution and other documentation, the constitution takes precedence.

**Amendment Process**:

- Amendments MUST be proposed via pull request to `.specify/memory/constitution.md`
- Amendment PRs MUST include: rationale, impact analysis, version bump justification
- Amendments require approval from project maintainers
- Version number MUST be incremented following semantic versioning:
  - **MAJOR**: Backward-incompatible principle removals or redefinitions
  - **MINOR**: New principles or sections added
  - **PATCH**: Clarifications, wording improvements, typo fixes

**Compliance Review**:

- All pull requests MUST verify compliance with this constitution
- Plan documents (`plan.md`) MUST include a "Constitution Check" section verifying principle adherence
- Complexity that violates simplicity principles MUST be justified and documented
- Non-compliance issues MUST be resolved before merging

**Version**: 1.0.0 | **Ratified**: 2026-01-30 | **Last Amended**: 2026-01-30
