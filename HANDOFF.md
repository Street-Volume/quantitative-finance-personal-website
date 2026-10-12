# Handoff: Quantitative Finance Personal Website

Read this before touching anything. The owner, Jalil, is **not a programmer** — he relies entirely on whichever agent is driving this repo to explain things in plain language, to never lose his work, and to keep the live site working. Optimize for reliability over cleverness.

Repo: **https://github.com/Street-Volume/quantitative-finance-personal-website**
Live site: **not deployed yet** — see §2.

## 1. Project summary

**What it is:** Jalil's personal portfolio site — a single-page resume/project showcase for "Abdul Jalil Moad, Quantitative Researcher." Header with name, title, a "Download Resume" button, and a GitHub link, followed by a featured project (a coffee commodity futures backtest with a line chart) and three smaller project cards (garment-manufacturing productivity model, startup KPI dashboards, stochastic modeling coursework).

**Stack:** a single `index.html`. React 18, ReactDOM, Recharts, and Babel standalone are all loaded from `unpkg.com` via `<script>` tags; Tailwind CSS is loaded from its play-CDN. JSX is written inline in a `<script type="text/babel">` block and compiled in-browser by Babel — **no build step, no npm, no node** (this machine typically has none of them anyway; confirm with `command -v node npm` before assuming otherwise).

**State:** minimal. Two commits before this session (`7cd3b57` initial, `decfa43` added the missing `index.html`). Everything in index.html is currently **placeholder/mock**:
- There was a "Download Resume" button linking to `resume.pdf`, which didn't exist — Jalil doesn't have a resume ready yet, so the button was removed (not just left dead). Add it back once he has a file.
- The featured project's chart uses **hand-written mock data**, not a real backtest result.
- There's no contact section (email, LinkedIn, etc.) beyond the GitHub link.

This is a much smaller, less actively developed project than Jalil's other repo, `Street-Volume/inference-atlas` (an interactive study-guide site — don't confuse the two if he says "my site" ambiguously; ask which one).

## 2. Deployment — NOT yet live

Unlike `inference-atlas`, **GitHub Pages has not been turned on for this repo.** `https://street-volume.github.io/quantitative-finance-personal-website/` currently 404s. Turning it on is a **one-time manual step in the GitHub web UI** that no git push can do:

1. On github.com, open this repo → **Settings** → **Pages** (left sidebar).
2. Under "Build and deployment," **Source: Deploy from a branch**.
3. **Branch: `main`**, folder **`/ (root)`** → Save.
4. Wait ~30–60 seconds, then the site is live at the URL above.

Walk Jalil through this directly (he's a GitHub beginner) rather than assuming he's done it. Once it's on, deployment for every future change is just:

```bash
git add -A
git commit -m "..."
git push origin main
```

with the same "allow 30–90 seconds, then `curl` the live URL" caution as `inference-atlas`.

**Repo visibility:** this repo is already public (confirmed — needed for free GitHub Pages hosting, same reasoning as `inference-atlas`). Don't make it private.

## 3. Git identity and push auth on this VM

Set up this session (commit `<pending>`):
- **Local git identity** (this repo only): `user.name "Jalil"`, `user.email "jalilmoad@gmail.com"`.
- **Push auth**: a GitHub Personal Access Token, via `git config --local credential.helper store` reading `~/.git-credentials` (`https://<token>:x-oauth-basic@github.com`) — the same credential file `inference-atlas` uses, since both repos belong to the same GitHub account. If that file is missing (fresh VM), walk Jalil through generating a new classic PAT with the `repo` scope at github.com/settings/tokens, same as documented in `inference-atlas`'s own HANDOFF.md §3.
- **This is a VM that may be wiped without warning.** Everything must live in git, pushed, not just on disk.

## 4. Autosave hook

`.claude/settings.json` (project-scoped, committed to this repo) has a `Stop` hook that runs after every Claude Code turn: commits any changes and pushes to `origin main`, surfacing a visible warning (via the Stop hook's `systemMessage` JSON output) if the push fails instead of swallowing the error silently. This is the same hardened version used in `inference-atlas` — see that repo's HANDOFF.md §5 and git history (`d4375bd`) for why it's built this way: an earlier, simpler version of this hook in `inference-atlas` failed silently when the push credential broke, which is exactly the failure mode Jalil can't self-diagnose.

The working tree should essentially never have uncommitted changes between sessions. If you find uncommitted changes, investigate rather than discarding them.

## 5. Known gaps (as of this writing)

- **GitHub Pages isn't on yet** — see §2. Nothing to click through until it is.
- **No resume button right now** (removed, see §1) — add it back once Jalil has a PDF ready.
- **The backtest chart is mock data.** Ask Jalil whether he has real backtest output to swap in, or whether the mock data should stay labeled as illustrative.
- **No contact/links section** beyond the one GitHub link in the header (no email, no LinkedIn).
- **No automated verification of any kind** — no CI, no link checker, nothing. For a single-file site this is low-risk, but if it grows past one page, consider the same static link/anchor-check approach used in `inference-atlas`.

If you're picking this up fresh after a VM wipe: `git clone https://github.com/Street-Volume/quantitative-finance-personal-website.git`, then redo §3 (git identity + push auth) before anything will commit or push.
