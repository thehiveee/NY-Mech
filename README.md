# New York Mechanical — website

Static site, ready for GitHub + Vercel. Upload the contents of this folder to the repo root.

## Before going live: replace these tokens

| Token | Where | What to put |
|---|---|---|
| `{{WEB3FORMS_ACCESS_KEY}}` | index.html | Access key from web3forms.com (set the client's inbox as the recipient) |
| `{{GA4_MEASUREMENT_ID}}` | index.html (2 places) | GA4 ID, e.g. `G-XXXXXXXXXX` |
| `{{POLICY_EFFECTIVE_DATE}}` | privacy.html, terms.html | Date the policies go live |
| `{{RETENTION_PERIOD}}` | privacy.html | e.g. "12 months" |

## Needs a human

- **Legal review:** privacy.html and terms.html are drafts. Have a US attorney review them before launch.
- **Confirm with the client:** "price before work starts", "5-star rated", "licensed & insured", "$0 down" financing, "free estimates on new systems", and the Bronx being served.
- **Domain:** in Vercel, add both `newyorkheating.net` and `www.newyorkheating.net`. HTTPS certificates are automatic. vercel.json redirects the bare domain to www and sends HSTS.
- **After launch:**
  - Submit `https://www.newyorkheating.net/sitemap.xml` in Google Search Console.
  - Run the live URL through pagespeed.web.dev.
  - In GA4, mark `generate_lead` and `phone_call` as key events.

## Design system

One typeface: **Schibsted Grotesk** (400–900), loaded from Google Fonts. Display
sizes run at weight 900 with tight negative tracking; body is 400/500. There is
no second family — hierarchy comes from scale and weight.

Structure is built from **hairline rules, not cards**. Sections are divided by
1px lines and share a single container grid, so the page reads as one piece of
architecture rather than a stack of boxes.

### Motion

| Where | What | Why |
|---|---|---|
| Hero thermostat | Dial sweeps up on load, then changes when the visitor picks a problem (54° for no heat, 86° for AC) | The signature moment, and it answers the visitor's own action. The dial illustrates the problem — it never claims to read the visitor's own thermostat |
| Hero headline | Three lines wipe up from a mask, staggered | One orchestrated page-load sequence |
| Header | Condenses and goes opaque once you scroll past the hero | Keeps the logo and nav legible over page content |
| Sticky call banner | Fixed to the bottom at all times, on every screen size; slides up once on load | The call button lives here instead of the header, so the primary action is never off screen |
| Call button | Breathes continuously — a 2.4s scale with an expanding ring; pauses on hover | Keeps drawing the eye to the one action that matters |
| Services | Red and blue top rules draw in; rows tint on hover | Reinforces heating/cooling as the organising idea |
| Stats | Count up once when scrolled into view | Draws the eye to the proof numbers |
| Area ticker | Continuous marquee, pauses on hover | Shows coverage without a list |
| Portrait image | Slow parallax | Depth, desktop only |
| FAQ | Height-animated accordion with a plus/minus toggle | Shows what changed |
| Exit intent | A dialog when the cursor leaves the top of the window | Catches desktop visitors about to close the tab with the phone number and a link to the form |

Everything is wrapped in `prefers-reduced-motion: reduce`, which disables all of
it and jumps to final states. The thermostat, counters and parallax each check
the same preference in JavaScript.

## Brand palette (taken from the logo)

| Token | Hex | Used for |
|---|---|---|
| `--ink` | `#222021` | Logo black — dark sections, headings, secondary buttons |
| `--red` | `#D42C2E` | Logo red (HEATING) — primary "Call" buttons, white text on top |
| `--blue` | `#3687C4` | Logo blue (COOLING) — accents, focus rings, cooling icons |
| white | `#FFFFFF` | Page background |

Because pure red and blue are too light for small text, the CSS also defines
darker text variants (`--red-t #C0252A`, `--blue-d #246087`) and lighter ones for
use on black (`--red-l #F0595C`, `--blue-l #6FB3E0`). All pass WCAG AA.
Red always means "call us"; blue is used for everything else.

## Logo files

The client supplied an updated logo (NYM with the flag, "HEATING & AIR
CONDITIONING INC."). It was lifted off its dark background into a clean
transparent cut-out and upscaled; files live in `logo-exports/`:

| File | Use |
|---|---|
| `nym-lockup-1x.png` | The lockup from the sharper second source (1056×592) |
| `nym-lockup-{2,3}x.png` | Upscaled, to 3168×1776 |
| `nym-logo-1x.png` | Full logo from the first source, lower resolution (636×492) |
| `nym-logo-{2,3,4}x.png` | Upscaled versions of that first source |
| `nym-logo-full.png` | Everything, including phone number and web address |
| `nym-logo-lockup.png` | Phone, web address and stars removed — this is what the site uses |
| `nym-logo-lockup-stars.png` | Phone and web address removed, five stars kept |
| `nym-logo-mark.png` | The "NYM" emblem alone — app icons, social avatars |

The client later supplied a tighter, higher-resolution copy of the same artwork
(about 1.7× the lettering resolution), and the site now uses that. It only
covers NYM, "HEATING & AIR" and "CONDITIONING INC." — its star row is cut off by
the frame — so the full lockup including phone number and web address still
comes from the first file.

The site uses `nym-logo-lockup` in the header, the footer and the social image —
the phone number and web address are stripped out, since they are illegible at
logo size and the page states both anyway. A version keeping the five stars is
included; at web sizes the stars reduce to a smudge, which is why the site does
not use it. Assets are served as WebP with a PNG fallback at roughly 2–3×
display size.

**The logo is embedded in the pages themselves**, as a data URI defined once in
CSS and used by both the header and the footer. It was previously an `<img>`
pointing at `/images/logo-lockup.webp`, which only resolves once the whole folder
is deployed — so the logo showed as a broken image when `index.html` was opened on
its own. Inlining costs about 42KB per page and removes a request; the external
files are still in `images/` if you would rather link to them.

**The artwork is white**, so it only works on dark backgrounds — which is why the
site masthead is dark. If the client can supply the original vector (.ai, .eps or
.svg), that would give a sharper result at large sizes than any upscale.

## What's included

| Item | How it's handled |
|---|---|
| Privacy / Terms | `/privacy`, `/terms` (clean URLs via vercel.json) |
| Secrets | None in code. The Web3Forms key and GA4 ID are public by design. |
| Force HTTPS | Vercel default, plus an HSTS header in vercel.json |
| Cookie consent | Banner using Google Consent Mode v2; nothing tracked until the visitor accepts. "Cookie settings" link in the footer. |
| Meta | Title, description and canonical on every page, plus LocalBusiness (HVACBusiness) schema |
| Social preview | images/og-image.jpg (1200×630), with OG and Twitter tags |
| Favicon | favicon.svg / .ico / 32px, apple-touch-icon, 512px icon, web manifest |
| SEO files | sitemap.xml, robots.txt |
| Alt text | Descriptive alt text on every image; decorative icons hidden from screen readers |
| Images | Own assets compressed. Gallery photos are requested at display size from the client's CDN with lazy loading. For best speed, download the originals into /images as WebP later. |
| Speed | Hero image preloaded, explicit image dimensions (no layout shift), fonts use `display=swap`, long cache on static assets |
| Contrast | Every text/background pair in the system was measured and passes WCAG AA, placeholders included. |
| Mobile / iPhone | `viewport-fit=cover` with safe-area insets on the container, call bar, cookie card and exit dialog, so nothing sits under the notch or home indicator in either orientation. Form inputs are exactly 16px — below that iOS Safari zooms the page on focus. Touch hit areas are enlarged under `@media (pointer:coarse)` rather than a width query, so a phone in landscape still gets them. No `100vh` anywhere (it breaks against the Safari toolbar). Checked at 375×667, 375×812, 393×852, 430×932 and landscape, plus iPad, with no horizontal scroll. |
| 404 | 404.html, which Vercel serves automatically |
| Links | All internal links and anchors checked, plus the borough pages |
| Form validation | Inline errors, 10-digit US phone check with auto-formatting, 5-digit ZIP, and an error message if sending fails |
| Spam protection | Honeypot field plus a minimum fill-time check (bots are silently dropped) |
| Exit intent | Desktop only (needs a hover-capable mouse), armed 4 seconds after load, shown at most once a week per visitor, and skipped entirely once the form has been submitted. Closes on Escape, the X, or a click outside; focus is trapped inside while open and returned afterwards. |
| Analytics | GA4 events: `phone_call` (by placement: `call_hero`, `call_stickybar`, `call_exit`, `call_emergency`, `call_final`, `call_footer`), `text_click`, `generate_lead`, `exit_intent_shown` |
| One CTA | Red is used only for "Call". The sticky bottom banner carries it site-wide; the estimate form is the secondary action. |
