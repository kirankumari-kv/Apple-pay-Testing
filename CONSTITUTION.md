# SupportTriage AI Engineering Constitution
**Version:** 1.0.0

This document is the supreme engineering authority for the **SupportTriage AI** project. All code, architecture, and engineering decisions must comply with the rules and standards defined herein. No pull request may be merged if it violates this constitution.

## Mission
To build a scalable, secure, and highly observable AI-powered customer support triage system that accurately routes, categorizes, and prioritizes incoming support tickets, reducing manual overhead and improving customer response times.

## Core Values
*   **Correctness** over speed
*   **Security** over convenience
*   **Simplicity** over cleverness
*   **Maintainability** over shortcuts
*   **Observability** over assumptions
*   **Explicitness** over magic
*   **Automation** over manual processes
*   **Testing** over trust

## Technology Stack

### Required Technologies
| Category | Technology |
| :--- | :--- |
| **Language** | TypeScript (Strict Mode) |
| **Frontend** | React, Next.js, Tailwind CSS |
| **Backend** | Node.js, Express |
| **Database** | PostgreSQL, Prisma ORM |
| **Testing** | Jest (Unit/Integration), Playwright (E2E) |
| **Infrastructure** | Docker, AWS (ECS, RDS) |

### Forbidden Technologies & Practices
*   Plain JavaScript in application code
*   Unmaintained dependencies
*   Experimental libraries in production without approval

## Repository Structure
*Assumption: A monorepo architecture best supports the full-stack nature of this project.*

The project utilizes a Turborepo-based monorepo structure:
```text
support-triage-ai/
├── apps/
│   ├── web/               # Next.js frontend application
│   └── api/               # Node.js/Express backend service
├── packages/
│   ├── ui/                # Shared React components
│   ├── database/          # Prisma schema and client
│   ├── config/            # Shared ESLint, TSConfig, Prettier
│   └── ai-core/           # Shared AI prompt and agent logic
└── package.json
```

## Language/Code Standards
*   **Language:** TypeScript (Strict Mode enabled). `any` is strictly forbidden.
*   **Naming Conventions:**
    *   Variables, functions, methods: `camelCase`
    *   Classes, React Components, Interfaces, Types: `PascalCase`
    *   Constants, Environment Variables: `UPPER_SNAKE_CASE`
    *   Files/Directories: `kebab-case`
*   **Component/File Size Limits:** 300 lines target, 500 mandatory refactor.

## Frontend Standards
*   **State Management:** Prefer React Server Components and server actions. Use React Context only for global UI state (e.g., theme, sidebar toggle).
*   **Styling:** Tailwind CSS exclusively. No inline styles or external CSS files.
*   **Data Fetching:** Use React Query for client-side fetching; native `fetch` with caching for Server Components.

## Backend/API & Validation Standards
*   **Architecture:** RESTful API design.
*   **Validation:** All incoming requests (body, query, params) MUST be validated using **Zod** before processing.
*   **Database Access:** All database interactions must go through the Prisma ORM. Raw SQL is forbidden unless explicitly approved for performance bottlenecks.

## Error Handling
Errors must be explicitly caught, categorized, and handled. Do not leak stack traces to the client.

**Allowed Error Categories:**
*   `VALIDATION_ERROR`
*   `AUTHENTICATION_ERROR`
*   `AUTHORIZATION_ERROR`
*   `BUSINESS_ERROR`
*   `EXTERNAL_SERVICE_ERROR`
*   `INFRASTRUCTURE_ERROR`
*   `UNKNOWN_ERROR`

## Logging
All logs must be structured JSON. `console.log` is forbidden in production code; use the designated logging library (e.g., Pino).

**Required Structured Log Fields:**
```json
{
  "event": "TICKET_ROUTED",
  "timestamp": "2023-10-27T10:00:00Z",
  "requestId": "req-12345",
  "userId": "usr-9876",
  "metadata": {
    "ticketId": "tkt-555",
    "confidenceScore": 0.95
  }
}
```

## Security
*   **Authentication & Authorization:** Must occur server-side. Never trust client-side claims.
*   **Secrets Handling:**
    *   From environment variables.
    *   From a secret manager (e.g., AWS Secrets Manager) in production.
    *   Never commit secrets (enforced via pre-commit hooks).
*   **Dependency Policy:**
    *   Must pass security scan (e.g., `npm audit`, Snyk).
    *   Must pass license review (MIT, Apache 2.0 preferred).
    *   Must be maintained.
    *   Prefer building over adding a dependency when smaller.

## Accessibility
*   **Standard:** WCAG 2.1 AA compliance is mandatory for all user-facing interfaces.
*   **Implementation:** Use semantic HTML. Ensure proper ARIA attributes, keyboard navigability, and sufficient color contrast.

## Performance
*   **Frontend:** Core Web Vitals must be met (LCP < 2.5s, FID < 100ms, CLS < 0.1).
*   **Backend:** 95th percentile API response time must be < 200ms (excluding external AI provider latency).

## Testing
*   **Minimum Coverage:** 80% minimum overall, 95% for critical business logic (e.g., ticket routing logic, authorization).
*   **Required Test Types:**
    *   **Unit Tests:** For all utility functions, business logic, and complex UI components.
    *   **Integration Tests:** For API endpoints and database queries.
    *   **End-to-End (E2E) Tests:** For critical user journeys (e.g., submitting a ticket, viewing a triage dashboard).

## CI/CD
All code must pass through automated pipelines before merging.
*   **CI Gates per PR:** Lint, Typecheck, Unit tests, Integration tests.
*   **Deployment:** Automated deployment to staging on merge to `main`. Production deployments require manual approval after staging validation.

## Documentation
*   **Code:** Use TSDoc for all exported functions, classes, and interfaces.
*   **API:** Maintain an up-to-date OpenAPI (Swagger) specification.
*   **Architecture:** Significant architectural changes require an Architecture Decision Record (ADR) in the `docs/adrs` directory.

## Observability
*   **Tracing:** Distributed tracing (OpenTelemetry) must be implemented across all services.
*   **Metrics:** Monitor API latency, error rates, and AI provider token usage.
*   **Health:** All services must expose a `/health` endpoint for infrastructure monitoring.

## AI Development Rules
*   **AI-Generated Code Policy:** AI code is UNTRUSTED — must be reviewed, tested, and validated before merge.
*   **Agent Restrictions (each without human approval):**
    *   May NOT deploy to production.
    *   May NOT rotate credentials.
    *   May NOT modify infrastructure.
    *   May NOT approve pull requests.

## Prompt/MCP/RAG Standards
*   **Prompts:** Version-controlled, documented, and tested. Prompt changes require code review.
*   **MCP (Model Context Protocol):** Integrations must be least-privilege, auditable, and revocable.
*   **RAG (Retrieval-Augmented Generation):** Sources must be trusted, versioned, and source-attributed in the final output.

## Code Review Standards
Every Pull Request description MUST answer the following questions:
1.  What changed?
2.  Why?
3.  Risks?
4.  Rollback plan?
5.  Testing evidence?

## Git Standards
*   **Branch Conventions:** `feature/*`, `bugfix/*`, `hotfix/*`, `chore/*`
*   **Commit Conventions:** Conventional Commits required (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `perf:`, `chore:`).

## Dependency Rules
*   Must pass security scan.
*   Must pass license review.
*   Must be maintained.
*   Prefer building over adding a dependency when smaller.

## Definition of Done
A feature or bugfix is not complete until all of the following are true:
*   [ ] Requirements implemented
*   [ ] Tests written
*   [ ] Tests passing
*   [ ] Typecheck passing
*   [ ] Lint passing
*   [ ] Security review completed
*   [ ] Documentation updated
*   [ ] Accessibility validated
*   [ ] Performance validated
*   [ ] Code reviewed

## Non-Negotiable Rules (NEVER / ALWAYS)
*   **NEVER** commit secrets, credentials, or API keys.
*   **NEVER** bypass CI/CD checks.
*   **NEVER** mutate state directly in React.
*   **ALWAYS** validate external inputs and API payloads.
*   **ALWAYS** write a regression test for a bug fix.
*   **ALWAYS** handle errors gracefully without exposing internal system details.

## Amendment Process
This constitution is a living document. To amend it:
1.  **Written Proposal:** Submit a PR modifying this document with a detailed justification.
2.  **Architecture Review:** The proposal must be reviewed by the lead architect/engineering manager.
3.  **Team Approval:** Requires consensus or majority vote from the core engineering team.
4.  **Version Increment:** Upon approval, increment the version number at the top of this document.