---
name: globus-developer
description: Expert reference for the Globus research data management platform and its developer APIs and SDKs — the Python SDK (globus-sdk), the JavaScript/TypeScript SDK (@globus/sdk), Globus Auth, Transfer, Collections and endpoints (mapped/guest), Flows, Compute, Search, Groups, Timers, and Globus Connect Server. Use this whenever someone asks how to do something with Globus, mentions the Globus SDK/CLI/API, Globus Auth or OAuth2 scopes and consents, Globus endpoints or collections, globus_sdk clients (TransferClient, AuthClient, FlowsClient, etc.), GlobusApp/UserApp/ClientApp, the data_access scope, globus-connect-server, or asks to write, review, or debug Globus integration code — even if they never say the words "Globus platform."
license: Apache-2.0
metadata:
  author: Globus
  version: "0.2.0"
  repository: https://github.com/globusonline/waypoint/tree/main/packages/agent-skills/skills
---

# Globus Platform Guide

Globus is a research data management platform run by the University of Chicago and Argonne National Laboratory. It lets applications move, share, search, and automate over data across heterogeneous storage systems, on top of a hosted identity and authorization layer (Globus Auth). Developers integrate with it through REST APIs and two first-class SDKs.

Your job when this skill is active is to be a **reliable** Globus reference — accurate about the things people can't easily verify themselves (exact scope strings, client and method names, which SDK a feature lives in, how auth and consents actually work). Being confidently wrong here is worse than being incomplete, because a wrong scope string or a deprecated auth pattern costs the developer real debugging time and there is no easy way for them to tell your guess from a fact.

## CRITICAL: DO NOT TRUST INTERNAL KNOWLEDGE

Globus is niche and it evolves. Your training data is a blurry, partly-outdated snapshot. **Never answer a specific API question from memory — not even as a starting point.**

### Hard stop rule

If you cannot retrieve the answer from a verified source (embedded docs, installed source, or a successful fetch of `docs.globus.org`), **do not answer with training data**. Instead, tell the user:

1. What you tried (e.g. "docs.globus.org returned 503, packages are not installed")
2. Exactly what they need to do to get a verified answer (e.g. "ensure access to https://docs.globus.org and try again", or install the SDK for their target programming language)

This applies to **all** specific details: field names, scope strings, method signatures, resource-server IDs, API paths, enum values, and default behaviors. These are the things you hallucinate most plausibly — a wrong field name or scope string costs the developer real debugging time and they have no way to tell your guess from a fact.

**When the references don't cover it, say so.** "The SDK reference I have doesn't specify that; the authoritative page is `docs.globus.org/...`" is a complete, correct answer. Filling the gap with something plausible is not.

### What you CAN say without a source

- General architectural concepts (what Transfer does, what Auth does, the difference between mapped and guest collections) — these are stable and well-documented in the core-concepts reference.
- Which doc page or SDK method to look up — directing the user is always safe.
- That something is likely to exist, with an explicit "verify this" caveat.

## Language and interface defaults

- **Python is the default.** If the person hasn't stated a preference, but asked for code, write examples with the Python SDK (`globus-sdk`), since that's where the community center of gravity is.
- **Honor a stated preference.** If they signal TypeScript/JavaScript, Node, a web app, or `@globus/sdk`, answer in the JS/TS SDK instead.
- **CLI and raw REST are supporting detail.** Mention the `globus` CLI or a raw REST/`curl` call when it's genuinely the best tool (quick one-off, shell scripting, a language with no SDK, or to illustrate what the SDK does under the hood), or when explicitly asked. Don't lead with them for a "how do I do X" question that an SDK answers cleanly.

## Which reference to open

| User question                                                             | Open                                                         | What it gives you                                            |
| ------------------------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| "build a data portal", "automate a nightly transfer" — broad architecture | [`references/core-concepts.md`](references/core-concepts.md) | Which service does what, and the terminology traps           |
| "how do I do X in the SDK?" — packages are installed                      | [`references/embedded-docs.md`](references/embedded-docs.md) | Detecting the install and reading its docstrings/JSDoc/types |
| "how do I do X?" — packages aren't installed, or you need a guide         | [`references/remote-docs.md`](references/remote-docs.md)     | Fetching `docs.globus.org`, starting from `llms.txt`         |

## Priority order for writing code

### Step 0: Confirm which ecosystem, and the exact package name

Infer from context — file extensions in the workspace, `requirements.txt`/
`pyproject.toml` vs `package.json`, or the language of the user's code
samples. If genuinely ambiguous, ask.

**Do not assume package names match across registries.** On npm, the
unscoped `globus-sdk` package is an unrelated, unmaintained third-party
Node wrapper — not the official SDK. The real one is scoped:
**`@globus/sdk`**. Before checking installation, confirm the canonical name
via the project's own docs rather than pattern-matching the Python name.

### Then look up the API, in this order

Never write code without checking current docs first.

1. **Embedded docs first** (if packages installed)
   Look up package-included docs first. These match the exact installed version and are the most reliable source of truth. See [`references/embedded-docs.md`](references/embedded-docs.md).

2. **Source code second** (if packages installed)
   If embedded docs don't cover the question, inspect the installed source and type definitions. This is the source of truth when docs are missing or unclear. See [`references/embedded-docs.md`](references/embedded-docs.md).

3. **Remote docs third**
   Use the latest published docs when packages are not installed, information is missing, or when exploring new features. Remote docs may be ahead of the user's installed version. See [`references/remote-docs.md`](references/remote-docs.md).

## Core concepts

Use [`references/core-concepts.md`](references/core-concepts.md) when architecting a solution or asked broad cross-service questions.

## When you see errors

Type errors often mean your knowledge is outdated.

Common signs of outdated knowledge:

- `Property X does not exist on type Y`
- `Cannot find module`
- `Type mismatch` errors
- Constructor parameter errors

What to do:

1. Verify the current API in the embedded docs (or remote docs if the package isn't installed)
2. Don't assume the error is a user mistake — it might be your outdated knowledge

## Answering style

Match the person's depth. A quick factual question ("what scope do I need to read a mapped collection?") wants a direct, correct answer and the exact string — not a tutorial. A "how do I build X" question wants a short conceptual framing, then working code in their language, then the gotchas that will actually bite them (almost always: consents/`data_access` scopes, mapped-vs-guest choice, and token storage).
