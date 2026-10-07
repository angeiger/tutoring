# Einfach Chemie – project rules for coding agents

This repository is the website **einfachchemie.de**, a small-group chemistry tutoring business run by Andreas Geiger. The website is in **German**; code, comments, commit messages and PR descriptions are in **English**.

## How deployment works (read first)

- Hosting: netcup Webhosting 1000 NUE (Plesk, Apache, PHP available), data centre in Nuremberg. HTTPS via Let's Encrypt with HTTP→HTTPS redirect is already active.
- Plesk **pulls `main` from GitHub automatically** (webhook on every push) and copies the repository as-is into `/einfachchemie.de/httpdocs`. There is **no build step on the server**.
- The web server's document root is **`frontend/`**. Only files in `frontend/` are publicly reachable.
- `backend/` sits outside the document root. Use it for PHP includes and configuration that must not be downloadable.
- **`main` is production.** Never push to `main` and never merge into `main` yourself. Work on a feature branch, push it, open a pull request, and let Andreas merge.

## Hard rules

- The repository is **public**. Never commit passwords, API keys, private notes, or Andreas's home address. The only postal address that may appear anywhere is the Impressum service address that Andreas provides.
- Never commit personal data of students or enquirers (no real form submissions, no test e-mails with real addresses).
- **No external requests from the website:** no Google Fonts, no CDNs, no analytics, no embedded maps, videos or social widgets, no reCAPTCHA. Fonts, scripts, styles and images are served from our own domain. This is why the site needs no cookie banner – keep it that way. No cookies at all.
- Only original texts and graphics. No figures copied from textbooks or other websites, no stock photos with unclear licences. Bundled fonts need an OFL/compatible licence file in the repo.
- Keep it simple: plain HTML, one CSS file, minimal vanilla JavaScript, and PHP only where the server must do something (the enquiry form). No frameworks, no bundlers, no npm dependencies required at runtime. The site must work with JavaScript disabled (JS only enhances).
- Anything that needs Andreas's decision gets a visible marker `TODO(Andreas): …` in the code or text and is listed in the PR description. Do not invent facts about Andreas (grades, experience, dates, prices) that are not in the brief.

## Conventions

- Mobile first. Most visitors arrive from a QR code on a flyer, so the phone layout is the primary layout. Check 360 px, 768 px and 1280 px widths.
- Accessibility: semantic HTML, `lang="de"`, one `h1` per page, labelled form fields, visible focus styles, skip link, WCAG AA contrast, `prefers-reduced-motion` respected. Support light and dark mode via `prefers-color-scheme`.
- Styles and scripts live in external files (`frontend/assets/css/`, `frontend/assets/js/`) so a strict Content-Security-Policy works. No inline `<style>`, `<script>` or `style=""`.
- German copy: address readers with **du**, short sentences, concrete and friendly, no marketing hype. Use correct German typography („…“, –, non-breaking spaces before units like „90 Min.“).
- Local preview: `php -S localhost:8000 -t frontend` (or any static server for pages without PHP).
- Small, logical commits. End commit messages with the attribution lines your harness specifies, if any.

## Contact and identity facts

- Business name: **Einfach Chemie** (wordplay „Ein Fach: Chemie“ is welcome).
- E-mail: **kontakt@einfachchemie.de** (mailbox on netcup).
- Visual identity so far: benzene-ring hexagon logo (see the old placeholder in git history), teal accent `#0f766e` (dark mode `#2dd4bf`), warm off-white background `#f7f5f0`, ink `#1c2b2a`.
