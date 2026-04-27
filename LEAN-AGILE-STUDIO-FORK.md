# lean-agile-studio fork of apache/pouchdb

**Fork owner:** dave-jackson-dev  
**Upstream:** https://github.com/apache/pouchdb  
**Branch:** `feat/nodesqlite-adapter`  
**Purpose:** Lean-Agile Studio persistence layer — tracking `pouchdb-adapter-nodesqlite` and `pouchdb-adapter-websql-core` for the VS Code extension host.

---

## Why this fork exists

The Lean-Agile Studio VS Code extension uses PouchDB as its document store and reactive change-feed engine (ADR-001). The extension host runs Node.js ≥ 22.5 (VS Code 1.116+), which ships `node:sqlite` as a built-in module.

This fork tracks:

1. **`pouchdb-adapter-nodesqlite`** (`packages/node_modules/pouchdb-adapter-nodesqlite`) — the official first-party PouchDB adapter for `node:sqlite`, introduced in upstream PR #9223. At the time of forking this was `7.0.0-prerelease` and not yet published to npm.

2. **`pouchdb-adapter-websql-core`** (`packages/node_modules/pouchdb-adapter-websql-core`) — the core WebSQL adapter that `pouchdb-adapter-nodesqlite` depends on. The DQS bug (double-quoted string literal in `SELECT HEX("a") AS hex`) present in `7.0.0` is **fixed in `7.2.3`** (single-quoted: `SELECT HEX('a') AS hex`). This fork is on `7.2.3`.

---

## Key packages

| Package | Version | Notes |
|---|---|---|
| `pouchdb-adapter-nodesqlite` | `7.0.0-prerelease` | Not yet on npm — used via local path reference |
| `pouchdb-adapter-websql-core` | `7.2.3` | DQS bug fixed — safe to use |
| `@neighbourhoodie/websql` | `2.0.4` | WebSQL→`node:sqlite` bridge (see `dave-jackson-dev/node-websql`) |

---

## Relationship to lean-agile-studio

| File | Role |
|---|---|
| `extensions/lean-agile-script/packages/extension/src/extension/PouchDbSpikeService.ts` | Spike proof-of-concept — to be replaced by production implementation using `pouchdb-adapter-nodesqlite` |
| `docs/lean-agile-studio/architecture/adr-001-pouchdb-sqlite-adapter.md` | Architecture Decision Record — references this fork |

---

## Upstream sync

To pull upstream changes:

```bash
git fetch upstream
git merge upstream/master
```

Monitor upstream for:
- Merge of PR #9223 (`pouchdb-adapter-nodesqlite`)
- Publication of `pouchdb-adapter-nodesqlite` to npm
- Merge of PR #9233 (LevelDB deprecation)

Once `pouchdb-adapter-nodesqlite` is published to npm, replace the local path reference in `lean-agile-studio` with the published package version.