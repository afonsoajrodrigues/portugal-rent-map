# # Portugal Rent Map

**How much does it cost to put a roof over your head in Portugal?** This project
answers that question one municipality at a time, with an interactive map that turns
a dry spreadsheet of rent prices into something you can actually read at a glance.

It was built as a data-journalism piece: a map you can explore, wrapped in a short
story about a housing market where, in the capital, a single one-bedroom flat can
swallow almost an entire salary.

![A choropleth map of Portugal showing rent prices by municipality](preview.png)

## What you're looking at

The map shades every municipality on the Portuguese mainland by its median rent.
Darker reds mean more expensive; pale yellows mean cheaper. Two buttons let you
switch the lens:

- **Price per m²** — the raw figure published by the statistics office.
- **Rent for a 1-bedroom (45 m²)** — the same data translated into a number that
  feels real: roughly what a small flat would cost you each month.

Hover over any municipality and a small card tells you its value, whether the figure
is its own or borrowed from its sub-region, and which district it belongs to.

## Run it on your own computer

The page loads its data (a map file and a spreadsheet) the moment it opens. Browsers
block that when you simply double-click an HTML file, so the map would come up blank.
The fix is to serve the folder through a tiny local web server — and Python, which
you probably already have, ships with one:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000> in your browser. Press `Ctrl + C` in the terminal
to stop the server when you're done.

## Put it online with GitHub Pages

Once it's on GitHub Pages the server problem disappears, because Pages serves the
files over the web for you.

1. Create a new **public** repository on GitHub.
2. Upload the files so that **`index.html` sits at the root** of the repository, not
   inside a sub-folder.
3. Open **Settings → Pages**, set the source to *Deploy from a branch*, choose the
   `main` branch and the `/ (root)` folder, and save.
4. Give it a minute. Your site goes live at
   `https://<your-username>.github.io/<repository-name>/`.

The paths inside `index.html` are relative, so everything works from the root with no
changes. One thing to watch: GitHub is case-sensitive, so file names have to match
exactly.

## What's in this repository

| File | What it is |
|------|------------|
| `index.html` | The whole thing — article, styling and the D3 map in a single file |
| `portugal_rents.csv` | The data: zone, level, price per m², and the 1-bedroom rent |
| `portugal_municipalities.geojson` | The borders of the 278 mainland municipalities |
| `preview.png` | The screenshot used above |
| `README.md` | This file |

## How it works, in plain terms

The page reads the map shapes and the price data at the same time, then matches each
municipality to its price by name — ignoring accents and capital letters so that
"Cávado" and "CAVADO" count as the same place. A Mercator projection flattens the
round Earth onto the screen, and a six-step colour scale paints each shape according
to its value. When you press a button, the map simply recolours itself with the other
number. It's all done with [D3.js](https://d3js.org/), a JavaScript library for
data-driven graphics.

## A note on the data and method

- The rent figures come from **INE** (Statistics Portugal), for the first quarter of 2026.
- **23 municipalities** have their own published figure. The rest inherit the value of
  their sub-region (NUTS III) — the tooltip always tells you which is which, so the map
  stays honest about what is measured and what is estimated.
- Only **mainland Portugal** is shown, because the source has no figures for the Azores
  or Madeira.
- The monthly rent for a one-bedroom flat assumes a size of **45 m²** (price per m² × 45).

## Credits

- Rent prices: INE (Statistics Portugal).
- Map geometry: simplified from the CAOP official administrative boundaries.
- Built with D3.js v7.

---

Found something off, or want to take the map further? The whole project lives in one
readable HTML file — open it, change a colour, add a paragraph, and make it yours.
