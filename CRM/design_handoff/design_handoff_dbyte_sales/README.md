# Handoff: DBYTE Sales · CRM comercial

## Overview

DBYTE Sales is an **internal CRM for the commercial team** at DBYTE SRL. It sits on top of Zoho Desk and Microsoft 365 and unifies:

- Client / contact management
- Quote generation (cotizaciones) & pipeline
- Product & supplier catalogs
- A **shared inbox with the Support department** (tickets flow both directions — Support → Commercial and Commercial → Support — via Zoho Desk)
- Technical consultations with support staff (incl. customer asset context from Snipe-IT)
- An AI assistant called **Delta** that summarizes tickets, flags at-risk clients, and suggests cross-sell

The app also has a **branded SSO-only login screen** locked to the `d-byte.com.ar` Microsoft 365 tenant.

## About the Design Files

The files in `references/` are **design references created in HTML + React (via Babel standalone) + Tailwind CDN** — prototypes showing intended look and behavior, **not production code to copy directly**.

Your task is to **recreate these designs in the target codebase's environment**, using its established stack and patterns. If no environment exists yet, pick the most appropriate framework (recommended: **Next.js 14+ / React 18 + Tailwind + shadcn/ui**, since the prototype maps 1:1 to that stack).

Do NOT ship the prototype HTML as production. It uses CDN Tailwind, in-browser Babel, inline styles for global hooks, and loose React without a build step — all fine for design iteration, none of it production-appropriate.

## Fidelity

**High-fidelity (hifi).** Pixel-perfect mockups with final colors, spacing, typography, motion, and interaction states. Colors are exact hex values, spacing is literal Tailwind numbers (p-5, gap-4, etc.), and component sizing is deliberate. Recreate pixel-faithfully, using the codebase's primitives.

---

## Brand & Design Tokens

### Colors

| Token    | Hex       | Tailwind alias  | Usage                                                |
|----------|-----------|-----------------|------------------------------------------------------|
| DVerde   | `#00CC99` | `dverde`        | Primary brand · CTAs · accents · active nav         |
| DAzul    | `#35A8E0` | `dazul`         | Secondary brand · gradient pair with DVerde         |
| DGris    | `#706F6F` | `dgris`         | Body text, secondary labels                          |
| DInk     | `#0B1020` | `dink`          | Dark-mode base / brand panel background             |
| Neutrals |           | `neutral-*`     | Standard Tailwind neutral scale for surfaces/borders |

**Brand gradient** (used in the iso logo and login hero): linear 135° from `#35A8E0` (top-left) to `#00CC99` (bottom-right).

**Semantic tones** (reused from Tailwind): `emerald-*` for success, `amber-*` for warnings, `red-*` for errors/destructive. Ticket-status dot colors and Badge tones are driven by these.

### Typography

- **Family:** Inter (weights 400/500/600/700/800). Loaded from Google Fonts in the prototype — in production, self-host via `next/font`.
- **Scale used** (exact):
  - Page H1: `22px` semibold (most screens) · `28px` semibold on login · `40px` semibold on login hero
  - Section titles: `14px` semibold
  - Body: `13.5px` / `14px` regular-medium
  - Metadata / labels: `12px` / `12.5px`
  - Micro (eyebrows, footer, legend): `11px` / `11.5px` uppercase tracking-[0.18em]–[0.22em]
- **Letter-spacing:** `tracking-tight` on display H1/H2; uppercase eyebrows use wide tracking as noted above.

### Spacing & Layout

- Tailwind default 4px scale. Cards use `p-5` / `p-6` / `p-7` / `p-8`. Dialog content uses `p-6–p-7`.
- Main app layout: **left sidebar 240px fixed + main content fluid + optional right rail**. Content max-widths vary by screen (see each screen below).
- Login: responsive **two-column grid** `lg:grid-cols-[1.1fr_1fr]`. Below `lg` the brand panel hides and the auth card centers.

### Radii & Shadows

- **Border radius:** `rounded-lg` (8px) for icon chips · `rounded-xl` (12px) for inputs/buttons · `rounded-2xl` (16px) for cards and dialogs · `rounded-3xl` (24px) for the large hero iso tile · `rounded-full` for badges, pills, avatars, theme toggle.
- **Card shadow (`.card-sheen`):**  
  `box-shadow: 0 1px 0 rgba(17,24,39,0.04), 0 24px 60px -20px rgba(6,17,31,0.18), 0 2px 6px rgba(17,24,39,0.04);` with a soft top-to-transparent white linear-gradient fill.
- **Dark equivalent:** subtle inner-top hairline + deep drop (`0 24px 60px -20px rgba(0,0,0,0.6)`).

### Motion

- **Durations:** 200–300ms for UI transitions; 600ms for card fade-ins.
- **Easings:** default Tailwind `ease-in-out` / `ease`; login stat-row uses a short `fadeIn` keyframe.
- **Keyframes used in the login** (`float` on the hero iso, `pulseDot` on live-sync indicator) — small, deliberate, don't over-animate.
- **Spinner:** `animate-spin` on `border-2` rings, 16–20px.

### Dark Mode

- **Class-based** via Tailwind `darkMode: 'class'`. Toggle writes to `localStorage.dbyte-theme` (`light` | `dark`) and initializes from stored value or `prefers-color-scheme`.
- **Dark surfaces:**
  - App / main bg: `#07111c`
  - Card bg: `#0E1826` with `border-white/10`
  - Secondary surface chips: `bg-white/[0.03]`
  - Text: `text-white` / `text-white/80` / `text-white/60` / `text-white/40`
- **Semantic colors in dark** use `{tone}-400/30` borders and `{tone}-500/10` fills (see login's denied panel and emerald verified chip for the pattern).

---

## Iconography

The prototype uses **Lucide** (`lucide@0.454.0`) as the icon library via a thin `LucideIcon` wrapper (see `references/DBYTE Sales CRM.html` head). In production, use **`lucide-react`** directly. Icon sizes: 14 / 16 / 17 / 18 px; stroke-width `1.8` on login, `2` elsewhere.

On the login screen, icons are inline SVGs (no lucide dep) because the page is a standalone entry point — in production you can just use `lucide-react` there too.

---

## Screens / Views

### 1. Login — `DBYTE Sales · Login.html`

**Purpose:** Gate the app behind SSO. Only accounts in the `@d-byte.com.ar` Microsoft 365 tenant are allowed.

**Layout:** Split screen, `lg:grid-cols-[1.1fr_1fr]`.

**Left panel (brand):**
- Dark mesh background: layered radial gradients in DAzul + DVerde over `#06111f → #0B1020 → #07111c`.
- 44×44 faint white grid pattern overlay at 60% opacity.
- Wordmark top-left: 36×36 iso tile (`rounded-lg bg-white/10 ring-1 ring-white/15`) + "DBYTE Sales" (extrabold + dimmed "Sales").
- **Hero** center: 168×168 frosted tile (`rounded-3xl bg-white/5 ring-1 ring-white/10`) containing the 112×112 iso with drop-shadow, gently floating (6s `float` animation). Next to it: uppercase eyebrow ("CRM Comercial · Interno") in DVerde, 40px H1 with a two-line headline (second line dimmed), and a 460px-max supporting paragraph.
- **Live status strip:** green pulse dot ("Zoho Desk sincronizado · hace 2 min") + shield icon ("SSO Microsoft 365") + globe icon ("ar · Buenos Aires"). 12.5px text.
- Footer: copyright + version + "Estado: operativo" at 11.5px white/40.

**Right panel (auth card):**
- Theme toggle pill (top-right, `absolute`): rounded-full, shows sun/moon in a 24×24 inverted chip + label "Claro" / "Oscuro". Border + bg shift between modes.
- Eyebrow: "ACCESO AL ESPACIO COMERCIAL" with a leading 24×1px neutral hairline.
- H2: "Ingresá con tu cuenta de DBYTE" (last word in DVerde), 28px semibold tracking-tight.
- Tenant hint paragraph with inline code chip showing `@d-byte.com.ar`.
- **Card** (`.card-sheen`):
  - **Tenant row:** building icon + label "Tenant" + value `d-byte.onmicrosoft.com` + small "Verificado" chip (emerald).
  - **Primary CTA:** 52px-tall rounded-xl button, `bg-neutral-900 text-white` in light, `bg-white text-neutral-900` in dark. Content: Microsoft 4-tile logo + "Continuar con Microsoft 365" + arrow that fades in on hover. While redirecting: spinner + "Redirigiendo a Microsoft…" and disabled.
  - **Optional email hint:** 11-tall input with a fixed `@d-byte.com.ar` suffix on the right. If user types a different domain, border becomes amber + a warning strip appears: "Solo se permiten cuentas de @d-byte.com.ar. Otros dominios serán rechazados por Microsoft."
  - **Denied state:** red card with alert icon, "Acceso denegado" headline, explanatory copy naming the tenant, and an underline link "Intentar con otra cuenta" to reset.
  - **Success state:** replaces the whole form with a centered check in a 56×56 emerald tile + "Autenticado correctamente" + sub copy + spinner.
  - **Trust strip:** 3-column grid of small chips — MFA obligatorio · SSO federado · Logs auditables (lucide icons: lock, fingerprint, shield).
- Secondary links row: "¿Problemas para ingresar?" / "Novedades" (sparkle icon).
- Legal microcopy below.

**Phases (state machine):**
```
idle → [click CTA] → redirecting → denied     (if non-tenant email was typed)
                                 → success    (otherwise; in prod, driven by MS response)
denied → [click "Intentar..."] → idle
```

**Production auth backend:** MSAL.js with a **single-tenant** app registration in Entra ID for `d-byte.onmicrosoft.com`. Do **not** use the "common" endpoint — use the tenant GUID directly, and reject any `id_token` whose `hd` / `preferred_username` domain isn't `d-byte.com.ar`.

---

### 2. Main Shell — `DBYTE Sales CRM.html`

**Layout:** 
- **Left sidebar (240px, fixed):** logo header, nav items with badge counts (amber for attention, neutral otherwise), and a "Delta" AI hint card pinned to the bottom.
- **Top bar:** search input (`Buscar clientes, cotizaciones…`), notifications bell with dot, help, user avatar + name + role.
- **Main content:** route-driven views. All content lives on `#FAFAFA` (light) / `#07111c` (dark).

**Nav items** (in order): Dashboard · Bandeja Comercial · Clientes · Cotizaciones · Productos · Proveedores · Consultas técnicas · Configuración. The first two show badges when there's work pending.

---

### 3. Dashboard

**Purpose:** At-a-glance pipeline health for the signed-in commercial rep.

**Layout:**
- Greeting header ("Buen día, Soledad 👋") + period subtitle + **Nueva cotización** primary CTA (DVerde).
- 4 KPI cards in a grid: Cotizaciones del mes · Monto en pipeline · Tasa de conversión · Consultas técnicas pendientes. Each has a delta indicator (▲ green or ↘ red).
- **Últimas cotizaciones** table (left, ~2/3 width): 5 most recent rows with código, cliente, fecha, total.
- **Actividad reciente** feed (right, ~1/3 width): timeline items with icon + title + metadata + relative time.

---

### 4. Bandeja Comercial (Commercial Inbox)

**Purpose:** Triage tickets routed from the Support department into Commercial via Zoho Desk. This is the core workflow of the app.

**Layout:**
- H1 "Bandeja Comercial" + subtitle explaining the Zoho Desk relationship.
- Sync status indicator top-right: "Zoho Desk · sincronizado".
- **Tabs** (with counts): En curso · Respondidas · Devueltas a Soporte · Todas.
- **Table:** Ticket (Zoho ID) · Asunto · Cliente · Origen (Soporte + técnico that routed it).
- Rows click through to a detail view (not fully mocked in these prototypes).
- **Primary CTA (top-right):** "Nuevo ticket a Soporte" — opens a dialog to route a commercial-originated ticket the other way.

**Planned expansion (mentioned in conversation):** a fourth tab **"En Soporte"** for tickets the commercial team has sent to Support and that are still being worked. Symmetric flow, same UI.

---

### 5. Clientes

**Purpose:** CRM client directory.

**Layout:** list + filter chips. Each client row: razón social, CUIT, industry, account owner avatar, pipeline value, last activity. Row click → client detail (contacts, quotes, tickets, assets pulled from Snipe-IT).

---

### 6. Cotizaciones

**Purpose:** Quote list and generation.

**Layout:** table with código, cliente, fecha, total, estado (Draft / Enviada / Aprobada / Rechazada). New-quote flow opens a dialog/stepper.

---

### 7. Productos & Proveedores

**Purpose:** Catalog of SKUs and supplier list. Read mostly; edit from Configuración.

---

### 8. Consultas Técnicas

**Purpose:** Commercial-team queries to the Support/technical team, separate from client-facing tickets. Status: `pendiente` | `respondida`.

---

### 9. Configuración

**Tabs:** General · Integraciones · Equipo · Plantillas.

**Integraciones** is the most important tab — it is **fully editable** in the prototype:

- **Rows are clickable.** Click a connected integration to open a Dialog with its config.
- Each row shows: icon tile · name · description · live summary of current config · "Conectado" / "Desconectado" badge · chevron.
- **Integrations and schemas:**

| Integration           | Editable fields                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------|
| Zoho Desk             | dominio (text) · deptoComercial (select) · deptoSoporte (select) · asignacion (round-robin/cliente/manual) · sincAuto (toggle) |
| Snipe-IT              | url (text) · mostrarEnCotizacion (toggle) · sincClientes (toggle)                                        |
| Microsoft 365 Outlook | remitente (text) · enviarCopia (toggle) · copiaA (text, shown only when enviarCopia=true) · trackApertura (toggle) |
| Microsoft Teams       | canal (select) · horario (select) · notifNuevoTicket · notifRespuestaSoporte · notifCotizacionAprobada (toggles) |
| AFIP / Facturación    | puntoVenta · tipoDefault (A/B/C/E) · ambiente (produccion/homologacion)                                  |
| Delta (IA)            | proveedor (select) · endpoint (text, shown for ollama/custom) · modelo (dynamic) · apiKey (password, hidden for ollama) · tono · sugerirCrossSell · resumirTickets · detectarRiesgo (toggles) |

- **Delta's provider options:** `anthropic`, `openai`, `google`, `azure`, `ollama`, `custom`. Changing provider **auto-selects the first model** in its list.
- **Modelo field is dynamic:**
  - For `anthropic` / `openai` / `google` / `azure`: Select dropdown populated from a per-provider list.
  - For `ollama` / `custom`: free-text Input with a `<datalist>` of suggestions + helper microcopy ("Modelo local servido por Ollama. Debe estar descargado en el host del endpoint." / "Nombre exacto del modelo según el endpoint OpenAI-compatible.").
- **API Key field is hidden entirely when provider = `ollama`** (no auth needed for local).
- **Dialog footer:** Desconectar link (destructive, text) on the left · Cancelar + Guardar cambios on the right.

Implement this with a `DynamicForm` that reads a declarative schema (`{key, label, type, options?, showIf?}`) — that's exactly how the prototype does it and it keeps adding new integrations cheap.

> **Note about "Clara → Delta":** an earlier version of the prototype called the AI assistant "Clara". The final name is **Delta** (reinforces the DBYTE D-shaped iso). Wherever you see "Clara" in `references/src/` treat it as a rename target — final production name is Delta.

---

## Interactions & Behavior

### Global
- **Theme toggle:** persists to `localStorage.dbyte-theme`; initializes from storage → else from `prefers-color-scheme`. Class toggled on `<html>`.
- **Focus states:** 2px outline in DVerde, 2px offset, 6px radius. Apply to all interactive elements (not just inputs).
- **Keyboard:** Dialogs close on `Escape`. Tables rows are buttons, focusable with Tab. Primary CTAs submit forms on Enter.

### Bandeja Comercial
- Tabs swap the table rows; counts update reactively.
- Clicking a row opens the ticket detail (not in this handoff — design to be confirmed).
- "Nuevo ticket a Soporte" dialog fields: cliente (autocomplete) + contacto (auto-fills primary) + canal de origen (email/tel/whatsapp/reunión/otro) + clasificación + prioridad + asunto + descripción. On submit → creates ticket in Zoho Desk dept "Soporte" and appends to `state.solicitudes` with estado `en-soporte`.

### Integraciones dialog
- Opens on row click when `estado === 'conectado'`.
- If `estado === 'desconectado'`, clicking the row reconnects instantly (prototype behavior — in production, launch the integration's OAuth flow).
- "Desconectar" sets `estado = 'desconectado'` and closes the dialog (surface a toast).
- "Guardar cambios" merges the edited config into the integration state and closes (surface a toast).

### Login
- State machine: `idle → redirecting → (success | denied)`. Time-boxed 1800ms in the mock; replace with the real MSAL flow.
- Email-hint field validates the suffix as the user types; amber outline + warning strip appear inline.

---

## State Management

In production, organize state as:

- **Auth** — user profile, tenant ID, MSAL tokens. Use MSAL-React's `useMsal()` or Entra's SDK. Persist in secure storage (httpOnly cookie if you have a BFF; else sessionStorage).
- **App data** — clientes, cotizaciones, solicitudes (tickets), consultas, productos, proveedores. Wire to Zoho REST APIs + your own backend. **React Query / TanStack Query** is the right tool here — every list view is a query, every mutation (new ticket, edit integration) is a mutation with cache invalidation.
- **UI state** — theme (localStorage), selected tab, dialog open/close, search text. Keep local to components or lift to a thin context.
- **Integrations config** — persist to your backend, not localStorage. Mutations happen from the Configuración dialog.

**Do not re-implement the prototype's single `useState` bag** — it's fine for a mock, it's not a production pattern.

---

## Backend Touchpoints (informational)

| Integration      | What the app calls it for                                               |
|------------------|-------------------------------------------------------------------------|
| Entra ID / MSAL  | SSO, tenant-restricted login                                            |
| Zoho Desk API    | Tickets (list, create, update, move between depts), contacts sync      |
| Microsoft Graph  | Send quote emails from user's mailbox, Teams channel notifications      |
| Snipe-IT API     | Customer asset lookup during support consultations                      |
| AFIP WS          | Electronic invoicing                                                    |
| AI provider API  | Delta's backing LLM — OpenAI-compatible endpoint, Anthropic, Gemini, or local Ollama |

---

## Assets

All under `references/assets/` and `references/brand/`:

- `dbyte-iso-gradient.png` — the D iso with the blue→green gradient. **Hero asset.** Use at multiple sizes.
- `dbyte-wordmark.png` — DBYTE typographic lockup.
- `brand/logos/` — additional logo variants (horizontal, vertical, mono, on-dark).

Request **SVG versions** from the DBYTE design team before shipping production — PNGs will not scale crisply on hi-dpi or at very small sizes.

---

## Files

- `references/DBYTE Sales · Login.html` — standalone login page with dark-mode support.
- `references/DBYTE Sales CRM.html` — main app shell; loads everything under `src/*.jsx`.
- `references/src/app.jsx` — root component, routing, global state seed.
- `references/src/ui.jsx` — primitives: Button, Card, Badge, Input, Select, Dialog, Tabs, Avatar, KPICard, TimelineItem, Icon, EmptyState, ToastProvider.
- `references/src/data.jsx` — seed data (clients, quotes, tickets, suppliers, consultations). Useful for understanding entity shapes.
- `references/src/screens_dashboard.jsx` — Dashboard view.
- `references/src/screens_clientes.jsx` — Clients list & detail.
- `references/src/screens_cotizaciones.jsx` — Quotes list & new-quote flow.
- `references/src/screens_solicitudes.jsx` — **Bandeja Comercial** (commercial inbox).
- `references/src/screens_other.jsx` — Productos, Proveedores, Consultas, **Configuración (incl. Integrations config dialog)**.

---

## Implementation Checklist (suggested order)

1. **Scaffold:** Next.js App Router + Tailwind + shadcn/ui. Copy design tokens into `tailwind.config.ts` (`dverde`, `dazul`, `dgris`, `dink`). Add `darkMode: 'class'`.
2. **Auth:** MSAL-React wired to your Entra app registration (single-tenant). Enforce tenant guard server-side too, not just in the UI.
3. **Login page** — pixel-faithful recreation. Include dark-mode toggle + localStorage persistence.
4. **App shell:** sidebar + topbar + theme provider. Wire nav to routes.
5. **Zoho Desk client** (server-side, not in the browser — Zoho tokens must not leak). Expose via route handlers.
6. **Bandeja Comercial** end-to-end (tabs, table, new-ticket dialog, ticket detail).
7. **Clientes / Cotizaciones / Productos / Proveedores** list views.
8. **Configuración → Integraciones** (schema-driven DynamicForm). Persist config to your backend.
9. **Delta (AI)** — implement the provider switch with a thin adapter so swapping OpenAI / Anthropic / Ollama is a config change, not a code change.
10. **Consultas técnicas** & the symmetric "En Soporte" inbox tab.

---

## What's NOT in this handoff

- Detailed mocks for: client detail page, ticket detail page, new-quote stepper internals, notifications panel.
- Final copy for AFIP/tax flows.
- Permissions model (the prototype treats everyone as admin).
- Mobile designs — the prototype is desktop-first; login is responsive but app shell is not.

Ask before inventing any of the above. Confirm with the commercial team's lead.
