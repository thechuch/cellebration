# Cellebration — Website Wireframe Preview

Interactive homepage wireframe with a **grayscale structural view** plus **four live color directions**. Use the theme switcher in the top-right to walk the client through the layout first, then preview each colorway in place.

## What's in this folder

```
wireframe/
├── index.html       # Full homepage wireframe + theme switcher + mobile drawer
├── styles.css       # All styles + 5 theme variable sets + hover menus
├── README.md        # This file
└── netlify.toml     # Optional Netlify config
```

Four files total. Zero build step. Zero dependencies.

## View locally

Double-click `index.html` in Finder — it opens in your default browser and works offline. No server required.

For the smoothest preview (proper font loading, sticky nav, smooth transitions), run a tiny local server:

```bash
cd wireframe
python3 -m http.server 8000
# then visit http://localhost:8000
```

## How to walk the client through it

1. **Start in Wireframe mode (the default).** This is what loads first. Gray-on-gray, dashed borders, placeholder image boxes with `IMG · PILLARNAME` labels. A striped banner at the top says *"Wireframe Preview · Structure & Content Only"*. The purpose of this view is to let George react to **layout and content** without being distracted by color choices or reading it as a finished pitch.
2. **Walk through the blocks in order.** Nav → hero → ecosystem grid → club → EBOO → featured services → as seen in → partnerships → social proof → lead-gen form → footer. Let him see the structure and the copy.
3. **Hover Services** to show the mega-menu with all six pillar cards. Hover Memberships to show the three-tier dropdown. These are the two "aha" moments — George has been hearing about the mega-menu in every document; showing it live is what makes it concrete.
4. **Click through the four colorways** when he's ready to talk look and feel. Each click re-skins the entire page in place — same scroll position, same content, same layout, just a different surface. Compare all four side-by-side by opening the site in multiple browser windows if needed.
5. **Ask the question.** *"Which direction feels like Cellebration to you?"* Get the answer in the room.

## The five preview modes

| Name | Description |
|---|---|
| **Wireframe** | Default. Grayscale, dashed borders, placeholder imagery. Structural preview only — makes it obvious to the client this is about bones, not pixels. |
| **Purple · Light** | Direction A, light mode. Cream background, deep purple type, magenta CTAs. Closest to the Jan 2026 catalog. |
| **Purple · Dark** | Direction A, dark mode. Deep purple base, cream type, magenta CTAs. |
| **White · Light** | Direction B, light mode. White background, purple headers + CTAs, dark gray body. |
| **White · Dark** | Direction B, dark mode. Near-black background, soft purple accents, off-white type. |

Selections persist across page refreshes within the same browser tab.

## The nav — what works

- **Four primary items** plus a persistent `Book Now` pill button top-right
- **Floating on scroll** — the nav compresses and docks to the top of the viewport as the user scrolls
- **Services mega-menu** — opens on hover (desktop) or tap (touch). Shows six pillar cards in a 3×2 grid with image thumbs and descriptions, plus a "See full service index →" footer link.
- **Memberships dropdown** — opens on hover (desktop) or tap (touch). Shows the three membership tiers with their positioning lines.
- **Click-outside** and `Escape` close any open menu.
- **Keyboard navigable** — tab through nav items, Enter/Space opens menus, Escape closes.

## Mobile — what works

- **Below 880px** the primary nav is replaced by a hamburger button on the right.
- **Tap the hamburger** → a slide-in drawer opens from the right with all four nav items.
- **Services and Memberships expand in place** — tap the section header to reveal the sub-items. The `＋` icon rotates to `×` to indicate the open state.
- **Book Now** becomes a full-width primary CTA at the bottom of the drawer.
- **Contact info** (email + clinic cities) sits at the very bottom of the drawer for quick reference.
- **Theme switcher** relocates to a fixed bottom bar so it doesn't cover the hamburger.
- **Tap outside the drawer**, press `Escape`, or tap the `×` to close.
- **Body scroll is locked** while the drawer is open so you don't accidentally scroll the page behind it.

## Deploy to Netlify (drag-and-drop — fastest path)

1. Zip this folder.
2. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
3. Drag the zip onto the page.
4. Netlify gives you a URL. Share it.

The included `netlify.toml` sets clean defaults — no further configuration needed.

## Deploy to GitHub Pages

1. Create a new repo (e.g. `cellebration-wireframe`).
2. Copy these four files into the repo root.
3. `git add . && git commit -m "Wireframe preview" && git push`.
4. In the repo on GitHub: **Settings → Pages → Source → Deploy from a branch → main / root → Save**.
5. GitHub gives you a `https://[username].github.io/cellebration-wireframe/` URL in about a minute.

## What this wireframe covers

- **Nav** — four items, floating on scroll, persistent Book Now pill
- **Hover mega-menu** — six-pillar Services dropdown with placeholder imagery
- **Hover dropdown** — three-tier Memberships dropdown with positioning copy
- **Mobile drawer** — slide-in nav with expandable sections, locked body scroll
- **Hero** with founder-led copy and dual CTAs
- **Ecosystem grid** with all six pillar cards
- **Cellebration Club** with the full member pricing table
- **EBOO for Everyone** keystone block
- **Featured Services** horizontal carousel (eleven services)
- **As Seen In** logo wall
- **Partnerships** with founder-video placeholder
- **Social proof** — Google Reviews + Instagram grid
- **Persistent lead-gen form** above the footer
- **Footer** with mirror-nav, both clinic addresses, Florida FDA disclaimer

## What this wireframe does *not* cover

Homepage-only preview. Doesn't render the pillar pages, membership landing pages, Club detail page, Team, Contact, or the full service index. Those come after the client signs off on the homepage direction.

## Notes for the walk-through

- **Imagery is placeholder.** All image tiles are CSS blocks with labels so the client focuses on layout and colorway, not stock art.
- **The phone number is `[Phone — TBD]`** pending client decision.
- **The founder / PNOĒ video is a placeholder block** pending production.
- **The "As Seen In" logos are text placeholders** pending logo-rights confirmation.
- **The Hyper Club** is represented as a parallel third membership. If the client reshapes it to an add-on inside the Cellebration Club, the Club block CTA updates accordingly.

## Browser support

Works in any modern browser: Safari, Chrome, Firefox, Edge, and mobile Safari / Chrome on iOS and Android. Built with CSS variables, CSS grid, `details`/`summary` for mobile accordion, and a small vanilla-JS layer for the drawer and click-toggle menus (no frameworks, no build tools).
