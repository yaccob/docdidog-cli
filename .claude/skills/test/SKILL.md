---
name: test
description: Run the full CLI test suite via make test
disable-model-invocation: false
---

Run the full test suite:

```bash
make test
```

This runs `test-cli.sh` and `test-watch.sh`.

- `test-cli.sh` requires Docker or Podman for rendering tests. Non-rendering tests (version, help, argument errors) always run.
- `test-watch.sh` requires Docker or Podman; skipped entirely if no container runtime is found.

Run ALL steps, do not stop on first failure. Collect all errors.

If all steps passed, write the session marker:

```bash
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) test passed" > /tmp/claude-test-passed-docdidog-cli.txt
```

If ANY step failed, do NOT write the marker — and delete it if it exists:

```bash
rm -f /tmp/claude-test-passed-docdidog-cli.txt
```

Report a complete summary with pass/fail counts.
