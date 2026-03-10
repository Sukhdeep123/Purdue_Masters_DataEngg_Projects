# CLAUDE.md

This file provides guidance to AI assistants (Claude and others) working in this repository.

## Repository Status

This repository is currently **newly initialized** and contains no source code yet.
This CLAUDE.md serves as the foundational conventions document to be updated as the project grows.

---

## Project Overview

> **TODO:** Update this section once the project is defined.
> Describe what this project does, its purpose, and its intended audience.

---

## Repository Structure

> **TODO:** Update this section as the codebase grows.

```
/
├── CLAUDE.md          # This file — AI assistant guidance
└── README.md          # (to be created) Project documentation
```

---

## Development Workflows

### Initial Setup

1. Clone the repository:
   ```bash
   git clone <repo-url>
   cd Sukhdeep
   ```

2. Install dependencies (update once a package manager is chosen):
   ```bash
   # e.g., npm install / pip install -r requirements.txt / go mod download
   ```

3. Copy environment variables (if applicable):
   ```bash
   cp .env.example .env
   ```

### Running the Project

> **TODO:** Add commands to start the development server, run the application, etc.

### Running Tests

> **TODO:** Add test commands once a testing framework is chosen.

### Building for Production

> **TODO:** Add build commands once the stack is determined.

---

## Git Conventions

### Branch Naming

| Branch Type | Pattern | Example |
|---|---|---|
| Features | `feature/<short-description>` | `feature/user-auth` |
| Bug fixes | `fix/<short-description>` | `fix/login-redirect` |
| Chores / maintenance | `chore/<short-description>` | `chore/update-deps` |
| AI-generated branches | `claude/<task-id>` | `claude/claude-md-mml8ccqhjxga6udo-51KaS` |

### Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<optional scope>): <short summary>

[optional body]

[optional footer(s)]
```

**Types:**
- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation only changes
- `style` — formatting, missing semicolons, etc. (no logic change)
- `refactor` — code refactoring (no feature or fix)
- `test` — adding or updating tests
- `chore` — build process, tooling, dependencies

**Examples:**
```
feat(auth): add JWT-based login endpoint
fix(api): handle null response from third-party service
docs: update README with setup instructions
```

### Pull Request Guidelines

- Keep PRs focused and small; one logical change per PR
- Write a clear title following commit message conventions
- Describe *what* changed and *why* in the PR body
- Link any related issues
- Ensure tests pass before requesting review

---

## Code Style and Conventions

> **TODO:** Populate this section once the language/framework is decided.

### General Principles

- **Clarity over cleverness** — write code that is easy to read and reason about
- **Minimal surface area** — only expose what is necessary
- **Single responsibility** — functions and modules should do one thing well
- **No magic numbers** — use named constants
- **Fail fast** — validate inputs at boundaries; surface errors early

### Naming Conventions

> Update with language-specific conventions once the stack is chosen.

| Construct | Convention | Example |
|---|---|---|
| Variables | `camelCase` | `userName` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRIES` |
| Functions/Methods | `camelCase` | `fetchUserData()` |
| Classes | `PascalCase` | `UserService` |
| Files | `kebab-case` | `user-service.ts` |

### Formatting

> **TODO:** Document formatter and linter configuration (e.g., ESLint + Prettier, Black, gofmt, etc.)

---

## Environment Variables

> **TODO:** Document all required environment variables with descriptions once they are defined.

| Variable | Required | Description | Example |
|---|---|---|---|
| `NODE_ENV` | Yes | Runtime environment | `development` |

Store secrets in `.env` (never commit this file). Use `.env.example` as the template.

---

## Testing Guidelines

> **TODO:** Update once a testing framework is selected.

### Principles

- Write tests for all non-trivial logic
- Test behavior, not implementation details
- Use descriptive test names: `it("should return 404 when user is not found")`
- Aim for fast, deterministic, isolated tests
- Mock external services and I/O

### Test Organization

```
tests/
├── unit/          # Pure function / isolated unit tests
├── integration/   # Tests that span multiple modules or external services
└── e2e/           # End-to-end / acceptance tests
```

---

## CI/CD

> **TODO:** Describe the CI/CD pipeline once it is configured.

Typical pipeline stages to set up:
1. Lint & format check
2. Unit tests
3. Integration tests
4. Build
5. Deploy (staging → production)

---

## Security Guidelines

- Never commit secrets, API keys, or credentials — use `.env` and a secrets manager
- Validate and sanitize all user inputs
- Use parameterized queries to prevent SQL injection
- Keep dependencies up to date; audit regularly (`npm audit`, `pip-audit`, etc.)
- Follow the principle of least privilege for service accounts and IAM roles

---

## For AI Assistants

### Key Rules

1. **Read before editing** — always read a file before making changes to it
2. **Minimal changes** — only modify what is necessary to fulfill the request; do not refactor unrelated code
3. **No unnecessary files** — do not create files unless explicitly required
4. **No over-engineering** — keep solutions simple; avoid abstractions for one-time use
5. **Security first** — do not introduce SQL injection, XSS, command injection, or other OWASP Top 10 vulnerabilities
6. **Confirm destructive actions** — ask before deleting files, dropping tables, or force-pushing

### Workflow

1. Understand the task fully before writing any code
2. Explore relevant files using `Glob` and `Grep` tools
3. Read the specific files that will be changed
4. Make targeted, focused edits
5. Run tests to verify correctness
6. Commit with a descriptive Conventional Commit message
7. Push to the designated branch (never `main`/`master` without explicit permission)

### Branch for AI Work

AI-generated changes must be pushed to branches prefixed with `claude/`:
```
claude/<task-description-and-session-id>
```

Never push directly to `main` or `master` without explicit user approval.

---

## Updating This File

This CLAUDE.md should be kept up to date as the project evolves. Update it when:
- New dependencies or tools are added
- Conventions change
- New workflows are established
- The project structure changes significantly
