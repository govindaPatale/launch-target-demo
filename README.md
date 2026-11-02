# Reading Nook — Adobe Target Recommendations training

## Deploy
Upload `index.html`, `about.html`, `catalog.html`, `book.html`, `catalog-data.js`, and `recommendations-feed.csv` to the GitHub Pages repository root. Keep the existing Adobe Tags development embed. This package preserves the current Web SDK embed from the live repository at time of preparation; review the script URL against your Tags property if it changes.

## Demo pages
- Homepage: `index.html`, Recommendations placeholder `#recs-target-slot`.
- Catalog: `catalog.html`, eight fictional products.
- Product page: `book.html?id=RN-001` ... `RN-008`, placeholder `#recs-product-slot`.

## Target catalog feed
`recommendations-feed.csv` has `## RECS` as the first line and column names following Adobe Target's CSV feed format. Upload it from Target Recommendations > Feeds if the UI supports manual file upload, or host it and configure a URL feed. Target ingestion may take time. Confirm catalog items in Catalog Search before creating the activity.

## Web SDK / Tags
The product page includes a dedicated `alloy('sendEvent')` call with `commerce.productViews` and `data.__adobe.target` attributes. This is a small training convenience; in production move it into a Tags rule (and avoid duplicate product view events). The existing global page-view / MVT rules still run and may be scoped separately in Tags later. The product event is sent after `alloy` becomes available, but it may run after the first page personalization request: **do not claim it is available on the first `__view__` decision.** For current-item criteria, modify the initial personalization request to include `entity.id` or use a targeted follow-up decision request after book details are known.

## Recommendations delivery
Use a **VEC Recommendations activity on the homepage** first (`#recs-target-slot`), with a broad criteria compatible with a home page, a collection of the eight books, and a 3-across design. The Web SDK existing `__view__` render decisions path handles VEC DOM actions. Do not use a Current Item criterion on the homepage because it has no `entity.id`. Pick a supported fallback criterion if the main algorithm has insufficient behavioral history. If selecting Form-based custom scope instead, you must apply the returned HTML proposition explicitly with `applyPropositions`.

## Note
All books and prices are fictitious. No real checkout is implemented. Catalog images use illustrative remote text placeholder URLs; replace with your own hosted images if needed.
