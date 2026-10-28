# GN Catalogue 2026

Interactive product catalogue (static site, hosted on Netlify).

```
data/products.xlsx   <- edit products here (Code, Title, Brand, Category, Specs, Active)
photos/              <- product photos, one file per code: 801.jpg / .png / .webp
strips/              <- spec banners (images) used when the "Specs" cell is empty
site/                <- generated website (index.html + products.json + images/) - this is what Netlify publishes
build.py             <- converts Excel + photos into site/products.json and site/images/
```

## Design
The front end is `site/index.html` (HTML, CSS and JS in one file, no build step). It loads `products.json` and shows: hero, category rail, filterable catalogue (grid or list), product drawer, a quote list that sends the selection to WhatsApp or email, brands, private label, showrooms and contact. Colours and fonts are CSS variables at the top of the file.

## Daily workflow
1. **Change a photo**: put a file named after the code in `photos/` (e.g. `801.jpg`), overwriting the old one.
2. **Add a product**: add a row in `data/products.xlsx` (new unique Code) and put `photos/<code>.jpg`.
3. **Edit text / hide a product**: edit the row; set *Active* to `no` to hide it.
4. If you type text in **Specs**, it replaces the spec banner image for that product.
5. Run `pip install -r requirements.txt` (once), then `python build.py`.
6. Preview locally: `python -m http.server -d site 8000` and open http://localhost:8000
   (do not double-click index.html: the browser blocks loading products.json from a file).

Tip: photos on a white background, about 800 px wide, look best. Keep Excel closed when running build.py.

## Deploy on Netlify
**A. Drag & drop (no git):** run `python build.py`, then drag the `site/` folder to https://app.netlify.com/drop.

**B. Git + GitHub (automatic):**
1. Push this whole folder to a GitHub repository.
2. In Netlify: Add new site > Import from Git > pick the repo. Build command: *(empty)*. Publish directory: `site` (already set in netlify.toml).
3. Every push now redeploys the site.
4. Optional: the GitHub Action in `.github/workflows/build.yml` runs `build.py` for you whenever you change `data/` or `photos/` on GitHub (even from the web interface or your phone), commits the new `site/`, and Netlify redeploys. You then never need Python on your computer.

## Custom domain
Netlify > Domain management > Add a domain (e.g. catalogue.yourdomain.com), then add the CNAME record shown there at your registrar.
