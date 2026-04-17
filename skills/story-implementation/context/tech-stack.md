# Tech Stack — VisitLeMarche.com

## Overview

VisitLeMarche.com is built on a WordPress-based architecture, extended with plugins, customisations, and third-party integrations.

The stack prioritises:
- Speed of iteration
- SEO performance
- Low operational overhead
- Integration with affiliate platforms

---

## Core Platform

- CMS: WordPress
- Theme: CheerUp (MyThemeShop)
- Hosting: (assumed typical WP hosting, e.g. VPS or managed WP)

---

## Key Plugins

### SEO & Structure
- Rank Math SEO
- XML Sitemap (via Rank Math)

### Multilingual
- Polylang
- Polylang AI Automatic Translation

### UI / Theme Enhancements
- Sphere Core (breadcrumbs, theme modules)

### Performance / Infra
- Cloudflare (CDN, caching, security)

---

## Content System

- Gutenberg editor (block-based)
- Custom layouts via theme + partial overrides
- Structured sections:
  - Where to stay
  - How to get there
  - Explore surroundings

---

## Integrations

### Travel & Booking

#### Flights
- TravelPayouts (white-label)
- avia.visitlemarche.com

#### Accommodation
- Aggregators (e.g. Vio, Booking, Kayak)
- Embedded widgets / maps

#### Activities
- GetYourGuide (affiliate links)

---

## Affiliate Model

- Cookie-based attribution (varies by provider)
- Deep linking to partner platforms
- Revenue from:
  - clicks
  - bookings

Constraints:
- Cross-domain attribution loss (e.g. .com → .it redirects)
- Short cookie windows on some providers

---

## Architecture Considerations

### Strengths
- Fast to publish content
- SEO-friendly
- Flexible integrations

### Limitations
- Plugin dependency complexity
- Debugging conflicts (e.g. Polylang + Rank Math + theme)
- Limited backend custom logic

---

## Known Customisations

- Breadcrumb localisation fixes (Polylang integration)
- Internal link translation logic
- Custom CSS for galleries, tables, layout tweaks
- Homepage custom blocks (mix of posts + custom content)

---

## Development Workflow

- Manual + AI-assisted development
- Direct WordPress editing
- Limited CI/CD (potential area for improvement)

---

## Data & Analytics

- Likely tools:
  - Google Analytics
  - Search Console

Used for:
- SEO performance tracking
- Content optimisation

---

## Performance Priorities

- Page speed (Core Web Vitals)
- Mobile optimisation
- Lightweight pages (avoid heavy scripts)

---

## Security

- Cloudflare protection
- Standard WordPress hardening

---

## Future Improvements (Important for PRDs)

- Introduce structured backend services (APIs)
- Better affiliate abstraction layer
- Tracking reliability improvements
- Automation pipelines (GitHub + CI/CD)
- Headless or hybrid architecture (long-term)

---

## Notes for AI Agents

When generating backlog or implementation:

- Prefer plugin-based solutions when possible
- Avoid heavy custom backend unless justified
- Consider multilingual implications (Polylang)
- Ensure SEO impact is preserved
- Be cautious with:
  - caching
  - URL structure
  - translations