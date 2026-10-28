# CDMX Food Guide — Nikolay's Famous

A curated Mexico City food guide: 277 places researched from Eater CDMX, The Infatuation, Michelin Guide Mexico 2024–25, World's 50 Best, Bon Appétit, Chilango, Nicholas Gilman's *Good Food in Mexico City*, and chef/publication recommendations.

**Live guide:** https://nikolaysfamous.com/cdmx-food-guide/

The guide is a single self-contained `index.html` — no build step, no dependencies. Search by name, neighborhood, dish or ingredient; filter by category; add personal notes and custom entries (stored in localStorage).

## To update

Source data lives in the `server` repo at `server/briefing/static/cdmx-data-source.json`. After editing the source JSON, run:

```
python3 server/briefing/tools/inject_cdmx.py static/cdmx-data-source.json
```

Then copy the regenerated `server/briefing/static/cdmx.html` over `index.html` here (`nikolaysfamous.com/cdmx-food-guide/index.html`; homeserver: `~/repos/nikolaysfamous.com/cdmx-food-guide/`) and push the `nikolaysfamous.com` repo.
