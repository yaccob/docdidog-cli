---
name: release
description: Cut a new CLI release — bump version, run tests, create GitHub release with artifacts
disable-model-invocation: false
---

Cut a new release of the docdidog CLI.

## Steps

### 1. Pre-flight checks

- Verify the test marker exists: `/tmp/claude-test-passed-docdidog-cli.txt`. If not, stop and tell the user to run `/test` first.
- Verify the working directory is clean (`git status`). If there are uncommitted changes, stop.
- Ask the user: **"What is the new version number?"** (e.g. `1.8.0`). Wait for the answer before proceeding.

### 2. Version bump

Update the version in:

- `bin/docdidog`: `VERSION="<new>"`
- `bin/docdidog-serve`: `VERSION="<new>"`

Also check `README.md` for any hardcoded version mentions and update them.

### 3. Run tests again

```bash
make test
```

If tests fail, stop. Do not proceed.

### 4. Commit and tag

```bash
git add bin/docdidog bin/docdidog-serve README.md
git commit -m "chore: release v<new>"
git tag v<new>
```

### 5. Handoff to user

The following steps require credentials and explicit user confirmation — report them clearly:

```bash
# Push git tag
git push origin v<new>

# Create GitHub release and attach CLI scripts as release artifacts
gh release create v<new> \
  bin/docdidog \
  bin/docdidog-serve \
  --title "v<new>" \
  --notes "..."
```

### 6. After GitHub release: Homebrew tap

Once the GitHub release exists and the archive URL is available, the Homebrew formula at `../homebrew-tap/Formula/docdidog.rb` needs to be updated:

1. Compute the SHA256 of the release archive:
   ```bash
   curl -sL https://github.com/yaccob/docdidog-cli/archive/refs/tags/v<new>.tar.gz | shasum -a 256
   ```
2. Update `url`, `sha256`, and `version` in the formula.
3. Commit and push the tap.

Report the expected formula changes to the user — do not push the tap without confirmation.
