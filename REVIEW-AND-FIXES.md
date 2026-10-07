# Adversarial review and fix plan

Reviewed 2026-10-07 at `main` (fb636e3) against `PLAN-UNIFY.md`, `plan.md` and `README.md`.
Method: greps for every banned string and claim, a local server, a 375px render of `/` and `/work/`,
a fresh-load test of the old-anchor forward, file and image inspection.
**Not tested:** Lighthouse, no-JS rendering of `/work/`, desktop WebGL frame rate, real-phone
performance, GitHub link health, Calendly. Anything below marked "unverified" needs those.

Credit first, so the rest is credible: the repo ships. Both pages return 200, no console errors, no
horizontal overflow at 375px, zero em/en dashes, zero pricing strings, zero CDN URLs, vendored libs
with a license table, landing has meta/OG/JSON-LD/noscript, sitemap lists both URLs, email is
entity-obfuscated on the landing. The problems are in what the docs *say* versus what is true, and
in things the plans promised and nobody checked.

---

## P0: broken or false right now

### 1. The old-link forwarding lands on nothing
The landing forwards `/#projects` to `/work/#projects`. Fresh-load test: URL becomes
`/work/#projects`, `getElementById('projects')` is `null`, `scrollY` is 0. The IDs in `/work/` are
`services, work, stack, about, experience, contact, other-work, cb-*`. There is no `#projects`,
`#tools`, `#working`, `#proj-clinical`, `#proj-ojph`, `#proj-jobhunter`, which is exactly the list
`PLAN-UNIFY.md` section 4 promised to forward. The README states the forward works. It "works" the
way a redirect to a 404 works.
**Fix:** add an explicit map in the landing script, old hash to new hash
(`#projects`->`#work`, `#tools`->`#stack`, `#working`->`#services`, `#proj-clinical`->`#cb-suite`,
`#proj-ojph`->`#cb-ojph`, `#proj-jobhunter`->`#other-work`). Keep unknown hashes forwarding to `/work/`
without a hash instead of a dead anchor. Add the check to QA.

### 2. `/work/` mobile page is 25,665px tall and 360 elements render under 12px
`plan.md` flagged 25,017px as "the #1 conversion problem" with a < 12,000px target. It got worse
(25,665px), and the "raise micro-labels to 12px" item is still open at larger scale (360 elements
vs 95). The unify pass rebuilt the page and silently dropped the lesson. The README's "Keep the
`<details class="fold">` behaviour" is not visible in the result.
**Fix:** see Phase B.

### 3. `/work/` lost its accessibility floor
`plan.md` listed skip link, semantic landmarks as "already passing, do not regress". Now on `/work/`:
no `<main>`, no skip link (measured: 0 of each). Image `alt` coverage was not fully checked: the
hero and about images have `alt="Jeyson Dagondon"`, other `<img>` tags are built in JS strings and need a
rendered check. The landing has `<main>` and a skip link, so the two pages are not at the same bar PLAN-UNIFY section 1 says they must meet.
**Fix:** wrap content in `<main id="main">`, add skip link, give every `<img>` a static `alt`
(empty `alt=""` for decorative), verify with an axe or Lighthouse run. See Phase C.

### 4. Audience pivot is half-applied
Decision: direct clients, Australia first, **not** OnlineJobs.ph employers. Still shipped:
- `/work/` contact block links the OnlineJobs.ph profile (`id 1595582`) and JSON-LD `sameAs` lists it.
- A whole project card (`cb-ojph`) and the `Job Hunter Dashboard` card both lead with OnlineJobs.ph.
An Australian clinic owner who clicks that profile sees a $/hr marketplace listing, the exact
anchor the pricing removal was meant to kill.
**Fix:** remove the OJ.ph link from contact and `sameAs`, or get an explicit owner ruling to keep it.

---

## P1: documentation that now lies

| Where | Claim | Reality |
|---|---|---|
| `PLAN-UNIFY.md` header | "Nothing in this plan has been built yet" | Phases 0 to 3 are built and pushed |
| `PLAN-UNIFY.md` sec 2 | "No dark mode... both deliberately light" | Both pages are warm dark; the decision flip is only in `README.md` and commit messages |
| `PLAN-UNIFY.md` sec 3 | Token names `--bg0 #f2f6fa`, `--cyan`, `--mag` | `tokens.css` is gold on near-black; those tokens no longer exist |
| `PLAN-UNIFY.md` Phase 0.3 | Archive `plan.md` to `docs/archive/` | Never done. `plan.md` is **gitignored and untracked**, so it is neither archived nor deleted, and it still tells readers to keep the light theme, publish a rate block and "HIPAA Certified" fixes that are now reversed. A future agent that reads it will undo decisions |
| `README.md` | "Every screenshot is `.webp` at 1000px wide" | `hero.png` 814 KB, `about.png` 939 KB, `hero-mobile.png` 467 KB, `og-card.png` 377 KB are PNG; 2.6 MB of the 2.8 MB `img/` |
| `README.md` | "the pages already obfuscate the email address" | `/work/` has a plain `mailto:donjeysonofficial@gmail.com`; the README itself prints it in clear text in a public repo |
| `README.md` | the CV compile command uses `/c/Users/PC/...` and `Dropbox\Resumes` | Machine-specific Windows paths; this checkout is macOS. The CV rebuild and `check-truth.py` gate are not reproducible from the repo |
| `README.md` | "the CV's rate line should go in the next rebuild" | Still open; `cv.pdf` was touched in the last commit ("Update cv.pdf") with no evidence the rate line was removed. Unverified |
| `work/index.html` L8 and L2195 | Comment cites `Dropbox/Resumes/!CAREER-TRUTH.md` and `portfolio-ref/_shot/make-hero-proto.py` | Leaks private local paths and an internal file name in public view-source. The plan's own rule is that client details are never published |

## P1: process and claims

- **The truth gate is asserted, not shown.** Phase 4 says run `check-truth.py` and diff against the
  ledger. Nothing in the repo records that it ran, or its output. 45+, 40 published, 7 portals,
  91.07%, 6 min to 40 s, 40 to 60 h/month are all stated as verified. The `plan.md` review already
  questioned 40 to 60 h/month (implies ~21 to 32 orders/day for one person) and asked for the owner's
  order volume. Grep for that arithmetic: not resolved anywhere.
- **Zoho case study status is unconfirmed.** PLAN-UNIFY Phase 0.1: publish only after the owner
  confirms production/pilot/prototype. A `cb-zoho` card shipped. There is no record of that
  confirmation. If it is a prototype and reads as production, that is the exact failure the
  "AI-native" claims ledger exists to prevent.
- **Two worktrees sit inside the repo** at `.claude/worktrees/{new-portfolio,unify-plan}`, each a
  full copy of the site (several MB, with their own `.git`). They are ignored via `.git/info/exclude`,
  not `.gitignore`, so the next clone has no protection and a careless `git add -A` elsewhere would not
  either. The `unify-plan` worktree is checked out on a branch literally named `molecules`, unrelated
  to its path. Stale branches, stale copies, easy to edit the wrong index.html.
- **Debug switches shipped to production.** The landing documents `?3d`, `?static`, `?q=full`.
  `?3d` forces WebGL on any device including ones that failed the capability gate. Harmless but
  undocumented to users; confirm it cannot be used to degrade the page for a visitor via a shared link.
- **`/work/` is now 257 KB, 4,508 lines, one file.** `plan.md` and PLAN-UNIFY both described it as
  ~89 KB / 1,422 lines. It nearly tripled. "No build step, single file" is now costing real
  maintainability: 2,755-char lines, inline Three.js scene code at the bottom of a portfolio page
  (`core3d`, `tex.generateMipmaps`), plus gsap, ScrollTrigger and lenis loaded on a page whose job is to
  be read.

## P2: performance and quality

- Hero image is a 2560x1600 RGBA **PNG** served to every viewport width via `sizes="100vw"`
  (`srcset` offers only 1200w and 2560w, both PNG). Convert to AVIF/WebP, add a 1600w step.
- Landing ships `three.module.min.js` 692 KB raw (~172 KB gzip) for the desktop tier. PLAN-UNIFY
  Phase 3.8 required a measured budget (< 1.5 s to interaction on a mid-range phone, 4G). No
  measurement is recorded. The mobile tier loads it too (`html.mob` still imports Three).
- `/work/` runs a second WebGL scene and a third animation lib on top of the portfolio content. Under
  the original `plan.md` "Experience mode: interface recedes" lens, the two pages now compete.
- README "Fonts:" line is orphaned (a floating paragraph that used to be a fix for a broken link).
- Landing `own` hash list includes `#atom-0N` and `#story` but not `#main` or the skip-link target.
  Verify keyboard users are not forwarded off the page.
- `sitemap.xml` `lastmod` is hardcoded `2026-10-05` and the site has changed since (images dated
  Oct 6); `changefreq`/`priority` are ignored by Google.
- `robots.txt` has no disallow for `.claude/` but it is not published, fine; confirm `.nojekyll` still
  required with `_`-prefixed paths absent.

## P3: nits
- `Healthcare operations` in JSON-LD `knowsAbout` on `/work/` brushes the "health is evidence, never
  the offer" rule. Decide and document, or reword.
- `og:image:alt` on `/work/` is the title again; describe the card.
- `plan.md` is the only plan that mentions Emil/impeccable gates; if those gates still apply, say so in
  one live doc, not three.

---

## Fix plan (do in order; each phase ends with a verification)

### Phase A: false statements and live bugs (1 to 2 hours)
1. Add the hash map to the landing forwarder; add `#projects`, `#tools`, `#working`, `#proj-*` cases.
   Verify: fresh load of each old hash ends scrolled to a real element.
2. Remove the OnlineJobs.ph link from `/work/` contact and `sameAs` (or record the owner's ruling in the
   README content rules).
3. Strip the `Dropbox/...`, `portfolio-ref/...` and `check-truth.py` path comments from `work/index.html`;
   keep the rule in README only.
4. Obfuscate the `/work/` `mailto:` the same way the landing does. Remove the plaintext address from
   README or accept it consciously and say so.
5. Mark `plan.md` as superseded. Untrack-and-delete or move it to `docs/archive/` as promised, and
   commit it (remove from `.gitignore`).
6. Rewrite the first half of `PLAN-UNIFY.md`: status "Phases 0 to 3 shipped", the theme decision
   (warm dark, why), the real token names.
7. Correct the README claims in the table above (webp, obfuscation, Windows paths to relative, file sizes).
**Verify:** `grep -nE 'Dropbox|portfolio-ref|onlinejobs.ph/jobseekers' work/index.html` -> 0 hits;
fresh load of `/#projects`, `/#tools`, `/#proj-ojph` each shows the right section.

### Phase B: `/work/` mobile length and legibility (half a day)
1. Target `scrollHeight` at 390px under 12,000 px. Collapse project bodies behind `<details>`;
   show Problem and Impact, hide Fix, as `plan.md` 1.2 proposed.
2. Floor all body and label text at 12px; labels at 11px only for pure decoration with `aria-hidden`.
   Measure with the same script used here (elements with text node and computed size < 12px): target
   under 20.
3. Do not add a feature to do this. Remove or demote: stack tiles grid, filter strip, logo grid, footer
   marquee, whichever do not serve a booking decision.
**Verify:** at 390px, `document.documentElement.scrollHeight < 12000`, small-text count < 20,
no horizontal overflow.

### Phase C: accessibility parity with the landing (2 to 3 hours)
1. `<main id="main">` around content, `<a class="skip" href="#main">`.
2. Confirm every rendered `<img>` has a meaningful `alt` (or `alt=""` if decorative).
3. Check focus order and visible `:focus-visible` on all `.btn` and nav links; verify reduced-motion
   leaves a readable page with JS on and with JS off.
4. Run Lighthouse (mobile and desktop) on both pages and attach scores to the README or a `docs/qa.md`.
**Verify:** Lighthouse accessibility >= 95 on both; `axe` reports no serious issues.

### Phase D: weight and performance (half a day)
1. Convert `hero.png`, `hero-mobile.png`, `about.png` to AVIF/WebP with a 3-step `srcset`; keep PNG
   only as fallback if a crawler needs it. Expect ~2.2 MB saved.
2. Measure the landing: transfer with and without Three.js, time to first interaction on a throttled
   mid-tier phone. Record in `docs/qa.md`. If over budget, apply the PLAN-UNIFY 3.8 gate (SVG only on
   phones) and do not load Three on `html.mob`.
3. Decide whether `/work/` really needs its own Three scene. If not, delete `core3d` and the
   loaders and drop that inline JS and a second GPU context.
4. Regenerate `sitemap.xml` lastmod on every deploy (script or manual checklist), drop `changefreq`.

### Phase E: truth and process hygiene (owner input needed)
1. Run `check-truth.py` on both pages and paste the summary into `docs/qa.md` with date and commit hash.
2. Owner: confirm Zoho order desk status and the real daily order volume; update the card and the
   40 to 60 h/month line to show its arithmetic (`orders/day x minutes saved`).
3. Owner: confirm the `cv.pdf` rate line is gone; if not, rebuild from Typst per the README.
4. Delete the two stale worktrees (`git worktree remove`) and the `molecules` branch if merged. Move
   the exclude rule into `.gitignore`.
5. Replace Windows paths in README with a short "CV build" note that does not depend on one machine.

### Phase F: closing the loop
Re-run this review's checks (hash forwarding, mobile height, small-text count, a11y, banned-string
grep, image weights) and append a dated "result" table at the bottom of this file. If a check was not
run, say "not run" rather than omitting it.

## Verification checklist (copy into a script)

```
grep -c 'Dropbox\|portfolio-ref\|jobseekers' work/index.html          # 0
grep -c '<main' work/index.html                                       # 1
grep -c 'class="skip"' work/index.html                                # >= 1
ls -l img/*.png | awk '{s+=$5} END{print s}'                          # < 500000
node -e "..."  # 390px: scrollHeight < 12000, text <12px count < 20
for h in '#projects' '#tools' '#proj-ojph'; do  # fresh load, assert target exists after forward
```

---

## Locked decisions (owner, 2026-10-07)

| # | Question | Decision |
|---|---|---|
| 1 | OnlineJobs.ph links | Remove the profile link (contact and JSON-LD `sameAs`). Keep the OJ.ph Cleaner card and Job Hunter in Other work |
| 2 | Mobile length | Collapse project and secondary detail behind `<details>`; keep the 3D scene on `/work/` as is |
| 3 | Zoho order desk | Status is a **pilot with one team**. Card says "In pilot" |
| 3b | 40 to 60 h/month | True for the owner's own 20 to 30 orders/day only (team is 4 workers, 80 to 120 orders/day). Page now states that basis. Not extrapolated to the team |
| 4 | Hero and portrait images | WebP with a 3-step srcset |
| 5 | `plan.md` | Deleted. Worktrees left in place. Email left as a plain `mailto:` on `/work/` |
| 6 | Mobile 3D on landing | Keep the animated mobile tier. CV checked: clean |

## Results (this pass)

| Check | Before | After | Status |
|---|---|---|---|
| Old anchors (`/#proj-ojph`, `/#tools`, `/#projects`) forward to a real element | landed at top of `/work/` | `/#proj-ojph` lands on `#cb-ojph` (tested fresh load); `#tools`, `#projects` mapped, same mechanism | done |
| `/work/` mobile height @375px | 25,665 px | 17,602 px | **partly met**: original target was < 12,000. Reaching it needs cutting content, not folding. Open decision below |
| Visible text under 12px @375px | 360 | 0 | done (60 CSS declarations raised to 12px) |
| `<main>` and skip link on `/work/` | 0 / 0 | 1 / 1 | done |
| Horizontal overflow @375px, desktop | none | none | still passing |
| Console errors `/work/` (mobile and desktop) | none | none | still passing |
| OJ.ph profile link / `jobseekers` on `/work/` | present | 0 | done |
| `Dropbox` / `portfolio-ref` in view-source | 2 | 0 | done |
| `img/` weight | 2.8 MB | 952 KB (hero 814 KB to 79 KB) | done; originals moved to `~/Desktop/anki-boi-img-originals/` |
| Zoho card status | "Final testing" | "In pilot" | done |
| Hours claim | bare "40 to 60 hours" | states "at my own 20 to 30 orders a day" | done |
| `plan.md`, stale PLAN-UNIFY header, README claims | stale | fixed (webp, obfuscation, Windows paths, CV note, content rules) | done |
| sitemap `lastmod` | 2026-10-05 | 2026-10-07, `changefreq` removed | done |
| Banned strings (em/en dash, `$n`, HIPAA Certified, CDN) | 0 | 0 | still passing |
| `cv.pdf` rate or phone number | unknown | none found | done |
| Lighthouse, axe, no-JS render of `/work/`, throttled-phone timing of the landing | not run | **not run** | open |
| `check-truth.py` | not run | **not run** (script lives outside this repo) | open, owner |
| Rendered `<img alt>` coverage | not run | not run | open |

## Still open

1. **Mobile length target.** 17.6k px. Options: fold each project card to a headline plus one line, drop the services tab stage on phones, or shorten the About photo. Needs a call on which content may be hidden.
2. **Order-volume claim.** If all four workers use the suite, the real saving is about 150 to 225 h/month. The ledger would need updating by the owner before any team figure is published.
3. **Hero `srcset`.** The 1200w entry is a portrait crop and the 1600w and 2560w entries are landscape, so a 1100px-wide desktop gets the portrait crop. Pre-existing; fix with `<picture>` and media conditions.
4. **Landing mobile 3D budget** was kept by decision; no measured load time exists. Run a throttled-profile test before launch.
5. Lighthouse and axe on both pages; rendered `alt` check; `check-truth.py` run recorded here.
