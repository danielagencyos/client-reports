# TheraPlay LA — Pre-Launch QA Audit

**Date:** 2026-08-07 · **Verdict: 🔴 NO-GO (yet)**

| | |
|---|---|
| Current live site | https://www.theraplayla.com (Squarespace, 90 indexed URLs) |
| Staging candidate | https://theraplay-la-mockup.vercel.app (Astro mockup, 11 pages, repo [`theraplay-la-mockup`](https://github.com/danielagencyos/theraplay-la-mockup)) |
| Result | 34 pass · **44 fail** · 48 manual · 10 n/a |

## 📄 Documents

- **[Full QA results](qa-results-theraplay-la-mockup-2026-08-07.md)** — every checklist item with evidence
- **[Redirect map + traffic analysis](redirect-map.md)** — proposed 301s + starter `vercel.json`

## The 8 launch blockers

1. **Contact form sends data nowhere** (`onsubmit="event.preventDefault()"` — no backend, no success state)
2. **No redirects** — 89/90 old URLs would 404; **~88% of organic traffic is blog posts that don't exist on the new site**
3. **Opening hours wrong** — staging says 8am–6pm + weekends; live business hours are 10am–6pm, weekends closed
4. **Mockup artifacts live** — "Website mockup concept" footer + fictional testimonials on the homepage
5. **No sitemap.xml / robots.txt**
6. **Zero analytics** — no GA4, no conversion tracking
7. **No privacy policy** — while the form collects a child's name, age, and health concerns (CCPA)
8. **Real booking flow missing** — live site's Google Calendar scheduler isn't on staging

The good news: copy is clean and typo-free, NAP is consistent and matches the live site, all links/images resolve, titles/descriptions/H1s are unique, no secrets in the repo, and TTFB is excellent. The blockers are mostly wiring, not rework — except the **blog/shop migration decision, which needs a team + client call** (the shop includes paid intake-screener products).

## Biggest strategic risk

The new site is 11 pages replacing 90. The blog *is* the SEO asset (reflex-series posts alone ≈ 84% of organic visits). Launching without migrating it — or at least redirecting + planning its rebuild — trades the site's entire organic footprint for a design refresh.

---
*Read-only audit by Claude Code for the Outrider Digital team. No forms submitted, nothing modified on either site.*
