# Embedded Docs Reference

Globus ships parallel SDKs — `globus-sdk` on PyPI (Python) and `@globus/sdk`
on npm (JavaScript/TypeScript) — covering the same underlying APIs. Before
reaching for web search or hosted docs, check whether the relevant one is
already installed: an installed package often carries enough local reference
material (docstrings, JSDoc, type stubs) to answer common questions without
a network round-trip. Don't assume either ecosystem ships full rendered
documentation, though — check explicitly rather than guessing.

## Why use embedded docs

- **Version accuracy**: embedded docs describe the exact version the user's code runs against, so the signatures you read are the ones they can call
- **No network required**: works when docs are blocked, offline, or slow
- **Training data may be outdated**: Globus APIs move faster than any knowledge cutoff

## Lookup process

Confirm which ecosystem you're in first — see "Step 0" in `SKILL.md`, which
also covers the npm package-name trap (`@globus/sdk`, not the unscoped
`globus-sdk`).

### Step 1: Detect installation without importing/requiring

**Python** — use `importlib.metadata`, not a live `import`, so a broken or
heavy import chain can't block the check:

```bash
python3 -c "import importlib.metadata as m; print(m.version('globus-sdk'))" 2>/dev/null || echo "not installed"
```

**JS/TypeScript** — use `npm ls`, which reads `package.json`/`node_modules`
metadata without executing any package code:

```bash
# local (project) install
npm ls @globus/sdk --depth=0 --json 2>/dev/null || echo "not installed"

# global install
npm ls -g @globus/sdk --depth=0 --json 2>/dev/null || echo "not installed globally"
```

A `package.json` `dependencies` entry alone isn't sufficient — it can be
listed but not actually installed (stale lockfile, fresh clone before
`npm install`). `npm ls` is the authoritative check because it reflects
what's actually in `node_modules`.

### Step 2: Locate the install path

**Python:**
```bash
pip show globus-sdk # see the Location: field
```

**JS/TypeScript:**
```bash
node -e "console.log(require.resolve('@globus/sdk/package.json'))"
```
Note this can resolve to a parent directory in monorepos/workspaces (npm
hoisting) rather than the local `./node_modules`.

### Step 3: Know what's actually available locally — this differs by ecosystem

| Ecosystem | Source                    | How to read it                                                    | Coverage                                                       |
| --------- | ------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------- |
| Python    | Docstrings                | `python3 -c "import globus_sdk; help(globus_sdk.TransferClient)"` | Per-class/method, often solid                                  |
| Python    | Type stubs (`.pyi`)       | read directly from install path                                   | Signatures only, no prose                                      |
| Python    | Source `.py`              | read directly                                                     | Ground truth, unstructured                                     |
| JS/TS     | `README.md`               | `cat node_modules/@globus/sdk/README.md`                          | Real usage-level narrative doc                                 |
| JS/TS     | JSDoc in `.d.ts` files    | read `dist/**/*.d.ts` directly                                    | Per-symbol; often includes `@see` links to hosted service docs |
| JS/TS     | Compiled source (`dist/`) | read directly                                                     | Typed, but built output — not original commented TS source     |

The JS/TS package generally ships more useful local documentation (a real
README plus JSDoc-annotated type declarations) than the Python package
(docstrings plus a bare `.pyi` stub). Don't assume this pattern holds for
other SDKs, though — verify per-package.

### Step 4: Decide where to look next

- **Installed + local docs answer the question**: use them, no network call needed.
- **Installed but local docs are thin/missing, or not installed**: fall back to remote docs — see `references/remote-docs.md`
