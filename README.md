# anki-boi.github.io

Source for **[anki-boi.github.io](https://anki-boi.github.io/)** — the portfolio of Jeyson Dagondon, RPh.

Healthcare workflow automation: I run the operations layer of a US telehealth clinic and build the
browser automation that removes the manual work from it. 45+ scripts and extensions in production
(40 of them published), Zoho CRM, HIPAA-aware.

## What's here

A single self-contained `index.html` (~74 KB) with no build step, no CDN and no external JS.
Self-hosted [Fraunces](https://fonts.google.com/specimen/Fraunces) (display) and
[Inter](https://fonts.google.com/specimen/Inter) (body) in `fonts/`, warm-paper and deep-ink
palette, scroll-driven reveals with a positional backstop, and `prefers-reduced-motion` covering
the infinite animations rather than just the entrances.

GitHub Pages serves `main` at the domain root. Edit `index.html`, commit, push.

```
python -m http.server 8899 --bind 127.0.0.1   # then open http://127.0.0.1:8899
```

`fonts/` and `img/` are committed deliberately — a push that forgets the fonts silently falls back
to Georgia and 404s both preloads.

## Content rules

Every claim on the site is checked against the owner's claims ledger before it ships. A claim goes
public only if it survived verification. Concretely, this page does not claim:

- EHR/EMR experience (never worked in an Epic/Cerner/Athena system)
- ICD coding or SQL
- a reporting-time percentage from supervised contract work
- an "Admin Manager" title — the role is **Medical Ops & Process Automation Specialist**

The script count is stated as `45+ in production, 40 published` because the remaining scripts carry
client identifiers and stay private. Client and vendor names are never published, in markup, commit
messages, or screenshots.

## Elsewhere

- **GitHub:** [github.com/anki-boi](https://github.com/anki-boi)
- **LinkedIn:** [linkedin.com/in/donjeysonmd](https://www.linkedin.com/in/donjeysonmd)
- **Email:** donjeysonofficial@gmail.com
