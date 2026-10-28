# Testimonials Grid Section

A responsive, bento-style testimonials grid built from the
[Frontend Mentor "Testimonials grid section"](https://www.frontendmentor.io/challenges/testimonials-grid-section-Nnw6J7Un7)
challenge. It's plain HTML and modular Sass, bundled with Parcel and deployed to GitHub Pages.

- **Repository:** https://github.com/JDevlinDev/testimonials-grid
- **Live site:** https://jdevlindev.github.io/testimonials-grid/

## Overview

The page shows five graduate testimonials as cards. The layout changes with the viewport:

| Viewport            | Layout                                                                  |
| ------------------- | ----------------------------------------------------------------------- |
| `< 48rem` (mobile)  | One column; cards stack in source order                                 |
| `≥ 48rem` (tablet)  | Two columns; card 1 spans the top row, card 5 fills the right column    |
| `≥ 62rem` (desktop) | Four-column bento grid; card 1 spans two columns, card 5 spans two rows |

The layout uses CSS Grid `grid-template-areas` only. There is no JavaScript.

## Tech Stack

- **HTML5**: semantic markup (`main`, `article`, `header`, `blockquote`)
- **Sass (SCSS)**: modular `@use` / `@forward` structure
- **[Parcel 2](https://parceljs.org/)**: dev server and production bundler
- **[gh-pages](https://www.npmjs.com/package/gh-pages)**: publishes `dist/` to the `gh-pages` branch
- **Prettier** and **SVGO**: formatting and SVG optimization (dev tooling)
- **Font:** [Barlow Semi Condensed](https://fonts.google.com/specimen/Barlow+Semi+Condensed), loaded from Google Fonts

## Getting Started

### Prerequisites

- Node.js (developed against Node 24) and npm

### Install

```bash
git clone https://github.com/JDevlinDev/testimonials-grid.git
cd testimonials-grid
npm ci
```

`package-lock.json` is committed, so `npm ci` installs the exact dependency versions the project was built with.

### Scripts

| Command           | Description                                                                                                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| `npm start`       | Starts the Parcel dev server at `http://localhost:4321`, bound to `0.0.0.0` so other devices on the LAN can reach it |
| `npm run build`   | Cleans old output, then makes a production build in `dist/` with relative asset paths (`--public-url ./`)            |
| `npm run push-gh` | Builds, then pushes `dist/` to the `gh-pages` branch                                                                 |
| `npm run cleanup` | Deletes `dist/` and `.parcel-cache/`                                                                                 |

The entry point is `src/index.html`, set by the `source` field in `package.json`.

## Project Structure

```
src/
├── index.html                  # Page markup (entry point)
├── images/
│   ├── bg-pattern-quotation.svg
│   └── image-*.jpg             # Testimonial avatars
└── scss/
    ├── style.scss              # Root stylesheet: pulls in base, page, components
    ├── base/
    │   ├── _reset.scss         # CSS reset
    │   ├── _theme.scss         # Design tokens (colors, font size) as CSS custom properties
    │   ├── _typography.scss    # Font family and text styles
    │   └── _accessibility.scss # .visually-hidden utility
    ├── components/
    │   ├── _grid.scss          # Grid container and responsive template areas
    │   └── _card.scss          # Card styles and per-card color variants
    ├── page/
    │   └── _home.scss          # Page-level layout (body background, centering)
    └── utils/
        └── _mixins.scss        # breakpoints() media-query mixin
```

## Accessibility

The design has no visible page title, but the page still needs an `<h1>` so screen-reader users get the same context sighted users get from the layout. The heading outline is:

```
h1  Verified Graduate Testimonials   (visually hidden)
└─ h2  One per testimonial card
```

The `<h1>` uses the `.visually-hidden` utility instead of `display: none`. The utility removes the heading from the visual layout but keeps it in the accessibility tree, so screen readers still announce it.

## Styling Conventions

### Naming

Classes use a `tg-` (testimonials grid) prefix with BEM-style elements:

- `.tg-grid`: the grid container
- `.tg-card`, `.tg-card__header`, `.tg-card__testimonial`: the card block and its elements

### Design tokens

Every color is defined in `src/scss/base/_theme.scss` in two layers:

```scss
--tg-value-violet: 263, 55%, 52%;               /* raw HSL channels */
--tg-color-violet: hsl(var(--tg-value-violet)); /* usable color */
```

Keeping the raw channels separate lets alpha variants be built with `hsla(var(--tg-value-*), <alpha>)`, for example `--tg-subtext-light` and the card box-shadow.

| Token                  | Value                | Usage                        |
| ---------------------- | -------------------- | ---------------------------- |
| `--tg-color-violet`    | `hsl(263, 55%, 52%)` | Card 1 background, accents   |
| `--tg-color-grayblue`  | `hsl(217, 19%, 35%)` | Card 2 background, body text |
| `--tg-color-blackblue` | `hsl(219, 29%, 14%)` | Card 4 background            |
| `--tg-color-white`     | `hsl(0, 0%, 100%)`   | Cards 3 and 5 background     |
| `--tg-color-lightgray` | `hsl(0, 0%, 81%)`    | Card 1 quote text            |
| `--tg-color-lightgb`   | `hsl(210, 46%, 95%)` | Page background, light text  |

### Breakpoints

Breakpoints are declared in `src/scss/utils/_mixins.scss` and used through the mixin:

```scss
@use "../utils/mixins" as *;

@include breakpoints("md") { /* min-width: 48rem */ }
```

Available keys are `xs` (0), `sm` (16rem), `md` (48rem), `lg` (62rem) and `xl` (75rem). An unknown key triggers a Sass `@warn` and falls back to `xs`.

### Card variants

Each card's color scheme and grid area is assigned by position with `:nth-child()` in `_card.scss` and `_grid.scss`. **Reordering or adding `<article>` elements in `index.html` changes their styling.** Keep the markup order in sync with the stylesheets.

## Deployment

The site is served from the `gh-pages` branch through GitHub Pages:

```bash
npm run push-gh
```

The script builds a clean production bundle, then uses `gh-pages` to replace the contents of the `gh-pages` branch with `dist/` and push it to `origin` as a new commit. `dist/` is git-ignored on `master`.

## Acknowledgements

- Design and assets: [Frontend Mentor](https://www.frontendmentor.io)

## License

[MIT](https://opensource.org/licenses/MIT) © Jason Sherrick
