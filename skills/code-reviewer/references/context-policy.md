# Context Policy

## Collect
- PR title, body, base branch, head branch, commits, changed file names, and patch.
- Existing PR comments, review threads, and requested changes to avoid duplicate feedback.
- Nearby project docs and configs when available.
- Tests or call sites directly affected by the changed files.

## Fetch Efficiently
Start from the diff. Fetch more only when it can change the review outcome:

1. Changed file patch.
2. Full changed file only if surrounding context is missing.
3. Neighboring tests or call sites.
4. Project docs/configs.
5. Similar implementations.
6. Base or head branch files when branch comparison is necessary.

Avoid loading large folders or full repositories unless the PR is architectural and the extra context is needed.

## Ask
Ask the user only when the answer can materially change the review. Do not ask everything; first use available docs, memory, PR comments, nearby code, tests, and similar implementations.

Ask for:

- Conflicting project conventions.
- Product/business rules needed to judge correctness.
- Private context missing from docs, code, or comments.
- High-impact architecture not confirmed by docs/code, such as layer boundaries, business-rule ownership, persistence boundaries, or whether a rule should exist.
- Publishing mode has not been chosen for the current PR.

Do not ask when the uncertainty is low-impact style, can be answered by fetching a small amount of additional context, or can be safely reported as residual risk.

## Architectural Questions
Ask before turning an ambiguous architecture choice into a finding. State:

- The observed PR choice.
- Checked context and missing evidence.
- The review decision depending on the answer.
- 2-3 neutral options plus `Other`.

Use this shape:

```markdown
I noticed that <architectural choice in the PR>, and I did not find docs or similar code that clearly establish this pattern. Which direction should I use for this review?

- <Option A: project accepts this pattern.>
- <Option B: expected alternative boundary or ownership.>
- <Option C: the rule/behavior should not exist.>
- Other: <user can type the intended convention.>
```

After the user answers, apply the answer to the review. If it looks durable and reusable, use `memory-policy.md` to ask whether the generalized rule should be saved.

## Tools
Use GitHub MCP for PR data whenever possible.

Use local shell commands only when a checkout is available and they reduce uncertainty, such as targeted `rg`, tests, type checks, or linters. Do not run commands that mutate tracked files during review unless the user explicitly requests it.
