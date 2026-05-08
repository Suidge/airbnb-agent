---
name: airbnb-search
description: Search Airbnb listings by location, date, and guest count, then filter by price, rating, bedrooms, and keyword match for parking, basement, and renovation details. Use when searching or comparing Airbnb rentals, analyzing listings, or finding short-term stays with specific requirements.
version: 1.0.0
---

# Airbnb Search

Search Airbnb via pyairbnb API (no login/browser required), filter by criteria, and analyze listing detail pages for parking, basement, and renovation keywords.

## Setup (one-time)

```bash
bash scripts/setup.sh
```

Creates `.venv/` with `pyairbnb` + `curl-cffi`. All scripts use `$SKILL_DIR/.venv/bin/python3`.

## Quick Reference

| Topic | File |
|-------|------|
| API fields & data structures | `references/api-fields.md` |
| Common city coordinates | `references/api-fields.md` (bottom) |

## Workflow

### 1. Search + Filter

```bash
.venv/bin/python3 scripts/search.py "Brighton" \
  --check-in 2026-08-27 --check-out 2026-09-04 --guests 3 --currency CNY \
  --bedrooms 2 --max-price 20000 --min-rating 4.8 \
  --center-lat 50.8225 --center-lng -0.1373 --max-distance-km 2.5
```

Outputs to `/tmp/airbnb_candidates.json` (sorted by distance).

### 2. Detail Analysis

```bash
.venv/bin/python3 scripts/details.py /tmp/airbnb_candidates.json \
  --top 15 --check-in 2026-08-27 --check-out 2026-09-04
```

Fetches detail pages and searches for:
- 🅿️ Parking: free parking, driveway, garage, 停车位
- ⚠️ Basement: basement, semi-basement, garden level, 地下室
- 🔨 Renovation: newly renovated, refurbished, brand new, 新装修

Output to `/tmp/airbnb_results.json` with `parking`/`basement`/`renovated` arrays per listing.

## Key Parameters

| Param | Default | Description |
|-------|---------|-------------|
| `--bedrooms` | 0 | Min bedrooms (0 = no filter) |
| `--max-price` | 0 | Max total price (0 = no filter) |
| `--min-rating` | 0 | Min rating 0-5 (0 = no filter) |
| `--max-distance-km` | 5 | Search radius |
| `--center-lat/lng` | 50.8225/-0.1373 | Brighton College |
| `--currency` | CNY | Price currency |
| `--language` | zh | Result language |

Coordinates for common cities: `references/api-fields.md`

## Limitations

- Searches by coordinate box, not place name (see `references/api-fields.md` for coords)
- Detail scraping uses HTML — subject to Airbnb layout changes
- No login (can't see personalized prices or wishlists)
- `curl-cffi` impersonation may break with Chrome updates
