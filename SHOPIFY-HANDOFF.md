# Shopify Build Handoff: Header and Navigation

This document is for the development team implementing the Cellebration header and navigation in Shopify. It covers where the reference code lives in this repo, how the hover menus are built, and a suggested path for porting them into a Shopify theme.

## Whose wireframe is this, and do we have access?

The wireframe was designed and built in-house by Small Circle, the agency on this project. There is no third-party ownership and nothing you need to request access to:

- **Full source code:** this repository. Clone it or browse it; everything is here.
- **Live preview:** https://cellebration.smallcircle.dev (same code, deployed via GitHub Pages)
- **No design tool behind it:** there is no separate Figma or XD file for the nav. The HTML, CSS, and JS in this repo are the spec. If you can read the code, you have the complete design.
- **Stack:** static HTML, one CSS file, and a small inline vanilla JS layer. No frameworks, no build step, no dependencies.

Copy, tier names, and pricing reflect client-approved content as of late April 2026. Treat this repo as the source of truth for structure and behavior. Final copy should remain editable by the client in Shopify wherever practical.

## What to build

The header consists of:

1. **Logo** (left), linking home
2. **Five primary nav items:** Services, Memberships, About, Locations, Contact
3. **Services mega menu** on hover/focus: a panel with six pillar cards in a grid (image thumb, title, one-line description), an "Individual Therapies" link list, and a "See All Services" footer link
4. **Memberships mega menu** on hover/focus: a three-column comparison panel (one column per membership tier, with eyebrow, name, price, feature list, and a "Most Popular" badge on the middle tier), plus a "Compare all plans" footer link
5. **Book Now pill button** (right), persistent at all times on desktop
6. **Shrink-on-scroll behavior:** the nav compresses and docks to the top of the viewport after the user scrolls past 32px
7. **Mobile (at or below 880px):** the nav collapses to a hamburger that opens a slide-in drawer; Services and Memberships expand in place inside the drawer; Book Now becomes a full-width CTA at the bottom of the drawer

Every page in this repo repeats the same header markup, so you only need one reference file: `index.html`.

## Code map

| Piece | File | Where |
|---|---|---|
| Header markup (full) | `index.html` | lines 59 to 241 |
| Services mega menu markup | `index.html` | lines 68 to 156 |
| Memberships three-column menu markup | `index.html` | lines 158 to 211 |
| Book Now button markup | `index.html` | line 233 |
| Mobile drawer markup | `index.html` | lines 246 to about 300 |
| Nav behavior JS (scroll shrink, drawer, touch toggle, click-outside, Escape) | `index.html` | inline script, roughly lines 692 to 885 |
| Sticky nav + scrolled state styles | `styles.css` | line 492 onward |
| Mega menu container, open/close rules, cards | `styles.css` | lines 554 to 755 |
| Membership comparison columns | `styles.css` | lines 756 to 860 |
| Individual Therapies link list | `styles.css` | lines 861 to 906 |
| Mobile drawer styles | `styles.css` | line 935 onward |
| Mobile breakpoint (hamburger swap, Book Now moves to drawer) | `styles.css` | `@media (max-width: 880px)` at line 1071 |
| Pill button base style | `styles.css` | line 359 |

Note: the stylesheet also contains five theme variable sets (the wireframe's colorway switcher). For the Shopify build you only need one set of values, supplied by the chosen brand direction.

## How the hover menus actually work

The mechanics are deliberately simple, which is why they port cleanly:

1. **Desktop open/close is pure CSS.** Each menu lives inside its nav item wrapper. The panel is absolutely positioned, hidden by default, and revealed by `:hover` and `:focus-within` on the wrapper:

   ```css
   .has-mega:hover .mega-menu,
   .has-mega:focus-within .mega-menu,
   .has-mega.is-open .mega-menu { /* visible state */ }
   ```

   `:focus-within` is what makes the menus keyboard accessible for free.

2. **The panels anchor to the nav, not the link.** `.primary-nav` is the positioning ancestor (see the comment near `styles.css` line 554), so wide panels center across the whole nav instead of hanging off one link.

3. **Touch support is a small JS layer.** On touch devices there is no hover, so the script toggles an `.is-open` class: first tap opens the menu, second tap on the link navigates. `mouseleave` closes on desktop, and document-level listeners close any open menu on outside click or Escape. See the inline script around lines 819 to 865 of `index.html`.

4. **The Memberships menu is the same pattern** with a different panel body: a three-column CSS grid (`.membership-compare-grid`) where each column is one large clickable `<a>` to that tier's page.

5. **The mobile drawer** is a fixed-position `<aside>` toggled by the hamburger. Services and Memberships use native `<details>`/`<summary>` for the expand-in-place accordion. Body scroll is locked while the drawer is open.

## Suggested Shopify implementation path

Theme specifics are your call, but this approach avoids the usual traps:

1. **Work in a duplicate theme,** never the live one. Build, review on the preview URL, then publish.
2. **Build the header as a theme section.** Replace or extend the theme's `sections/header.liquid` with this markup (or create a `custom-header.liquid` section and assign it in the theme's header group). The header in this repo is plain HTML, so it drops into Liquid without translation.
3. **The Book Now button is just an anchor** with two classes, placed inside the header markup next to the nav (`index.html` line 233). In Liquid, render it from a section setting (`text` + `url`) so the client can change the label or booking link without a developer. Note it intentionally disappears from the bar below 880px and reappears as the full-width CTA inside the drawer.
4. **Port the CSS into a theme asset** (for example `assets/custom-header.css`, loaded from the section with `{{ 'custom-header.css' | asset_url | stylesheet_tag }}`). Take the nav, mega menu, membership column, drawer, and 880px media query blocks listed in the code map. Resolve the CSS variables to final brand values, or define one `:root` block.
5. **Port the JS as-is** into the section or an asset file. It is small, dependency-free vanilla JS: scroll shrink, drawer open/close with scroll lock, touch toggle, click-outside, Escape.
6. **Decide how menu content is managed.** Shopify's native Navigation menus (linklists) only give you labels and URLs. They cannot express the rich card content (descriptions, prices, badges, feature lists). Two workable options:
   - **Section blocks:** one block per mega menu card / membership column, with fields for title, description, price, badge, link, image. Client-editable in the theme editor. This is usually the right call.
   - **Hardcode for v1** exactly as in this repo, and move to blocks later. Acceptable if speed matters, since the content is client-approved.
7. **Watch for theme/app interference.** If the existing theme's header has its own sticky/mega menu JS, disable or remove it rather than layering both. Test with any installed apps that inject header elements.

## Acceptance checklist

The build matches the wireframe when all of these hold:

- [ ] Hovering Services on desktop opens the six-card mega menu; hovering Memberships opens the three-column comparison panel
- [ ] Menus also open via keyboard (Tab to the item, menu appears on focus; Escape closes)
- [ ] On touch devices: first tap opens the menu, second tap on the nav link navigates
- [ ] Clicking outside or pressing Escape closes any open menu
- [ ] Book Now pill is always visible at the top right on desktop, regardless of scroll position
- [ ] Nav compresses and docks to the viewport top after scrolling (compare against the live preview)
- [ ] At or below 880px: hamburger replaces the nav; drawer slides in from the right; Services and Memberships expand in place; Book Now is a full-width CTA at the drawer bottom; body scroll locks while open; tap-outside, the close button, and Escape all close it
- [ ] No console errors, no framework or library added for the nav

## Running the reference locally

```bash
git clone https://github.com/thechuch/cellebration.git
cd cellebration
python3 -m http.server 8000   # then open http://localhost:8000
```

Or simply double-click `index.html`. Compare anything ambiguous against the deployed copy at https://cellebration.smallcircle.dev.

Questions about intent or content should go through the Small Circle project channel.
