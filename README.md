# TrenchingView docs

User documentation for [trenchingview.io](https://trenchingview.io), built with Mintlify. Sibling of `auramarkets-docs`:
same theme, same voice, same rules.

- `docs.json` — site config and the full navigation (every page must be listed here).
- `getting-started/`, `dossier/`, `signals/`, `concepts/`, `faq/` — the pages, one `.mdx` each with `title` and `description` frontmatter.

## Rules

1. **Update the page when the feature ships.** A user-facing change on the site (the verdict card, the Project panel, the team rule, plans and credits, Videos, the radar, TrenchBrain) gets its page edited in the same session as the code.
2. **Voice:** casual and confident, never self-deprecating or apologetic. "Active development", not "things break". Second person. Bold the key terms. Tables for anything with rows and columns; `Steps`, `CardGroup`, `Note`, `Warning`, `Accordion` for structure. Each page ends on what the feature does *not* do, stated plainly.
3. **Facts from the code, not from memory.** Plan numbers come from `config/thresholds.yaml` → `pricing:` and `plans:`; check names from `docs/PATTERNS.md`; thresholds from `thresholds.yaml`.
4. **Owner-only surfaces stay out:** the Dashboards menu, `/admin`, the Telegram bot's internals.

## Checking a change

```
python - <<'EOF'
import json, os, glob, re
cfg = json.load(open("docs.json")); pages = [p for g in cfg["navigation"]["tabs"][0]["groups"] for p in g["pages"]]
print("missing:", [p for p in pages if not os.path.exists(p + ".mdx")])
files = glob.glob("**/*.mdx", recursive=True)
print("orphans:", [f[:-4].replace(os.sep, "/") for f in files if f[:-4].replace(os.sep, "/") not in pages])
EOF
```

Mintlify rebuilds on push to `main`.
