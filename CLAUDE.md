# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**DBYTE Website** — A single-page, production static HTML website for DBYTE SRL (infrastructure & IT services company). The site is hosted on Vercel and requires **no build process, no package manager, no framework**. All HTML, CSS, and JavaScript are embedded in a single file.

- **Live**: https://www.d-byte.com.ar
- **GitHub**: https://github.com/bertuja/DBYTE
- **Vercel Team**: dbytesrls-projects
- **Google Analytics**: G-EJ5JBGWP5W

## File Structure

```
public/
├── index.html                    # Main website (~72 KB, all HTML/CSS/JS embedded)
├── sitemap.xml                   # XML sitemap for Google indexing
├── robots.txt                    # Crawler directives + sitemap reference
└── assets/
    ├── iso-gradient-blanca.png   # Brand logo (white gradient)
    └── iso-original.png          # Brand logo (original)

vercel.json            # Vercel config (outputDirectory: public)
CLAUDE.md              # This guidance document
```

**Note:** Last updated 2026-05-02 (Formspree + reCAPTCHA integration, Google Analytics 4, comprehensive SEO improvements)

## Website Architecture

The `public/index.html` file contains **8 major sections** (nav, hero, about, services, ticket, alliances, contact, footer), each with distinct styling and interactivity:

### 1. **Navigation (`.nav`)**
- Fixed sticky header with blur backdrop effect
- **Logo**: Enhanced prominence (130px desktop → 95px scrolled; 100px mobile)
- **Wordmark**: DBYTE (20px) + IT·SEC·CLOUD tag (11px, green accent)
- Nav links + green CTA button on right
- Scroll state: becomes darker and more compact after 80px scroll
- Mobile: nav links hidden, logo + CTA visible (900px breakpoint)

### 2. **Hero Section (`.hero`)**
- Two-column grid: left (text) + right (AI terminal simulation)
- **Left**: Animated eyebrow → gradient line → rotating title (Infraestructura/Ciberseguridad/Continuidad/Escalabilidad) → subtitle → two CTAs
- **Right**: Terminal window with animated output that rotates through 7 service scenarios (AI, Security, Management, Licensing, Network, Cloud & DevOps) — each has unique output lines, timing, and chips (floating badges with metrics)
- Entrance animations: staggered fadeUp with delays (0.2s–0.8s)

### 3. **About Section (`.about`)**
- Two-column layout: label + large headline text with gradient accents (via `<em>`)
- Below: 3-column stats grid (19 years, 300+ clients, 5000+ implementations)
- Scroll-reveal animations with delay classes (`.d1`–`.d3`)

### 4. **Services Section (`.services`)**
- 6 rows (`.svc-row`): Automatización & IA, Seguridad Informática, Gerenciamiento IT, Licenciamiento, Redes y Vigilancia, Cloud & DevOps
- **Featured row** (Automatización): has gradient background on hover, affects icon/text styling
- On hover: icon rotates & scales, name changes color, description slides in, arrow appears
- Interactive: hovering a service row jumps the terminal simulation to that service's scenario
- Scroll-reveal with delays (`.d1`–`.d5`)

### 5. **Ticket Section (`.ticket`)**
- Centered CTA section with background ISO logo (very light opacity)
- Title with gradient accent, CTA button links to https://hdesk.d-byte.com.ar

### 6. **Alliances Section (`.alliances`)**
- White background (only section with inverted colors)
- Marquee carousel of 10 partner logos (Google, Microsoft, AWS, etc) that loop infinitely
- Duplicated track for seamless infinite scroll

### 7. **Contact Section (`.contact`)**
- Gradient background (green→teal→blue)
- Two-column: left (form + metadata), right (embedded Google Maps + location badge with pulse animation)
- Form: floating-label fields that animate on focus/fill with `data-fs-field` attributes
- **Form submission**: Uses Formspree (ID: `mrejbnyo`) + Google reCAPTCHA v3 (site key: `6LeCzNUsAAAAAPK_F769ReDoPEgHVl_k3vFPeHcG`)
- **Feedback**: Shows green success message "✓ ¡Gracias! Hemos recibido tu mensaje. Nos contactaremos en breve." for 3 seconds, then auto-resets
- Error messages shown with red alert if submission fails

### 8. **Footer (`.footer`)**
- Three-column grid: brand + description, nav links, contact links
- Top/bottom borders, small text, social media links

## CSS Architecture

**Design System** (CSS variables in `:root`):
```css
--green: #00CC99           /* Brand primary */
--blue:  #35A8E0            /* Brand secondary */
--grad: linear-gradient(135deg, #00CC99 0%, #35A8E0 100%)  /* Brand gradient */

--bg:        #000000              /* Main background */
--surface:   #111820         /* Card/container backgrounds */
--fg:        #FFFFFF              /* Main text color */
--fg-mute:   #706F6F         /* Secondary text */
--fg-dim:    rgba(255,255,255,0.5)  /* Dimmed text */

--display:   'Montserrat'    /* Headlines */
--body:      'Roboto'           /* Body text */
--ease: cubic-bezier(0.22, 1, 0.36, 1)  /* Easing function for all transitions */
```

**Responsive Breakpoints**:
- `900px` — Mobile layout (nav collapses, cursor hidden, section padding reduced)
- `1200px` — Remove corner decorations in hero
- `1100px` — Hero grid collapses to single column
- `600px` — Additional mobile optimizations

**Key Animation Classes**:
- `.reveal` — Scroll-triggered fade-up animation; apply delay classes `.d1`–`.d6` for staggered timing
- `.entering` / `.exiting` — Text rotator animation (title word swap)
- Custom keyframes: `fadeUp`, `lineGrow`, `clipReveal`, `clipHide`, `marquee`, `termPulse`, `pulse`

## JavaScript (8 Self-Contained Modules)

All JavaScript is organized as **IIFE (Immediately Invoked Function Expressions)** at the bottom of the file. Each module is independent:

1. **Custom Cursor** — Tracks mouse, renders dot + ring, expands on hover over interactive elements
2. **Nav Scroll State** — Adds `.scrolled` class to nav after 80px scroll
3. **Hero Text Rotator** — Cycles through 4 hero title words every 2.6s with clip-path animations
4. **Terminal Simulation** — Renders 7 service scenarios as animated "terminal output"; interactive service rows jump to that scenario
5. **Scroll Reveal** — Uses IntersectionObserver to add `.in` class when elements enter viewport (reusable `window.observeRevealElements()`)
6. **Floating-Label Form** — Toggles `.filled` class on inputs/textareas when they have value
7. **reCAPTCHA v3 Token Generation** — Intercepts form submit, generates reCAPTCHA token via Google API, then dispatches submit event for Formspree
8. **Form Reset on Success** — Watches for success message visibility and auto-resets form after 3 seconds

**Terminal Scenarios** (in SCENARIOS array, ~line 1953):
Current scenarios: `ai`, `security`, `management`, `licensing`, `network`, `cloud` (AWS). Each has a `key` (matches service row `data-svc`), `tag` (status badge), `title`, metrics (chipNum, chipLabel, chipMetric, chipMetricLabel), and array of `lines`. Lines have a `c` (class type: 'p', 'i', 'a', 'ok', 'warn', 'danger', 'meta', 'bar') and `t` (text). Each line type has a delay before rendering (700ms for prompts, 800ms for agent, 1500ms for progress bars, etc.).

## Common Development Tasks

### 1. **Edit Website Content**
- Edit text in `public/index.html` — no compilation needed
- Test locally by opening `file:///Users/bertuja/Proyectos\ 2026/DBYTE/public/index.html` in browser (or use live server)
- Colors use CSS variables (`:root`); change `--green`, `--blue`, or `--grad` to rebrand

### 2. **Add or Modify a Service**
Currently 6 services: Automatización (featured), Seguridad, Gerenciamiento, Licenciamiento, Redes, Cloud & DevOps.

**To add a new service:**
1. In the services section (around line 1637), insert a new row with correct `d#` delay class and `data-svc` key:
```html
<a href="#contacto" class="svc-row reveal d6" data-cursor data-svc="your-key">
  <span class="svc-num">07</span>
  <div class="svc-icon"><!-- SVG icon (24×24 viewBox) --></div>
  <div><div class="svc-name">Service Name</div><div class="svc-desc">Description text</div></div>
  <div class="svc-arrow"><!-- Arrow SVG --></div>
</a>
```
2. Add corresponding scenario to the SCENARIOS array (line ~1953) with matching `key` so hovering updates terminal
3. Renumber subsequent services and delay classes

**Recent changes:** Software Factory removed, Virtualization → Cloud & DevOps (AWS), 6 services total

### 3. **Update Contact Info**
Search for the current phone/email/address in the HTML (use Find & Replace):
- **Address**: "Gavilán 1207, Entrepiso AB · CP 1407, CABA" (contact section + footer + meta)
- **Phone**: "+54 9 11 31040042" (appears in contact section, footer, meta tag)
- **Email**: "info@d-byte.com.ar" (appears in contact section, footer, meta tag)
- **Map embed**: Google Maps iframe in contact section (update coordinates if office moves)

### 4. **Change Brand Colors**
Edit CSS variables (lines ~26–46). All interactive elements and gradients use `--green`, `--blue`, `--grad`.

### 5. **Deploy to Vercel**
```bash
cd /Users/bertuja/Proyectos\ 2026/DBYTE

# Preview deploy (creates unique URL)
vercel deploy

# Production deploy (updates live site)
vercel deploy --prod

# View recent deployments
vercel ls

# Inspect a deployment
vercel inspect <url>

# View build logs
vercel logs <url>
```

Vercel detects static HTML files automatically (no build step). Output directory is `public/`.

### 6. **Update GitHub**
```bash
git add public/ vercel.json sitemap.xml robots.txt
git commit -m "Your message"
git push origin master
```

No automatic GitHub→Vercel sync is configured; deploy must be manual.

## SEO & Meta Tags Configuration

**Head section** includes:
- **Title**: "DBYTE — Infraestructura IT, Ciberseguridad, Continuidad"
- **Description**: Comprehensive description with keywords (ciberseguridad, cloud, AWS, PyMEs, etc.)
- **Canonical**: `https://www.d-byte.com.ar` (prevents duplicate content)
- **Open Graph**: og:title, og:description, og:image, og:type (improves social sharing)
- **Twitter Card**: twitter:card, twitter:title, twitter:description, twitter:image (for X/Twitter previews)
- **Robots directive**: `index, follow, max-image-preview:large` (tells Google how to crawl/index)

**Schema Markup** (JSON-LD):
- **LocalBusiness**: Company info (name, address, phone, service areas)
- **Organization**: Company branding + services list
- **FAQPage**: 4 common questions with answers (improves featured snippets in Google)

**Indexing Files**:
- **sitemap.xml**: Lists 5 main pages/sections (home, hero, nosotros, servicios, contacto)
- **robots.txt**: Allows all crawlers, disallows admin paths, points to sitemap

**Next SEO steps** (not yet implemented):
- Submit sitemap.xml to Google Search Console (https://search.google.com/search-console)
- Complete Google Business Profile (https://business.google.com)
- Add structured data testing in Google's Rich Results Test
- Monitor Core Web Vitals in Search Console
- Build backlinks from industry directories and Argentine tech sites

## Third-Party Integrations

All integrations are configured in `public/index.html` via CDN or JavaScript APIs (no backend server):

| Service | Purpose | Key/ID | Status |
|---------|---------|--------|--------|
| **Google Analytics 4** | Visitor tracking & behavior analysis | `G-EJ5JBGWP5W` | ✅ Active |
| **Google reCAPTCHA v3** | Bot protection for forms | Site: `6LeCzNUsAAAAAPK_F769ReDoPEgHVl_k3vFPeHcG` | ✅ Active |
| **Formspree** | Email form submission handler | Form ID: `mrejbnyo` | ✅ Active |
| **Google Fonts** | Montserrat + Roboto typefaces | CDN via `fonts.googleapis.com` | ✅ Preconnected |
| **Simple Icons** | Alliance logo SVGs (Google, AWS, etc) | CDN via `cdn.jsdelivr.net/npm/simple-icons` | ✅ CDN |

**To reconfigure:**
- **Formspree**: Replace form ID in HTML (`formId: 'mrejbnyo'`) and contact `mrejbnyo@formspree.io`
- **reCAPTCHA**: Update site key in HTML and link to Google Search Console
- **Google Analytics**: Update ID in gtag script
- **Fonts**: Modify `<link>` href in `<head>`

## Performance & Accessibility Notes

- **Fonts**: Montserrat (headlines) and Roboto (body) loaded from Google Fonts via `<link preconnect>`; can customize URLs in `<head>`
- **Custom Cursor**: Disabled on mobile (`matchMedia 900px`) and renders via two fixed-position divs (`.cursor-dot` + `.cursor-ring`)
- **Animations**: Respect `prefers-reduced-motion` media query — all animations disabled if user preference detected
- **Terminal Simulation**: Uses `requestAnimationFrame` for smooth cursor blinking and progress bar animations
- **Images**: Brand logo SVG symbols inlined at top of body; raster images (assets) lazy-loaded via `<img>`
- **Form Submission**: Real submission via Formspree AJAX (no page reload); includes reCAPTCHA v3 bot protection
- **Analytics**: Google Analytics 4 via gtag.js (loaded async, non-blocking)
- **Security**: reCAPTCHA v3 (invisible) validates human intent before form submission

## Git & Vercel Setup

- **Git**: Initialized locally, pushed to https://github.com/bertuja/DBYTE (branch: master)
- **Vercel Project**: `dbytesrls-projects/dbyte-website` (linked via `.vercel/project.json`)
- **Production Alias**: https://www.d-byte.com.ar (custom domain)
- **Also reachable at**: https://dbyte-website.vercel.app
- **.gitignore**: Includes `.vercel/`, `.claude/`, `node_modules/`, other untracked directories

## Recent Implementations (2026-05-02)

- ✅ **Google Analytics 4** — Tracking ID: `G-EJ5JBGWP5W` (added via gtag.js script)
- ✅ **Google reCAPTCHA v3** — Site key: `6LeCzNUsAAAAAPK_F769ReDoPEgHVl_k3vFPeHcG`, Secret: configured in Formspree
- ✅ **Formspree Integration** — Real email submissions to `mrejbnyo@formspree.io`
- ✅ **Form Feedback** — Success/error messages with auto-reset after 3 seconds
- ✅ **SEO Overhaul**:
  - sitemap.xml with all main sections
  - robots.txt with crawler directives
  - Open Graph + Twitter Card meta tags
  - Canonical link to prevent duplicate content
  - Keywords, robots meta directives
  - FAQSchema for featured snippets
  - Improved heading hierarchy (H1 → H2s)

## Known Limitations & Future Improvements

1. **Terminal scenarios are hardcoded** — New services require manual JavaScript array update in SCENARIOS (around line ~1953). Could refactor into data attributes or JSON file for dynamic loading.
2. **Mobile terminal** — Terminal simulation is hidden on mobile (CSS `display: none` at 600px); could be redesigned for smaller screens or replaced with static info.
3. **No blog/content system** — Site is static single-page; adding case studies or blog posts would require either:
   - Dynamic HTML generation from JSON (JavaScript-based)
   - External CMS integration (Strapi, Sanity, etc.)
4. **Sitemap is manual** — sitemap.xml updated by hand; if adding new major sections, remember to update it
5. **CSS/JS line numbers drift** — Selectors are stable, but line numbers change; refer to section headers instead when referencing code
6. **Google Search Console not yet configured** — Site is live but not submitted to GSC; next step for better visibility
