# Skill Masterclass

A hands-on masterclass workspace for building reusable AI agent skills for QA and SDET workflows. The current project includes an STLC-organized skill library, framework-specific automation packs, and generated Jira/VWO test-planning artifacts.

## What This Repository Contains

| Path | Purpose |
|---|---|
| `skillmasterclass/index.html` | Standalone Skill Masterclass web page by SusheelKumar. |
| `skillmasterclass/skills/README.md` | STLC-oriented skill-suite overview and installation notes. |
| `skill_dir/STLC/` | Main STLC skill library, organized from requirement analysis through test closure. |
| `skill_dir/STLC/01_jira-requirement-analyzer/` | Jira readiness-analysis skill that flags gaps, ambiguities, risks, and author questions. |
| `skill_dir/STLC/test-plan-creator/` | Reusable skill package for creating review-ready test plans from Jira tickets. |
| `skill_dir/Test Automation/` | Playwright and Selenium framework skill packs. |
| `.claude/test-plan-creator/` | Claude-compatible copy of the test-plan skill. |
| `.gemini/test-plan-creator/` | Gemini-compatible copy of the test-plan skill. |
| `.github/test-plan-creator/` | GitHub/Copilot-oriented copy of the test-plan skill. |
| `output/test-plans/` | Generated Markdown test-plan outputs. |
| `output/pdf/` | Generated PDF test-plan outputs. |
| `old/` | Earlier simple skill and app-level VWO test-plan draft retained for reference. |

## Skill Library

The primary reusable skills now live under `skill_dir/`. They are grouped by testing lifecycle phase and automation stack so a QA/SDET can move from requirement analysis to test closure without hunting through unrelated folders.

### Requirement Analysis

`skill_dir/STLC/01_jira-requirement-analyzer/` checks whether a Jira ticket is ready to test. It:

- Fetches the ticket via Jira/Rovo MCP or REST.
- Scores requirements as clear, ambiguous, or missing.
- Separates findings into gaps, ambiguities, and risks.
- Drafts specific clarifying questions for the ticket author.
- Creates a lightweight HTML report named `jira-analyzed-<ticket-id>.html` under `/tmp`.
- Stops at a **Human Review Gate** before questions are treated as final.

### Test Planning

`skill_dir/STLC/test-plan-creator/` is designed to turn a Jira ticket into a QA-owned draft test plan.

It follows this workflow:

1. Fetch the Jira ticket by key.
2. Capture the summary, description, acceptance criteria, links, attachments, components, and version metadata.
3. Run a requirement-gap analysis using `skill_dir/STLC/test-plan-creator/references/requirement-checklist.md`.
4. Fill the standard plan structure from `skill_dir/STLC/test-plan-creator/references/test-plan-template.md`.
5. End with a mandatory **Human Review Gate** so the plan is never treated as final without QA/product approval.

The skill intentionally avoids fabricating missing requirements. Missing acceptance criteria, unclear error states, undefined environments, unsupported edge cases, and vague non-functional expectations are surfaced as questions for the ticket author.

### Test Automation

`skill_dir/Test Automation/` contains framework packs for:

- Playwright skills: locator fixing, test generation, page objects, fixtures, API testing, network mocking, traces, accessibility, visual regression, CI, and flaky debugging.
- Selenium skills: driver management, locator strategy, test generation, page objects, data-driven testing, waits, Grid, reports, cross-browser runs, framework scaffolding, and flaky debugging.

## Jira Fetching

The Jira helper script is:

```bash
bash "skill_dir/STLC/test-plan-creator/scripts/fetch_jira.sh" VWO-49
```

It expects these environment variables:

```bash
JIRA_BASE_URL="https://your-domain.atlassian.net"
JIRA_EMAIL="you@example.com"
JIRA_TOKEN="your-api-token"
```

Keep credentials in `.env`. The `.env` file is intentionally ignored by git.

## Generated Artifacts

### VWO App Test Plan

`old/app-vwo-test-plan.md` contains an earlier broad test plan for `app.vwo.com`, covering:

- Authentication and account access.
- Dashboard and navigation.
- Project/tracking setup.
- Experiments, personalization, feature rollouts, and behavior analytics.
- Reporting, integrations, roles, permissions, security, accessibility, and performance.

### Jira VWO-49 Test Plan

The current generated deliverables are:

- `output/test-plans/VWO-49-test-plan.md`
- `output/pdf/VWO-49-test-plan.pdf`

These were generated from Jira ticket `VWO-49`: **[VWO] Add Passkey and SSO Login Options to VWO Login Page**.

The plan includes:

- Ticket snapshot and scope.
- Requirement checklist results.
- Gaps and questions for the ticket author.
- P0/P1/P2 test scenarios.
- Test data and environment needs.
- Risks and assumptions.
- Entry and exit criteria.
- Human Review Gate.

### Jira VWO-49 Requirement Analysis

A readiness analysis was also created for Jira ticket `VWO-49` using the requirement analyzer skill. It found the ticket was **not ready to test** until missing environment, SSO provider, passkey support, test-data, error-message, regression, security, and rollout details are clarified.

The generated HTML report is intentionally written to `/tmp/jira-analyzed-VWO-49.html` during execution and is not committed because `/tmp` artifacts are local run outputs.

## PDF Output

PDF generation was done from the Markdown test plan and visually verified from rendered PNG pages. Generated PDFs should stay in `output/pdf/`, while temporary render files belong under `tmp/` and are ignored by git.

## Recommended Usage

1. Add Jira credentials to `.env`.
2. Fetch a ticket using `skill_dir/STLC/test-plan-creator/scripts/fetch_jira.sh`.
3. Analyze readiness using `skill_dir/STLC/01_jira-requirement-analyzer/`.
4. Draft the test plan using `skill_dir/STLC/test-plan-creator/`.
5. Save Markdown under `output/test-plans/`.
6. Generate PDF under `output/pdf/`.
7. Review the Human Review Gate before converting the plan into detailed test cases or automation.

## Repository Hygiene

- Do not commit `.env`, tokens, screenshots from temporary rendering, or local dependency folders.
- Keep reusable skill instructions inside `skill_dir/`.
- Keep generated customer/ticket outputs inside `output/`.
- Keep draft or legacy examples inside `old/` only when they remain useful for reference.

## Current Status

- STLC skill library: created under `skill_dir/STLC/`.
- Test automation skill packs: created under `skill_dir/Test Automation/`.
- Jira requirement analyzer: created.
- Test plan creator: moved under `skill_dir/STLC/test-plan-creator/`.
- Jira fetch helper: created.
- Requirement checklist: created.
- Test-plan template: created.
- VWO app-level plan: created.
- Jira `VWO-49` readiness analysis: created as a local `/tmp` HTML report during execution.
- Jira `VWO-49` Markdown plan: created.
- Jira `VWO-49` PDF plan: created and verified.
