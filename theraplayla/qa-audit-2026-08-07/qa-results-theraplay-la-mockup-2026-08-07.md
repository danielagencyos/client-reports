# Website Go-Live QA Results — TheraPlay LA

**Audited:** 2026-08-07 · **Staging:** https://theraplay-la-mockup.vercel.app (Astro 6 + Tailwind 4, repo `danielagencyos/theraplay-la-mockup`, HEAD `e63b79b` = deployed) · **Replaces:** https://www.theraplayla.com (Squarespace, 90 sitemap URLs)
**Method:** read-only audit — full crawl of all 11 staging pages, sitewide link/image testing, source-code inspection, live-site cross-check, Semrush traffic data. **No forms were submitted** (no permission given; the form is inert anyway). Browser-rendering checks (breakpoints, console, real devices) had no tooling in this environment and are marked MANUAL.

---

## Summary

### Verdict: 🔴 **NO-GO**

The staging site is a well-built *mockup* — clean copy, consistent NAP, unique titles, zero broken links — but it is not yet a *launchable website*. The contact form sends data nowhere, there is no analytics, no redirects from 90 indexed URLs, no privacy policy, and the site still labels itself a mockup with fictional testimonials. Most blockers are 1–2 days of engineering; the content-loss decision (blog + shop) needs a team/client call.

### Scorecard

| Section | ✅ Pass | ❌ Fail | 🟡 Manual | ⚪ N/A |
|---|---|---|---|---|
| Content & Copy | 4 | 3 | 4 | 2 |
| Design & Layout | 3 | 5 | 8 | 0 |
| Forms | 4 | 10 | 0 | 2 |
| Navigation & Links | 6 | 3 | 1 | 2 |
| SEO & Meta | 6 | 8 | 2 | 0 |
| Technical & Performance | 5 | 3 | 7 | 0 |
| Accessibility | 2 | 6 | 3 | 1 |
| Tracking & Analytics | 2 | 2 | 5 | 3 |
| Security & Legal | 2 | 4 | 5 | 0 |
| Launch Day & Handover | 0 | 0 | 13 | 0 |
| **Total** | **34** | **44** | **48** | **10** |

### 🚨 Launch-blocking failures (Critical)

1. **Contact form is non-functional.** `<form onsubmit="event.preventDefault()">` — no action, no endpoint, no success state. Every enquiry is silently discarded while the page promises "We'll reach out within one business day."
   *Fix:* wire to Formspree/Basin/API route + styled success state + notification email to the client + spam protection.
2. **No 301 redirects — 89 of 90 live URLs would 404 at launch.** Semrush shows **~88% of organic traffic comes from blog posts** (palmar grasp 644 visits/mo, ATNR 585, Moro 499…) and the staging site has no blog, no shop, and no redirect config.
   *Fix:* ship the redirect map in [redirect-map.md](redirect-map.md) via `vercel.json`; decide the blog/shop migration plan (see below).
3. **Opening hours are wrong.** Staging says "Monday - Friday: 8am - 6pm / Weekend intensives by arrangement"; the live site's configured hours are **Mon–Fri 10am–6pm, weekends closed**. *Fix:* confirm with client, correct footer/contact page.
4. **Mockup content still live.** Footer on all 11 pages: "· Website mockup concept". Homepage testimonials are explicitly "Illustrative testimonials for mockup purposes" (fictional parents Jessica M., David R., Priya S.). *Fix:* remove label; replace with real, permission-cleared testimonials.
5. **No sitemap.xml and no robots.txt** (both 404). *Fix:* add `@astrojs/sitemap` + `site` URL in astro.config, static robots.txt.
6. **Zero analytics.** No GA4, no GTM, no conversion tracking of any kind. Launching means flying blind and losing the pre/post comparison. *Fix:* install GA4 (+ consent banner), define form/tel/mailto conversions.
7. **No privacy policy** — on a site whose form collects a child's name, age, and health concerns (CCPA applies; HIPAA-adjacent sensitivity). *Fix:* privacy policy page + footer link + form consent line before launch.
8. **The real booking flow is missing.** Live `/discovery-call` embeds a Google Calendar appointment scheduler; staging has no scheduler, only the dead form. *Fix:* embed/link the existing Google Calendar scheduler.

### Other failures (High)

- **SEO regression vs live:** no canonical tags (and `/about` + `/about/` both serve 200 with identical HTML), no JSON-LD (live has LocalBusiness schema today), staging is currently **indexable by Google** (no robots/noindex/auth on the vercel.app URL — risk of duplicate-content competition at launch).
- **Content loss needs a decision:** entire blog (35 URLs — the traffic engine), entire shop (28 URLs **including paid intake-screener products — a revenue line**), and 9 dedicated service pages (teletherapy, nutrition, School of Sleep ×2, primitive reflex integration, DMI, 2E support, in-school therapy, regional center) have no staging equivalent.
- **Images unoptimised:** 11 MB in `public/images`, zero WebP/AVIF, three PNGs over 1 MB (team-jonathon 1.35 MB, stock-teletherapy 1.24 MB, team-flo 1.04 MB), homepage payload ~1.5 MB, and images served with `cache-control: max-age=0`.
- **No social links anywhere on staging** (live links facebook.com/doctormarielly, instagram.com/theraplay_la, instagram.com/doctormarielly) and **no Google Maps link/embed** for the clinic address.
- **Footer legal links absent:** no privacy, no terms.
- **10 hero images have empty `alt=""`** (content-bearing clinic photos on every page's hero).
- **Keyboard access:** desktop Services dropdown is hover-only (unusable by keyboard), no skip link, no visible focus styles on nav.
- **404 page:** bad URLs return Vercel's bare plain-text error (correct 404 status, zero branding).
- **Form details (once wired):** no spam protection, no autocomplete attributes, no styled error states, no CRM integration.
- **Image licensing:** `stock-*.{jpg,png}` files and clinic/team photos were gathered from public sources for the mockup — licensing/permission must be cleared before commercial launch.
- **Claims needing client sign-off:** "4.5★ Google Rating", "13+ Years of Experience", "6 to 9 months of progress in as little as 3 weeks".

### Manual items for the team (cannot be verified from outside)

Browser/device: hero contrast, horizontal scroll @320–1440px, mobile menu behaviour, card grids, sticky header, truncation, 200% zoom, cross-browser (esp. Safari), real iOS+Android devices, console errors, print check.
Performance: PageSpeed Insights re-run (**keyless API quota was exhausted today — HTTP 429**; TTFB proxies were excellent: 154–230 ms).
Client/accounts: confirm phone (424) 392-4568 & canonical brand spelling ("TheraPlay LA" vs live schema's "TheraPlayLA"), hours, team roster ("Esther" has no surname), claim sign-offs, testimonial permissions, stock-photo licences, Vercel/GitHub 2FA & access roles, legal entity details, cookie policy once analytics added, GSC property + sitemap submission, GBP website link at launch, uptime monitoring, backup/rollback plan, and the entire Launch Day & Handover section.

---

## Full checklist

Legend: ✅ PASS · ❌ FAIL · 🟡 MANUAL (needs human/browser) · ⚪ N/A

## Content & Copy

- [x] **All copy proofread** ⚠️ CRITICAL — ✅ PASS: full visible text of all 11 pages extracted and read; no spelling/grammar errors found.
- [ ] **No placeholder text** ⚠️ CRITICAL — ❌ FAIL: no lorem/TBC/TODO, but footer says "· Website mockup concept" on every page and homepage testimonials are labelled "Illustrative testimonials for mockup purposes".
- [x] **Business name, phone, email correct** ⚠️ CRITICAL — ✅ PASS (with note): (424) 392-4568 / hello@theraplayla.com / identical on all 11 pages and match live site data. Note: live schema uses "TheraPlayLA" (one word), staging "TheraPlay LA" — pick one canonical form; confirm phone with client.
- [ ] **Address & opening hours correct** ⚠️ CRITICAL — ❌ FAIL: address matches (1070 S La Brea Ave, LA, CA 90019) but hours don't — staging "Mon–Fri 8am–6pm + weekend intensives" vs live's configured Mon–Fri **10am**–6pm, weekends closed.
- [ ] **Prices / offers accurate** (High) — 🟡 MANUAL: no prices shown; claims needing sign-off: "4.5★ Google Rating", "13+ Years of Experience", "6–9 months of progress in as little as 3 weeks", "free discovery call".
- [ ] **Tone matches brand** (High) — 🟡 MANUAL: no client style guide available to check against.
- [ ] **No truncated text** (High) — 🟡 MANUAL: needs browser at breakpoints.
- [x] **Dates & copyright year current** (Medium) — ✅ PASS: "© 2026 TheraPlay LA" (correct year; hardcoded — make dynamic).
- [ ] **Testimonials approved** (High) — ❌ FAIL: all three homepage testimonials are fictional mockup content; must be replaced with real, permission-cleared quotes.
- [ ] **Team bios & photos current** (Medium) — 🟡 MANUAL: 6 members listed (Dr. Marielly, Marci Silver, Esther, Jonathon Tenorio, Flo Lopez, Ana Lopez); "Esther" lacks a surname; verify roster/photos with client.
- [x] **CTAs consistent** (Medium) — ✅ PASS: "Book a Free Discovery Call" dominant; minor variants (header "Book a Discovery Call", form "Request a Free Discovery Call") are acceptable.
- [ ] **Downloadables work** (Medium) — ⚪ N/A: none on staging.
- [ ] **Empty states covered** (Medium) — ⚪ N/A: no blog/news section on staging.

## Design & Layout

- [ ] **Hero text readable** ⚠️ CRITICAL — 🟡 MANUAL: heroes use gradient overlays over photos; verify contrast at all breakpoints in browser.
- [x] **Fonts loading correctly** (High) — ✅ PASS (HTTP): Google Fonts CSS (Fraunces, Nunito Sans, Caveat) returns 200; render check manual. Consider self-hosting (only third-party dependency on the site).
- [x] **No broken images** ⚠️ CRITICAL — ✅ PASS: all 27 unique image URLs return 200 across all pages.
- [ ] **Images not distorted** (High) — 🟡 MANUAL: one rotated photo already fixed (commit e63b79b); visually re-check all.
- [ ] **Images optimised** (High) — ❌ FAIL: 11 MB in public/images, 0 WebP/AVIF, 3 PNGs > 1 MB, 11 more files 300 KB–1 MB, homepage payload ~1.5 MB, hero `loading="eager"` on every page, no `astro:assets` pipeline.
- [x] **Logo correct in header & footer** (High) — ✅ PASS (with note): links to homepage from every page, loads fine; it's a 167 KB PNG (25 KB mobile variant) — SVG/compressed version recommended; sharpness check manual.
- [ ] **No horizontal scroll** ⚠️ CRITICAL — 🟡 MANUAL: needs browser at 320/375/768/1024/1440px.
- [ ] **Card grids even** (Medium) — 🟡 MANUAL.
- [ ] **Section rhythm consistent** (Medium) — 🟡 MANUAL.
- [ ] **Sticky header behaves** (Medium) — 🟡 MANUAL.
- [ ] **Hover & focus states** (Medium) — ❌ FAIL: form inputs have focus rings; nav links/buttons rely on browser defaults, no global `:focus-visible` styling.
- [ ] **Animations behave** (Medium) — ❌ FAIL: `.fade-in-up` starts at `opacity:0` driven by IntersectionObserver — content is invisible if JS fails; no `prefers-reduced-motion` support.
- [ ] **Brand colours only** (Medium) — 🟡 MANUAL: Tailwind theme tokens defined; visual sweep needed.
- [ ] **Favicon & touch icons** (Medium) — ❌ FAIL: only a PNG `rel="icon"` (logo file); `/favicon.ico` 404, no apple-touch-icon, no web manifest.
- [ ] **404 page designed** (High) — ❌ FAIL: no `404.astro`; bad URLs get Vercel's bare plain-text "NOT_FOUND" (correct 404 status, zero branding/links).
- [ ] **Print check** (Low) — 🟡 MANUAL.

## Forms

- [ ] **Every form submits** ⚠️ CRITICAL — ❌ FAIL: `<form onsubmit="event.preventDefault()">`, no action/endpoint/fetch anywhere — submissions are discarded.
- [ ] **Notifications arrive** ⚠️ CRITICAL — ❌ FAIL: nothing is ever sent; no recipient configured.
- [ ] **Reply-to set correctly** (High) — ❌ FAIL: no backend exists.
- [ ] **Success state shows** (High) — ❌ FAIL: submit does nothing visually — no message, no thank-you page.
- [ ] **Thank-you page tracked** (High) — ❌ FAIL: no thank-you page and no conversion event.
- [x] **Validation works** (High) — ✅ PASS: native `required` on name/email/phone + `type=email`/`type=tel` (browser-default messages only).
- [ ] **Error states styled** (Medium) — ❌ FAIL: no custom inline error styling.
- [ ] **Spam protection on** (High) — ❌ FAIL: no reCAPTCHA/Turnstile/honeypot.
- [ ] **Test submissions removed** (Medium) — ⚪ N/A: none submitted during audit.
- [x] **Labels & placeholders** (Medium) — ✅ PASS: every field has a visible `<label for>`; placeholders are guidance only.
- [x] **Focus states visible** (Medium) — ✅ PASS: `focus:ring-2` on all inputs.
- [x] **Mobile-friendly inputs** (High) — ✅ PASS: correct email/tel input types (tap-size check manual).
- [ ] **Autofill works** (Low) — ❌ FAIL: no `autocomplete` attributes on any field.
- [ ] **CRM / integration firing** (High) — ❌ FAIL: no CRM/email-platform integration.
- [ ] **Consent handled** (High) — ❌ FAIL: no consent checkbox or privacy link — form collects a child's name/age/health concerns.
- [ ] **File uploads work** (Medium) — ⚪ N/A.

## Navigation & Links

- [x] **All nav links work** ⚠️ CRITICAL — ✅ PASS: every header/footer/in-page link resolves 200; no `#` stubs, no empty hrefs.
- [x] **No broken links sitewide** ⚠️ CRITICAL — ✅ PASS: sitewide crawl — 13 unique hrefs + 27 images + CSS + fonts all 2xx; zero redirect chains.
- [x] **Active page highlighted** (Low) — ✅ PASS: current nav item styled `text-brand-primary` (add `aria-current="page"` for screen readers).
- [ ] **Mobile menu works** ⚠️ CRITICAL — 🟡 MANUAL: JS toggle looks sound in code but has no `aria-expanded`, no Escape-close, no focus trap; browser test required.
- [x] **Logo links home** (Medium) — ✅ PASS: from every page.
- [ ] **External links behave** (Medium) — ❌ FAIL: there are **zero external links** — no social profiles (live has 3) and no Google Maps link on the address.
- [x] **Phone & email links** (High) — ✅ PASS: `tel:+14243924568` and `mailto:hello@theraplayla.com` consistent sitewide (confirm number with client).
- [ ] **Anchor links land correctly** (Medium) — ⚪ N/A: no in-page anchors.
- [ ] **Breadcrumbs correct** (Low) — ⚪ N/A.
- [ ] **Footer links complete** (High) — ❌ FAIL: no privacy policy, no terms, no legal links at all.
- [ ] **Redirects in place** ⚠️ CRITICAL — ❌ FAIL: 89/90 live URLs 404 on staging (only `/contact` matches); no redirect config exists. Full proposed map: [redirect-map.md](redirect-map.md).
- [x] **No orphan pages** (Medium) — ✅ PASS: all 11 pages reachable from nav/footer.

## SEO & Meta

- [x] **Unique title tags** ⚠️ CRITICAL — ✅ PASS: all 11 unique/descriptive; homepage runs 78 chars (trim recommended).
- [x] **Unique meta descriptions** (High) — ✅ PASS: all unique; 4 pages exceed ~160 chars (myofunctional 169, OT 179, speech 170, team 168).
- [x] **One H1 per page** (High) — ✅ PASS: exactly one per page, all unique (heading-order issue on /team logged under Accessibility).
- [x] **Indexing enabled** ⚠️ CRITICAL — ✅ PASS: no noindex/nofollow anywhere; nothing to remove at launch.
- [ ] **Staging blocked** (High) — ❌ FAIL: the vercel.app mockup is fully indexable **right now** (no robots.txt, no noindex, no auth) — add noindex until launch, then remove.
- [ ] **XML sitemap live** ⚠️ CRITICAL — ❌ FAIL: /sitemap.xml 404 (no sitemap integration, no `site` in astro.config).
- [ ] **Search Console verified** ⚠️ CRITICAL — 🟡 MANUAL: to be done at launch (property, sitemap submission, coverage watch).
- [ ] **Canonical tags correct** (High) — ❌ FAIL: no canonical on any page; `/about` and `/about/` both 200 with identical HTML (no trailingSlash policy).
- [ ] **Redirects preserve equity** ⚠️ CRITICAL — ❌ FAIL: Semrush — blog posts = ~88% of organic traffic (top page 644 visits/mo), all 404 with no redirect; see [redirect-map.md](redirect-map.md).
- [ ] **Schema markup valid** (High) — ❌ FAIL: zero JSON-LD on staging (live site has LocalBusiness schema — this is a regression). Add LocalBusiness/MedicalClinic + FAQPage (5 pages have FAQ sections).
- [ ] **Open Graph & Twitter cards** (Medium) — ❌ FAIL: no og:/twitter: tags on any page — shares render bare.
- [ ] **Image alt text** (High) — ❌ FAIL: 10 hero images (content-bearing clinic photos) have `alt=""`; all other images have descriptive alts.
- [x] **URL structure clean** (Medium) — ✅ PASS: lowercase, hyphenated, logical `/services/*` hierarchy.
- [ ] **No duplicate content** (High) — ❌ FAIL: trailing-slash duplicates (above); www/http canonicalization is DNS/launch-day config.
- [ ] **Google Business Profile** (High) — 🟡 MANUAL: update GBP website link at launch; NAP on site matches GBP address per research.
- [x] **Local landing pages** (Medium) — ✅ PASS: 5 service pages target distinct keywords ("pediatric occupational therapy los angeles", etc.); note lost dedicated pages (reflex integration, DMI, 2E) reduce coverage.

## Technical & Performance

- [x] **Production build clean** ⚠️ CRITICAL — ✅ PASS (by deployment evidence): Vercel serves all 11 pages from HEAD `e63b79b`; repo clean and matches origin/main. (Local build not run — no Node in audit env.)
- [x] **HTTPS everywhere** ⚠️ CRITICAL — ✅ PASS (staging): valid cert, HSTS w/ preload, no mixed content (only external ref is Google Fonts over https). Production domain re-check at launch.
- [ ] **Domain & DNS correct** ⚠️ CRITICAL — 🟡 MANUAL (launch day). Baseline recorded: apex A → Squarespace (198.185.159.144/145, 198.49.23.144/145), www CNAME ext-cust.squarespace.com, **MX → Google Workspace (aspmx.l.google.com) — do not touch MX when switching**.
- [ ] **Core Web Vitals pass** (High) — 🟡 MANUAL: PSI API quota exhausted today (HTTP 429) — re-run. Proxies: TTFB 154–230 ms (excellent), but ~1.5 MB image payload risks mobile LCP.
- [ ] **Lighthouse scores** (High) — 🟡 MANUAL: same PSI re-run.
- [ ] **Caching & compression** (Medium) — ❌ FAIL (partial): `/_astro/*` immutable 1-year ✅, Brotli ✅, but `/images/*` served `cache-control: max-age=0, must-revalidate` — 11 MB of images re-validated every visit.
- [ ] **No console errors** (High) — 🟡 MANUAL: needs browser.
- [ ] **Cross-browser check** (High) — 🟡 MANUAL.
- [ ] **Real-device mobile check** (High) — 🟡 MANUAL.
- [x] **404 & error handling** (Medium) — ✅ PASS (status): unknown URLs return true 404 (no soft-404). Design failure logged under Design & Layout.
- [ ] **Backups & rollback** ⚠️ CRITICAL — 🟡 MANUAL: rollback = DNS revert to Squarespace (keep subscription active until redirects proven); confirm plan and archive old site content (esp. blog + shop copy) before any cancellation.
- [ ] **Uptime monitoring** (Medium) — ❌ FAIL: none configured (add uptime + SSL-expiry monitor at launch).
- [x] **Environment variables** ⚠️ CRITICAL — ✅ PASS: no API keys used anywhere; no sandbox keys; local `.env.local` (Vercel OIDC token) is gitignored.
- [ ] **Third-party embeds work** (High) — ❌ FAIL: none exist — the live site's Google Calendar appointment scheduler (real booking flow) and any map embed are missing.
- [x] **Structured deploy** (Medium) — ✅ PASS: git → GitHub → Vercel, clean working tree.

## Accessibility

- [ ] **Colour contrast passes** (High) — 🟡 MANUAL: spot-check with tooling (hero overlays help but verify).
- [ ] **Keyboard navigation** (High) — ❌ FAIL: desktop Services dropdown is CSS hover-only (not keyboard-operable); no skip-to-content link; mobile menu lacks focus management/Escape.
- [ ] **Visible focus indicator** (High) — ❌ FAIL: forms yes; nav/buttons browser-default only, no `:focus-visible` styles.
- [ ] **Alt text meaningful** (High) — ❌ FAIL: 10 content-bearing hero images with `alt=""`.
- [x] **Form labels associated** (High) — ✅ PASS: all inputs have `<label for>`.
- [ ] **Heading order logical** (Medium) — ❌ FAIL: /team skips h1→h3 (team cards are h3 with no h2 before them).
- [ ] **Link text descriptive** (Medium) — ❌ FAIL: six identical "Learn more" links on / and /services (add aria-labels or descriptive text).
- [ ] **Motion respects preferences** (Medium) — ❌ FAIL: no `prefers-reduced-motion`; scroll animations always run (nothing flashes).
- [ ] **Zoom to 200%** (Medium) — 🟡 MANUAL.
- [x] **Language attribute set** (Low) — ✅ PASS: `lang="en"` on all pages.
- [ ] **Media has alternatives** (Medium) — ⚪ N/A: no video/audio.
- [ ] **Automated scan run** (High) — 🟡 MANUAL: Lighthouse a11y blocked by PSI quota; run axe/Lighthouse before launch.

## Tracking & Analytics

- [ ] **GA4 installed & receiving** ⚠️ CRITICAL — ❌ FAIL: no GA4 (or any analytics) on any page.
- [ ] **Conversion events firing** ⚠️ CRITICAL — ❌ FAIL: nothing to fire — no form backend, no tags.
- [ ] **Google Tag Manager clean** (High) — ⚪ N/A: GTM not used.
- [ ] **Search Console linked** (High) — 🟡 MANUAL: after GA4 + GSC setup at launch.
- [ ] **Ads conversion tracking** ⚠️ CRITICAL — 🟡 MANUAL: confirm whether ads are running; no tags present on staging.
- [ ] **Call tracking working** (High) — ⚪ N/A: not used.
- [ ] **Cookie consent compliant** (High) — 🟡 MANUAL: currently zero cookies/third-party tags (only Google Fonts) so nothing to consent to; a banner/consent mode becomes required the moment GA4 is added (CA traffic → CCPA notice).
- [ ] **Internal traffic excluded** (Medium) — 🟡 MANUAL: set up agency/client IP filters with GA4.
- [x] **UTM handling** (Medium) — ✅ PASS: static pages, no parameter-stripping redirects.
- [ ] **Heatmapping (optional)** (Low) — ⚪ N/A: not in scope/package.
- [ ] **Annotate the launch** (Medium) — 🟡 MANUAL.
- [x] **Baseline recorded** (Medium) — ✅ PASS (organic): Semrush per-page traffic baseline captured in this audit (top pages table in redirect-map.md); GA4/GSC export still needed from client accounts.

## Security & Legal

- [ ] **Privacy policy live** ⚠️ CRITICAL — ❌ FAIL: none exists; form gathers child health-adjacent data; CCPA applies (CA business).
- [ ] **Cookie policy accurate** (High) — 🟡 MANUAL: no cookies set today; add policy together with analytics/consent banner.
- [ ] **Terms & conditions** (High) — ❌ FAIL: none present.
- [ ] **Legal entity details** (Medium) — 🟡 MANUAL: confirm required entity details with client.
- [ ] **Admin access secured** ⚠️ CRITICAL — 🟡 MANUAL: verify 2FA + access on Vercel/GitHub/domain registrar; no CMS.
- [ ] **Least-privilege access** (High) — 🟡 MANUAL.
- [x] **Software up to date** (High) — ✅ PASS: Astro ^6.3, Tailwind ^4.3 (current); minimal dependency surface.
- [ ] **Security headers set** (Medium) — ❌ FAIL: only HSTS; no CSP, X-Content-Type-Options, X-Frame-Options, or Referrer-Policy; `access-control-allow-origin: *` set. Add via vercel.json headers.
- [x] **No exposed secrets** ⚠️ CRITICAL — ✅ PASS: no keys/credentials in client source or repo; `.env*` gitignored.
- [ ] **Image/content licences** (High) — ❌ FAIL (pending clearance): `stock-*` images + clinic/team photos were gathered from public sources for the mockup — replace with client-owned/licensed assets or obtain rights before launch.
- [ ] **GDPR data handling** (High) — ❌ FAIL: no consent capture, no privacy notice, no DPA trail for whatever form backend gets chosen.

## Launch Day & Handover

All items pending — this is a mockup being evaluated for promotion to production. Key notes recorded during audit:

- [ ] **Client sign-off received** ⚠️ CRITICAL — 🟡 MANUAL: not yet sought; blockers above must be fixed first.
- [ ] **Launch window agreed** (High) — 🟡 MANUAL.
- [ ] **DNS switch & propagation** ⚠️ CRITICAL — 🟡 MANUAL: move apex A + www CNAME to Vercel; **leave Google Workspace MX/SPF/DKIM untouched**; verify via whatsmydns.
- [ ] **Full post-launch crawl** ⚠️ CRITICAL — 🟡 MANUAL: re-crawl on theraplayla.com after switch.
- [ ] **Smoke-test everything live** ⚠️ CRITICAL — 🟡 MANUAL: form → inbox, tel links, booking scheduler, GA4 realtime.
- [ ] **Email deliverability intact** ⚠️ CRITICAL — 🟡 MANUAL: send/receive test after DNS change (MX documented above).
- [ ] **Search Console re-check** (High) — 🟡 MANUAL: submit sitemap, request indexing of key pages, watch coverage 48h.
- [ ] **Old site decommissioned** (Medium) — 🟡 MANUAL: keep Squarespace paused until redirects proven; **export blog + shop content first**.
- [ ] **Client walkthrough done** (High) — 🟡 MANUAL: note — static Astro site has no CMS; agree the content-edit workflow with client.
- [ ] **Documentation delivered** (High) — 🟡 MANUAL.
- [ ] **Monitoring for week one** (High) — 🟡 MANUAL.
- [ ] **Announce & promote** (Medium) — 🟡 MANUAL.
- [ ] **Post-launch review booked** (Medium) — 🟡 MANUAL: 30-day review vs the Semrush baseline in this repo.

---

*Audit run by Claude Code (Outrider Digital) on 2026-08-07. Evidence: full-page crawls, sitewide HTTP status sweeps, source inspection of `danielagencyos/theraplay-la-mockup@e63b79b`, Semrush US organic data, live-site fetches. Read-only — no forms submitted, nothing modified.*
