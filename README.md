# 🇧🇪 Leuven Tourism & Cafe Map

An interactive multilingual map of Leuven's tourist attractions and cafes with WiFi/socket filtering.

**Live demo → [your-username.github.io/leuven-map](https://your-username.github.io/leuven-map)**

![Languages](https://img.shields.io/badge/languages-EN%20%7C%20NL%20%7C%20FR%20%7C%20DE%20%7C%20中文-blue)
![Data](https://img.shields.io/badge/data-Google%20Places%20%2B%20OSM-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## Features

- 🌍 **5 languages** — English, Nederlands, Français, Deutsch, 繁體中文
- 🏛 **17 attractions** from [Visit Leuven](https://www.visitleuven.be) with multilingual names & descriptions
- ☕ **Cafe database** scraped via Google Places API + OSM
- 📶 **WiFi & Socket filter** — find laptop-friendly cafes near any attraction
- 🌙 **Dark / light theme**
- 📱 **Mobile-friendly** responsive layout

---

## Quick Start (GitHub Pages)

### Step 1 — Fork & enable Pages

```
1. Fork this repository
2. Go to Settings → Pages
3. Source: Deploy from branch → main → / (root)
4. Save → your site is live at https://YOUR-USERNAME.github.io/leuven-map
```

The site loads with **demo data** immediately. Replace `leuven_data.json` with real scraped data for production use.

---

## Getting Real Data

### Requirements

```bash
pip install requests openpyxl pandas beautifulsoup4
```

### Get a Google Places API Key

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → Enable **Places API**
3. Credentials → Create API Key
4. **Cost estimate**: ~$1.50 per run (well within $200/month free tier)

### Run the scraper

```bash
python leuven_scraper_v3.py --google-key YOUR_API_KEY
```

This generates:
```
leuven_output/
├── leuven_data.json        ← copy this to repo root
└── leuven_tourism_cafes.xlsx
```

### Update the map

```bash
# Copy new data to repo
cp leuven_output/leuven_data.json ./leuven_data.json

# Commit and push
git add leuven_data.json
git commit -m "Update cafe data $(date +%Y-%m-%d)"
git push
```

GitHub Pages redeploys automatically within ~1 minute.

---

## Project Structure

```
leuven-map/
├── index.html              ← the map (multilingual, self-contained)
├── leuven_data.json        ← GeoJSON data (attractions + cafes)
├── leuven_scraper_v3.py    ← scraper script
└── README.md
```

### GeoJSON Schema

Each **attraction** feature includes:

```json
{
  "feature_type": "attraction",
  "name": "Groot Begijnhof",
  "names": { "en": "...", "nl": "...", "fr": "...", "de": "...", "zh": "大貝居安院" },
  "descriptions": { "en": "...", "nl": "...", "fr": "...", "de": "...", "zh": "..." },
  "category": "religious",
  "nearby_cafes": [...]
}
```

Each **cafe** feature includes:

```json
{
  "feature_type": "cafe",
  "wifi": "yes | no | unknown",
  "socket": "yes | no | unknown",
  "rating": 4.3,
  "open_now": "yes | no | unknown"
}
```

---

## Data Sources

| Source | Used for |
|--------|----------|
| [Visit Leuven](https://www.visitleuven.be) | Attraction list & URLs |
| [Google Places API](https://developers.google.com/maps/documentation/places/web-service) | Cafe names, addresses, ratings, opening hours, reviews |
| [OpenStreetMap Overpass](https://overpass-api.de) | WiFi & socket tags |

---

## Local Development

```bash
# Serve locally (needed for fetch() to work)
python -m http.server 8080
# Open http://localhost:8080
```

> **Note**: Opening `index.html` directly via `file://` will show the drag-and-drop upload screen instead of auto-loading the JSON. Use a local server or GitHub Pages for the full experience.

---

## Updating Data

Re-run the scraper periodically to keep cafe hours and ratings current:

```bash
python leuven_scraper_v3.py --google-key YOUR_KEY
cp leuven_output/leuven_data.json .
git add leuven_data.json && git commit -m "Data refresh" && git push
```

---

## License

MIT — free to use, adapt, and deploy.
