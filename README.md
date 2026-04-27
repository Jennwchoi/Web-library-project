# The Quiet Library

> *A web library for the kind of reading you didn't plan.*

A 3D web library where visitors wander through a Gaussian splat bookshelf, click on glowing books, and read full-text classics from Project Gutenberg in a warm, paper-textured reading panel.

![The Quiet Library](./screenshot.png)

---

## Why this exists

Walking into a physical library, you rarely leave with only the book you came for. You browse spines, get distracted by a cover, pull something off a shelf you'd never have searched for. The serendipity is the point.

Web libraries tend to be the opposite: a search bar, a result, an exit. The visit ends the moment the information is found.

**The Quiet Library is an attempt to bring browsing back online** — to recreate the experience of *finding what you didn't know you were looking for*. You enter a 3D space. You don't know what's on each shelf until you reach it. The architecture itself slows you down, in the same way a quiet reading room does.

---

## What it does

- **Loads a real Gaussian splat bookshelf** captured with [SuperSplat](https://playcanvas.com/supersplat/editor) and rendered live in the browser
- **Interactive book markers** glow on the splat — click to open the full text in a side reading panel
- **Reads from Project Gutenberg** — fifteen public-domain classics included by default (Pride and Prejudice, Frankenstein, Moby-Dick, Dracula, etc.)
- **Warm vintage reading room atmosphere** — Three.js room with floor, ceiling, walls, hanging pendant lights, and procedurally-generated background bookshelves
- **Admin mode** for placing book markers on the shelf via click-to-place, with JSON export

---

## Tech stack

| Layer | Tool |
|-------|------|
| 3D capture | [SuperSplat](https://playcanvas.com/supersplat/editor) (PLY export) |
| Splat rendering | [`@mkkellogg/gaussian-splats-3d`](https://github.com/mkkellogg/GaussianSplats3D) |
| Scene / lighting | [Three.js](https://threejs.org/) (r170) |
| Typography | [Playfair Display](https://fonts.google.com/specimen/Playfair+Display) + [EB Garamond](https://fonts.google.com/specimen/EB+Garamond) |
| Text source | [Project Gutenberg](https://www.gutenberg.org/) (public domain) |
| Module loading | Native browser ES modules + import maps |
| Local dev server | [`serve`](https://www.npmjs.com/package/serve) (any static host works) |

No frameworks. No build step. One HTML file.

---

## Getting started

### Files in this repo

```
the-quiet-library.html        the entire app
bookshelf compressed.ply      Gaussian splat scene (350MB-ish)
gaussian-splats-3d.module.js  splat rendering library
three.module.js               Three.js library
```

### Run locally

```bash
cd "Jenn 3d"
npx serve .
```

Then open the printed URL (usually `http://localhost:3000`) and click `the-quiet-library.html`.

### Place books on the shelf

1. Press `Shift + A` to open admin mode
2. Click `+ PLACE BOOK`, then click anywhere on the splat
3. Choose a book from the picker
4. Use `Splat Y: ▲ / ▼` if the splat doesn't sit flush with the floor
5. When done, click `EXPORT JSON` and paste the array into the `HOTSPOTS` constant near the top of the HTML

After saving, every visitor sees the books in the positions you set.

---

## Adding more books

The catalogue lives in the `BOOKS` array in `the-quiet-library.html`:

```js
{ id: 1342, title: "Pride and Prejudice", author: "Jane Austen", year: 1813, color: "#7A3B1A" }
```

`id` is the Project Gutenberg ebook ID — find it in the URL of any book on gutenberg.org. The reader auto-fetches the plain-text version, strips the Gutenberg header/footer, and formats it.

---

## Design notes

- **Coordinate system** — Gaussian splat data uses `cameraUp = [0, -1, 0]`. The Three.js room is wrapped in a Group rotated `Math.PI` around Z so it renders correctly under the flipped camera while still being writable in standard Y-up.
- **Carpet glow** — a soft warm rectangle on the floor at low opacity. It's the only light-emitting surface the user sees as a "panel," anchoring the room visually.
- **Pendant lights** — hang from the ceiling on thin cords. Each casts a warm point light on the floor.
- **Background bookshelves** — three procedurally-generated walls of randomly-sized books in muted colors, fading into fog. They're not interactive; they exist purely to give the captured splat a place to be.

---

## Credits

Bookshelf splat captured by Jenn Choi.
Texts from [Project Gutenberg](https://www.gutenberg.org/) (public domain).
Splat rendering by [Mark Kellogg](https://github.com/mkkellogg/GaussianSplats3D).

---

*"The Quiet Library is a place to lose the question and find the answer to one you hadn't asked yet."*
