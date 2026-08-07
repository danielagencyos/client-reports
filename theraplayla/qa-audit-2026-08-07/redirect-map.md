# TheraPlay LA — Proposed 301 Redirect Map (old Squarespace → new site)

**Status sweep (2026-08-07):** all 90 sitemap URLs from www.theraplayla.com tested against the staging site — **89 return 404; only `/contact` resolves.** Shipping without redirects loses effectively all indexed URLs.

## Traffic reality (Semrush, US, organic)

| Live page | Est. visits/mo | Share |
|---|---|---|
| /theraplay-blog/…palmar-grasp-reflex | 644 | 21.7% |
| /theraplay-blog/…atnr-asymmetrical-tonic-neck-reflex | 585 | 19.7% |
| /theraplay-blog/…moro-reflex | 499 | 16.8% |
| /theraplay-blog/…spinal-galant-reflex | 337 | 11.3% |
| /theraplay-blog/…landau-reflex | 236 | 7.9% |
| /theraplay-blog/…tonic-labyrinthine-reflex-tlr | 184 | 6.2% |
| / (homepage) | 152 | 5.1% |
| /theraplay-blog/…rooting-reflex | 115 | 3.9% |
| /shop-ot-approved-toys | 100 | 3.4% |

**≈88% of organic traffic is blog posts and ≈3% is the shop — none of which exist on the new site.** Redirecting a ranking blog post to the homepage does *not* preserve its rankings; Google treats irrelevant redirects as soft-404s. **Strong recommendation: migrate the blog (at minimum the reflex series) before or at launch**, and decide the shop's fate (the intake-screener products are a paid revenue line).

## A. Direct equivalents — clean 1:1 redirects (10)

| Old path | → New path |
|---|---|
| /theraplayla | /about |
| /meet-the-team | /team |
| /explore-our-space | /our-space |
| /what-we-do-new-page | /services |
| /pediatric-occupational-therapy | /services/occupational-therapy |
| /pediatric-speech-therapy | /services/speech-therapy |
| /orofacial-myofunctional-therapy | /services/myofunctional-therapy |
| /pediatric-feeding-and-swallowing | /services/feeding-therapy |
| /intensives | /services/intensives |
| /contact | /contact (already resolves) |

## B. Near-equivalents — closest topical page (dedicated content lost)

| Old path | → Suggested | Note |
|---|---|---|
| /primitive-reflex-integration | /services/occupational-therapy | real service page today; consider rebuilding — supports the reflex blog cluster |
| /dynamic-movement-intervention | /services/occupational-therapy | DMI page lost |
| /twice-exceptional-2e-support | /services/occupational-therapy | 2E page lost |
| /discovery-call | /contact | live page embeds Google Calendar scheduler — re-embed it on /contact |
| /common-questions | /services | FAQ page lost (service pages have FAQ sections) |
| /testimonials | / | real testimonials needed on new site first |
| /nutrition-consulting | /services/feeding-therapy | |
| /teletherapy | /services | |
| /school-of-sleep, /school-of-sleep-contd | /services | sleep program pages lost |
| /in-school-therapy-services | /services | |
| /regional-center-services | /about | footer mentions Regional Center vendor |
| /in-the-press | /about | press section exists on /about |
| /press-inquiries | /contact | |
| /join-our-mailing-list | /contact | lead magnet lost |
| /professional-development, /ruth-ragland-symposium-2025 | /contact | event landing pages |

## C. Blog (35 URLs) — **content-loss decision required**

Preferred: rebuild `/blog` and migrate posts 1:1, then redirect `/theraplay-blog/<slug>` → `/blog/<slug>`.
Fallback (accepts traffic loss): topical redirects — reflex-series posts → /services/occupational-therapy; feeding/chewing/picky-eating posts → /services/feeding-therapy; snoring/tonsils/sleep posts → /services/myofunctional-therapy; remainder + `/theraplay-blog`, category & tag pages → /.

## D. Shop (28 URLs) — **business decision required**

9 shop-list pages (/shop, /shop-ot-approved-toys, /shop-our-favorite-books, /shop-feeding-and-oral-development, /shop-oral-hygiene, /shop-travel, /shop-handwriting, /shop-executive-functioning, /shop-water-play), 13 curated product pages (/nasal-hygiene, /sensory-integration, /bilateral-coordination, /visual-schedules, /back-to-school, /k-beauty, /infant-development, /hand-eye-coordination, /classroom-optimization, /adaptive-seating, /outdoor-sensory-play, /books-for-language-learning, /nutritious-alternatives), and **/shop-intake-screeners + 5 paid product pages (revenue line!)** → interim: all → / ; intake screeners → /contact until a replacement purchase flow exists.

## Starter vercel.json (group A + B; extend per C/D decisions)

```json
{
  "redirects": [
    { "source": "/theraplayla", "destination": "/about", "permanent": true },
    { "source": "/meet-the-team", "destination": "/team", "permanent": true },
    { "source": "/explore-our-space", "destination": "/our-space", "permanent": true },
    { "source": "/what-we-do-new-page", "destination": "/services", "permanent": true },
    { "source": "/pediatric-occupational-therapy", "destination": "/services/occupational-therapy", "permanent": true },
    { "source": "/pediatric-speech-therapy", "destination": "/services/speech-therapy", "permanent": true },
    { "source": "/orofacial-myofunctional-therapy", "destination": "/services/myofunctional-therapy", "permanent": true },
    { "source": "/pediatric-feeding-and-swallowing", "destination": "/services/feeding-therapy", "permanent": true },
    { "source": "/intensives", "destination": "/services/intensives", "permanent": true },
    { "source": "/primitive-reflex-integration", "destination": "/services/occupational-therapy", "permanent": true },
    { "source": "/dynamic-movement-intervention", "destination": "/services/occupational-therapy", "permanent": true },
    { "source": "/twice-exceptional-2e-support", "destination": "/services/occupational-therapy", "permanent": true },
    { "source": "/discovery-call", "destination": "/contact", "permanent": true },
    { "source": "/common-questions", "destination": "/services", "permanent": true },
    { "source": "/testimonials", "destination": "/", "permanent": true },
    { "source": "/nutrition-consulting", "destination": "/services/feeding-therapy", "permanent": true },
    { "source": "/teletherapy", "destination": "/services", "permanent": true },
    { "source": "/school-of-sleep", "destination": "/services", "permanent": true },
    { "source": "/school-of-sleep-contd", "destination": "/services", "permanent": true },
    { "source": "/in-school-therapy-services", "destination": "/services", "permanent": true },
    { "source": "/regional-center-services", "destination": "/about", "permanent": true },
    { "source": "/in-the-press", "destination": "/about", "permanent": true },
    { "source": "/press-inquiries", "destination": "/contact", "permanent": true },
    { "source": "/join-our-mailing-list", "destination": "/contact", "permanent": true },
    { "source": "/professional-development", "destination": "/contact", "permanent": true },
    { "source": "/ruth-ragland-symposium-2025", "destination": "/contact", "permanent": true },
    { "source": "/theraplay-blog/:path*", "destination": "/", "permanent": true },
    { "source": "/shop-intake-screeners/:path*", "destination": "/contact", "permanent": true },
    { "source": "/shop:path(-.*)?", "destination": "/", "permanent": true }
  ]
}
```

*Note: the blanket `/theraplay-blog/:path*` → `/` rule is the accept-the-loss fallback — replace with per-post redirects if/when the blog is migrated. Remaining curated shop paths (e.g. /nasal-hygiene, /k-beauty) need individual rules; full 90-URL list in `live-urls.txt`.*
