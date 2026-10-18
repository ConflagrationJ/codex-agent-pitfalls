# Codex Agent Pitfalls

Field notes from building Codex skills, subagent workflows, browser extensions, and release pipelines. This is not an official OpenAI document. It records failures that looked like model problems but were often packaging, lifecycle, or verification problems.

## Read this first

- Install location is not the same as activation policy.
- A session does not always see a skill installed after the session started.
- Static tests, mock behavior tests, and real browser behavior are three different claims.
- A multi-agent team needs a coordinator, explicit scopes, and independent review; more agents is not automatically better.
- Browser automation must distinguish extension reload, page refresh, background-worker generations, and popup user activation.
- One authority should own concurrency; competing local files are not a transaction.
- Never publish private paths, tokens, customer data, or unverified claims.

## Contents

- [Chinese field guide](docs/CODEX-AGENT-PITFALLS.zh-CN.md)
- [English field guide](docs/CODEX-AGENT-PITFALLS.en.md)

## Scope

The examples are intentionally generic. They apply to Codex skills, coding subagents, local browser extensions, automation heartbeats, and GitHub release workflows. Product-specific rules and upstream schemas always take precedence over this repository.

## Contributing lens

When adding a pitfall, include:

1. symptom;
2. root cause;
3. minimal fix;
4. how to verify it without overclaiming;
5. what remains unverified.

## License

MIT. See `LICENSE`.
