# Unify plan: landing page + portfolio, one theme

Status: approved decisions captured 2026-10-05. Nothing in this plan has been built yet.
Supersedes `plan.md`, which is a historic worklog from 2026-09-24 and should be archived in Phase 0.

## 1. Decisions (locked with Jeyson on 2026-10-05)

| Question | Decision |
|---|---|
| What "Automation and Health" means | Visual and narrative motif only. The offer stays "AI-native workflow automation". Health appears as the evidence (RPh, the clinic, the prescription pipeline) and in the design language (molecules, clinical references). The README rule "healthcare is evidence, never the offer" stands. The banned phrase "Healthcare Workflow Automation" stays banned. |
| Job of the landing page | A cinematic front door. The existing scroll story (`portfolio-ref/portfolio-template/index.html`) becomes a short intro with one job: get the visitor to book a call or open the portfolio. No services, pricing, FAQ or form on it. |
| Primary reader | Direct clients. Australian first, US second. Not OnlineJobs.ph employers, not recruiters. This replaces the README's "primary audience is a global OnlineJobs.ph employer". |
| Hosting | Same GitHub Pages repo. Landing becomes `index.html` at the root. Portfolio moves to `/work/index.html`. |
| Pricing | Removed everywhere. The "$15/hr · $2,000/mo" hero chip and the rate block in How I work go. Replacement line: pricing is scoped per engagement, book a call. |
| Primary CTA | Calendly: `https://calendly.com/donjeysonofficial/30min`. Email stays as the secondary action. |
| Featured projects | 1. Clinical automation suite (userscript-showcase, flagship, stays first). 2. Zoho order desk as a words-only case study, code stays private. 3. Anki MCQ Importer. 4. PH Pharmacy Setup Guide. 5. OJ.ph Cleaner, reframed as improving a site countless Filipinos rely on, not as a job-hunt tool. |
| Demoted to Other work | Job Hunter Dashboard, True-Anki-MCQ Note Template, DateCard. Everything else in `repos/` (idle-habits, skua-mcp, forex, adaptivedge, h3 backup) stays off the site. |
| Technical bar for the landing | Same standard as the portfolio: self-hosted libraries, no CDN, a no-JS fallback that renders the name, tagline and both CTAs, full meta description and OpenGraph tags. |

## 2. Assumptions I am proceeding on (say so if any is wrong)

- "Colorado Medical Solutions" stays as the employer name, in Experience only, per the existing geography rule. It is not used in any service copy or on the landing page.
- No dark mode on either page. Both are deliberately light and stay that way.
- New and existing copy on both pages uses no em dashes and no hyphen or en dash stand-ins. The live portfolio uses em dashes heavily today, so Phase 2 includes a rewrite pass.
- The Australian angle is expressed through facts we can defend: Cagayan de Oro is UTC+8, the same clock as Perth and 1.5 to 2 hours behind Darwin, Brisbane, Melbourne and Sydney, so a full overlap with any Australian business day is real. No claims about Australian healthcare systems, AHPRA, Medicare, practice management software or local experience. The IBH cover letter already states none of that exists.
- The Zoho order desk case study is published only after Jeyson confirms its real status (daily production use, pilot, or prototype). The card is written to match that status and nothing more.
- The portfolio stays a single self-contained HTML file. The landing becomes a second self-contained HTML file. The only shared files are `/fonts/` (already byte-identical between the two sources) and a new `/assets/tokens.css` that both pages load so the palette cannot drift.
- `cv.pdf` is modified and uncommitted in the main checkout. This plan does not touch it. Rebuild it from Typst when the rate line and audience change, per the README's "do not hand-edit cv.pdf" rule.

## 3. What the sweep found (the facts the plan is built on)

**Live portfolio (`anki-boi.github.io`)**
- One 89 KB `index.html`, 1,422 lines, inline CSS and JS, no build step, no CDN, no analytics, works without JavaScript.
- Light "lab" theme. Tokens: `--bg0 #f2f6fa`, `--bg1 #eaf1f8`, `--panel #ffffff`, `--cyan #0a85ad`, `--mag #cf1b76`, `--lime #3f7d1e`, `--amber #d97b06`, `--ink #101b28`, `--txt #17222f`, `--mut #38485f`.
- Fonts self-hosted: Fraunces (display), Inter (body), JetBrains Mono (labels).
- Sections: hero, about, tools, projects, experience, working, contact. Hero H1 is "AI-native workflow automation, built by a pharmacist."
- Public rates in two places. References "available on request". No testimonial exists.
- Debt: README intro still leads with "Healthcare workflow automation", contradicting its own rule further down. README has a broken JetBrains Mono link and a stale file size. CSS header comment still says "neon terminal". `plan.md` references line numbers and items that have since shipped.

**Landing source (`portfolio-ref/portfolio-template/index.html`)**
- 111 KB, 2,282 lines. A pinned 800vh stage with five legs: The Chaos, The Connection, The System, The Agents, The Quiet. Ends with a footer that says "End of demo".
- Already written in Jeyson's voice with his real story (twelve portals, one userscript, 45+ scripts, agents, sign-off). Already uses the exact same tokens and the same three fonts. Visually the two pages are unified today.
- Dependencies from CDN: GSAP 3.12.5 (70 KB), ScrollTrigger (42 KB), Lenis 1.1.20 (13 KB), Three.js 0.170.0 (675 KB). About 800 KB of JavaScript to vendor.
- Renders nothing without JavaScript. No meta description, no OpenGraph. Decorative SVGs have no titles. Mono text runs 10 to 13 px.
- Engine constraints from its own comments: leg 3 is hardcoded to a hub plus 8 nodes, leg 2 node coordinates are hardcoded in two places, the monogram depends on the display font name. Copy is safe to change through the CONFIG object. Geometry is not.
- The parent folder `portfolio-ref/` also holds `portfolio-site/index.html`, a fuller multi-section portfolio adapted from a third party's GoHighLevel page. Its own `AUDIT.md` found 16 borrowed or invented claims before a provenance guard was added. That file is not the base for anything here. The live portfolio is the base.

**Claims and positioning sources**
- `Dropbox/Resumes/!CAREER-TRUTH.md` is the claims ledger. `check-truth.py` is the gate. Every sentence of new copy must trace to it.
- Four resume variants exist (AI-native master, Health fork, AI engineering, Practice Manager). The site follows the AI-native master.
- The GitHub profile README (`repos/profile`) carries the same spine and the contract marker ("engaged, not employed").
- The whole professional track record is one contract that started April 2026. The site must not imply seniority or management.

## 4. Information architecture after the change

```
anki-boi.github.io/
  index.html            landing (scroll story, front door)
  work/index.html       portfolio (today's index.html, moved)
  assets/tokens.css     shared palette, type scale, chamfer, grain
  assets/vendor/        gsap.min.js, ScrollTrigger.min.js, lenis.min.js, three.module.min.js
  fonts/                unchanged, shared
  img/                  unchanged, plus a landing og.png
  cv.pdf                unchanged
  sitemap.xml           two URLs
```

Navigation between the two:
- Landing nav button: "Book a call" (Calendly). Landing leg 5 primary: "See the work" to `/work/`. Landing footer: "View the work", "Book a call", "Résumé (PDF)", "Email me".
- Portfolio brand mark links back to `/`. Portfolio "Let's talk" and the contact section's primary action become Calendly. Email stays as the second tile.

Old deep links such as `anki-boi.github.io/#projects` will now land on the landing page. GitHub Pages cannot do server redirects. Mitigation: a tiny inline script at the top of the landing checks `location.hash` against the known anchor list (`#about #tools #projects #experience #working #contact #proj-clinical #proj-ojph #proj-jobhunter`) and forwards to `/work/` with the same hash before anything renders. The noscript fallback shows a plain link to the portfolio.

## 5. Phases

### Phase 0. Hygiene and owner checks (half a day)
1. Confirm the real status of the Zoho order desk (production, pilot, prototype). Write the case study to that status.
2. Rebuild `cv.pdf` from Typst with the rate line removed, commit it. Do not hand-edit.
3. Move `plan.md` to `docs/archive/plan-2026-09-24.md`. Update the README to point at this file.
4. Fix the README: replace the healthcare-first intro with the AI-native line, fix the JetBrains Mono link, update the file size, replace the OnlineJobs.ph audience line with "direct clients, Australia first".
5. Update the CSS header comment in the portfolio from "neon terminal" to the light lab theme.

### Phase 1. Shared design system (half a day)
1. Create `assets/tokens.css` from the portfolio's `:root` block. Include the palette, type families, `--maxw`, `--gut`, `--ease`, the chamfer variable and the grain opacity.
2. Make both pages load it. Keep each page's own layout CSS inline. Verify the landing shaders still pick up `--accent` and `--hot` (map them to `--cyan` and `--mag`, or alias both names in the tokens file).
3. Document the motif in the README in one paragraph: health is the evidence and the texture, automation is the offer. List which elements carry the health motif (molecule ticker, clinical chips, pharmacy references in the story) so future copy stays consistent.

### Phase 2. Portfolio changes, now at `/work/` (one to two days)
1. Move `index.html` to `work/index.html`. Update canonical, og:url, sitemap and the JSON-LD url. Fix relative paths for fonts, img and cv.pdf.
2. Remove pricing. Delete the "$15/hr · $2,000/mo" chip. Rewrite the rate block in How I work to: pricing is scoped per engagement, book a 30 minute call to talk through the workflow. Grep for `$15`, `2,000`, `2000`, `/hr`, `/mo` and confirm zero hits.
3. Wire Calendly as the primary action in the hero ("Book a call"), in How I work, and in the contact section. Email becomes the second action everywhere.
4. Australia and timezone. Replace "flexible timezone coverage" chips with the concrete fact: UTC+8, same clock as Perth, 1.5 to 2 hours behind the Australian east coast, full overlap with any Australian business day, morning overlap with US Pacific. Say it once in the hero card and once in How I work.
5. Projects. Keep the clinical suite first. Add the Zoho order desk case study card (problem, fix, impact, no repo link, a line saying the code is private because it touches a live clinic). Keep Anki MCQ Importer and promote PH Pharmacy Setup Guide to a full card (it has the only live demo and screenshots). Rewrite the OJ.ph Cleaner card: the problem is a job board that countless Filipinos depend on and that hides pay and shows stale posts, the fix is a Chrome extension that filters and normalises salaries locally with no analytics, the impact is the "176 of 227" line. Move Job Hunter Dashboard to Other work.
6. Em dash pass over every string on the page. Rewrite, do not just delete the dash. Grep for the em dash, the en dash and a spaced hyphen used as a dash and confirm zero hits.
7. Reduce "45+" to the two permitted mentions after the edits. Confirm "Healthcare Workflow Automation" appears nowhere.
8. Mobile check at 360, 390 and 768 px. Keep the `<details class="fold">` behaviour.

### Phase 3. Landing page at root (two to three days)
1. Copy `portfolio-ref/portfolio-template/index.html` into the repo as `index.html`. From now on it is edited by hand in this repo. The Python build chain in `portfolio-ref` is retired for this file.
2. Vendor the four libraries into `assets/vendor/` with their exact versions and a `LICENSES.md` (GSAP standard license, MIT for Lenis and Three.js). Replace the CDN URLs, including the dynamic Three.js import.
3. No-JS fallback. Add a static block inside `<main>` with the name, "AI-native workflow automation, built by a pharmacist.", RPh and clinic line, "Book a call" and "See the work" links. JavaScript hides it when the engine starts. Also add `<noscript>` text pointing at `/work/`.
4. SEO. Title, meta description, canonical `https://anki-boi.github.io/`, OpenGraph and Twitter tags, a landing `og.png` (1200x630), JSON-LD Person pointing at `/work/` as the main page.
5. Copy pass through CONFIG only. Keep the five-leg arc. Leg 1 stays the clinic chaos (this is where the health motif lives). Leg 5 tagline stays. Change `navCta` to "Book a call" with the Calendly URL. Change the footer from "End of demo" to a real sign-off with "View the work", "Book a call", "Résumé (PDF)", "Email me". No pricing anywhere. No em dashes.
6. Anchor forwarding script from section 4.
7. Accessibility floor. Titles on the decorative SVG groups or `aria-hidden` on them. Raise mono label sizes to 12 px minimum. Confirm the `prefers-reduced-motion` path shows the SVG scenes and all five captions without scrolling 800vh. Add a skip link to the footer CTAs.
8. Performance budget. Measure total transfer with and without Three.js. If the particle system costs more than about 1.5 seconds to first interaction on a mid-range phone over 4G, load Three.js only on pointer-fine desktop and show the SVG fallback on phones. The engine already has an SVG fallback for WebGL failure, so this is a gate change, not new code.

### Phase 4. QA and launch (one day)
1. Truth gate. Run `check-truth.py` against both pages, or diff every claim by hand against `!CAREER-TRUTH.md`. Confirm the verified facts only: 45+ scripts, 40 published, 7 portals, 4 vendors, about 6 minutes to about 40 seconds, 40 to 60 hours a month, RPh Nov 2025, 91.07%, HIPAA trained (never "certified"), engaged not employed.
2. Banned strings grep on both files: pricing figures, "Healthcare Workflow Automation", "HIPAA Certified", "Admin Manager", EHR, EMR, ICD, SQL, em dashes, "End of demo".
3. Link check: every href resolves, Calendly opens, cv.pdf downloads, all GitHub links return 200.
4. No-JS check on both pages. Reduced-motion check. Lighthouse on both at mobile and desktop, target 90 or better on accessibility and SEO, performance as measured in Phase 3 step 8.
5. Deep-link check: open `/#projects`, `/#contact`, `/#proj-ojph` and confirm each forwards to `/work/` with the hash intact.
6. Update `sitemap.xml` to two URLs with today's lastmod. Push to main. Verify GitHub Pages serves both paths. Update the LinkedIn, OnlineJobs.ph and GitHub profile links if any pointed at anchors.

### Phase 5. After launch (open, no dates)
- Collect one written reference from the clinic that can be quoted with permission. Until then References stays "available on request" and the landing makes no social proof claim.
- Decide on privacy-respecting analytics (Plausible or none). The site has none today, and that is a defensible choice.
- When the Zoho order desk reaches production, update its card from the Phase 0 status to the real outcome with a number.
- When Australian client work exists, add it as evidence. Not before.

## 6. What this plan will not do, so expectations are grounded

- It will not make the site read as senior or as a team. The whole professional record is one contract from April 2026, and the claims ledger forbids anything that implies more.
- It will not claim Australian experience, Australian healthcare knowledge, or practice management software skills. The Australian pitch is timezone, reliability and a licensed clinician who builds. That is all the evidence supports today.
- It will not produce testimonials. None exist. "Available on request" stays until a real one is written down.
- It will not make the landing light. Self-hosting the engine means roughly 800 KB of JavaScript, dominated by Three.js. Phase 3 step 8 is the control, and the honest fallback is SVG only on phones.
- It will not give true redirects. GitHub Pages cannot. Old anchor links get a client-side forward, which fails for crawlers and for visitors with JavaScript off, who see a link instead.
- It will not change the story's geometry. Legs 2 and 3 are hardcoded in the engine. Copy changes are safe. New legs or node counts mean rewriting WebGL and GSAP code, which is out of scope.
- It will not touch Calendly, LinkedIn, OnlineJobs.ph or the Resumes folder. Those updates (removing the rate from the OJ profile, aligning the LinkedIn headline, rebuilding the CV) are listed for Jeyson in Phase 0 and Phase 4.
- Effort figures are rough working estimates for one person editing by hand with agent help. They are not commitments.

## 7. Order of work and the first command

Phase 0 first, because the Zoho order desk status and the CV rebuild gate copy in Phases 2 and 3. Phases 1 and 2 can run together once Phase 0 is done. Phase 3 depends on Phase 1 for the tokens file. Phase 4 runs last.

First concrete step after approval of this plan:

```
git checkout -b unify-phase-0
git mv plan.md docs/archive/plan-2026-09-24.md
```
