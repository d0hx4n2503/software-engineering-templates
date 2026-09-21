# Contributing Guide

Thank you for your interest in contributing to / using `software-engineering-template`. This document has two parts: **(A)** how to use this repo as a template for a new project, and **(B)** how to contribute improvements to the template repo itself.

---

## A. Using This Repo for a New Project

> Full details live in `00-overview/getting-started-checklist.md` and `00-overview/how-to-use-this-template.md`. Below is a quick summary.

1. **Use this template** on GitHub to create a new repo from this template.
2. Read `00-overview/approved-tech-stack.md` — only use languages/frameworks/tools listed as approved, unless an ADR (`02-architecture-design/architecture-decision-records/`) documents the reason for an exception.
3. Fill in the sample config files for your actual project:
   - `03-devops-infra-security/environments/.env.example`
   - `01-code-management/coding-standards/<your-language>/`
4. Delete directories that don't apply to your project (e.g. if the project has no GraphQL, remove `graphql-schema-template.graphql`) — **do not delete top-level folders (01-05)**, only remove the unused files inside them.
5. Update the root `README.md` with your actual project info, keeping the link that points back to `00-overview/`.
6. From here on, every task/PR in the project must follow `01-code-management/definition-of-ready.md` and `definition-of-done.md`.

## B. Contributing Improvements to the Template Repo Itself

### Process

1. Fork or create a new branch following `01-code-management/git-branching-workflow.md`.
2. If the change affects an **architectural/process decision** (e.g. changing the branching standard, changing the CI tool), open an RFC first at `02-architecture-design/request-for-comments/` to gather team feedback, then record the final decision as an ADR.
3. For small fixes/content updates, you may open a PR directly.
4. Make sure the PR satisfies `01-code-management/code-review-checklist.md` before requesting review.
5. Requires at least **1 approval** from the CODEOWNERS responsible for the affected directory (see `.github/CODEOWNERS`).

### Commit & Branch Conventions

- Branch: follow the format in `git-branching-workflow.md` (e.g. `feature/<description>`, `fix/<description>`).
- Commit message: [Conventional Commits](https://www.conventionalcommits.org/) — `feat:`, `fix:`, `docs:`, `chore:`...
- Each PR should focus on a single logical change — avoid bundling unrelated changes together.

### Checklist Before Opening a PR

- [ ] Updated/synced the folder-tree description comments in `README.md` if files/directories were added or removed.
- [ ] New content includes concrete examples, not just theory (applies to every `*-template.md`).
- [ ] Does not break the existing `00 → 05` numbering structure — new content goes in the right existing group; don't create a new numbered group without an RFC/ADR.
- [ ] Listed any external references the content is based on.
- [ ] Self-reviewed against `code-review-checklist.md`.

### Reporting Bugs & Suggesting Features

- Bugs in template content: use `.github/ISSUE_TEMPLATE/bug_report.md`.
- Suggestions for new sections/templates: use `.github/ISSUE_TEMPLATE/feature_request.md`.
- **Security vulnerabilities**: do not open a public issue — follow `SECURITY.md` instead.

### Code of Conduct

All contributions must comply with `CODE_OF_CONDUCT.md`.

---

*Questions? Reach the `{{TEAM_NAME}}` team via `{{CONTACT_CHANNEL}}`.*