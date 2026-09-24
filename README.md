# anki-boi.github.io

Source for **[anki-boi.github.io](https://anki-boi.github.io/)** — the portfolio of Jeyson Dagondon, RPh.

Healthcare workflow automation: I run the operations layer of a telehealth clinic and build the
browser automation that removes the manual work from it. 45+ scripts and extensions in production
(40 of them published), Zoho CRM, HIPAA privacy & security trained.

## What's here

A single self-contained `index.html` (~80 KB) with no build step, no CDN and no external JS.
Self-hosted [Fraunces](https://fonts.google.com/specimen/Fraunces) (display),
[Inter](https://fonts.google.com/specimen/Inter) (body) and
[JetBrains Mono](https://fonts.google.com/specimen/JetBrains Mono) (terminal/labels) in
`fonts/`. Light lab theme: a canvas layer of drifting ball-and-stick molecules and an SVG
circuit-board layer with travelling current pulses, both stripped back to decoration and skipped
entirely on coarse pointers and narrow viewports. Motion is one `@starting-style` entry that
lands in 0.28s; content is visible by default, so a throttled rAF or a JS failure cannot hide
the page. Hover effects are gated behind `@media (hover: hover)`, everything is covered by
`prefers-reduced-motion` (static molecule frame, no pulses, no transitions), and the whole page
still renders complete with JS disabled.

Long-form blocks — each project's problem / fix / impact and the About career timeline — are
wrapped in `<details class="fold" open>`. Desktop keeps them open; on phones they start collapsed
and open with one tap, which is what keeps the mobile page at ~12,000 px instead of ~25,000 px.
Nothing is hidden: the metrics line above each fold carries the number.

GitHub Pages serves `main` at the domain root. Edit `index.html`, commit, push.

```
python -m http.server 8899 --bind 127.0.0.1   # then open http://127.0.0.1:8899
```

`fonts/` and `img/` are committed deliberately — a push that forgets the fonts silently falls back
to Georgia and 404s both preloads. The `jobhunter-*` screenshots are `.webp`; the `.png` originals
were superseded and removed.

## Content rules

Every claim on the site is checked against the owner's claims ledger before it ships. A claim goes
public only if it survived verification. Concretely, this page does not claim:

- EHR/EMR experience (never worked in an Epic/Cerner/Athena system)
- ICD coding or SQL
- a reporting-time percentage from supervised contract work
- an "Admin Manager" title — the role is **Medical Ops & Process Automation Specialist**
- a bare **"HIPAA Certified"** — HHS/OCR warns about that phrasing, so the page states the actual
  credential: **HIPAA Privacy & Security trained**. Do not upgrade it to CHP/CHPS unless the exam
  certificate exists.

Also deliberately absent until the owner supplies them: a `/cv.pdf` download, a client testimonial
or reference line, and a written response-time commitment. Do not invent these.

**Geography rule.** The employer's location appears only in the Experience section, where the fact
lives (Colorado Medical Solutions, and the US compounding pharmacies the orders go to). Positioning,
coverage and availability copy stays geography-neutral and says **flexible timezone coverage** — the
page must not read as US-hours-only, because the primary audience is a global OnlineJobs.ph employer.

## Positioning

The page frames the owner as **AI-native and platform-agnostic**, and it does that by showing the
toolchain rather than asserting an adjective. Concretely:

- The hero lede says the suite is *maintained through an agent stack I run myself* — a capability
  statement, not an authorship one. It says who is accountable for the work, not who typed it.
- The About section carries the frame in one sentence: *agent orchestration and models served on my
  own machine are simply the toolchain I work in now, the way most people work in an editor.* That
  sentence sits inside the existing "no favourite platform" paragraph, so AI-native and
  platform-agnostic are one idea: the toolchain is portable, and so is the work.
- The toolbox groups carry the receipts (Hermes orchestration, pi/DeepSeek harnesses, vLLM,
  llama.cpp, ComfyUI, AI video, KokoroTTS). The hero does not repeat them.
- The **H1 and the whole title family** read *AI-Native Workflow Automation*: `<title>`, `og:title`,
  `twitter:title`, `og:image:alt`, the nav brand subtitle, the footer and JSON-LD `jobTitle`. The
  phrase "Healthcare Workflow Automation" must not come back — it was the strongest scope signal on
  the site, much stronger than any single word in the body copy.

**Scope rule.** Healthcare is *evidence*, never the *offer*. It is allowed to name the employer, the
RPh credential, the clinic the scripts run in, and a project's own problem statement ("wrong-patient
entries", "patient-data handoffs"). It is not allowed in service-scope copy — the copy that says what
the owner will do for a reader. That copy says "your data", "your team", "your deadline". Concretely:
`#working` says *never allowed to guess with your data*, not *on a patient record*; the pullquote says
*your team*, not *your practice*; the meta description says *manual work from operations*, not
*clinical admin*; the Skills lede lists *automation, AI, data, and the clinical systems* in that
order, so the portable skills lead.

Do not add a bare "AI-native" badge or hero chip. An unbacked positioning adjective is the same
failure as the old "HIPAA Certified" claim — the evidence has to be adjacent or the label reads as
decoration. The footer stays on *"every change tested, committed, and reviewed by a human"*: with an
AI-native workflow on the page, the human-accountability line is doing the trust work and must not be
diluted.

The script count is stated as `45+ in production, 40 published` because the remaining scripts carry
client identifiers and stay private. It appears twice on the page (hero lede, clinical-suite
heading) and nowhere else — repetition reads as padding. Client and vendor names are never
published, in markup, commit messages, or screenshots.

## Elsewhere

- **GitHub:** [github.com/anki-boi](https://github.com/anki-boi)
- **LinkedIn:** [linkedin.com/in/donjeysonmd](https://www.linkedin.com/in/donjeysonmd)
- **Email:** donjeysonofficial@gmail.com
