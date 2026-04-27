# Contributing to RayCtify

> *"Algorithmic fairness is only as strong as the rigor of the people who build it."*

Thank you for your interest in contributing to RayCtify. Because this system operates at the intersection of financial compliance, machine learning, and civil rights law, the bar for contributions is intentionally high. Every line of code in this repository has the potential to influence real lending decisions, which means correctness, security, and auditability are not optional — they are the foundation.

This document explains everything you need to know before making your first contribution. Please read it in full before opening an issue or submitting a Pull Request.

---

## Table of Contents

1. [Code of Conduct](#1-code-of-conduct)
2. [Before You Start: The Mindset](#2-before-you-start-the-mindset)
3. [How to Report a Bug](#3-how-to-report-a-bug)
4. [How to Suggest a Feature](#4-how-to-suggest-a-feature)
5. [The Contribution Workflow](#5-the-contribution-workflow)
6. [Branch Naming Conventions](#6-branch-naming-conventions)
7. [Coding Standards](#7-coding-standards)
8. [Testing Requirements](#8-testing-requirements)
9. [Pull Request Standards](#9-pull-request-standards)
10. [Security Disclosures](#10-security-disclosures)
11. [Review Process & SLAs](#11-review-process--slas)

---

## 1. Code of Conduct

RayCtify is built on the principle that fair systems require fair builders. All contributors — regardless of experience level, background, or affiliation — are expected to engage with professionalism, patience, and intellectual honesty.

In practical terms, this means that feedback on code should always be directed at the code itself, never at the person who wrote it. It means that disagreements about technical direction should be resolved through evidence and reasoned argument, not seniority or volume. And it means that the shared goal of every contributor is the same: making algorithmic lending fairer, more auditable, and more trustworthy.

Any conduct that undermines that environment — including harassment, dismissiveness, or bad-faith engagement — will result in the removal of contributor access without appeal.

---

## 2. Before You Start: The Mindset

Before writing a single line of code, it helps to understand *why* RayCtify's standards are structured the way they are.

This is not a general-purpose web application. The outputs of this system are used by compliance officers, legal teams, and regulators to make decisions about whether a lending model is legally defensible. That context creates a specific set of constraints that you'll see woven throughout this guide.

**Correctness over cleverness.** A clever algorithmic shortcut that produces a subtly wrong Bias Penalty score is worse than a straightforward implementation that is unambiguously correct. When in doubt, prioritize the approach that is easiest to audit, not the approach that is most technically impressive.

**Explainability is a feature.** Every calculation that touches a fairness metric, a model coefficient, or a policy guardrail should be written so that a data scientist who has never seen this codebase before can read the code and immediately understand what it is doing and why. This is not negotiable — regulators may one day ask exactly that question.

**Security is not a layer; it is a constraint.** The zero-retention architecture is a core promise of this product. Any contribution that introduces file writes, caching, logging of PII, or persistent state of any kind will be rejected at review, regardless of how well it is implemented.

---

## 3. How to Report a Bug

If you have discovered a bug, the most helpful thing you can do is report it clearly before attempting to fix it. This gives the maintainers the opportunity to confirm the issue, assess its severity, and ensure the fix aligns with the broader architecture.

To report a bug, open a GitHub Issue using the **Bug Report** template. A useful bug report includes a clear description of the observed behavior and the expected behavior, the steps required to reproduce the issue reliably, your environment details (Python version, OS, dependency versions from `pip freeze`), and — if applicable — a minimal example that demonstrates the problem in isolation.

If the bug relates to a fairness calculation, a bias metric, or a Reference Model output, please flag it as **`severity: compliance`** in the issue labels. These reports are escalated immediately, as they have potential regulatory implications.

---

## 4. How to Suggest a Feature

New feature proposals should be opened as GitHub Issues using the **Feature Request** template *before* any implementation work begins. This step exists to protect your time — there is nothing more frustrating than spending a week building something only to discover that it conflicts with an architectural decision made six months ago.

A good feature proposal explains the problem it solves (not just the solution it proposes), why that problem is within scope for RayCtify's core mission, and what the expected behavior would look like from a user or API perspective. If your proposal touches any of the five pillars, explain which pillar it extends and how it interacts with the existing logic.

Features that introduce new demographic categories, new fairness metrics, or new policy guardrails will require a brief written justification citing the relevant regulatory or academic basis for the change. This is not bureaucracy for its own sake — it ensures that every fairness-related decision in the codebase has a documented rationale that can withstand external scrutiny.

---

## 5. The Contribution Workflow

The standard workflow for all contributions follows a fork-and-pull-request model. Here is the full sequence from start to finish.

**Step 1 — Fork the repository.** Click the Fork button on GitHub to create your own copy of the repository under your account. All work happens in your fork, never directly on the `main` branch of the upstream repository.

```bash
# Clone your fork locally
git clone https://github.com/YOUR_USERNAME/rayctify.git
cd rayctify

# Add the upstream repository as a remote so you can stay in sync
git remote add upstream https://github.com/your-org/rayctify.git
```

**Step 2 — Sync with upstream.** Before starting any new work, make sure your local copy reflects the latest state of the upstream `main` branch to minimize merge conflicts.

```bash
git fetch upstream
git checkout main
git merge upstream/main
```

**Step 3 — Create a feature branch.** Never work directly on `main`. Create a new branch named according to the conventions described in Section 6.

```bash
git checkout -b feat/arena-cohort-export
```

**Step 4 — Write your code.** Implement your changes, following the coding standards in Section 7 and the testing requirements in Section 8. Commit frequently with clear, descriptive messages (see below).

**Step 5 — Push and open a Pull Request.** Push your branch to your fork and open a Pull Request against the upstream `main` branch using the PR template. Fill out every section of the template — incomplete PRs will be returned without review.

```bash
git push origin feat/arena-cohort-export
```

### Commit Message Format

RayCtify uses a structured commit message format based on the Conventional Commits specification. Each commit message should follow this pattern:

```
<type>(<scope>): <short imperative summary>

<optional longer body explaining WHY, not WHAT>
```

The `type` should be one of `feat` (new feature), `fix` (bug fix), `refactor` (code restructuring with no behavior change), `test` (adding or updating tests), `docs` (documentation only), or `security` (security-related changes). The `scope` should reference the relevant pillar or module, such as `interceptor`, `arena`, `vaccine`, `guardrails`, or `reference-model`.

A well-formed example: `feat(arena): add per-cohort bias penalty aggregation`. A poorly formed example: `fixed stuff`. The former is auditable; the latter is not.

---

## 6. Branch Naming Conventions

Branch names should be lowercase, hyphen-separated, and prefixed with the type of change being made. This keeps the repository's branch list navigable and makes the CI/CD pipeline's filtering logic reliable.

For new features, use the prefix `feat/` followed by a short description, such as `feat/vaccine-adversarial-export`. For bug fixes, use `fix/` followed by the issue number and a short description, such as `fix/42-dti-normalization-overflow`. For documentation updates, use `docs/`, and for security patches, always use `security/` so that reviewers know to prioritize the review.

---

## 7. Coding Standards

### Python (Backend)

All Python code must be formatted with **Black** and linted with **Ruff** before submission. These tools are included in the development dependencies and can be run as follows:

```bash
# Format all Python files
black backend/

# Lint and auto-fix where possible
ruff check backend/ --fix
```

Beyond formatting, there are a few standards specific to RayCtify's domain. Any function that performs a fairness calculation, modifies model weights, or applies a policy guardrail must include a docstring that explains the mathematical or regulatory basis for its logic — not just what the function does, but *why* it does it that way. Type annotations are required on all function signatures. Magic numbers are not permitted anywhere in the fairness or reference model logic; every threshold or coefficient must be assigned to a named constant with a comment explaining its origin.

```python
# ✅ Correct — named constant with documented basis
# Per FFIEC guidance, deep subprime is defined as FICO < 500
DEEP_SUBPRIME_THRESHOLD: int = 500

# ❌ Incorrect — unexplained magic number
if credit_score < 500:
    ...
```

### TypeScript / React (Frontend)

All frontend code must pass ESLint with the project's shared configuration. Components should be written as typed functional components using React hooks. Props interfaces must be explicitly defined — no implicit `any` types. Framer Motion animations should be defined as named `variants` objects rather than inline, so that they remain readable and adjustable without hunting through JSX trees.

---

## 8. Testing Requirements

Given the compliance context of this system, testing is treated as documentation, not just quality assurance. A test suite that passes gives an auditor confidence that the system behaves as specified under a range of conditions.

**Every new feature must be accompanied by tests.** Pull Requests that introduce new logic without corresponding tests will not be merged, regardless of how well the code itself is written.

For the backend, tests are written with **pytest** and live in the `backend/tests/` directory, mirroring the module structure of the source code. For fairness-critical logic — anything in `interceptor/`, `arena/`, `reference_model/`, or `guardrails/` — tests must cover not just the happy path but also edge cases that reflect real-world regulatory concerns. These include applicants at the exact boundary of a policy threshold, feature vectors where all demographic proxies are present, and batch inputs that contain mixed demographic cohorts.

```bash
# Run the full test suite from the backend directory
pytest backend/tests/ -v --cov=backend --cov-report=term-missing
```

The minimum acceptable code coverage for the `arena/`, `interceptor/`, and `guardrails/` modules is **90%**. For all other modules, the minimum is **80%**. PRs that reduce coverage below these thresholds will be blocked by the CI pipeline.

---

## 9. Pull Request Standards

A Pull Request is not just a code delivery mechanism — it is a communication artifact that will be read by reviewers, referenced in the future when someone asks "why was this changed?", and potentially cited in a compliance audit. Treat it accordingly.

Every PR must include a clear summary of the changes made and the problem they solve, a reference to the GitHub Issue that motivated the change (use `Closes #42` syntax to auto-close the issue on merge), a description of how the changes were tested, and a checklist confirming that linting passes, all tests pass, and no new file writes or PII logging have been introduced.

If your PR touches a fairness metric, a bias calculation, or a Reference Model coefficient, you must also include a brief **Fairness Impact Assessment** — a short written explanation of how the change affects the system's fairness outputs and why that effect is correct and intended.

---

## 10. Security Disclosures

**Do not open a public GitHub Issue to report a security vulnerability.** Public disclosure before a patch is available puts institutions using RayCtify at risk.

Instead, send a detailed report to **security@rayctify.io** with the subject line `[SECURITY] Brief description of the vulnerability`. Your report should include a description of the vulnerability, the conditions under which it can be triggered, and — if known — a suggested remediation approach.

The maintainers commit to acknowledging all security reports within **48 hours** and providing a remediation timeline within **5 business days**. Contributors who responsibly disclose valid security vulnerabilities will be credited in the release notes of the patching release, unless they prefer to remain anonymous.

Any vulnerability that could compromise the zero-retention guarantee — such as a pathway that causes model data or PII to be written to disk or logged — is treated as **Critical severity** and addressed before any other work.

---

## 11. Review Process & SLAs

Once a Pull Request is submitted, here is what to expect.

Within **3 business days**, a maintainer will perform an initial triage review to confirm that the PR template is complete, the CI pipeline is passing, and the change is in scope. PRs that fail triage will be returned with specific feedback rather than simply closed.

For PRs that pass triage, a full technical review will be completed within **7 business days**. Reviews may result in one of three outcomes: approval and merge, a request for changes with specific feedback, or a rejection with a documented explanation if the change is fundamentally incompatible with the project's architecture or mission.

Please do not ping maintainers individually to expedite reviews. If a review SLA has been missed, a single polite comment on the PR thread is the appropriate escalation path.

---

<div align="center">

*Every contribution to RayCtify is a contribution to fairer lending. We are grateful for your time and expertise.*

**RayCtify — Achieving Algorithmic Equilibrium**

</div>
