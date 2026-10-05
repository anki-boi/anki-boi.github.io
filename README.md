# anki-boi.github.io

Source for **[anki-boi.github.io](https://anki-boi.github.io/)**, the site of Jeyson Dagondon, RPh.

AI-native workflow automation, built by a pharmacist. I run the operations layer of a live
telehealth clinic and build the browser automation and agent tooling that removes the manual work
from it. 45+ scripts and extensions in production (40 of them published), Zoho CRM, HIPAA Privacy
& Security trained.

## What's here

Two self-contained pages, no build step, no CDN, no analytics:

```
index.html            landing: a short cinematic scroll story, the front door
work/index.html       portfolio: services, work, about, experience, stack, contact
assets/tokens.css     the shared palette, type families, spacing and motion tokens
assets/vendor/        GSAP 3.12.5 + ScrollTrigger, Lenis 1.1.20, Three.js 0.170.0 (see LICENSES.md)
fonts/                Fraunces (display), Inter (body), JetBrains Mono (labels), self-hosted
img/                  screenshots, portraits, og.png share card
cv.pdf                built from Typst, see The CV
sitemap.xml           both URLs
```

The plan behind this layout is [`PLAN-UNIFY.md`](PLAN-UNIFY.md).

Fonts: [Fraunces](https://fonts.google.com/specimen/Fraunces),
[Inter](https://fonts.google.com/specimen/Inter) and
[JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono).

**One theme.** Both pages are a light "lab" theme (pale blue grey, cyan accent, magenta used
sparingly) and load `assets/tokens.css` before their own inline CSS. Change a colour there, never
inline, or the two pages drift. Each page keeps its historic token names as aliases (see the file
header). There is no dark mode, deliberately.

**The motif.** Health is the evidence and the texture; automation is the offer. The health motif
lives in the landing's first leg (the clinic chaos: portals, order sheets, prescriptions), in the
molecule and clinical details of the visuals, and in the evidence copy (RPh, the clinic, the
prescription pipeline, the PH Pharmacy Setup Guide). It never appears in the service copy that says
what Jeyson will do for a reader. New visuals may lean clinical; new offer copy may not.

**Navigation.** The landing's nav button and footer go to Calendly
(`https://calendly.com/donjeysonofficial/30min`) and to `/work/`. The portfolio's logo links back
to `/`. Old portfolio deep links such as `/#projects` are forwarded to `/work/#projects` by a small
inline script at the top of the landing (GitHub Pages cannot do server redirects).

GitHub Pages serves `main` at the domain root. Edit, commit, push.

```
python -m http.server 8899 --bind 127.0.0.1   # then open http://127.0.0.1:8899
```

`fonts/`, `img/` and `assets/vendor/` are committed deliberately. A push that forgets the fonts
silently falls back to Georgia and 404s the preloads; a push that forgets the vendor folder breaks
every animation. Every screenshot is `.webp` at 1000px wide, and the `width`/`height` attributes are
the true intrinsic size, which stops lazy images from shifting the layout as they arrive.

## The CV

`cv.pdf` at the site root is `resume-kb.typ` from `Dropbox\Resumes\!Clinical-Data-Automation
Master Resume` compiled a second time, with one switch:

```
typst compile --font-path fonts --input web=1 resume-kb.typ /c/Users/PC/repos/anki-boi.github.io/cv.pdf
```

`web=1` drops the phone number from the header. The deliverable PDF keeps it; the published copy
does not, because this is a public, scrapable, permanent URL and the pages already obfuscate the
email address. Do not hand-edit `cv.pdf`. Rebuild it, or the two copies of the same source start
disagreeing. The site no longer publishes a rate, so the CV's rate line should go in the next
rebuild.

`check-truth.py` in the Resumes folder gates it (present, 1 page, no `+63`, still linked, and a
drift WARN against the KB master). Run it after touching either file.

## Content rules

Every claim on the site is checked against the owner's claims ledger before it ships. A claim goes
public only if it survived verification. Concretely, neither page claims:

- EHR/EMR experience (never worked in an Epic/Cerner/Athena system)
- ICD coding or SQL
- a reporting-time percentage from supervised contract work
- an "Admin Manager" title. The role is **Medical Ops & Process Automation Specialist**
- a bare **"HIPAA Certified"**. HHS/OCR warns about that phrasing, so the pages state the actual
  credential: **HIPAA Privacy & Security trained**. Do not upgrade it to CHP/CHPS unless the exam
  certificate exists.
- a price. Pricing is scoped per engagement on a call.

Also deliberately absent: a client testimonial, and a written response-time commitment. Do not
invent these. References stay "available on request" until a real one is written down.

No em dashes, en dashes or spaced hyphens used as dashes in any copy. Rewrite the sentence.

**Audience.** Direct clients, Australia first, the US second.

**Geography rule.** The employer's location appears only in the Experience section, where the fact
lives (Colorado Medical Solutions, and the US compounding pharmacies the orders go to). Availability
copy states the one defensible fact instead: Cagayan de Oro is UTC+8, the same clock as Perth and
1.5 to 2 hours behind the Australian east coast, so the overlap with any Australian business day is
full. No claims of Australian experience, AHPRA, Medicare or practice management software.

## Positioning

The site frames the owner as **AI-native and platform-agnostic**, and it does that by showing the
toolchain rather than asserting an adjective. Concretely:

- The hero lede says the suite is *maintained through an agent stack I run myself*: a capability
  statement, not an authorship one. It says who is accountable for the work, not who typed it.
- The About section carries the frame in one sentence: *agent orchestration and models served on my
  own machine are simply the toolchain I work in now, the way most people work in an editor.*
- The toolbox groups carry the receipts (Hermes orchestration, pi/DeepSeek harnesses, vLLM,
  llama.cpp, ComfyUI, AI video, KokoroTTS). The hero does not repeat them.
- The **H1 and the whole title family** read *AI-Native Workflow Automation*: `<title>`, `og:title`,
  `twitter:title`, `og:image:alt`, the nav brand subtitle, the footer and JSON-LD `jobTitle`. The
  phrase "Healthcare Workflow Automation" must not come back. It was the strongest scope signal on
  the site, much stronger than any single word in the body copy.

**Scope rule.** Healthcare is *evidence*, never the *offer*. It is allowed to name the employer, the
RPh credential, the clinic the scripts run in, and a project's own problem statement ("wrong-patient
entries", "patient-data handoffs"). It is not allowed in service-scope copy, the copy that says what
the owner will do for a reader. That copy says "your data", "your team", "your deadline".

Do not add a bare "AI-native" badge or hero chip. An unbacked positioning adjective is the same
failure as the old "HIPAA Certified" claim: the evidence has to be adjacent or the label reads as
decoration. The human accountability line (*every change tested, committed, and reviewed by a
human*) is doing the trust work and must not be diluted.

The script count is stated as `45+ in production, 40 published` because the remaining scripts carry
client identifiers and stay private. It appears twice on the portfolio (hero lede, clinical suite
heading) and nowhere else. Client and vendor names are never published, in markup, commit messages
or screenshots.

## Elsewhere

- **Book a call:** [calendly.com/donjeysonofficial/30min](https://calendly.com/donjeysonofficial/30min)
- **GitHub:** [github.com/anki-boi](https://github.com/anki-boi)
- **LinkedIn:** [linkedin.com/in/donjeysonmd](https://www.linkedin.com/in/donjeysonmd)
- **Email:** donjeysonofficial@gmail.com
