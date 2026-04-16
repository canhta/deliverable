---
name: deliverable-upgrade
description: >-
  Use when the user asks to upgrade, update, or get the latest
  version of deliverable. Detects install type (git, npx, vendored), checks for
  updates, runs the upgrade, and shows what changed.
---

# Upgrade Deliverable

Upgrade deliverable to the latest version and show what's new.

## When to use

- "upgrade deliverable", "update deliverable", "get latest version"
- "check for updates"

## Flow

### Step 1: Detect install type

```bash
INSTALL_DIR=""
INSTALL_TYPE=""

if [ -d "$HOME/.claude/skills/deliverable/.git" ]; then
  INSTALL_TYPE="global-git"
  INSTALL_DIR="$HOME/.claude/skills/deliverable"
elif [ -d ".claude/skills/deliverable/.git" ]; then
  INSTALL_TYPE="local-git"
  INSTALL_DIR=".claude/skills/deliverable"
elif [ -d ".agents/skills/deliverable" ]; then
  INSTALL_TYPE="npx-skills"
  INSTALL_DIR=".agents/skills/deliverable"
elif [ -d "$HOME/.claude/skills/deliverable" ]; then
  INSTALL_TYPE="global-vendored"
  INSTALL_DIR="$HOME/.claude/skills/deliverable"
elif [ -d ".claude/skills/deliverable" ]; then
  INSTALL_TYPE="local-vendored"
  INSTALL_DIR=".claude/skills/deliverable"
fi

echo "Install type: $INSTALL_TYPE"
echo "Location: $INSTALL_DIR"
```

If not found, tell the user to install first: `npx skills add canhta/deliverable`

### Step 2: Check for updates

```bash
LOCAL_VER=$(cat "$INSTALL_DIR/VERSION" 2>/dev/null || echo "unknown")
REMOTE_VER=$(curl -sf --max-time 5 "https://raw.githubusercontent.com/canhta/deliverable/main/VERSION" 2>/dev/null || echo "")
echo "Local: $LOCAL_VER"
echo "Remote: $REMOTE_VER"
```

If `LOCAL_VER` equals `REMOTE_VER`, tell user: _"Already on the latest version (v`LOCAL_VER`)."_ and stop.

If curl fails, fall back to git-based check for git installs:

```bash
cd "$INSTALL_DIR"
git fetch origin
LOCAL=$(git rev-parse HEAD)
REMOTE=$(git rev-parse origin/main)
echo "LOCAL=$LOCAL"
echo "REMOTE=$REMOTE"
```

### Step 3: Upgrade

Ask the user before proceeding: _"Update available. Upgrade now?"_

**For git installs (global-git, local-git):**

```bash
cd "$INSTALL_DIR"
OLD_HEAD=$(git rev-parse HEAD)
git pull origin main
NEW_HEAD=$(git rev-parse HEAD)
```

Show what changed:

```bash
git log --oneline "$OLD_HEAD".."$NEW_HEAD"
```

**For npx-skills:**

```bash
npx skills add canhta/deliverable --yes
```

**For vendored installs (global-vendored, local-vendored):**

```bash
TMP_DIR=$(mktemp -d)
git clone --depth 1 https://github.com/canhta/deliverable.git "$TMP_DIR/deliverable"
mv "$INSTALL_DIR" "$INSTALL_DIR.bak"
mv "$TMP_DIR/deliverable" "$INSTALL_DIR"
rm -rf "$INSTALL_DIR/.git" "$INSTALL_DIR.bak" "$TMP_DIR"
```

### Step 4: Confirm

Tell the user:

- Install type and location
- What changed (commit log for git, version bump for vendored)
- _"Upgrade complete. All 8 skills updated."_
