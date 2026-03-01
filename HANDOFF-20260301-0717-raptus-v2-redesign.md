# HANDOFF — Raptus v2 "Therapy Session" Redesign

episode: EP-RAPTUS-V2

## Polaris
> Convert casual visitors into Raptus buyers via a page that *feels* like the film — clinical, seductive, escalating — while doubling as a calling card for Fertility producer outreach.

## Status: DEPLOYED — Live at horizonlinefilms.com/raptus

All work complete. Nothing uncommitted that's broken. Changes deployed to Cloudflare Pages but **not yet committed to git**.

---

## What Was Built (This Session)

Complete rebuild of the Raptus film page from a standard indie brochure into a "therapy protocol that goes wrong" — the deeper you scroll, the more intimate and controlling the page becomes.

### Files Changed

| File | Action | Lines | Notes |
|------|--------|-------|-------|
| `src/layouts/RaptusBase.astro` | Rewritten | 139 | New design system, nav, footer, scanline overlay |
| `src/pages/raptus.astro` | Rewritten | 922 | All 9 sections + sequel teaser (4 iterations) |
| `src/pages/raptus/screening.astro` | **New** (untracked) | 222 | Industry code-gated screening room |
| `src/pages/raptus/watch.astro` | Rewritten | 245 | Retheemed to match v2 red palette |
| `src/pages/raptus/fr.astro` | Deleted | — | i18n pages removed |
| `src/pages/raptus/es.astro` | Deleted | — | i18n pages removed |
| `src/pages/raptus/ja.astro` | Deleted | — | i18n pages removed |

### Git State
- **Branch:** `main`
- **Last commit:** `d77dc79` — `[RAPTUS] v1 identity separation`
- **Uncommitted:** All v2 changes (modified RaptusBase, raptus, watch; deleted fr/es/ja; new screening.astro)
- **screening.astro is UNTRACKED** — needs `git add` before commit

---

## Design System

| Token | Value | Usage |
|-------|-------|-------|
| Black | `#0a0a0a` | Page background |
| Obsidian | `#111` | Card backgrounds, section alternation |
| Blood Red | `#c41e1e` | Accent, CTAs, protocol labels, links |
| Chrome | `#c8c8c8` | Body text |
| Skin | `#e8d5c4` | Hover states, director confession accent |
| Display font | Space Grotesk 300-700 | Headings, body copy |
| Mono font | JetBrains Mono 300-500 | Protocol labels, clinical elements, nav |
| Texture | CSS scanline overlay on `body::after` | Faint CRT feel |
| Motion | CSS-only | Boot-up, cursor flicker, heartbeat pulse, status blink |

---

## Page Structure (raptus.astro — 9 Sections + Sequel)

### 1. Consent Gate (fixed overlay)
- Clinical consent screen: "By proceeding, you acknowledge this session may provoke strong reactions..."
- "I CONSENT TO THIS SESSION" button dismisses (vanilla JS, ~15 lines)
- `sessionStorage` remembers consent so it doesn't re-show on revisit
- CSS: `position: fixed; inset: 0; z-index: 10000; background: #0a0a0a`

### 2. Live Signal — "SESSION LOG — PATIENT RESPONSES"
- **18 curated review quotes** from Film Threat, Letterboxd, IMDb, Rotten Tomatoes
- Each quote tagged with a clinical **diagnosis** (dx): ELEVATED RESPONSE, COGNITIVE DISSONANCE, DISORIENTATION, SYSTEM OVERLOAD, MORAL CONFUSION, etc.
- Disclaimer: "Discomfort is not a side effect. It is the treatment."
- **Quote ordering deliberately interleaved** by source — no same-author adjacency across the 4-column grid
- Sources: Film Threat (Bobby LePire 8.5/10), Letterboxd (Billy Ray Brewton ★★★½, eXistenZZ ★★★½, Nikola Gocic ★★½, joshrowley ★★½), IMDb (7/10 featured review, individual reviews), RT (audience score)
- Grid: `repeat(4, 1fr)` desktop → `repeat(2, 1fr)` tablet → `1fr` mobile

### 3. Session Frames
- 5 film stills as "SESSION 01" through "SESSION 05"
- Heartbeat pulse CSS animation on frame borders
- Images reference `{base}/images/session-01.jpg` through `session-05.jpg`
- **NOTE: These images may not exist yet** — the section will render but frames may be empty

### 4. Full Briefing
- YouTube trailer embed: `https://www.youtube.com/embed/bQD0oJBSf7o`
- Post-trailer CTA: "The session has started. You can't unsee it."

### 5. Director's Confession
- Personal letter from Bennet (first-person, italic, raw tone)
- NOT a bio — a confession about why the film exists
- Red border-left accent, skin-tone text (`#e8d5c4`)
- Signed: `— Bennet de Brabandere, Writer/Director`

### 6. Session Windows (Visual Only — Countdown Deferred)
- 3 bonus content cards: "Session Transcript" (screenplay PDF), "The Clinical Recordings" (BTS), "Patient Zero Interview" (cast Q&A)
- Each shows "ACCESS CLOSES: TBD" placeholder
- Tagline: "The film stays. The sessions don't."
- **Live countdown timers are DEFERRED** to a future iteration

### 7. Choose Your Session (Purchase Cards)
- Rent: $2.99 — "One night. One protocol. 48-hour streaming access."
  - Gumroad link: `https://horizonline2.gumroad.com/l/raptus-rent`
- Own: $6.99 (RECOMMENDED) — "The session remembers you. Download & stream forever."
  - Gumroad link: `https://horizonline2.gumroad.com/l/raptus-own`
- Also on: Apple TV (US, UK, Germany)
- Gumroad JS loaded via `<script src="https://gumroad.com/js/gumroad.js">`
- Buttons have `gumroad-button` class for overlay trigger

### 7.5. Sequel Teaser — "NEXT SESSION"
- Title: `RAPTUS: HARD WIRED`
- Tagline: "The protocol was never deleted. It evolved."
- Status: `IN DEVELOPMENT`
- Pulsing red dot animation

### 8. Producer Hint (Industry Screening Link)
- Card-style button linking to `/raptus/screening`
- "SCREENING PROTOCOL" label in red + "Industry & press — enter access code →"
- Bordered card with subtle red tint, hover brightens
- **Updated this session** to be more visible (was nearly invisible `#333` text before)

### 9. Join the Protocol
- Email signup: POSTs to Supabase `axuaxxvnbyngxupcikmi.supabase.co/rest/v1/raptus_signups`
- Supabase anon key hardcoded in client JS (read-only public key, acceptable for inserts)
- Social links: Letterboxd, IMDb, Rotten Tomatoes (actual URLs)

---

## Screening Room (screening.astro)

- URL: `/raptus/screening`
- Client-side code validation against hardcoded map:
  ```javascript
  var codes = {
    'FERTILITY2026': 'https://player.vimeo.com/video/PLACEHOLDER',
    'RAPTUS-SCREEN': 'https://player.vimeo.com/video/PLACEHOLDER',
    'HORIZON2026': 'https://player.vimeo.com/video/PLACEHOLDER'
  };
  ```
- **Vimeo URLs are PLACEHOLDER** — need real video IDs when ready
- Not secure (codes visible in page source) — acceptable for convenience gating
- After valid code: hides form, shows Vimeo embed + film metadata + contact email

---

## Watch Page (watch.astro)

- URL: `/raptus/watch`
- Retheemed from gold to red palette matching v2
- Same Gumroad links as main page
- Therapy-voice copy: "SESSION READY", "Begin Session", "Own the Protocol"
- YouTube trailer embed
- "Also available on Apple TV" footer note

---

## Deployment

| Setting | Value |
|---------|-------|
| Platform | Cloudflare Pages |
| Project name | `horizon-line-site` |
| Domain | horizonlinefilms.com |
| Build command | `npm run build` (Astro 5 static) |
| Output dir | `dist/` |
| Credentials | `~/.claude/credentials.env` (CLOUDFLARE_API_TOKEN, CLOUDFLARE_ACCOUNT_ID) |

**Deploy command:**
```bash
source ~/.claude/credentials.env && \
CLOUDFLARE_API_TOKEN="$CLOUDFLARE_API_TOKEN" \
CLOUDFLARE_ACCOUNT_ID="$CLOUDFLARE_ACCOUNT_ID" \
npx wrangler pages deploy dist/ --project-name=horizon-line-site --commit-dirty=true
```

---

## Deferred / Future Work

1. **Live countdown timers** in Session Windows (section 6) — currently static placeholders
2. **Real Vimeo URLs** in screening.astro — swap PLACEHOLDER when video is uploaded
3. **Session frame images** — `session-01.jpg` through `session-05.jpg` may need to be added to `public/images/`
4. **Supabase table** — `raptus_signups` table needs to exist for email capture to work (may already exist from prior work)
5. **Git commit** — all v2 changes are deployed but uncommitted

---

## Key Creative Decisions Made This Session

1. **Negative reviews as clinical diagnoses** — User's idea. Instead of hiding bad reviews, every review (positive, mixed, negative) gets tagged with a therapy diagnosis (COGNITIVE DISSONANCE, SYSTEM OVERLOAD, etc.). "Discomfort is not a side effect. It is the treatment." This turns criticism into content that reinforces the film's themes.

2. **Sequel teaser** — "Raptus: Hard Wired" section added between purchase cards and producer hint. Minimal — just title, tagline, and IN DEVELOPMENT status.

3. **Source interleaving** — Reviews ordered so no same-author quotes appear adjacent in the 4-column grid. Alternates Film Threat / Letterboxd / IMDb / RT across rows.

4. **Producer hint visibility** — Upgraded from nearly-invisible #333 text to a bordered card with red accent. Still clinical, now findable.

---

## Review Sources Used

| Source | Author | Rating | Used Quotes |
|--------|--------|--------|-------------|
| Film Threat | Bobby LePire | 8.5/10 | 2 quotes |
| Letterboxd | Billy Ray Brewton | ★★★½ | 3 quotes |
| Letterboxd | eXistenZZ | ★★★½ | 2 quotes |
| Letterboxd | Nikola Gocic | ★★½ | 2 quotes |
| Letterboxd | joshrowley | ★★½ | 2 quotes |
| IMDb | Featured Review | 7/10 | 2 quotes |
| IMDb | Various | various | 3 quotes |
| Rotten Tomatoes | Audience | 57% | 2 quotes |

---

## Resume Command
```
claude-task raptus-v2
# OR for interactive:
/hrs
```
