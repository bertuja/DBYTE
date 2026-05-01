# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**DBYTE Website** — A single-page, production static HTML website for DBYTE SRL (infrastructure & IT services company). The site is hosted on Vercel and requires **no build process, no package manager, no framework**. All HTML, CSS, and JavaScript are embedded in a single file.

- **Live**: https://dbyte-website.vercel.app
- **GitHub**: https://github.com/bertuja/DBYTE
- **Vercel Team**: dbytesrls-projects

## File Structure

```
public/
├── index.html          # Main website (~61 KB, everything embedded)
└── assets/
    ├── iso-gradient-blanca.png   # Brand logo (white gradient)
    └── iso-original.png          # Brand logo (original)

vercel.json            # Vercel config (outputDirectory: public)
CLAUDE.md              # This guidance document
```

**Note:** Last updated 2026-05-01 (logo enhanced, services reorganized)

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
- Title with gradient accent, CTA button

### 6. **Alliances Section (`.alliances`)**
- White background (only section with inverted colors)
- Marquee carousel of 10 partner logos (Google, Microsoft, AWS, etc) that loop infinitely
- Duplicated track for seamless infinite scroll

### 7. **Contact Section (`.contact`)**
- Gradient background (green→teal→blue)
- Two-column: left (form + metadata), right (embedded OpenStreetMap + location badge with pulse animation)
- Form: floating-label fields that animate on focus/fill
- Form submit: fake client-side handler (shows "✓ Enviado" for 2.4s, then resets)

### 8. **Footer (`.footer`)**
- Three-column grid: brand + description, nav links, contact links
- Top/bottom borders, small text

## CSS Architecture

**Design System** (CSS variables in `:root`):
```css
--green: #00CC99           /* Brand primary */
--blue: #35A8E0            /* Brand secondary */
--grad: linear-gradient(135deg, #00CC99 0%, #35A8E0 100%)  /* Brand gradient */

--bg: #000000              /* Main background */
--surface: #111820         /* Card/container backgrounds */
--fg: #FFFFFF              /* Main text color */
--fg-mute: #706F6F         /* Secondary text */
--fg-dim: rgba(255,255,255,0.5)  /* Dimmed text */

--display: 'Montserrat'    /* Headlines */
--body: 'Roboto'           /* Body text */
--ease: cubic-bezier(0.22, 1, 0.36, 1)  /* Easing function for all transitions */
```

**Responsive Breakpoints**:
- `900px` — Mobile layout (nav collapses, cursor hidden, section padding reduced)
- `1200px` — Remove corner decorations in hero
- `1100px` — Hero grid collapses to single column

**Key Animation Classes**:
- `.reveal` — Scroll-triggered fade-up animation; apply delay classes `.d1`–`.d6` for staggered timing
- `.entering` / `.exiting` — Text rotator animation (title word swap)
- Custom keyframes: `fadeUp`, `lineGrow`, `clipReveal`, `clipHide`, `marquee`, `termPulse`, `pulse`

## JavaScript (7 Self-Contained Modules)

All JavaScript is organized as **IIFE (Immediately Invoked Function Expressions)** at the bottom of the file. Each module is independent:

1. **Custom Cursor** — Tracks mouse, renders dot + ring, expands on hover over interactive elements
2. **Nav Scroll State** — Adds `.scrolled` class to nav after 80px scroll
3. **Hero Text Rotator** — Cycles through 4 hero title words every 2.6s with clip-path animations
4. **Terminal Simulation** — Renders 7 service scenarios as animated "terminal output"; interactive service rows jump to that scenario
5. **Scroll Reveal** — Uses IntersectionObserver to add `.in` class when elements enter viewport
6. **Floating-Label Form** — Toggles `.filled` class on inputs/textareas when they have value
7. **Form Fake Submit** — Prevents default form submission, shows success message, resets form

**Terminal Scenarios** (in SCENARIOS array, ~line 1835):
Current scenarios: `ai`, `security`, `management`, `licensing`, `network`, `cloud` (AWS). Each has a `key` (matches service row `data-svc`), `tag` (status badge), `title`, metrics (chipNum, chipLabel, chipMetric, chipMetricLabel), and array of `lines`. Lines have a `c` (class type: 'p', 'i', 'a', 'ok', 'warn', 'danger', 'meta', 'bar') and `t` (text). Each line type has a delay before rendering (700ms for prompts, 800ms for agent, 1500ms for progress bars, etc.).

## Common Development Tasks

### 1. **Edit Website Content**
- Edit text in `public/index.html` — no compilation needed
- Test locally by opening `file:///Users/bertuja/Proyectos\ 2026/DBYTE/public/index.html` in browser (or use live server)
- Colors use CSS variables (`:root`); change `--green`, `--blue`, or `--grad` to rebrand

### 2. **Add or Modify a Service**
Currently 6 services: Automatización (featured), Seguridad, Gerenciamiento, Licenciamiento, Redes, Cloud & DevOps.

**To add a new service:**
1. In the services section (around line 1447), insert a new row with correct `d#` delay class and `data-svc` key:
```html
<a href="#contacto" class="svc-row reveal d6" data-cursor data-svc="your-key">
  <span class="svc-num">07</span>
  <div class="svc-icon"><!-- SVG icon (24×24 viewBox) --></div>
  <div><div class="svc-name">Service Name</div><div class="svc-desc">Description text</div></div>
  <div class="svc-arrow"><!-- Arrow SVG --></div>
</a>
```
2. Add corresponding scenario to the SCENARIOS array (line ~1835) with matching `key` so hovering updates terminal
3. Renumber subsequent services and delay classes

**Recent changes:** Software Factory removed (was #4), Virtualization → Cloud & DevOps (now #6 with AWS focus)

### 3. **Update Contact Info**
Search for the current phone/email/address in the HTML:
- Address: "Gavilán 1207" (line ~1660)
- Phone: "+54 11 4000 0000" (multiple places)
- Email: "info@d-byte.com.ar" (multiple places)
- Map embed URL: OpenStreetMap iframe (line ~1676)

### 4. **Change Brand Colors**
Edit CSS variables (lines 14–37). All interactive elements and gradients use `--green`, `--blue`, `--grad`.

### 5. **Deploy to Vercel**
```bash
cd /Users/bertuja/Proyectos\ 2026/DBYTE

# Preview deploy (creates unique URL)
vercel deploy --scope dbytesrls-projects

# Production deploy (updates live site)
vercel deploy --prod --scope dbytesrls-projects

# View recent deployments
vercel list --scope dbytesrls-projects

# Inspect a deployment
vercel inspect <url> --scope dbytesrls-projects

# View build logs
vercel logs <url> --scope dbytesrls-projects
```

Vercel detects static HTML files automatically (no build step). Output directory is `public/`.

### 6. **Update GitHub**
```bash
git add public/ vercel.json <other files>
git commit -m "Your message"
git push origin master
```

No automatic GitHub→Vercel sync is configured; deploy must be manual or via Vercel dashboard.

## Performance & Accessibility Notes

- **Fonts**: Montserrat (headlines) and Roboto (body) are loaded from Google Fonts; customize via `<link>` in `<head>`
- **Custom Cursor**: Disabled on mobile (`matchMedia 900px`) and renders via two fixed-position divs (`.cursor-dot` + `.cursor-ring`)
- **Animations**: Respect `prefers-reduced-motion` (line 1277) — all animations disabled if user has reduced-motion preference
- **Terminal Simulation**: Uses `requestAnimationFrame` for smooth cursor blinking and progress bar animations
- **Images**: Brand logo SVG symbols are inlined at top of body; raster images (assets) are referenced as `<img src="assets/..."`
- **Form**: No server-side submission; fake client-side handler for demo purposes

## Git & Vercel Setup

- **Git**: Initialized locally, pushed to https://github.com/bertuja/DBYTE
- **Vercel Project**: `dbytesrls-projects/dbyte-website` (linked via `.vercel/project.json`)
- **Alias**: https://dbyte-website.vercel.app (production domain)
- **.gitignore**: Includes `.vercel/`, `.claude/`, other untracked directories

## Recent Updates

- ✅ **Logo enhancement** (Commit: aee2e4b) — Logo size 130px (desktop) / 95px (scrolled) / 100px (mobile), text scaled to 20px/11px
- ✅ **Services refactor** (Commit: 54c054c) — Removed Software Factory, changed Virtualization → Cloud & DevOps (AWS), 6 services total
- ✅ **Terminal scenarios updated** — 7 active scenarios (removed software, added cloud AWS scenario)

## Known Limitations & Future Improvements

1. **Form submission is fake** — Currently shows success message but does not send data. To integrate with email/CMS, update form submit handler (line ~2100) to POST to an endpoint.
2. **Terminal scenarios are hardcoded** — New services require manual JavaScript array update; could be refactored into data attributes or JSON file.
3. **No analytics configured** — Consider adding Vercel Analytics or Sentry for production monitoring.
4. **Mobile terminal** — Terminal is hidden on mobile (`.term-chip` display: none at 600px); could be redesigned for smaller screens.
5. **Line numbers subject to change** — CSS/JS selectors are stable; line numbers may drift as content updates
