  # Plan: `pr-walkthrough-companion` — Public Claude Code Skill Repo

  ## Context

  The problem: teams using agentic PR review tools (like `pr-review-toolkit`) accumulate *cognitive debt* — the reviewer clicks "OK-APPLY" without ever building
  a mental model of the PR. Velocity > comprehension.

  Two-phase flow:
  - **Phase 1 (new)**: Socratic conversational walkthrough — forces comprehension before approval.
  - **Phase 2**: Hands off to `pr-review-toolkit` for automated quality checks.

  ---

  ## Final Repo Layout

  ```
  pr-walkthrough-companion/
  ├── README.md
  ├── SKILL.md
  ├── references/
  │   ├── cognitive-debt.md
  │   ├── walkthrough-principles.md
  │   └── pr-review-toolkit-bridge.md
  ├── assets/
  │   └── example-session.md
  └── docs/
      └── index.html              ← landing page, GitHub Pages
  ```

  ---

  ## SKILL.md Frontmatter

  ```yaml
  ---
  name: pr-walkthrough-companion
  description: >
    Two-phase PR review companion. Phase 1: Socratic conversational walkthrough that builds
    the reviewer's mental model through guided questions — fighting cognitive debt by ensuring
    comprehension before approval. Phase 2: hands off to pr-review-toolkit for automated
    quality checks. Use when you want to actually understand a PR, not just rubber-stamp it.
  license: MIT
  user-invokable: true
  argument-hint: "<pr_number_or_branch_or_url>"
  compatibility: "Requires gh CLI. Phase 2 requires pr-review-toolkit installed."
  metadata:
    author: manuartero
    version: 0.1.0
    tags: [code-review, pr, cognitive-debt, pairing, comprehension]
  ---
  ```

  ---

  ## SKILL.md Procedure

  **Pre-phase (silent):** `gh pr view <PR> --json ...` + `gh pr diff <PR>`. Build internal mental model. Do not output yet.

  **Phase 1 — 5 stations (ask → wait → reveal):**

  1. **Goal** — "Based on the title/description, what problem do you think this is solving?"
  2. **Architecture** — "These are the changed files: `[list]`. Which is the entry point? Walk me through your mental model."
  3. **Decisions** — Pick 2–3 notable choices from the diff. "The author chose X. What would you have done?"
  4. **Risk** — "If you had to name one thing that could go wrong here, what would it be?"
  5. **Ownership** — "Could you explain this PR to a teammate? What are you still unsure about?" *(Don't proceed to Phase 2 until confirmed.)*

  **Phase 2:** Invoke `pr-review-toolkit`. Frame its output back to Phase 1 context ("Remember the X we discussed in station 3? The toolkit flagged...").

  **Final summary:** 3-bullet mental model recap the reviewer can paste into the PR comment.

  ---

  ## Landing Page (`docs/index.html`)

  Static, no build step. Style: openspec.dev-inspired — monospace font stack, CSS custom props for light/dark, single centered column ≤720px, one accent color
  (muted blue or amber).

  Sections: Hero + 2 CTAs → The problem → ASCII flow diagram → Install code block → Usage → The 5 stations → Footer.

  Deploy via GitHub Pages from `/docs`.

  ---

  ## Validation Checklist

  - [ ] Frontmatter has `name`, `description`, `license: MIT`, `metadata.author`, `metadata.version`
  - [ ] SKILL.md has H1 title + three sections: `When to use`, `Procedure`, `Output format`
  - [ ] No `../` path traversal in any markdown links
  - [ ] All files referenced in SKILL.md (`references/`, `assets/`) exist
  - [ ] README has install + usage instructions

  ---

  ## Implementation Order

  1. Repo skeleton (all folders + empty files)
  2. `SKILL.md`
  3. `references/` (3 files)
  4. `assets/example-session.md`
  5. `README.md`
  6. `docs/index.html`
  7. Manual validation checklist
  8. Enable GitHub Pages from `/docs`
