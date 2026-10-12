# Handoff: Quantitative Finance Personal Website

Read this before touching anything. The owner, Jalil, is **not a programmer** — he relies entirely on whichever agent is driving this repo to explain things in plain language, to never lose his work, and to keep the live site working. Optimize for reliability over cleverness.

Repo: **https://github.com/Street-Volume/quantitative-finance-personal-website**
Live site: **https://street-volume.github.io/quantitative-finance-personal-website/** (GitHub Pages turned on this session — see §2)

## 1. Project summary

**What it is:** Jalil's personal portfolio site — a single-page resume/project showcase for "Abdul Jalil Moad, Quantitative Researcher." Header with name, title, a "Download Resume" button, and a GitHub link, followed by a featured project (a coffee commodity futures backtest with a line chart) and three smaller project cards (garment-manufacturing productivity model, startup KPI dashboards, stochastic modeling coursework).

**Stack:** a single `index.html`. React 18, ReactDOM, Recharts, and Babel standalone are all loaded from `unpkg.com` via `<script>` tags; Tailwind CSS is loaded from its play-CDN. JSX is written inline in a `<script type="text/babel">` block and compiled in-browser by Babel — **no build step, no npm, no node** (this machine typically has none of them anyway; confirm with `command -v node npm` before assuming otherwise).

**State:** small but no longer untouched. Two commits existed before this session (`7cd3b57` initial, `decfa43` added the missing `index.html`); this session added `HANDOFF.md` + the autosave hook, turned on GitHub Pages, removed the dead resume button, and labeled the backtest chart as illustrative (see §5 for exactly what's still open, and `git log` for the full commit history). Remaining in index.html:
- The featured project's chart uses **hand-written mock data**, now clearly labeled "Illustrative sample data — not live or verified results" rather than presented as real.
- There's no contact section (email, LinkedIn, etc.) beyond the GitHub link &mdash; asked, not yet answered.
- The other three project cards (garment-manufacturing model, startup KPI dashboards, stochastic modeling coursework) haven't been reviewed for accuracy/updates &mdash; not yet asked about.

This is a much smaller, less actively developed project than Jalil's other repo, `Street-Volume/inference-atlas` (an interactive study-guide site — don't confuse the two if he says "my site" ambiguously; ask which one).

## 2. GitHub Pages deployment — live as of this session

Pages was off when this repo was first found this session (`https://street-volume.github.io/quantitative-finance-personal-website/` 404'd); Jalil turned it on by hand (Settings → Pages → Deploy from a branch → `main` / `/ (root)` → Save) and it's confirmed live now. If you ever find it 404ing again (e.g. after he recreated the repo, or changed the branch), that's the one-time manual step to redo &mdash; no `git push` turns Pages on by itself.

Deployment for every change from here is just:

```bash
git add -A
git commit -m "..."
git push origin main
```

with the same "allow 30–90 seconds, then `curl` the live URL" caution as `inference-atlas`.

**Repo visibility:** this repo is already public (confirmed — needed for free GitHub Pages hosting, same reasoning as `inference-atlas`). Don't make it private.

## 3. Git identity and push auth on this VM

Set up this session (commit `edae715`):
- **Local git identity** (this repo only): `user.name "Jalil"`, `user.email "jalilmoad@gmail.com"`.
- **Push auth**: a GitHub Personal Access Token, via `git config --local credential.helper store` reading `~/.git-credentials` (`https://<token>:x-oauth-basic@github.com`) — the same credential file `inference-atlas` uses, since both repos belong to the same GitHub account. If that file is missing (fresh VM), walk Jalil through generating a new classic PAT with the `repo` scope at github.com/settings/tokens, same as documented in `inference-atlas`'s own HANDOFF.md §3.
- **This is a VM that may be wiped without warning.** Everything must live in git, pushed, not just on disk.

## 4. Autosave hook

`.claude/settings.json` (project-scoped, committed to this repo) has a `Stop` hook that runs after every Claude Code turn: commits any changes and pushes to `origin main`, surfacing a visible warning (via the Stop hook's `systemMessage` JSON output) if the push fails instead of swallowing the error silently. This is the same hardened version used in `inference-atlas` — see that repo's HANDOFF.md §5 and git history (`d4375bd`) for why it's built this way: an earlier, simpler version of this hook in `inference-atlas` failed silently when the push credential broke, which is exactly the failure mode Jalil can't self-diagnose.

The working tree should essentially never have uncommitted changes between sessions. If you find uncommitted changes, investigate rather than discarding them.

## 5. Status (as of this writing)

**Resolved this session:**
- GitHub Pages is live (§2).
- Resume button removed rather than left dead (Jalil confirmed no resume file ready) — add it back once he has one.
- Backtest chart labeled "Illustrative sample data — not live or verified results" rather than presented as real (Jalil confirmed he has no real backtest output to swap in).

**Still open, asked but not yet answered:**
- Whether to add a contact section (email/LinkedIn) beyond the one GitHub link in the header.
- Whether the other three project cards (garment-manufacturing, startup KPI dashboards, stochastic modeling) need any content updates.

**Not yet asked / lower priority:**
- No automated verification of any kind — no CI, no link checker. Low-risk for a single-file site; if it grows past one page, consider the same static link/anchor-check approach used in `inference-atlas`.

If you're picking this up fresh after a VM wipe: `git clone https://github.com/Street-Volume/quantitative-finance-personal-website.git`, then redo §3 (git identity + push auth) before anything will commit or push.
