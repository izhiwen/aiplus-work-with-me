# Profile Recovery and Backup Guide

Local-only recovery and backup guidance for an installed
`AiPlus-Work-with-Me` profile.

> Paths in this guide use `$PROFILE_SRC` for your local fork checkout and
> `$PROFILE_INSTALLED` for the AiPlus-managed install directory. Set them
> once and the recipes below copy verbatim:
>
> ```bash
> export PROFILE_SRC="$HOME/Projects/AiPlus-Work-with-Me"   # your fork checkout
> export PROFILE_INSTALLED="$HOME/.config/aiplus/profiles/AiPlus-Work-with-Me"
> ```

## Backup Strategy

### 1. Source Repository (Primary Backup)

The profile source in `$PROFILE_SRC` is the source of truth.

```bash
cd "$PROFILE_SRC"
git status
git diff --check
```

Keep this directory in version control or regular backups.

### 2. Installed Profile Backup

The installed profile lives at `$PROFILE_INSTALLED`.

```bash
tar -czf ~/aiplus-profile-backup-$(date +%Y%m%d).tar.gz "$PROFILE_INSTALLED"
```

### 3. Before Reinstall Checklist

Before running `aiplus profile install`:

- [ ] Back up current installed profile
- [ ] Check if you have custom edits in installed profile not in source
- [ ] Document any drift between source and installed

## Recovery Scenarios

### Scenario 1: Corrupted profile.toml

```bash
cp "$PROFILE_SRC/profile.toml" "$PROFILE_INSTALLED/profile.toml"
aiplus profile doctor AiPlus-Work-with-Me
```

### Scenario 2: Missing `.aiplus/` Directory in Installed Profile

```bash
mkdir -p "$PROFILE_INSTALLED/.aiplus/memory"
cp "$PROFILE_SRC/.aiplus/manifest.json"      "$PROFILE_INSTALLED/.aiplus/"
cp "$PROFILE_SRC/.aiplus/AGENTS.aiplus.md"   "$PROFILE_INSTALLED/.aiplus/"
cp "$PROFILE_SRC/.aiplus/REFRESH_PROMPT.txt" "$PROFILE_INSTALLED/.aiplus/"
aiplus profile doctor AiPlus-Work-with-Me
```

### Scenario 3: Sync Policy Errors

```bash
cp "$PROFILE_SRC/sync/policy.toml" "$PROFILE_INSTALLED/sync/policy.toml"
python3 -c "import tomllib; tomllib.load(open('$PROFILE_INSTALLED/sync/policy.toml', 'rb'))"
```

### Scenario 4: Session Opt-Out

No file changes needed. Simply say in the session:

- "本次忽略我的偏好"
- "关闭 work-with-you"
- "只看项目规则"
- (or English equivalents)

The profile is ignored for the current session only. Next session reverts to normal.

### Scenario 5: Full Profile Reset

```bash
# 1. Backup current
mv "$PROFILE_INSTALLED" "${PROFILE_INSTALLED}-backup-$(date +%Y%m%d)"

# 2. Reinstall from source
aiplus profile install AiPlus-Work-with-Me --user --yes

# 3. Manually sync supplemental files (until CLI supports full sync)
cp -r "$PROFILE_SRC/USER.md"     \
      "$PROFILE_SRC/MEMORY.md"   \
      "$PROFILE_SRC/preferences" \
      "$PROFILE_SRC/sync"        \
      "$PROFILE_INSTALLED/"

# 4. Copy .aiplus/ structure
cp -r "$PROFILE_SRC/.aiplus" "$PROFILE_INSTALLED/"

# 5. Verify
aiplus profile doctor AiPlus-Work-with-Me
```

## Drift Detection

Compare source and installed profiles:

```bash
#!/bin/bash
SOURCE="${PROFILE_SRC:-$HOME/Projects/AiPlus-Work-with-Me}"
INSTALLED="${PROFILE_INSTALLED:-$HOME/.config/aiplus/profiles/AiPlus-Work-with-Me}"

echo "=== File Presence Drift ==="
for file in profile.toml AGENTS.profile.md USER.md MEMORY.md; do
  if [ -f "$SOURCE/$file" ] && [ -f "$INSTALLED/$file" ]; then
    echo "PASS $file (both present)"
  elif [ -f "$SOURCE/$file" ] && [ ! -f "$INSTALLED/$file" ]; then
    echo "DRIFT $file (missing in installed)"
  fi
done

echo ""
echo "=== Directory Drift ==="
for dir in preferences sync docs .aiplus; do
  if [ -d "$SOURCE/$dir" ] && [ -d "$INSTALLED/$dir" ]; then
    echo "PASS $dir/ (both present)"
  elif [ -d "$SOURCE/$dir" ] && [ ! -d "$INSTALLED/$dir" ]; then
    echo "DRIFT $dir/ (missing in installed)"
  fi
done
```

## What NOT to Publish

Never include in public assets or backups sent externally:

- Filled-in `USER.md` content
- Filled-in `MEMORY.md` content
- Filled-in `preferences/` content
- `sync/projects.toml` with real project paths
- `secret-aliases.tsv` with your real vault namespace
- `.aiplus/memory/` contents
- Any real memory records

## Known Limitations

1. `aiplus profile install` does not automatically sync `.aiplus/` directory or supplemental files.
2. `user context` command outputs full `USER.md` content (local only, but be aware).

## Recovery Checklist

- [ ] Source repo accessible
- [ ] Backup tarball available (if created)
- [ ] `profile doctor AiPlus-Work-with-Me` passes
- [ ] No secret values exposed in output
- [ ] Global config untouched
