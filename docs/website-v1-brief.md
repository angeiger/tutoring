# Brief: build version 1 of einfachchemie.de

You are building the first real version of the website for **Einfach Chemie**, a small-group chemistry tutoring business. Read `CLAUDE.md` first – its rules (deployment, public repo, no external requests, branch + PR workflow) apply to everything below.

**Goal:** a static, fast, trustworthy one-page site that answers three questions within ten seconds – *What is offered? Why this tutor? How do I book?* – plus the legally required pages and a working enquiry form. Replace the current "Bald online" placeholder.

**Deliverable:** a pull request from branch `website-v1` into `main`, with screenshots and a list of open `TODO(Andreas)` items. Do not merge it.

---

## 1. The business (source of truth for all copy)

**Who:** Andreas Geiger, chemist.
- M.Sc. Biomedizinische Chemie, Johannes Gutenberg-Universität Mainz
- B.Sc. Chemie, Goethe-Universität Frankfurt
- Abitur in Hessen with Chemie as Leistungskurs – he knows the Hessian Landesabitur from the inside
- Research: master's thesis in antibiotics research; research internships in AI-assisted drug design and medicinal chemistry; bachelor's thesis on red-shifted photocages
- About to start a PhD in organic chemistry in Frankfurt (`TODO(Andreas)`: confirm wording and whether to name the university)
- Do **not** publish grades unless Andreas confirms (`TODO(Andreas)`).

**What:** chemistry tutoring in small groups for people who *have to pass* chemistry as a required subject. Seasons are arranged so university and Abitur courses don't overlap.

**Target groups (show exactly these four):**

| Group | Offer | When | Group size |
|---|---|---|---|
| Pharmazie students (Goethe-Uni Frankfurt) | Exam preparation for the chemistry modules: weekly group plus crash course before exams | weekly groups Nov–Jan, crash course before the exams | 4–8 |
| Medizin, Vorklinik | Crash course for the chemistry exam | before the exam | 6–10 |
| Biologie students | Crash course organic chemistry | before the exam | 6–10 |
| Abitur Chemie GK/LK in Hessen | Crash course for the Landesabitur | Easter holidays 2027 | 4–8 |

Do not list or target students of Chemie, Biochemie or Lehramt Chemie (conflict of interest). Simply leave them out; don't explain why on the site.

Exact module names, exam dates and course dates are not fixed yet – show "Termine folgen – jetzt unverbindlich anfragen" and mark `TODO(Andreas)`.

**Prices (test prices, per person):**

| Format | Group size | Price |
|---|---|---|
| Kostenlose Probestunde, 90 Min. (online) | open | free |
| Wochengruppe, 90 Min. | 4–8 | 20 € pro Termin |
| Klausur-Crashkurs, 2 Tage × 5 Std. | 6–10 | 100 € |
| Abitur-Osterkurs, 4 Tage × 3 Std. | 4–8 | 160 € |

Prices are final prices; as a small business under § 19 UStG no VAT is charged – say „Kleinunternehmer gemäß § 19 UStG, daher keine Umsatzsteuer“ once near the prices.
Planned discounts (`TODO(Andreas)` confirm before showing): 10 € off per referred person who books; a discount for members of cooperating Fachschaften.

**Format – online and in person:**
- In-person teaching in Frankfurt is the goal (better learning). At the start, online is the main route: video call (Jitsi Meet) plus a drawing tablet for live structures and mechanisms. No account needed for participants, just a link.
- A group moves to in-person sessions once it has about 6 participants (room rent). Location `TODO(Andreas)` (e.g. a seminar room near Campus Riedberg).
- Participants from outside Frankfurt (e.g. Abitur students elsewhere in Hessen) stay online.

**Abitur content (Kerncurriculum gymnasiale Oberstufe Hessen, version 2024) – use for the Abitur card/section:**
- Q1 Stoffgruppen: chemische Bindungen und Strukturen, Alkanole und Carbonylverbindungen, Alkansäuren und ihre Derivate
- Q2 Naturstoffe und Synthesechemie: Naturstoffe, Grundlagen der Kunststoffchemie
- Q3 Chemisches Gleichgewicht: Gleichgewichte und ihre Einstellung, Protolysegleichgewichte, Redoxgleichgewichte
- Q4 Energie und Nachhaltigkeit: energetische und kinetische Aspekte chemischer Reaktionen
- Exam 2027: choose 3 of 4 proposals; LK 300 min, GK 255 min; only the official enclosed Formelsammlung is allowed from 2027.
- Landesabitur Chemie: 21 April 2027; Hessian Easter holidays most likely 22 March – 2 April 2027 (`TODO(Andreas)`: verify both before publishing dates).
- Under-18s: the contract is concluded with a parent/guardian.

**How booking works (show as 4 steps):** Anfrage über das Formular → Bestätigung per E-Mail mit Termin und Zahlungsinfos → Bezahlung (Vorkasse, `TODO(Andreas)`: Überweisung and/or PayPal) → Teilnahme online oder vor Ort.

---

## 2. Site structure

```
frontend/
  index.html               one-pager (sections below)
  teilnahmebedingungen.html
  impressum.html
  datenschutz.html
  widerruf.html            withdrawal information + withdrawal function (see 4.4)
  danke.html               shown after a successful enquiry
  404.html
  anfrage.php              enquiry form handler
  widerruf.php             withdrawal form handler
  .htaccess
  robots.txt
  sitemap.xml
  assets/css/style.css
  assets/js/main.js
  assets/img/              logo.svg, favicon.svg, apple-touch-icon.png, og-image.png, portrait placeholder
  assets/fonts/            optional, self-hosted woff2 + licence
backend/
  config.php               recipient address, sender address, dev-mode switch, limits
  lib/                     shared PHP helpers (validation, mail sending, rate limit)
```

`anfrage.php` loads config with `require __DIR__ . '/../backend/config.php';`. If the server's `open_basedir` blocks this after deployment, fall back to a `frontend/_private/` folder protected by `.htaccess` (`Require all denied`) and note it in the PR.

### One-pager sections (`index.html`)

1. **Header:** logo + „Einfach Chemie“, compact nav (Kurse, Ablauf, Über mich, FAQ, Anfrage). Mobile: simple disclosure menu that also works without JS (e.g. anchor list or `<details>`).
2. **Hero:** one sentence on what this is and for whom, e.g. „Chemie-Nachhilfe in kleinen Gruppen – für Pharmazie, Medizin, Biologie und das Chemie-Abitur in Hessen.“ Primary button „Kostenlose Probestunde anfragen“ → `#anfrage`. Optional playful line with „Ein Fach: Chemie“.
3. **Kurse & Preise:** one card per target group (offer, period, format online/vor Ort, group size, price) plus a card for the free trial session. Cards must be easy for Andreas to edit by hand: mark each with an HTML comment like `<!-- KURS: Pharmazie -->`.
4. **So läuft's ab:** the 4 booking steps.
5. **Online & vor Ort:** the format explanation above, short.
6. **Über mich:** photo placeholder (`TODO(Andreas)`: own photo), degrees, research background, why he teaches (`TODO(Andreas)`: 2–3 sentences in his own words – draft a neutral placeholder and mark it).
7. **FAQ** (`<details>` elements): Wie groß sind die Gruppen? Online oder vor Ort? Was kostet es? Welche Vorkenntnisse brauche ich? Was muss ich mitbringen (online: Laptop/Tablet, Headset)? Was passiert, wenn ich absagen muss? (link to Teilnahmebedingungen) Ich bin unter 18 – geht das?
8. **Anfrage:** the enquiry form (4.2).
9. **Footer:** © year Einfach Chemie, kontakt@einfachchemie.de, links: Impressum, Datenschutz, Teilnahmebedingungen, Widerruf / „Vertrag widerrufen“.

---

## 3. Design

- Continue the placeholder's identity: benzene-ring hexagon logo, teal accent `#0f766e` (dark mode `#2dd4bf`), warm off-white `#f7f5f0`, ink `#1c2b2a`. Light and dark mode via `prefers-color-scheme`.
- Feel: clear, calm, competent, friendly – a young scientist who explains well, not a school-like franchise and not a flashy startup. Generous whitespace, strong typography, few colours.
- Chemistry motifs only as subtle, self-made SVG line art (hexagon grid, skeletal formula strokes, a reaction arrow as a divider). No clip-art, no stock photos.
- Fonts: system font stack, or one self-hosted variable font with an OFL licence (e.g. a humanist sans) in `assets/fonts/` with its licence file. Never load fonts from Google.
- Mobile first; the hero and the CTA must be visible without scrolling on a 360×740 phone.
- Small motion only (hover/focus), disabled under `prefers-reduced-motion`.

---

## 4. Functionality

### 4.1 General
- Every page: `<title>`, meta description, canonical URL (`https://einfachchemie.de/...`), Open Graph tags with `og-image.png` (1200×630, generate from an SVG you design), favicon SVG + PNG fallback.
- Remove the placeholder's `noindex`. Add `robots.txt` and `sitemap.xml`. Mark `danke.html` and `404.html` as `noindex`.
- `.htaccess`: `ErrorDocument 404 /404.html`; `Options -Indexes`; deny access to dotfiles except `.well-known` (Let's Encrypt renewal must keep working); long cache headers for `/assets/`; security headers: `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` (camera, microphone, geolocation off), `Content-Security-Policy: default-src 'self'; img-src 'self' data:; style-src 'self'; script-src 'self'; form-action 'self'; frame-ancestors 'none'; base-uri 'self'`. Wrap directives in `<IfModule>` where possible; note in the PR that Andreas must check the live site for HTTP 500 after deploying. No HSTS yet.

### 4.2 Enquiry form (`#anfrage` → `anfrage.php`)
Fields:
- Name (required)
- E-Mail (required, `type="email"`)
- Ich bin … (required select): Pharmazie-Student:in · Medizin-Student:in · Biologie-Student:in · Schüler:in (Abitur Hessen) · Elternteil · Sonstiges
- Interesse (required select): Kostenlose Probestunde · Wochengruppe · Klausur-Crashkurs · Abitur-Osterkurs · Allgemeine Frage
- Bevorzugt: online · vor Ort in Frankfurt · egal
- Nachricht (optional, max 2000 chars)
- „Wie hast du von mir erfahren?“ (select): Fachschaft · Flyer/Aushang · Instagram · Freunde/Empfehlung · Google · Sonstiges
- Hidden `src`: `main.js` copies the `?src=` query parameter (e.g. `?src=flyer-riedberg`, allowed chars `[a-z0-9-]`, max 40) into this field. This is how marketing channels are measured – no analytics tool.
- Notice under the button (no checkbox): „Mit dem Absenden werden deine Angaben zur Bearbeitung deiner Anfrage verwendet. Mehr in der [Datenschutzerklärung].“ Plus: „Du bist unter 18? Dann frag bitte gemeinsam mit einem Elternteil an.“

Handler requirements (`anfrage.php`):
- POST only; server-side validation of every field (whitelist the select values); trim and length limits.
- Spam protection without third parties: honeypot field (visually hidden, `tabindex="-1"`, `autocomplete="off"`), minimum fill time via a timestamp field (reject < 3 s), simple per-IP rate limit (e.g. max 5 per hour, stored in a writable dir outside the docroot if available – otherwise skip and note it).
- Reject CR/LF in name and e-mail (header injection). Validate e-mail with `filter_var`.
- Send **one** plain-text e-mail via PHP `mail()` to `kontakt@einfachchemie.de`, `From: Einfach Chemie <kontakt@einfachchemie.de>`, envelope sender `-fkontakt@einfachchemie.de`, `Reply-To:` the enquirer, UTF-8 with `mb_encode_mimeheader` for the subject. Subject: `Anfrage: {Interesse} – {Name}`. Body lists all fields incl. `src`, date/time.
- **No automatic reply to the enquirer** (would let bots send mail to arbitrary addresses).
- Success → `303` redirect to `/danke.html`. Failure → a simple German error page with a link back; never echo raw input without `htmlspecialchars`.
- Store nothing on the server (no database, no log of submissions).
- Dev mode (`backend/config.php`, auto-on when `SERVER_NAME` is `localhost`/`127.0.0.1`): instead of sending, write the would-be e-mail to `backend/dev-mail.log` (git-ignored).
- PHP version: write for PHP 8.1+. `TODO(Andreas)`: confirm in Plesk (Hosting-Einstellungen → Web-Scripting) that PHP ≥ 8.1 is active for einfachchemie.de.

### 4.3 Legal pages – drafts, clearly marked for review
Write complete German drafts, but put a visible review note in the PR (not on the page) that Andreas must check them, e.g. with the text generator included in his "Dein Impressum" subscription or a lawyer. Do not present them as legal advice.

**Impressum (`impressum.html`)**, „Angaben gemäß § 5 DDG“:
- Andreas Geiger – Einfach Chemie (Chemie-Nachhilfe)
- Address: `TODO(Andreas): Impressum-Adresse von Dein Impressum eintragen` – **never** a home address.
- Kontakt: E-Mail kontakt@einfachchemie.de; second fast contact route: link to the enquiry form.
- No VAT ID (Kleinunternehmer) – omit that line.
- Verbraucherstreitbeilegung: statement that he is neither willing nor obliged to take part in dispute resolution before a consumer arbitration board.
- Do **not** add the EU online dispute resolution (OS) platform link – the platform was shut down on 20 July 2025.

**Datenschutzerklärung (`datenschutz.html`):**
- Controller: Andreas Geiger, Impressum address (same TODO), kontakt@einfachchemie.de.
- Hosting: netcup GmbH, Emmy-Noether-Straße 10, 76131 Karlsruhe; servers in Nuremberg (Germany); data processing agreement (Art. 28 DSGVO) in place.
- Server log files (IP address, date/time, requested URL, referrer, user agent): Art. 6 Abs. 1 lit. f DSGVO; retention `TODO(Andreas)`: check the log retention in Plesk and fill in.
- TLS encryption.
- Enquiry form and e-mail: which data, purpose (answering the enquiry, arranging courses), legal basis Art. 6 Abs. 1 lit. b (pre-contractual) and lit. f; the „Wie hast du von mir erfahren?“ answer and the `src` parameter are used only to see which advertising channels work; the e-mail is stored in the mailbox at netcup; retention: deleted when no longer needed, at the latest when statutory retention periods (tax law for booked courses) end.
- Online lessons via Jitsi Meet: `TODO(Andreas)`: which instance (public meet.jit.si, operated by 8x8 Inc., USA – then explain the third-country transfer and its legal basis – or an EU-hosted instance). Draft both variants as HTML comments, show neither until decided.
- No cookies, no tracking, no external fonts or services – say so explicitly.
- Rights of data subjects (Art. 15–21 DSGVO), including the right to object (Art. 21) highlighted; right to lodge a complaint, naming the competent authority in Hessen: Der Hessische Beauftragte für Datenschutz und Informationsfreiheit (verify the current postal address).
- No obligation to provide data; no automated decision-making.

**Teilnahmebedingungen (`teilnahmebedingungen.html`)** – draft with every number marked `TODO(Andreas)`:
- Contract: enquiry is non-binding; the contract is concluded when Andreas confirms by e-mail.
- Payment: Vorkasse by `TODO` days before the course; weekly group billed per session or as a block (`TODO`).
- Cancellation by participant: free until 48 h before the session (suggested default), afterwards full price; a substitute participant may take the place.
- Minimum group size: the course only takes place with at least `TODO` participants; otherwise it is cancelled and payments are refunded in full.
- Cancellation by the tutor (e.g. illness): replacement date or full refund.
- Online sessions are not recorded without the explicit consent of everyone present; course materials are for personal use only.
- Minors: the contract is concluded with a parent/guardian.
- Reference the right of withdrawal (`widerruf.html`).

### 4.4 Right of withdrawal (`widerruf.html` + `widerruf.php`)
Courses are booked by consumers at a distance, so a 14-day right of withdrawal applies.
- Page with the statutory Widerrufsbelehrung for services (incl. the rule that, if the course should start within the withdrawal period, the participant must expressly request this and pays a proportional amount when withdrawing after the start) and the model withdrawal form.
- Since 19 June 2026, § 356a BGB requires a withdrawal function for distance contracts concluded via an online interface. It is unclear whether contracts concluded by e-mail after a form enquiry fall under it, so implement it anyway. It is cheap:
  - A link „Vertrag widerrufen“ in the footer of every page, leading to `widerruf.html#widerrufen`.
  - Step 1: form with name, contract identification (course name and date or booking confirmation), e-mail for the confirmation – nothing else, and no question about the reason.
  - Step 2: a confirmation step with a button labelled exactly „Widerruf bestätigen“.
  - Handler: same protections as `anfrage.php`. It sends the withdrawal to kontakt@einfachchemie.de **and** an immediate confirmation to the given e-mail with content, date and time of the withdrawal. The confirmation text is fixed; only name, contract reference and timestamp are inserted, escaped.
- Mark the page `TODO(Andreas)`: have the wording checked.

---

## 5. Workflow

1. Read `CLAUDE.md` and this brief. Inspect the repo (`git log`, file tree).
2. `git switch -c website-v1` from an up-to-date `main`.
3. Build in this order, committing after each step: skeleton + CSS design system → one-pager content → enquiry form + handler → legal pages → withdrawal function → `.htaccess`, robots, sitemap, icons, OG image → polish.
4. Verify before opening the PR:
   - Local preview with `php -S localhost:8000 -t frontend`. If PHP isn't installed locally, install it if that's quick, otherwise test static pages with any static server, run `php -l` wherever possible and say in the PR what could not be tested.
   - Screenshots at 360, 768 and 1280 px, light and dark mode (Playwright or similar if available) – attach them to the PR or commit them under `docs/screenshots/`.
   - Form handler: valid submission (dev log), each invalid case, honeypot, too-fast submission, header-injection attempt.
   - `grep -rn "http" frontend/` shows no external resource URLs (outgoing text links such as the Kultusministerium are fine, but no loaded resources).
   - All internal links and anchors resolve; HTML validates (e.g. `npx html-validate`, if available); no inline styles/scripts (CSP).
   - Lighthouse (if available): aim for ≥ 95 in Performance, Accessibility, Best Practices, SEO on mobile.
5. Push the branch and open a PR (`gh pr create`; if `gh` isn't available, push and print the compare URL). The PR description contains:
   - what was built, with screenshots,
   - every `TODO(Andreas)` with file and line,
   - a **launch checklist**: Impressum address filled in · legal texts reviewed · Jitsi decision made · PHP version checked · after merge: test the enquiry form with a real message to kontakt@einfachchemie.de, test the withdrawal form, check the live site for HTTP 500 errors from `.htaccess`.
6. Do not merge. Merging into `main` publishes the site immediately.

## 6. Out of scope for v1 (only if Andreas asks later)

QR codes per marketing channel (SVG, pointing to `https://einfachchemie.de/?src=<channel>`, stored outside `frontend/`), flyer and business-card layouts, testimonials, downloadable cheat sheets, a waiting list/newsletter (needs double opt-in), online payment, an English version, a parents' section.
