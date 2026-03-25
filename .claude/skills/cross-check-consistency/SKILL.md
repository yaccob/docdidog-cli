---
name: cross-check-consistency
description: Cross-check CLI options, README, and tests for consistency
disable-model-invocation: false
---

Cross-check that the CLI, its documentation, and its tests are consistent.

## What to check

### 1. CLI options: --help ↔ README ↔ tests

Run the CLI and capture its help output:

```bash
./bin/docdidog --help 2>&1
./bin/docdidog-serve --help 2>&1
```

Then:
- Compare every option in `--help` against `README.md` — is each option documented?
- Compare every option documented in `README.md` against `--help` — no phantom options.
- Check `test/test-cli.sh` — is each option exercised by at least one test?

### 2. Version consistency

Check that the version number is identical in:
- `bin/docdidog` (`VERSION=`)
- `bin/docdidog-serve` (`VERSION=`)
- `README.md` (if mentioned)

### 3. Docker image reference consistency

The image name `yaccob/docdidog` appears in:
- `test/test-cli.sh` (server startup)
- `test/test-watch.sh` (server startup)
- `README.md`

Check all three use the same image name.

### 4. Server URL default consistency

The default server URL (`http://localhost:3300`) must match across:
- `bin/docdidog` (`DEFAULT_SERVER=`)
- `README.md` usage examples
- Tests (port used in test startup)

### 5. Error messages: code ↔ tests

For each error path in `bin/docdidog` (no input, file not found, unknown format, etc.):
- Is there a test that verifies the exact error message text?
- Does the test check for non-zero exit code?

## Output

Report findings as:
- **Errors** — factually wrong (must fix)
- **Stale** — phantom docs or tests for removed features (must fix)
- **Gaps** — undocumented or untested behavior (should fix)
- **Suggestions** — clarity improvements (nice to have)

For each finding: source file, relevant content, what needs to change.

After the report, ask whether to apply fixes.
