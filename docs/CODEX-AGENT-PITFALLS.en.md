# Codex / Agent Workflow Pitfalls

> Version `v0.1.0`, recorded 2026-10-18. Applies to Codex skills, subagents, browser extensions, automations, and GitHub releases. This is not official documentation; upstream schemas and product policy win.

## 0. Pick the right layer

| Need | Shape |
|---|---|
| Prompting, workflow, templates, role constraints | Skill |
| Real tools, lifecycle, background services | Plugin/extension or service |
| Scheduled checks, reminders, polling | Automation / heartbeat |
| Independent execution, review, parallel reads | Subagent |
| Capability the host does not provide | Do not fake it in a skill; add a tool or degrade |

**P1 — A skill is not a runtime.** A skill is loadable guidance and resources. Automatic invocation is routing eligibility, not guaranteed execution. Use a host plugin, automation, or service for background work or side effects.

## 1. Skill installation and invocation

**P2 — Duplicate skill names break resolution.** Keep backups outside `$CODEX_HOME/skills`; exactly one `SKILL.md` may own a given `name`.

**P3 — A session may not see a newly installed skill.** Skill metadata is loaded when a session starts. Open a new chat after installation.

**P4 — The implicit-invocation flag is not a global “always run” switch.** `agents/openai.yaml` controls whether the skill is eligible for automatic routing; the model still selects it from the task and description.

**P5 — “Automatic” does not mean “mandatory.”** State boundaries in the description. For mandatory hooks or side effects, use the host's runtime mechanisms.

## 2. Subagent workflow

**P6 — Do not let the author approve their own work.** Use separate execution units. High-risk work gets a plan red team before implementation.

**P7 — A review without a verdict is not a gate.** Require severity, trigger, consequence, evidence, minimal fix, and pass/conditional/fail.

**P8 — Direct peer chat can destroy review independence.** Default to file handoffs; reviewers see raw contracts, diffs, and logs before author summaries.

**P9 — Bind every task to a revision and write scope.** Serialize shared files and invalidate evidence when the revision drifts.

**P10 — Treat provider/proxy 502s as infrastructure failures.** Preserve the handoff and retry or hand off; do not infer that the implementation is correct or incorrect.

## 3. Browser and extension work

**P11 — Page refresh is not extension reload.** Content scripts, background workers, manifests, permissions, and installed packages have different lifetimes.

**P12 — Source and runtime stage must match.** Validate manifest references and hashes. Tests should load the actual extension directory, not only a root helper.

**P13 — `window.open` may depend on user activation.** For deterministic navigation, use a fixed tab managed by the extension background and bind it to a task ID.

**P14 — DOM presence is not success.** Count unique jobs, relevance, complete text, duplicates, noise, blocked pages, and truncation separately.

**P15 — Make tab failures visible.** Distinguish auto-close, manual close, load error, and replacement; notify the runner immediately.

## 4. Concurrency, locks, and data integrity

**P16 — Do not use unlink-then-create as a lock.** Let one server own a lease, or use a proven atomic OS primitive. A file is an advisory signal, not automatically a transaction.

**P17 — Status files need TTL, token, generation, and liveness checks.** Stale files must not trigger false failure or success.

**P18 — Text length is not completeness.** Use structure, source, tail markers, truncation words, and security-page checks.

## 5. Verification and evidence

**P19 — Static, mock, and real-environment tests are different claims.** Report each layer separately. A static pass is not a browser validation.

**P20 — Use a reproducible revision.** Prefer `git rev-parse` or a deterministic worktree snapshot, normalize line endings, and ignore caches.

## 6. Release engineering

**P21 — Browser login is not Git CLI authentication.** Browser cookies, Git Credential Manager, and SSH keys are different channels. Never commit a PAT.

**P22 — Separate release ownership for different runtimes.** Use separate repositories or namespaced tags (`core-v*`, `host-v*`) and pin compatibility.

**P23 — A tag without evidence is incomplete.** Release notes need compatibility, commit, verification level, known limitations, and rollback instructions.

## 7. Symptom index

| Symptom | Check first |
|---|---|
| Skill exists but `$name` fails | duplicate names, new chat |
| Automatic selection does not happen | policy file, description, session load time |
| Browser keeps old code | refresh vs extension reload vs background restart |
| Mock tests pass but real browser fails | actual stage, permissions, tab lifecycle |
| No `NAV open` occasionally | popup activation, fixed tab, tab lifecycle |
| Two workers run together | one authoritative server lease |
| Table numbers drift | revision, line endings, cache, untracked files, definitions |
| GitHub web login but push fails | Git credential, SSH, remote |

## 8. Minimum acceptance checklist

1. Unique skill name, visible in a new chat.
2. Auto and explicit invocation each verified once.
3. Subagents have task IDs, base revisions, write scopes, and independent review.
4. Extension source/stage/manifest agree and permissions are active.
5. Real browser shows open, stored, closed or failure events.
6. One authoritative lease owns concurrency.
7. Release contains compatibility, evidence, and rollback notes.

## Version history

- `v0.1.0` (2026-10-18): initial public field guide derived from local skill, subagent, extension, lock, and release work; private data and product details removed.
