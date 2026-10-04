# Little Shelf · Kệ sách của bé

A gentle digital home library for children’s books — built for Chanel Luu’s son. Categorize the shelf, watch genre coverage, heart favorites, and get age-aware recommendations (including Vietnamese and bilingual titles that often never show up in US ISBN databases).

**Live:** https://chanelluuhai.github.io/home-library/

## What’s included

| File | Role |
|------|------|
| `index.html` | App shell, bottom dock, sheets |
| `styles.css` | Sage glass UI |
| `app.js` | Shelf, scan, favorites, recommendations, profile, import/export |
| `catalog.js` | Curated real-book recommendation catalog (no invented titles) |
| `.nojekyll` | Serve files as-is on GitHub Pages |

No build step. Data stays in this browser under `localStorage` key `littleShelf.v1`.

## Adding books

ISBN is **never** required.

1. **Scan** — Camera barcode scanner uses the browser `BarcodeDetector` API when available (`ean_13`, `ean_8`, `upc_a`, `upc_e`). If it is missing, `html5-qrcode` loads from unpkg as a fallback.
2. After a scan, the app looks up **Open Library** first (`/isbn/{isbn}.json` and `search.json?isbn=`), with covers from `covers.openlibrary.org`. If that is thin, it tries **Google Books** `volumes?q=isbn:` (no API key). Subjects map onto the genre list; you can edit everything before saving.
3. If lookup fails (common for Vietnamese books not sold in the US), the add sheet still opens with the code filled in. Type the title and save. Failed lookup never blocks save.
4. **Add without a barcode** is equally prominent — title required; author, language (Vietnamese is easy to pick), genres, optional age range, notes, optional ISBN, favorite toggle.

## Favorites & recommendations

Tap the heart on any book card or in the detail sheet. Favorites filter on the Shelf view and boost recommendation scoring:

- Age window: catalog age range must overlap the child’s age ± 6 months
- +3 genre overlap with a favorite
- +2 genre overlap with any owned book
- +2 Vietnamese/bilingual when that share of the shelf is under 30%, or he has a Vietnamese/bilingual favorite (empty shelf still surfaces young Vietnamese nursery books)
- +1 same author as a favorite
- +1 fills a genre with fewer than 2 owned books

Reasons are short and human (“Fits 1 year 9 months”, “Because he loves bedtime books”, “The Vietnamese shelf is light”).

## Age tracking

Default birthday is **2025-01-15** (mid-January 2025). As of October 2026 that is about **1 year 9 months** (~21 months). Age is always recomputed from the birthday in Profile — change the exact day if needed. Recommendations and age chips use that computed age.

## Vietnamese / non-ISBN books

Many beloved Vietnamese children’s books will not resolve via ISBN. Document them anyway: language → Vietnamese, genres → Vietnamese / Nursery rhymes / Folk tales, save without an ISBN. The Genres view has a dedicated Vietnamese shelf callout with a count of `vi` + `bilingual` books.

## Backup

Profile sheet → **Export library JSON** / **Import library JSON**. Useful when switching phones or clearing browser data.

## Design

Matches the craft of the Toddler Spots app (frosted glass, large radii, soft shadows, bottom dock, slide-up sheets) with a **different** palette and type:

| Token | Value |
|-------|-------|
| `--accent` | `#6E8B71` |
| `--accent-soft` | `#E6F0E4` |
| `--accent-dark` | `#3F5C43` |
| `--sage` | `#A9C4A4` |
| `--sage-deep` | `#5E7A5C` |
| `--ink` | `#243028` |
| `--bg` | `#E7EBE4` → `#E4EDE3` → `#D3E4D2` |

**Typography:** [Fraunces](https://fonts.google.com/specimen/Fraunces) for the wordmark and titles; [Outfit](https://fonts.google.com/specimen/Outfit) for UI. Chosen to feel warm and cutesy while staying distinct from Toddler Spots’ Alice + Nunito on blush pink.

## Local preview

Open `index.html` in a browser, or from this folder:

```bash
python3 -m http.server 8080
```

Camera scan needs HTTPS (or localhost).
