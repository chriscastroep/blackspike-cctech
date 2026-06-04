# chriscastrotech.com

The marketing and portfolio site for **Chris Castro Technical Solutions** — built on [Astro 5](https://astro.build/) + [Tailwind CSS 4](https://tailwindcss.com/) with JSON-driven content.

**Live:** <https://chriscastrotech.com>  
**Deploy platform:** Cloudflare Pages (project: `cctech-spike`)

---

## Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | [Astro](https://astro.build/) | ^5.11.0 |
| CSS | [Tailwind CSS](https://tailwindcss.com/) | ^4.1.11 |
| Package manager | [pnpm](https://pnpm.io/) | (see lockfile) |
| Fonts | Inter / InterDisplay (local) | — |
| Carousel | [Swiper.js](https://swiperjs.com/) | ^11.2.10 |
| Deploy | Cloudflare Pages | — |

---

## Development

```bash
# Install dependencies (run once, or after package changes)
pnpm install

# Start dev server (hot-reload) at http://localhost:4321
pnpm run dev

# Build for production
pnpm run build   # output → dist/

# Preview production build locally
pnpm run preview
```

> **Note:** The build requires the `sharp` image processing library. If it's missing, add it first:
> ```bash
> pnpm add sharp && pnpm install && pnpm run build
> ```

---

## Deployment

Deployments are handled automatically by **Cloudflare Pages**:

| Branch | Environment | URL |
|--------|-------------|-----|
| `main` | Production | <https://chriscastrotech.com> |

### Build Settings (Cloudflare Pages)
- **Project name:** `cctech-spike`
- **Build command:** `pnpm run build`
- **Build output directory:** `dist`
- **Node.js version:** 22
- **Compatibility flags:** `nodejs_compat`

Every push to `main` triggers a new production deployment. PRs create preview deployments automatically.

---

## Content Architecture

All site content lives in `src/data/*.json`. **No code changes required** to update text, links, or site copy — just edit the JSON files.

| File | Sections it drives |
|------|--------------------|
| `global_settings.json` | Site title, description, nav links, social image, base URL, theme colour |
| `home.json` | Hero section, intro text, CTAs |
| `services.json` | Services section — names, descriptions, icons |
| `pricing.json` | Pricing plans — tiers, features, prices |
| `case_studies.json` | Portfolio / case studies grid |
| `clients.json` | Client logos / partner section |
| `testimonials.json` | Customer quotes carousel |
| `faq.json` | FAQ accordion — questions and answers |
| `newsletter.json` | Newsletter / CTA section |
| `credits.json` | Site credits / footer |

### How to Update Content

1. Edit the relevant JSON file in `src/data/`
2. The component that renders that section reads the JSON directly — no template changes needed
3. Commit and push to `main` — Cloudflare Pages deploys within ~1–2 minutes

**Example — add a new FAQ:**
```json
// src/data/faq.json
[
  {
    "question": "Do you offer remote support?",
    "answer": "Yes! We provide remote support via secure screen sharing for most software issues."
  }
]
```

### Astro Components

Components live in `src/components/` and are driven by the JSON data. To add a new section:
1. Create a `.astro` component
2. Import the relevant JSON (`import data from '../data/my_section.json'`)
3. Add it to the appropriate page in `src/pages/`

---

## Project Structure

```
src/
├── assets/         # Fonts (Inter, InterDisplay) and static assets
├── components/     # Astro components (one per section)
├── data/           # JSON content files — edit these to update site copy
├── layouts/        # Page layouts
└── pages/          # Route pages (index.astro = homepage)
public/             # Static files served as-is (images, favicons)
astro.config.mjs    # Astro config (Tailwind vite plugin, font config)
package.json        # Dependencies and scripts
```

---

## Notes

- Fonts are **local** (self-hosted) — Inter Regular/SemiBold/Bold and InterDisplay Regular/Medium/SemiBold
- Tailwind 4 is configured as a Vite plugin (`@tailwindcss/vite`) — no `tailwind.config.js` required
- SVGs are imported as Astro SVG components (no external sprite sheet needed)
- The experimental Astro Fonts API (`experimental.fonts`) is used for font optimization
- `astro dev --host` is set in the dev script to expose the server on the local network

---

## Infrastructure

Managed via Terraform in [midcityit/tf-int](https://github.com/midcityit/tf-int):
- `cloudflare-pages.tf` — Pages project config, build settings, custom domain binding
- DNS records pointing `chriscastrotech.com` → Cloudflare Pages are in `cloudflare-dns.tf`
