# CDMX Food Guide — Research & Build Plan

**Goal:** Expand from 64 → ~200 entries. Every entry scored on 4 dimensions (1–10), sourced from
professional / Spanish-language writing, with links to sources. The guide should read like a
serious cultural document, not a tourist list.

**Status as of 2026-10-27: DONE.** Stages 1–11 ran (Sonnet, staged); stage 12 merged 325 records → 282 unique, verified each (open/closed, address, scores, sources) and matched Michelin 2026; 5 closed places dropped → 277 published. No score cutoff (user choice): sort/filter on the page instead. Verification workdir: homeserver ~/cdmx-verify (merge.py, run.sh, build.py).

**Original status as of 2026-10-20:**
- 64 entries in `server/briefing/static/cdmx-data-source.json` — no scores, no source links
- HTML built (hidden neighborhood pills toggle, 1–10 scoring display, source link rendering)
  at `nikolaysfamous.com/cdmx-food-guide/index.html` — awaiting scored data
- Research folder `cdmx-food-guide/research/` is empty — all stages below write here
- Lives in the `nikolaysfamous.com` repo (GitHub `nikolaykukushkin/nikolaysfamous`, Pages → nikolaysfamous.com).
  Homeserver clone: `~/repos/nikolaysfamous.com/`. Do NOT create separate repos/folders for this guide.

---

## Entry schema

Each entry in `cdmx-data-source.json` must have these fields:

```json
{
  "name": "Full name of the place",
  "neighborhood": "Polanco | Roma Norte | Roma Sur | Condesa | Centro Histórico | Coyoacán | San Rafael | Doctores | Tepito | Xochimilco | Narvarte | Del Valle | Santa María la Ribera | Juárez | Lomas | San Ángel | Tlalpan | Iztapalapa | Tepepan | other",
  "category": "tacos | fine-dining | mariscos | cantina | pulquería | mezcalería | bar | market | antojitos | regional | bakery | cafe",
  "subcategory": "optional — e.g. 'Tacos al pastor', 'Tasting menu', 'Mariscos raw bar'",
  "description": "2–4 sentences. What makes it worth going. Specific dishes, history, atmosphere. No filler.",
  "special_quality": "One crisp sentence — the single most specific reason this place is unmissable.",
  "price_range": "$ | $$ | $$$ | $$$$",
  "address": "Street address or intersection, neighborhood",
  "google_maps_query": "Name, neighborhood, Ciudad de Mexico",
  "reservation_url": "URL or null",
  "reservation_type": "walk-in | reservations-recommended | reservations-required | no-reservations",
  "menu_url": "URL or null",
  "scores": {
    "iconic": 7,
    "hype": 5,
    "food": 9,
    "weird": 3
  },
  "source_links": [
    { "title": "Publication or author name", "url": "https://..." }
  ]
}
```

### Scoring rubric (1–10)

**ICONIC** — How historically significant / institutionally important is this place?
- 9–10: Founded 1900s–1960s, defining institution (El Nivel 1855, Café de Tacuba 1912, Bar La Opera 1876)
- 7–8: Multi-decade fixture, in the fabric of the neighborhood, referenced in literature or film
- 5–6: Established, respected, 10–20 years
- 3–4: Newer but already culturally embedded
- 1–2: Recent, not yet significant

**HYPE** — How present is it in tourist guides, international press, social media?
- 9–10: On every "best of" list, TripAdvisor top, Conde Nast, NYT, Instagram viral
- 7–8: Well-known internationally, in Eater/Infatuation/Bon Appétit, high Google reviews
- 5–6: Locally known, sometimes in press, moderate following
- 3–4: Word-of-mouth only, low profile
- 1–2: Deliberately hidden, anti-hype

**FOOD** — How good is the food, as assessed by professionals, writers, local critics?
- 9–10: Michelin starred / Bib Gourmand, Latin 50 Best, OAD top-rated, critic consensus excellent
- 7–8: Multiple strong professional reviews, chef-recommended, Chilango/Milenio/El Universal praised
- 5–6: Solid professional coverage, locally acclaimed
- 3–4: Anecdotally good, limited professional coverage
- 1–2: Unknown quality (do not include)

**WEIRD** — How much does it stick out from norms?
- 9–10: Sui generis — a format that exists nowhere else (the only Michelin-starred taco stand on earth, a pulquería inside a boxing gym, a clandestine restaurant behind a bookshelf)
- 7–8: Unusual format, unexpected combination, breaks genre norms
- 5–6: Has a distinctive angle but not wildly unusual
- 3–4: Conventional format
- 1–2: Completely standard

**Inclusion threshold:** FOOD ≥ 7 AND (ICONIC + HYPE + FOOD + WEIRD) ≥ 25.
If a place doesn't meet this, it stays in the research checkpoints but does NOT go into the final guide.

---

## How to run a stage

Each stage is a standalone Claude Code task. Open Claude Code, navigate to `~/repos/nikolaysfamous.com/cdmx-food-guide`,
and paste the prompt for the stage. Each stage:
- Searches the web in English AND Spanish
- Writes its findings to `research/stage-N-<slug>.json`
- Does NOT modify `cdmx-data-source.json` (integration is the final step)

Suggested token budget per stage: ~50k–80k (medium research depth).

---

## Stages

### Stage 1 — Tacos (all styles)

**Focus:** Every major taco tradition and the best practitioners of each in CDMX.
Cover: tacos al pastor (the original spit-roast), tacos de canasta (steamed, basket vendors),
tacos de carnitas (Michoacán-style pork), tacos de barbacoa (overnight pit, weekend morning),
tacos de birria (Jalisco-style beef/goat broth-dipped), tacos de guisado (home-cooking stews),
tacos de mariscos (baja-style fish/shrimp), tacos de suadero (CDMX native — brisket in fat),
tacos de tripa, tacos de cochinita pibil.

**Sources to search:** "mejores tacos Ciudad de México" (Chilango), "best tacos Mexico City" (Eater,
Serious Eats, Saveur), "tacos canasta CDMX" (local blogs), "tacos de suadero historia" (food history),
"Guía México Desconocido tacos", "Nicholas Gilman tacos CDMX", "tacos Michelin México 2024",
Chilango.com taco rankings, Vice México street food.

**Output:** `research/stage-1-tacos.json`

**Note:** El Califa de León, El Huequito, El Vilsito, Los Cocuyos, El Turix already in the 64.
Add new entries only; note duplicates for scoring pass.

---

### Stage 2 — Fine dining & tasting menus

**Focus:** The serious restaurant scene — Michelin-starred and Bib Gourmand, Latin 50 Best,
OAD-ranked, and places that have defined contemporary Mexican cuisine.

**Sources to search:** Michelin Guide Mexico 2024–25 (full list), Latin America's 50 Best 2024,
"Guía Obertura 2024" (Mexico's homegrown guide), "mejores restaurantes Ciudad de México 2024"
(La Lista México), OAD Mexico rankings, "fine dining Mexico City" (Eater, Food & Wine, Bon Appétit),
"Elena Reygadas", "Enrique Olvera Pujol", "Quintonil Jorge Vallejo", "Máximo Bistrot Eduardo García",
"Sud 777 Edgar Núñez", "chefs estrellas Michelin México". Chef interviews in English and Spanish.

**Output:** `research/stage-2-fine-dining.json`

**Note:** Pujol, Quintonil, Rosetta, Máximo Bistrot, Sud 777, Nicos already in the 64. Look for
what's missing: Alcalde (Guadalajara but worth noting?), Merotoro, Expendio de Maíz Sin Nombre,
Restaurante San Ángel Inn, Contramar (already in 64 as mariscos).

---

### Stage 3 — Cantinas & pulquerías

**Focus:** The historic drinking/eating culture. CDMX cantinas (free botanas with drinks, dark wood,
tiled floors) and pulquerías (fermented agave, centuries old). These are social institutions.

**Sources to search:** "cantinas históricas Ciudad de México" (El Universal, Chilango), "pulquerías CDMX
historia", "mejores cantinas México DF", "cantinas con botanas gratis CDMX", "pulque CDMX cultura",
"Andrés Iniesta cantinas México" (food writer), Nicholas Gilman cantinas guide, "La Mascota cantina",
"El Nivel 1855", "Las Duelistas boxing pulquería", "cantinas para perderse CDMX", "cultura cantinera México".

**Output:** `research/stage-3-cantinas.json`

**Note:** La Mascota, El Nivel, Las Duelistas already in the 64. Find the 5–10 others that belong.

---

### Stage 4 — Mariscos & seafood

**Focus:** Mexico City's surprising seafood scene — 2,200m above sea level, yet some of Latin
America's best raw bars and cevicherías. Covers: ceviche, aguachile, raw oysters, tostadas de
marlin, sashimi-style mariscos.

**Sources to search:** "mariscos Ciudad de México mejores" (Chilango, El Financiero), "cevicherías CDMX",
"aguachile CDMX", "ostiones CDMX restaurante", "Contramar history" (already in 64), "La Docena CDMX",
"mariscos al estilo Nayarit CDMX", "Bellopuerto mariscos", "el mejor ceviche en México DF",
"tostadas de marlin CDMX", Mexico City food blogs in Spanish on seafood.

**Output:** `research/stage-4-mariscos.json`

---

### Stage 5 — Markets

**Focus:** The great public markets of CDMX — not just tourist markets but working mercados
with extraordinary food stalls inside.

**Sources to search:** "mercados de Ciudad de México gastronomía" (Mexico Desconocido, Chilango),
"Mercado de San Juan exotic ingredients", "Mercado Jamaica flowers and food", "Mercado de la Merced
history", "Mercado de Medellín Condesa", "Mercado de Coyoacán tostadas", "Mercado de Tepito",
"mercados con cocinas económicas CDMX", "Mercado de Sonora", "Mercado Moctezuma", food market
history guides in Spanish.

**Output:** `research/stage-5-markets.json`

---

### Stage 6 — Antojitos, street food & tortas

**Focus:** The cooked street food canon — not tacos (covered in Stage 1) but everything else:
tortas, tlayudas, tamales, gorditas, quesadillas de huitlacoche, esquites, elotes, pambazo,
memelas, sopes, huaraches, tlacoyos.

**Sources to search:** "antojitos mexicanos CDMX mejores" (Chilango, Vice México), "tortas CDMX historia",
"tamales de CDMX" (Tamal de Elote, Tamales Especiales Claudia), "quesadillas huitlacoche CDMX",
"gorditas de nata CDMX", "pambazos CDMX", "Siembra Tortillería Bib Gourmand", "mejores quesadillas
CDMX", "elotes y esquites CDMX", "tlayudas en México DF", Nicholas Gilman antojitos guide.

**Output:** `research/stage-6-antojitos.json`

---

### Stage 7 — Mezcalerías & cocktail bars

**Focus:** Mezcal culture and the cocktail bar scene. CDMX is arguably the world capital of mezcal
drinking. Also: serious cocktail bars doing Mexican-inflected drinks.

**Sources to search:** "mezcalerías CDMX mejores" (Saveur, El Universal, Chilango), "La Clandestina
mezcal CDMX", "Los Amantes mezcalería", "Pulquería Los Insurgentes", "bares de mezcal Ciudad de México",
"coctelería mexicana CDMX", "mezcal artesanal bares DF", "Bar Cambalache CDMX", "La Botica mezcal",
"bares con cócteles mexicanos CDMX 2024", "mezcal maestros ensambles raros CDMX".

**Output:** `research/stage-7-mezcal-bars.json`

---

### Stage 8 — Regional Mexican (non-CDMX origin, best in CDMX)

**Focus:** Places in CDMX that serve exceptional food from other Mexican regions: Oaxacan moles and
tlayudas, Veracruz seafood, Yucatecan cochinita, Pueblan mole negro, Michoacán carnitas,
Guerrero pozole.

**Sources to search:** "cocina oaxaqueña en Ciudad de México", "restaurantes de Veracruz en CDMX",
"Yucatán cochinita pibil CDMX", "mole negro mejor CDMX", "Puebla en CDMX restaurante", "comida
regional México DF", "Xaachila restaurante oaxaqueño", "Regional cocinas de México CDMX",
"Casa de Toño pozole", food critics on regional Mexican in CDMX, Chilango regional guides.

**Output:** `research/stage-8-regional.json`

---

### Stage 9 — Spanish-language deep dive

**Focus:** Sources and institutions that only appear in Spanish-language writing — local food critics,
neighborhood blogs, the CDMX food-obsessed Twitter/Instagram accounts, regional newspapers.
This stage finds places that international guides have completely missed.

**Sources to search (all in Spanish):** Chilango.com (full food archive 2018–2024), Vice México cocina,
El Universal gastronomía, Milenio cultura y gastronomía, Proceso gastronomía, Expansión lifestyle,
Time Out México comida, "Instagram cuentas de comida CDMX", "bloggers comida Ciudad de México",
"críticos gastronómicos mexicanos", "guías de comida callejera CDMX", "foros de comida CDMX",
"Chilango 50 mejores restaurantes 2023 2024", "El Gourmet México", "La Jornada gastronomía CDMX",
"Reforma gastronomía México". Search for critics by name: Alonso Ruvalcaba, Ricardo Muñoz Zurita
(chef-historian), Cristina Barros, Marco Buenrostro.

**Output:** `research/stage-9-spanish-sources.json`

**Note:** This stage may surface duplicates already in earlier stages — that's fine, it adds
Spanish-language source links to existing entries.

---

### Stage 10 — Historical institutions (pre-1960)

**Focus:** The real old guard. Places where the city's history lives in the walls — cantinas,
cafés, fondas, restaurants, and pulquerías operating for 50+ years. Historical context, famous
regulars, what has and hasn't changed.

**Sources to search:** "restaurantes históricos México DF" (Artes de México, historia gastronómica),
"Café de Tacuba historia 1912", "Bar La Opera historia 1876", "fondas antiguas Ciudad de México",
"Fonda El Refugio historia", "Hostería de Santo Domingo historia", "El Cardenal historia",
"cantinas centenarias CDMX", "patrimonio gastronómico México DF", "lugares históricos para comer
CDMX", Nicholas Gilman historical guide, "historia culinaria Ciudad de México".

**Output:** `research/stage-10-historical.json`

---

### Stage 11 — Bakeries & exceptional cafes

**Focus:** Pan dulce, sourdough, pastelería, and the handful of cafés genuinely worth including
(inclusion bar is very high — a café must be either historically significant or a culinary
institution in its own right).

**Sources to search:** "panaderías artesanales CDMX mejores" (Chilango, El Universal), "Panadería Rosetta
Elena Reygadas rol de guayaba", "Gala panadería chef", "CUMBÉ café CDMX", "BUNA café cafechata",
"mejores cafés de especialidad Ciudad de México", "pan de dulce tradicional CDMX",
"Cayetana panadería", "Expendio de Maíz Sin Nombre tortillas", "Siembra Tortillería nixtamal",
"churros Churrería El Moro 1935 historia", Spanish-language baking press.

**Output:** `research/stage-11-bakeries.json`

---

### Stage 12 — Synthesis, scoring & final data build

**This is the integration stage. Run AFTER stages 1–11 are complete.**

**Instructions for the agent running this stage:**

1. Read all 11 checkpoint files from `cdmx-food-guide/research/`
2. Read the existing 64 entries from `server/briefing/static/cdmx-data-source.json`
3. Deduplicate: match by name (fuzzy) and address. For duplicates, merge source_links from all
   sources; keep the richer description.
4. Score every entry on all 4 dimensions (1–10) using the rubric above.
5. Filter: keep only entries where FOOD ≥ 7 AND (sum of all 4 scores) ≥ 25.
6. Target: ~200 entries after filtering. If significantly over, raise the threshold.
   If significantly under, lower to FOOD ≥ 6 AND sum ≥ 22.
7. Write the full merged, scored, filtered set to `server/briefing/static/cdmx-data-source.json`
   (overwrite).
8. Run: `cd ~/repos/server && python3 briefing/tools/inject_cdmx.py briefing/static/cdmx-data-source.json`
9. Copy updated HTML into the site repo:
   - `cp ~/repos/server/briefing/static/cdmx.html ~/repos/nikolaysfamous.com/cdmx-food-guide/index.html`
10. Report: entry count, category breakdown, top 5 highest-FOOD scores, top 5 highest-WEIRD scores.

**Output:** Updated `cdmx-data-source.json` + regenerated HTML in `nikolaysfamous.com/cdmx-food-guide/`.

---

## Publishing (after Stage 12)

The guide is already served from the `nikolaysfamous.com` repo (GitHub Pages, custom domain set).
Publishing = commit + push:
```
cd ~/repos/nikolaysfamous.com
git add cdmx-food-guide && git commit -m "CDMX food guide — $(date +%Y-%m-%d)"
git push
```
Live at `https://nikolaysfamous.com/cdmx-food-guide/`.

---

## Running order

Stages 1–11 are fully independent and can be run in any order, or in parallel sessions.
Stage 12 requires all of 1–11 to be done first.

Recommended batching to stay within token limits:
- **Session A:** Stages 1, 3, 5 (tacos, cantinas, markets)
- **Session B:** Stages 2, 4 (fine dining, mariscos)
- **Session C:** Stages 6, 7, 8 (antojitos, mezcal, regional)
- **Session D:** Stages 9, 10 (Spanish deep dive, historical)
- **Session E:** Stage 11 (bakeries/cafes)
- **Session F:** Stage 12 (synthesis — most token-intensive; use extended thinking / Opus)

---

## Checkpoint file format

Each stage writes a JSON array of entries following the entry schema above.
Minimal viable checkpoint entry (scores and source_links are required):

```json
[
  {
    "name": "...",
    "neighborhood": "...",
    "category": "...",
    "description": "...",
    "special_quality": "...",
    "price_range": "$$",
    "address": "...",
    "google_maps_query": "..., Ciudad de Mexico",
    "reservation_url": null,
    "reservation_type": "walk-in",
    "menu_url": null,
    "scores": { "iconic": 8, "hype": 6, "food": 9, "weird": 4 },
    "source_links": [
      { "title": "Chilango — Los 50 mejores 2024", "url": "https://..." }
    ]
  }
]
```

Entries without source_links or scores are incomplete and should not be written.
