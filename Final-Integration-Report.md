# Final Integration Report

Date: 2026-08-31  
Live site: https://jqxcode.github.io/taipei-china-itinerary/  
Published commit: `51673503833981877e0a9b08faa05a2a3dd9284e`

## Completed

- Replaced the abandoned active route with Taipei → Shanghai Hongqiao/Qibao → Suzhou → Nanjing → Zhejiang Taizhou → central Shanghai.
- Kept Hangzhou only as an explicitly unused/archive reference.
- Synchronized the itinerary, Nanjing/Suzhou/Taizhou plans, HSR documents/maps, hotels, reservations, eSIM guidance, TWAC departure details, Taipei food map, and Ximending plan.
- Added the Nanjing bathhouse day, corrected Taizhou West/Taizhou station usage, and added the low-intensity Linhai day trip.
- Added six properly attributed fallback images for Jiufen and Yangmingshan while retaining Beitou as the active 9/12 plan.
- Rebuilt the public repository from a strict file allowlist. Ticket files, PDFs, workbooks, backups, research files, credentials, and personal travel-document material were excluded.
- Removed the old per-location mini maps from the published asset set. The itinerary now has exactly one combined numbered daily map for each day from 9/9 through 9/20.

## Verification

- Generator completed without traceback.
- `DAYS = 18`; `DAYS_ZH = 18`.
- Daily maps: 12 figures and 12 unique `day-2026-09-DD.png` assets.
- Missing local HTML image/map references: 0.
- Staged security scan: `tickets/` 0; `.pdf` 0; `%23` 0; `passport` 0; `permit` 0; email patterns 0; confirmation/order-code patterns 0.
- Staged PDF/XLSX files: 0; non-daily published map assets: 0.
- Incorrect Jiangsu Taizhou wording: 0.
- Food map: all 40 entries listed, 38 verified branch pins, 10 visible drink markers, and 2 unresolved entries clearly not plotted.
- GitHub Pages build status: `built`.
- Live HTTP checks returned 200 for the site root, main itinerary, a daily map, and a referenced Jiufen image.
- Live itinerary scan: sensitive patterns 0; active-day Hangzhou headings 0; 12 daily maps; 6 fallback images.

## Remaining true blockers / user decisions

1. Confirm spending before any paid action: airport transfer, mainland HSR, hotels, admission/bathhouse bookings, or similar purchases.
2. Confirm the budget before upgrading or reissuing Mom's ticket for a lie-flat cabin.
3. The official English NPM docent option is unavailable; choosing a commercial live-English guide requires product selection and payment approval. The audio-guide fallback remains available.
4. Google My Maps/Saved List requires a logged-in manual step or user-present browser automation.
