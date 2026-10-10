# Portfolio improvement plan — anki-boi.github.io

**Goal:** make a prospective client feel *this person is a professional I can rely on.*
Not "can this person build things" — that's already proven. The gap is trust, reliability, and decision friction.

**Target:** `index.html` (single file, no build step). Line numbers refer to the current working copy.

**Execute phases in order.** Phase 0 is one sitting. Do not start Phase 4 — craft polish on a page nobody reaches is wasted work.

---

## Skills applied

This plan is governed by two design skills. Read both before editing.

### `impeccable` (v4.3.1) — the framework and the quality floor

- **Mode: `Experience`.** *"The visitor is inside the work itself. Portfolios, galleries, showcases. Let the artifact lead from the first viewport; the interface recedes."* This is the single most useful lens on this page: **the interface is currently doing a lot of talking** (terminal card, chemistry ticker, molecule canvas, circuit board, HUD brackets, progress bar, cursor spotlight). Under Experience mode, the work leads and the chrome recedes. That is the principled reason to cut Phase 4.5, not just the performance one.
- **This is a refinement, not a redesign.** Per the skill: *"Refinement preserves; redesign replaces. Refinement keeps the incumbent identity, behavior, copy, and everything outside scope."* So: keep the light lab world, the typeface pairing, the structure. Do not pick a replacement visual world. Do not touch factual copy to add or strengthen claims — ask the owner first (Phase 0.1 and 0.4 do this).
- **References loaded for this work:** `reference/craft-floor.md` (the quality floor and the absolute bans — see "Bans this page hits" below), `reference/polish.md` (triage order and finish criteria), `reference/distill.md` (Phase 2), `reference/routing.md` (command gating).
- **Run `craft-floor.md` again immediately before any UI edit**, including small refinements. It is the check list, not a one-time read.
- **`impeccable context` will report `NO_PRODUCT_MD`** — the repo has only `README.md`. The skill explicitly allows a narrow refinement to proceed on the incumbent implementation without blocking. **Optional, non-blocking:** run `/impeccable init` and `/impeccable document` once to capture `PRODUCT.md` / `DESIGN.md`. The `:root` token block (lines 41–49) is already a real design system; documenting it makes every later edit safer.

### `emil-design-eng` — Emil Kowalski's craft rules for motion and interaction

Its review format is mandatory: findings are expressed as a **Before / After / Why table**, not prose bullets. See Phase 4.1. The rules that bind this page: UI motion stays **under 300ms**; stagger is **30–80ms**; **only animate `transform` and `opacity`**; hover animations are gated behind `@media (hover: hover) and (pointer: fine)`; pressable elements need an `:active` state; CSS variables are inheritable so setting one on a parent forces a style recalc on every child.

### Gates to run (impeccable commands, in this order)

| When | Command | Purpose |
|---|---|---|
| Once, before editing | `impeccable context --target index.html` | loads project context; keep cwd at the project |
| Before Phase 0 | `/impeccable critique index.html` | establishes the P0/P1 backlog this plan then closes; retain the returned `snapshot_file` |
| Phase 0 | `/impeccable clarify index.html` | the copy work (0.4, 0.5, 0.6) |
| Phase 1 | `/impeccable adapt index.html` | device sizes and the mobile nav |
| Phase 2 | `/impeccable distill index.html` | redundancy, progressive disclosure, "if it's said elsewhere, don't repeat it" |
| Phase 4 | `/impeccable quieter index.html` | the overstimulating FX layers |
| Phase 4 | `/impeccable typeset` + `/impeccable layout` | type scale, measure, contrast |
| Phase 4 | `/impeccable optimize index.html` | canvas, FX, image weight |
| Final | `/impeccable polish index.html` | final quality pass; then close the critique snapshot |
| Final | `/impeccable audit index.html` | a11y / perf / responsive technical check |

After polish clears every Priority Issue from the snapshot:
`.pi/skills/impeccable/scripts/impeccable critique-storage close "<target>" "<snapshot_file>"`

---

## Measured findings

| Check | Result |
|---|---|
| Render at 1440×900 / 820×1180 / 390×844 | no console errors, no horizontal overflow |
| Page height desktop / mobile | 13,778 px / **25,017 px** (~15 / **~30** screens) |
| Nav links at ≤840px | `About`, `Skills`, `Work`, `Experience` all `display:none`, no menu control |
| `45+` occurrences | 7 |
| Elements rendering at 11.2px / 10.24px | 74 / 21 |
| `--mut-2 #77889f` on `--bg0 #f2f6fa` | **3.26:1** — fails AA (needs 4.5:1) |
| `--mag #d6217c` on `--bg0`, small text | **~4.28:1** — fails AA |
| `@media (hover: hover)` gates | **0** — against 14 `:hover` rules |
| `:active` rules | **0** |
| `.proj .sub` measure | `max-width:82ch` — exceeds the 65–75ch band |
| `transition: all` | 0 ✓ |
| Outbound GitHub links | 7/7 return HTTP 200 |
| `15 stars` claim | verified against the GitHub API (exactly 15) |
| CV download / testimonial / "how I work" / rate | all absent |

Not verified: visual appearance (this model cannot view images). Layout judgements below are inferred from measured geometry, computed contrast, and DOM state.

---

## Bans this page hits (impeccable `craft-floor.md`)

These are its own words. Note that commit `060ce5c` already removed numbered eyebrows and kickers — the ban was recognised and applied **incompletely**.

| craft-floor rule | Violation |
|---|---|
| *"A kicker or eyebrow above a heading. This one is a ban, not a default: no brief earns it back."* | The `.flag` badge above every project `<h3>` — 7 instances: `Flagship · In production`, `Full-Stack · 359 tests`, `Open Source · 15 stars`, `If you came from OnlineJobs.ph`, `AI Pipeline`, `Product`, `Domain Knowledge` |
| *"The hero-metric template: big number, small label, supporting stats, accent."* | `.stats` (4 tiles) and every `.metrics` row (4 more) |
| *"Same-size cards of icon plus heading plus text as the page structure."* | `.tools` — 6 identical icon + `<h3>` + chips cards |
| *"Monospace as a costume for 'technical' rather than for code, data, or measurement."* | `.term` — a terminal card whose header reads `profile.sys — bash` and which contains no bash |
| *"Motion: one authored moment, not scattered effects and not one identical entrance on every section."* | `.rv` — one identical entrance applied to ~40 elements across every section |
| *"A zero-offset colored halo is decoration."* | the `#mol` canvas radial-gradient halos, drawn at (0,0) |
| *"Type: body measure 65–75ch."* | `.proj .sub{max-width:82ch}` (line 314) |
| *"Browser surfaces… text selection, the caret, custom scrollbars, focus rings, underline offset."* | `::selection` ✓, `text-underline-offset` ✓, `:focus-visible` ✓ — but **`caret-color` and scrollbar theming are missing.** The skill calls this *"the cheapest signal that a page was built rather than assembled, and the one models skip most reliably."* |

Also confirmed compliant and **not** to be "fixed": no `transition: all`; smooth scroll disabled under reduced motion; the light theme is deliberate — craft-floor says pick light/dark from the use scene, and the scene (a clinic manager screening candidates on a work laptop) justifies light.
**Never nest cards inside cards** — the current `.proj` card containing `.psi-item` panels is already at the limit; do not add a third level.

---

## What must NOT be changed

The page's actual assets. Do not "simplify" these away.

- **Problem → fix → impact.** The right frame. Shorten the execution; keep the frame.
- **Every claim keeps its artifact link.** 7/7 currently return 200.
- **The `176 of 227` admission** (a filter that had stopped filtering). The single most trust-building sentence on the page. Add a second admission if one exists.
- **The claims ledger in `README.md`.** Genuine differentiator — and Phase 0.1 makes the hero obey it.
- Already-passing a11y: alt text on all 12 images, skip link, `prefers-reduced-motion`, semantic headings, `aria-hidden` on decoration.
- Self-hosted fonts, no CDN, no tracking, no build step.

---

## Phase 0 — trust repairs (one sitting, ~1 hour)

Highest trust gain per minute on the page. Do all seven.

### 0.1 Name the HIPAA credential instead of using the bare adjective
**Line 490.** `<span>HIPAA Certified</span>`

Real, exam-based individual HIPAA certifications exist (`CHP`/`CHSS` from ecfirst/AIHC, `CHPS` from AHIMA). **Keep the claim.** The problem is that a bare adjective is unverifiable while a named credential is checkable — and HHS/OCR publishes an explicit warning about misleading HIPAA marketing claims, so a compliance-literate reader reflexively asks *"certified by whom?"* and assumes the weakest answer.

Replace with the actual credential:
- `CHPS · Healthcare Privacy & Security` (AHIMA)
- `CHP · Certified HIPAA Professional`
- `HIPAA Privacy & Security trained` — only if it is a training completion certificate, not an exam credential

Update JSON-LD `knowsAbout` (line ~25) and the meta description (line 7) to match.
**Verify:** `grep -ci 'HIPAA Certified' index.html` → 0.

### 0.2 Make Experience strictly reverse-chronological
**Lines 960 / 974 / 986.** DOM order is `Apr 2026 – Present` → `2020 – 2023` → `Aug 2024 – Mar 2025`. A scanner reads **2026, 2020, 2024** — it looks like an error or like hiding something, and the out-of-place item is the pharmacy internship that *proves* the RPh credential you lead with.

Move the `Aug 2024 – Mar 2025` block above the `2020 – 2023` block.
**Verify:** reading only the `.job-when` cells top to bottom yields a monotonically descending sequence.

### 0.3 Delete the fake "2026" milestone from the About timeline
**Lines 576–578.** `2026 / AI agent orchestration becomes daily work` sits in the same visual slot as employment, so it reads as *a second job in 2026*. It isn't. Its content (RTX 3090, "no cloud token spend for routine work") is information for the owner, not a buyer — and those skills are already in the `AI Agents & LLMs` toolbox card.

Delete the block.
**Verify:** the About timeline reads `Now` → `2021 – 2025` → `2020 – 2023`.

### 0.4 Make the `40–60 hours a month` claim self-defending
**Clinical-suite card, impact column**, paired with `~6 min → ~40 s per order`.

The arithmetic implies **21–32 orders/day, handled by one person, every working day.** Plausible for a peptide/GLP-1 telehealth clinic — but the reader has to do the division, and if it doesn't hold you've taught them to distrust your numbers, which is this page's only real currency.

Add one clause showing the work: `~35 orders/day × 5 min saved ≈ 60 h/month`. Auditable beats impressive. *(Per impeccable: ask before changing claims — this is the one item that needs the owner's confirmation of the real order volume.)*

### 0.5 Reframe the "not detail-oriented" confession
**Line 564.** `I&rsquo;m not naturally a detail-oriented person — my own assessments say so.`

The mitigation that follows is persuasive (process over willpower). The confession that precedes it contradicts `"Wrong-patient and wrong-vial entries designed out"` three screens below, on a page asking to touch prescriptions.

Keep the honesty, drop the framing. e.g. *"I don't rely on being detail-oriented. I rely on systems that don't let a detail slip."*

### 0.6 Fix or cut the footer's last line
**Line 1030.** `<span>Built by hand — and by the agent stack, of course.</span>`

To a client who can't read code, "the agent stack" reads as *who is accountable when this breaks in production?* Reframe from tooling identity to quality control (tests, CI, verification, human sign-off) or cut it. It is the last thing they read.

### 0.7 Dead code + layout fallback (one edit, five small changes)
- **Line 481:** `id="heroline"` is referenced nowhere — leftover from the scramble effect removed in `060ce5c`. Delete the attribute.
- **Lines 164 & 332:** `.mk` is defined twice with overlapping declarations. Keep the second (it carries `--mk-c`/`.pain`/`.win`); delete the first.
- **Line 140:** `min-height:100svh` has no fallback. Add `min-height:100vh;` before it or older Safari/Chrome collapse the hero.
- Reduced-motion block references a `.dot` animation that no longer exists. Remove the stale selector.
- Add an `apple-touch-icon`.

**Verify:** `grep -c heroline` → 0; `grep -c '^\.mk{'` → 1.

---

## Phase 1 — make it navigable on a phone (half a day)

### 1.1 Add a mobile menu
**Line 134.** `@media(max-width:840px){nav ul li:not(:last-child){display:none}...}`

A fixed nav that hides its own links and offers no control reads as *broken*, and broken is the opposite of reliable. Confirmed absent: no hamburger/menu-toggle exists in the file.

Minimum viable: a `<details>`/`<summary>` disclosure (native, zero JS) or a horizontally scrollable anchor row. Four links: About, Skills, Work, Experience.
**Verify:** at 390px all four links are visible and each scrolls to its section.

### 1.2 Cut mobile page height from 25,017px
Structural, and the #1 conversion problem: a visitor who bounces at screen 5 never reaches Experience or Contact.

Apply `/impeccable distill`: progressive disclosure is the named technique. At `max-width:640px`, render only *Problem* and *Impact* in each `.psi` group (hide *Fix*), or make `.psi` a disclosure. Target **< 12,000px**.
**Verify:** `document.body.scrollHeight` at 390px.

---

## Phase 2 — restructure Projects (1 day) — `/impeccable distill`

### 2.1 Delete the `.flag` eyebrows
Per craft-floor, this is an absolute ban: *"no brief earns it back… delete the label and let the heading speak."* Seven `.flag` badges currently sit above project headings. Remove them. Where the information is real (`In production`, `15 stars`, `359 tests`), fold it into the project's own metrics row — where it becomes evidence instead of a label.

### 2.2 Reorder the cards
A clinic owner's first impression is currently a job-board scraper. New order:
`Clinical automation suite` → `OJ.ph Cleaner` → `Job Hunter Dashboard` → rest.

### 2.3 Add a 3-card "at a glance" strip above the detail cards
One line each: what / what changed / the number. This is how you honour the section's own promise (*"if you only have a minute"*) — currently each `.psi` column carries 4–6 bullets of 2–4 lines instead of the promised one marked line. Per distill: *"Progressive disclosure: what can be hidden until needed?"*

### 2.4 Move the off-domain projects into a compact `Other work` row
`DateCard` (a dating app), `PH Pharmacy Setup Guide`, `Anki MCQ` templates — title, one line, link. Nothing deleted; the healthcare reader's path is uncluttered. Three of seven cards are irrelevant to a healthcare-workflow buyer and dilute rather than broaden the offer.

### 2.5 Trim `45+` from 7 mentions to 2
Hero lede (483), hero creds chip (487), terminal card (508), About paragraph (557), project title (697), metrics divider (728), experience bullet (967). Keep the hero or the stats bar plus the project title. Per distill: *"If it's said elsewhere, don't repeat it here."* Repetition is the clearest tell of a junior portfolio — a senior says it once and links the evidence.

### 2.6 Replace the two non-metrics
**Lines 688, 731.** `1 req — Per scroll, throttled` and `0 — Shared failure domains` are engineering in-jokes in a row whose job is showing a clinic owner results. Replace with outcomes.

### 2.7 Promote the "refuses to guess" pattern
It repeats across three projects (OJ.ph keeps the hourly figure when hours are unstated; a lookup returning two matches halts; the questionnaire is never auto-submitted). For a **healthcare** buyer, *"the automation stops and waits for a human instead of guessing"* **is the product** — and it is currently buried as bullet #3 of an impact column. Move it into the hero lede or the terminal card.

---

## Phase 3 — give the client what they need to decide (1 day)

All five are confirmed absent.

| # | Add | Why |
|---|---|---|
| 3.1 | **`/cv.pdf`**, linked from the hero CTA row and the contact grid | Most-requested artifact in freelance hiring. A client who wants to forward you or run you past HR has nothing. |
| 3.2 | **Rate / availability block:** engagement type, hours per week, timezone overlap in the *client's* time, earliest start | Every client's first reply is "what's your rate and when can you start?" `available for remote work` answers neither. |
| 3.3 | **"How I work":** response time, channel, handover, **and what happens when something breaks** | The missing answer to "can I rely on you." Entirely absent today. |
| 3.4 | **One proof from a human** — a testimonial, or an explicit *"references available under NDA"* | Absence is indistinguishable from "nobody will vouch for me." |
| 3.5 | Move the dual-ISP/battery clause out of the Contact lead sentence and into a reliability chip | Genuine reassurance for a PH-based hire, currently buried mid-paragraph about something else. The reliability clients weigh first is **communication**, not infrastructure. |

---

## Phase 4 — craft (`craft-floor.md` + `emil-design-eng`)

### 4.1 Motion and interaction — Emil's required format

| Before | After | Why |
|---|---|---|
| `.rv{transform:translateY(26px);transition:opacity .95s,transform .95s;transition-delay:var(--d,0ms)}` | `translateY(14px)`; `transition:opacity .28s var(--ease),transform .36s var(--ease)`; cap `--d` at `80ms` | Emil: UI motion stays under 300ms; stagger band is 30–80ms. 26px is hero-scale displacement for body content, and 950ms + 240ms stagger = ~1.2s before a section is readable |
| `.card{transition:transform .55s,border-color .55s,box-shadow .55s}` | `.2s` | Hover is a tens-of-times-per-day interaction — Emil: "remove or drastically reduce". >300ms reads as lag, not polish |
| `.btn{transition:…transform .5s,…}` and no `:active` rule anywhere | `transition:transform 160ms ease-out` + `.btn:active{transform:scale(.97)}` | Emil: "Buttons must feel responsive… this gives instant feedback, making the UI feel like it is truly listening." Press feedback is 100–160ms. Currently a click gives zero feedback |
| 14 `:hover` rules, **0** gated by a hover media query (`.card:hover`, `.chip:hover`, `.creds span:hover`, `.btn:hover`, `.job:hover`, `.c-item:hover`, `.tool:hover`…) | wrap every hover transform in `@media (hover: hover) and (pointer: fine)` | Emil: "Touch devices trigger hover on tap, causing false positives." On a phone these states stick after a tap. The JS already gates its spotlight this way (line ~1135) — the CSS does not |
| `.job{transition:padding-left .6s}` / `.job:hover{padding-left:10px}` (lines 366–367) | drop it, or `transform:translateX(10px)` | Emil: "Only animate transform and opacity… Animating padding, margin, height, or width triggers all three rendering steps." This animates layout on a full-width row |
| `card.style.setProperty('--mx',…); card.style.setProperty('--my',…)` on `.proj` (lines 1140–1141) — ~40 children, fired per `pointermove` | set the gradient on a dedicated absolutely-positioned overlay child, or drop the spotlight | Emil: "Changing a CSS variable on a parent recalculates styles for all children." This is the named anti-pattern, on the largest cards on the page, on every pointer event |
| `.js .rv{opacity:0}` + IntersectionObserver + error-handler + two timed sweeps (~40 lines of JS) | `@starting-style{opacity:0;transform:translateY(14px)}` with the transition on `.rv` | Emil's modern entry pattern. Content is visible by default, so a JS failure or a throttled rAF cannot hide the page — which deletes the safety-net code it currently needs (a `distill` win too) |
| `.tick-track{animation:tickmove 46s linear infinite;will-change:transform}` | keep | Constant motion → `linear` ✓; `will-change:transform` present ✓ |
| `.tile{…}` stagger at 240ms | cap at 80ms | Emil: keep stagger 30–80ms; longer delays make the interface feel slow |

Per craft-floor, also: **do not add motion to make the polish visible.** The correction here is *less and faster* motion, and ideally **one authored moment** rather than the same entrance on ~40 elements.

### 4.2 Typography and contrast
- 74 elements render at 11.2px, 21 at 10.24px. The labelling system that communicates structure is the smallest text on the page. Lift micro-labels to ≥12px.
- `--mut-2 #77889f` → 3.26:1 on `--bg0`; needs ≥4.5:1. Used by `.job-when` (368), `.stat span`, `.c-item b`, footer.
- `--mag #d6217c` at 10.24–10.56px → ~4.28:1; also fails. Darken for small text.
- `.proj .sub{max-width:82ch}` → bring into the 65–75ch band.
- Size steps: the craft-floor asks for *obvious* scale and weight steps — check the h1→h2→h3→body curve once the micro-labels move up.

### 4.3 Browser surfaces
Add `caret-color` from the palette and theme the scrollbar. craft-floor: *"the cheapest signal that a page was built rather than assembled."* `::selection`, `:focus-visible`, and `text-underline-offset` are already themed — finish the set.

### 4.4 X-ray the module: honour the mode
The page is `Experience` mode. Its own summary of a portfolio is *"Let the artifact lead from the first viewport; the interface recedes."* Apply that as the test for every decorative element: **does this help the visitor see the work, or does it compete with it?**

Per craft-floor, the `.tools` grid is the named "same-size cards of icon plus heading plus text as the page structure" pattern — six identical icon + `<h3>` + chips cards. Change the form: the chips are a dense vocabulary list, and they'd read faster as grouped inline text with the daily drivers marked, than as six boxes. Likewise the two hero-metric templates (`.stats` and the `.metrics` rows) are the exact pattern the floor names; fold the numbers into the sentence that needs them rather than giving each its own tile.

### 4.5 Quieter — the FX layers
Apply `/impeccable quieter`. The 14-molecule canvas with per-molecule radial gradients and O(n²) collisions runs on `requestAnimationFrame` forever, plus a fixed full-screen SVG with 8 animated dash-offset pulses. **None of it is visible behind the white cards** — the molecules only show through in the gutters. It costs frames on mid-range Android to decorate the margins, and it competes with the work.

- Skip `#mol` and `.fx-circuit` on coarse pointers and ≤900px.
- Reduce opacity on desktop.
- Per craft-floor, the canvas halos are "zero-offset colored halo = decoration". Consider dropping the halo entirely and keeping only the crisp strokes.
- **Then delete the `text-shadow` hack at line 253** — 12 selectors carry `text-shadow:0 0 5px rgba(242,246,250,.9),0 0 16px …` purely to stay legible over the FX. It is a workaround for FX that are too strong, and it blooms every paragraph on 1× displays.

### 4.6 Affordances
`.card:hover`, `.chip:hover`, `.c-item:hover` lift and repaint, but `.tool`, `.chip`, and `.c-item` (outside the links) are not clickable. Either make them do something or remove the hover states. A hover that ignores a click teaches the user to distrust the page's affordances.

### 4.7 Assets
`img/` is 2.19 MB: three `jobhunter-*.png` at ~400 KB each, `ph-guide.webp` 194 KB, three `datecard-*.png` ~125 KB. Convert the PNGs to WebP (~1 MB saved). Preview images are content, not decoration — refine them, don't cut them.

### 4.8 Trivia that compounds
`robots.txt`, `sitemap.xml`, `<meta property="og:site_name">`, `apple-touch-icon` (see 0.7), and obfuscate the plaintext email against scrapers.

---

## Phase 5 — decisions only the owner can make

1. **Audience.** Is the primary buyer a US clinic/hiring manager, or an OnlineJobs.ph employer? The page serves both: card #1 speaks to the OJ.ph job-seeker audience and the contact grid links the OJ.ph profile (which anchors rate expectations at $4–8/hr), while the rest is written for a $25–40/hr US client. Pick one primary and demote the other to a line.
2. **Whether a 5-month engagement can carry the page's weight.** Today is Sep 2026; the current role began Apr 2026. The page presents it as identity. Either broaden the framing so the 2020–2023 automation history is a first-class period, or present the current work as an *exhibit* rather than the whole thesis.
3. **Confirm the order volume** behind the 40–60 h/month figure (Phase 0.4). Per impeccable, claims are not changed without the owner.

---

## Verification

The check that matters: **open the live site on a phone at 390px, no dev tools, and pretend you're a clinic owner with 90 seconds.** Can you learn what this person does, what they cost, whether they'll reply, and how to contact them — without scrolling 30 screens? Today: no, because the nav hides its own links.

**Run this in bounded passes, not a loop** (impeccable's rule): build a phase fully, inspect **desktop and mobile together in one batched round**, fix everything that round shows in one batch, confirm with at most one more round, then stop. Open-ended self-QA burns time doing worse what the finish handoffs do better.

```
        document.body.scrollHeight @390px   → < 12000    (now 25017)
        all 4 nav links clickable @390px    → yes        (now hidden)
        contrast(--mut-2, --bg0)            → ≥ 4.5:1    (now 3.26:1)
        contrast(--mag,   --bg0) small text → ≥ 4.5:1    (now 4.28:1)
        @media (hover:hover) gates          → ≥ 14       (now 0)
        :active rules on pressables         → ≥ 1        (now 0)
        grep -c 'padding-left .6s'          → 0
        grep -c 'hipaa certified'           → 0
        grep -c 'heroline'                  → 0
        grep -c '45+'                       → 2          (now 7)
        grep -c 'class="flag"'              → 0          (now 7)
        elements at ≤11.5px font-size       → 0          (now 95)
```

Regression guards: 7/7 project links return 200; zero console errors at 1440/820/390; no horizontal overflow; all 12 images load; reduced-motion still resolves to a static, fully-visible page with no JS.
