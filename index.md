# How this site works (`index.html`)

This repo is a **single-page personal site** for GitHub Pages. Everything visitors see is driven by one HTML file plus a few static assets (images).

---

## What you’re looking at

| Piece | Role |
|--------|------|
| **`index.html`** | The entire website: structure, copy, styling hooks, and a tiny script for the footer year. |
| **`images/`** | Static assets (e.g. `github-avatar.png` for the favicon and hero photo fallback). |
| **No bundler** | There is no React/Vite/webpack pipeline. The browser loads `index.html` and Tailwind from a CDN. |

---

## How we “build” it

We **don’t compile** the site in the traditional sense:

1. **Tailwind CSS via CDN** — A `<script>` loads [Tailwind’s CDN](https://tailwindcss.com/docs/installation/play-cdn). Another inline `<script>` sets `tailwind.config` (here: custom `fontFamily.sans` for a clean system / SF-style stack).
2. **Utility classes** — Layout, spacing, color, typography, and responsive behavior are expressed as Tailwind classes on HTML elements (e.g. `max-w-[980px]`, `md:grid-cols-3`, `rounded-2xl`).
3. **GitHub Pages** — Publishing serves `index.html` at the site root. Whatever you commit is what ships.

**Trade-off:** Fast and simple to edit; **no PurgeCSS** — unused Tailwind rules still ship via CDN (acceptable for a small page).

---

## HTML structure (semantic layout)

The document follows a straightforward landmark outline:

```text
<body>
  <header>     <!-- sticky nav + LinkedIn -->
  <main id="top">
    <section>  <!-- hero: headline, intro, photo, CTAs, three highlight cards -->
    <section id="about"> … </section>
    <section id="leadership"> … </section>
    <section id="focus"> … </section>
    <section id="stack"> … </section>
    <section id="projects"> … </section>
    <section id="links"> … </section>
  </main>
  <footer>     <!-- copyright + mini links -->
</body>
```

- **`main id="top"`** — Scroll target for “back to top” and the logo link.
- **`<section id="…">`** — Each major block has an **anchor id** matching the header nav (`#about`, `#leadership`, etc.) so in-page links jump smoothly (`scroll-behavior: smooth` is set in `<style>`).

Inside sections, content is usually:

- A **constrained column**: `mx-auto max-w-[980px] px-4` (readable line length, centered).
- **CSS Grid** for card rows (`grid gap-4 md:grid-cols-3`) or a **two-column** split for “Technologies & tools”.

---

## Visual design approach

- **Neutral base** — Page background `neutral-50`; some sections use `bg-white` with light borders to create subtle bands.
- **Typography** — System-ui / SF-style sans stack; headings use `font-semibold` and tight tracking; body text uses muted neutrals (`text-neutral-600`).
- **Cards** — Soft tinted panels (`sky`, `indigo`, `violet`, etc.) with light borders, faint rings, and optional hover shadow — intended to feel calm and professional, not flashy.
- **Hero image** — Circular avatar with CSS filters (`brightness`, `saturate`), loaded from **`images/github-avatar.png`** with an **`onerror`** fallback to the GitHub avatar URL if the file is missing.

---

## Small scripts

At the bottom of `index.html`, a short inline script sets the footer year:

```js
document.getElementById("year").textContent = new Date().getFullYear();
```

No frameworks — plain DOM API.

---

## Assets checklist

| Asset | Typical use |
|--------|----------------|
| `images/github-avatar.png` | Favicon, apple-touch-icon, hero `<img>` `src` |

Keep these paths stable if you change filenames; update `<link>` + `<img>` accordingly.

---

## Local preview

From this directory:

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000` — use a server instead of opening the file URL directly so paths and behavior match GitHub Pages more closely.

---

## Editing tips

1. **Navigation** — Header `<a href="#…">` must match a section `id`.
2. **Copy changes** — Edit text inside the relevant `<section>`; preserve classes unless you’re intentionally changing layout.
3. **New section** — Add `<section id="newid">`, mirror an existing block’s wrapper classes, and add a nav link.
4. **External links** — Use `target="_blank"` and `rel="noreferrer"` (already used for outbound links).

---

## Related files

| File | Purpose |
|------|---------|
| `index.html` | Live site markup + styling classes |
| `index.md` | This documentation (not served as the homepage; **`index.html` is the default document on GitHub Pages**) |

If anything in this doc drifts from `index.html`, treat **`index.html` as the source of truth** and refresh this file when you make structural changes.
