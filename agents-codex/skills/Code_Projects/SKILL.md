---
name: Code_Projects
description: "Check for locally cloned repos and projects before fetching from the web or cloning. Use PROACTIVELY when any repo URL or project name is mentioned."
---

# Code Projects — Check Before Fetching

Use `$Code_Projects` in chat. The skill name is `Code_Projects`.

Before using WebFetch, WebSearch, or `git clone` on any repo, check whether it's already available locally. Full reference: `Pages/Dev Environment — Project Layout.md`.

---

## Layout Convention

```
~/Projects/
  remote/           # clones of remote repos — mirror the web URL path
    github.com/
      <owner>/
        <repo>/
  local/            # local-only projects (no remote)
    <name>/
```

**Remote repo rule:** strip `https://` from the URL, prepend `~/Projects/remote/`.

```
https://github.com/breferrari/obsidian-mind
→ ~/Projects/remote/github.com/breferrari/obsidian-mind

https://github.com/pyinfra-dev/pyinfra
→ ~/Projects/remote/github.com/pyinfra-dev/pyinfra
```

---

## Known Local Projects

| Path | Description |
|------|-------------|
| `~/Projects/local/Infrastructure` | Personal homelab — pyinfra, Nix, Terraform → [[Projects/indefinite/personal-infrastructure/_index_]] |
| `~/Projects/local/CareerPrep` | Career prep materials → [[Projects/archived/career-prep/_index_]] |
| `~/Projects/local/ableton-scripts` | Custom MIDI Remote Scripts for Ableton |
| `~/Projects/remote/github.com/breferrari/obsidian-mind` | obsidian-mind vault template — inspiration for this vault |
| `~/Projects/remote/github.com/pyinfra-dev/pyinfra` | pyinfra source |
| `~/Projects/remote/github.com/cristian-caloghera/homelab` | Reference homelab setup |

---

## Check Before Fetching

```bash
# Check if a repo is already cloned
ls ~/Projects/remote/github.com/<owner>/<repo>/ 2>/dev/null || echo "NOT CLONED"

# Check a local project
ls ~/Projects/local/<name>/ 2>/dev/null || echo "NOT FOUND"

# See everything available
ls ~/Projects/remote/github.com/
ls ~/Projects/local/
```

---

## Decision Tree

```
User mentions a repo URL or project name
  │
  ├─ Check local path first (ls command above)
  │
  ├─ EXISTS → read from local path directly
  │           no network needed
  │
  └─ NOT FOUND → choose clone destination:
        │
        ├─ Will reuse across sessions → clone to ~/Projects/remote/<host>/<owner>/<repo>/
        │
        └─ Throwaway (one-off inspection) → mktemp -d
```

**Never clone to:** cwd, home dir, or inside the vault folder.

---

## Cloning (when local copy is absent)

```bash
# Persistent clone (reusable)
git clone https://github.com/<owner>/<repo> ~/Projects/remote/github.com/<owner>/<repo>

# Throwaway
tmpdir=$(mktemp -d)
git clone https://github.com/<owner>/<repo> "$tmpdir"
# ... work ...
rm -rf "$tmpdir"
```

---

## When to Use This Skill

- Any mention of a GitHub URL or repo name
- User says "look at X repo" or "check how Y project does Z"
- Before calling WebFetch on a raw GitHub URL
- Before suggesting `git clone` to the user
