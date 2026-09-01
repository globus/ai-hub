# Remote Docs Reference

How to look up current documentation from https://docs.globus.org when local packages aren't available or you need conceptual guidance.

**Use this when:**

- Globus packages aren't installed locally
- You need conceptual explanations or guides
- You want the latest documentation (may be ahead of installed version)

## Documentation site structure

Globus docs are available at **https://docs.globus.org** and organized as follows:

- **Getting Started**: concepts, features, and implementation details
- **Reference**: service and agent (installed software) reference documentation
- **Solutions & Guides**: practical approaches for using Globus in research
  environments, integrating with other platforms, and building science gateways

## Lookup workflow

### 1. Start from the index

Fetch `https://docs.globus.org/llms.txt`. It is an agent-friendly map of the
whole documentation set — organization, available topics, and direct links —
so it tells you which page to read next without guessing at paths.

### 2. Or construct the URL directly

Documentation follows predictable patterns:

- Service API reference: `https://docs.globus.org/api/{service}/` — e.g. `https://docs.globus.org/api/groups/`
- Guides: `https://docs.globus.org/guides/{topic}/` — e.g. `https://docs.globus.org/guides/auth-user-guide/`

Prefer step 1 when you aren't sure a path exists.

### 3. Cite what you used

Give the person the URL you pulled the answer from, so they can confirm an
exact scope string or signature themselves.

## When to use remote vs embedded docs

| Situation                  | Use                                                 |
| -------------------------- | --------------------------------------------------- |
| Packages installed locally | **Embedded docs** (matches their installed version) |
| Packages not installed     | **Remote docs**                                     |
| Need conceptual guides     | **Remote docs**                                     |
| Need exact API signatures  | **Embedded docs** (if available)                    |
| Exploring new features     | **Remote docs** (may be ahead of installed version) |
| Need working examples      | **Both** (embedded for types, remote for guides)    |

1. **Start from `llms.txt`** when unsure about URL structure, rather than guessing a path
2. **Prefer embedded docs** when packages are installed (version accuracy)
3. **Use remote docs** for conceptual understanding and guides
4. **Combine both** for comprehensive understanding

## Hard stop: if the fetch fails

If `docs.globus.org` returns an error (4xx, 5xx, timeout, or network block), **do not answer with training data**. Instead:

1. Tell the user what failed and why (e.g. "docs.globus.org returned 503" or "blocked by network policy")
2. Give them the exact URL to check themselves once access is restored
3. If packages are installed, pivot to embedded docs (see `references/embedded-docs.md`) — that is a verified fallback
4. If neither remote docs nor embedded docs are available, stop and say so explicitly

A failed fetch is not a green light to recall from memory. The fetch exists precisely because memory is unreliable for Globus specifics.
