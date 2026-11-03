# btq.sa — بي بوتيك (landing + store page)

Static RTL Arabic site. No build step.

## Run locally
    python3 -m http.server 8000      # then open http://localhost:8000
(any static server works; do NOT open index.html via file:// — models are loaded with fetch)

## Deploy
Upload the whole folder as-is to GitHub Pages / Netlify / Vercel / any static host. Keep the folder layout.

## Structure
- `index.html`  — the whole page: markup + CSS (`<helmet><style>`) + logic (`class Component extends DCLogic`).
- `support.js`  — the Design Component runtime that renders `<x-dc>`, `{{holes}}`, `<sc-for>`, `onClick="{{fn}}"`. Required; keep it next to index.html. (It also bundles React.) If you want to drop it, the page must be ported to a normal framework.
- `assets/`     — everything the page loads:
  - `*.txt`  = base64-encoded GLB models (decoded in the browser, then handed to <model-viewer>)
  - `*.png`  = poster images + the wrist photos + logo icon
  - `*.svg`  = brand pattern + frieze
  - `f7d94a…js` = @google/model-viewer (UMD build); `ea9a1f…js` = meshopt decoder stub
- `models/`     — the same 6 models as real `.glb` files (for reference / re-export). Not used by the page. They are meshopt-compressed (EXT_meshopt_compression).

## Where things live in index.html
- Products: method `pieces()` — one object per piece: `{ data, poster, tint, name, sub, price, alt, orbitR }`.
  `gridOnly: true` keeps a piece out of the hero carousel (the ring).
  Prices that read `[السعر]` are placeholders — set `const P` / each `price`.
- Product page copy + wrist photos: method `pvInfo(i)` (`desc`, `g` heading, `imgs: [{src,pos,alt}]`).
- Dark/light theme: CSS variables in `:root` / `html[data-theme=dark]`; toggle = `toggleTheme()`; choice saved in localStorage `btq-theme`.
- Hero: swipe/arrow-free carousel (`go()`, pointer swipe in `componentDidMount`), auto-rotation `rotateEvery` prop.
- Product cards: `.pCard2` styles + `gridPieces` in `renderVals()`.

## Models → file mapping
| name | asset | models/ |
|---|---|---|
| محور الكون (globe) | assets/9f6bfeb4….txt | models/globe-mehwar-alkawn.glb |
| ساعات سعودي تايم | assets/adf6c107….txt | models/watches-saudi-time.glb |
| كوب السفر | assets/813cdcd5….txt | models/travel-tumbler.glb |
| مجسم البشت | assets/00046d4f….txt | models/bisht-sculpture.glb |
| خاتم ذهبي | assets/daee4269….txt | models/gold-ring.glb |
| (pour section, unused now) | assets/2bcc8850….txt | models/pour-tumbler-no-lid.glb |

## Known gaps
- Placeholder prices `[السعر]` for 4 of 5 products; placeholder product descriptions.
- Wrist photos exist only for the watches (2 of 3 slots); other products show an empty "photo" slot.
- Footer links are `#`.
- Fonts (IBM Plex Sans Arabic, Aref Ruqaa) load from Google Fonts.
