# Contributing

Thanks for your interest in the MCP Python SDK. This document explains how the project takes contributions and why, and then how to set up a development environment if you're working on a change we've agreed on.

## Before You Start

> [!IMPORTANT]
> **The most useful contribution is a good issue. Pull requests from outside the maintainer team are only reviewed when a maintainer has assigned you the linked issue; anything else is closed automatically.** The rest of this section explains why, and what we do welcome.

### Why issues, not pull requests

This SDK is maintained by a very small team. Since AI coding agents became the norm, every open issue attracts pull requests within hours — mostly generated, mostly plausible-looking, and each one still costs a maintainer the same time to properly review as it did when writing it took a human a weekend, so that trade no longer works. The maintainers drive agents that are tuned to this codebase and its conventions every day; when an issue is clear, producing a fix that fits how the SDK wants to work is faster for us than reverse-engineering someone else's patch, and reviewing someone else's agent output is strictly more work than reviewing our own.

What we can't generate is your context: what you were doing, what you expected, the minimal reproduction, the environment it breaks in, the constraint we haven't thought of. That's the scarce part, it's what a good issue carries, and it's what we ask for.

### How pull requests get in

A PR from someone outside the maintainer team stays open only if **all** of these hold:

1. Its description links an open issue in this repository with a closing keyword (`Fixes #123`, `Closes #123`, `Resolves #123`).
2. **You are assigned to that issue by a maintainer**, or the issue carries the [`help wanted`](https://github.com/modelcontextprotocol/python-sdk/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22) label (which means we'd take a PR for it from anyone).

Anything else is labeled `missing-issue-link`, gets a comment explaining this, and is closed by a bot within a minute of opening. If you've already opened one, **it reopens automatically** the moment a maintainer assigns you the issue, so don't open a new PR — edit the one you have, and push fixes as new commits rather than force-pushing while it's closed (GitHub can't reopen a PR whose branch was rewritten). This applies to typo and docs fixes too; for those, an issue pointing at the problem is honestly all we need.

Assignment is a maintainer decision ([who that is](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/MAINTAINERS.md#python-sdk)). A bare "can I take this?" or "please assign me" doesn't influence it and is the most common noise on the tracker, so please don't — and if you're driving an agent, don't let it. What does help is a comment that shows you've engaged with the issue: confirming the repro, asking about the intended behaviour, or saying briefly how you'd approach it. That's the conversation we assign on. If you reported the issue and would like to fix it yourself, say so in the issue body; whoever reported an issue has first claim if we do want an outside PR for it.

Being assigned is a commitment both ways: we'll review the PR properly, and you'll see it through review yourself. If you can't explain a part of your own diff, we'll unassign so someone else can pick it up.

Maintainers and a small group of trusted regular contributors are exempt from the gate, as are Dependabot and the project's own automation. A maintainer can also wave a specific PR through by reopening it.

### Who we actively want to hear from

- **You hit a real bug.** File it with a minimal reproduction. If you already have a fix, say so in the issue and link your branch — no need to open the PR yet. If we'd rather take it from you than write it ourselves, we'll assign you the issue and you can open it then.
- **You want to learn the codebase or become a regular contributor.** Genuinely welcome, and worth our time in a way drive-by patches aren't. Start by filing or triaging issues well; when you want to take one on, comment with how you'd approach it rather than just claiming it. People who do this consistently get added to the trusted-contributor group and skip the gate entirely — if you think you're there, ask in [#python-sdk-dev on the MCP Contributors Discord](https://discord.gg/6CSzBmMkjX). `good first issue` still requires assignment precisely because we want that conversation first.
- **You maintain another MCP SDK or work on the spec.** Say so in #python-sdk-dev or on the issue; a maintainer can reopen a specific PR past the gate, and you're who the trusted-contributor group is for.

### AI-assisted contributions

We use AI tooling constantly and have no problem with you using it too. The rules are about the human, not the tool:

- **Disclose it.** One line in the PR or issue description.
- **Own it.** You can explain the change and the reasoning in your own words. When a maintainer asks a question, the answer comes from you, not pasted from a chat window.
- **No autonomous agents.** Issues, PRs, or comments produced by an agent with no human who has actually hit the problem and read the output are closed on sight. If your agent is filing PRs against our open issues, stop; the gate above exists because of exactly this.
- **Keep issues short and factual.** What happened, what you expected, how to reproduce. Please don't paste an LLM's speculative root-cause analysis or a proposed patch into the issue body — an incorrect diagnosis is harder to work with than none, and it's the one part we can regenerate.

Undisclosed AI contributions get closed. Repeat offenders are blocked from the `modelcontextprotocol` org. The org-wide [AI contribution policy](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/AI_POLICY.md) also applies.

### The SDK is opinionated

Not every contribution will be accepted, even with a working implementation and an assigned issue. We prioritize maintainability and consistency over adding capabilities. This is at maintainers' discretion.

These always need discussion on an issue before anyone writes code:

- New public APIs or decorators
- Architectural changes or refactoring
- Changes that touch multiple modules
- Features that might require spec changes (these need a [SEP](https://github.com/modelcontextprotocol/modelcontextprotocol) first)

### Issue labels

| Label | Meaning |
|-------|---------|
| [`help wanted`](https://github.com/modelcontextprotocol/python-sdk/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22) | We'd take a PR for this from anyone — no assignment needed |
| [`good first issue`](https://github.com/modelcontextprotocol/python-sdk/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) | Approachable without deep codebase knowledge; still needs assignment — comment with your approach, not just a claim |
| [`ready for work`](https://github.com/modelcontextprotocol/python-sdk/issues?q=is%3Aopen+is%3Aissue+label%3A%22ready+for+work%22) | Triaged and queued for a **maintainer** — not an invitation for PRs |
| `needs confirmation`, `needs repro`, `needs decision`, `needs design` | Not actionable yet; more information or a maintainer call is needed first |

## Development Setup

1. Make sure you have Python 3.10+ installed
2. Install [uv](https://docs.astral.sh/uv/getting-started/installation/)
3. Fork the repository
4. Clone your fork: `git clone https://github.com/YOUR-USERNAME/python-sdk.git`
5. Install dependencies:

```bash
uv sync --frozen --all-extras --dev
```

6. Set up pre-commit hooks:

```bash
uv tool install pre-commit --with pre-commit-uv --force-reinstall
```

## Development Workflow

1. Choose the correct branch for your changes:

   | Change Type | Target Branch | Example |
   |-------------|---------------|---------|
   | New features and fixes for v2 | `main` | New APIs, refactors |
   | Security fixes for v1 | `v1.x` | Critical patches |
   | Critical bug fixes for v1 | `v1.x` | Backports of severe bugs |

   > **Note:** `main` is the current stable line (v2). The `v1.x` branch is the previous major's maintenance line and receives only security and critical bug fixes.

2. Create a new branch from your chosen base branch

3. Make your changes

4. Ensure tests pass:

```bash
uv run pytest
```

5. Run type checking:

```bash
uv run pyright
```

6. Run linting:

```bash
uv run ruff check .
uv run ruff format .
```

7. Update README snippets if you modified `docs_src/` code embedded in the README:

```bash
uv run scripts/update_readme_snippets.py
```

8. (Optional) Run pre-commit hooks on all files:

```bash
pre-commit run --all-files
```

9. Open a pull request against the branch you started from — see [Pull Requests](#pull-requests); you need to be assigned to the linked issue first

## Code Style

- We use `ruff` for linting and formatting
- Follow PEP 8 style guidelines
- Add type hints to all functions
- Include docstrings for public APIs

## Pull Requests

By the time you open a PR, you should be assigned to the issue it fixes (see [How pull requests get in](#how-pull-requests-get-in)) and the "what" and "why" should already be settled there. This keeps reviews focused on implementation.

- Put `Fixes #<issue>` in the description — the intake gate looks for it.
- If your PR was auto-closed, don't open another. Fix the description or wait to be assigned; it reopens itself. Don't force-push or rebase the branch while it's closed.
- Tick "Allow edits by maintainers" so we can push small fixes rather than round-trip.

### Scope

Small PRs get reviewed fast. Large PRs sit in the queue.

A few dozen lines can be reviewed in minutes. Hundreds of lines across many files takes real effort and things slip through. If your change is big, break it into smaller PRs or get alignment from a maintainer first.

### What gets rejected

- **No assigned issue**: closed automatically, as above
- **Scope creep**: changes that go beyond what was discussed on the issue
- **Misalignment**: even well-implemented features may be rejected if they don't fit the SDK's direction
- **Overengineering**: unnecessary complexity for simple problems
- **Undisclosed or unreviewed AI output**: see [AI-assisted contributions](#ai-assisted-contributions); this includes PR descriptions that read like an unedited transcript of everything the model did

### Checklist

1. Update documentation as needed
2. Add tests for new functionality
3. Ensure CI passes
4. Address review feedback

## Code of Conduct

Please note that this project is released with a [Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
