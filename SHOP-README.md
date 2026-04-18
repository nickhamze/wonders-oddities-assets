# Wonders & Oddities — WooCommerce Test Import

A synthetic product catalog for stress-testing WooCommerce themes. Every product is fictional (Bottled Monday Morning, Portable Hole, Invisible Umbrella, Moon Dust, etc.) — nothing a real store would sell, which keeps the test data from looking like a half-finished real shop.

## What's in the box

| File | Purpose |
| --- | --- |
| `wonders-oddities-products.csv` | 53-row WooCommerce import file (30 parent products + 23 variations), 67 columns, every field populated |
| `images/` (30 PNGs) | Product illustrations, 800×800 |
| `images/downloads/` (4 files) | Artifacts referenced by the 3 downloadable products — `one-hand.wav`, `lost-recipe.pdf`, `imaginary-deed.pdf`, `imaginary-seal.png` |

## Where the images live

The CSV's image and download URLs already point at the public GitHub repo:

```
https://github.com/nickhamze/wonders-oddities-assets
```

All 34 assets are hosted at the repo root. The CSV references them via GitHub's raw content URL:

```
https://raw.githubusercontent.com/nickhamze/wonders-oddities-assets/main/<filename>
```

No find-and-replace needed — the CSV is ready to import as-is.

## Catalog composition

30 parent products / 53 total rows:

- 18 simple products
- 6 variable products with 23 variations total
- 3 grouped products (bundles)
- 3 external / affiliate products (Buy-on-other-site CTA)
- 3 downloadable products (WAV / PDF / ZIP)
- 2 virtual services (no shipping)
- Featured, on-sale, low-stock, out-of-stock, and backorder states all represented
- Upsells, cross-sells, and grouped-product SKU references all internally valid

## Importing to WooCommerce

1. **WooCommerce → Products → Import → Choose File** → pick `wonders-oddities-products.csv`.
2. On step 2, WooCommerce auto-maps every column. If it flags any as unmapped (rare), accept the suggestions — all column names match WooCommerce's canonical schema.
3. Click **Run the importer**.
4. WooCommerce fetches each image URL from GitHub and attaches it to the matching product. This takes ~2–4 seconds per image; roughly 2 minutes for 30 parents + galleries.

If images fail to fetch, confirm the repo is public and that `https://raw.githubusercontent.com/nickhamze/wonders-oddities-assets/main/wonders-bottled-morning.png` loads in a browser tab.

## Column coverage

All 67 columns in the canonical WooCommerce import schema:

**Core identity:** ID, Type, SKU, GTIN/UPC/EAN/ISBN, Name, Published, Is featured?, Visibility in catalog, Short description, Description

**Pricing & sale:** Regular price, Sale price, Date sale price starts, Date sale price ends

**Tax:** Tax status, Tax class

**Inventory:** In stock?, Stock, Low stock amount, Backorders allowed?, Sold individually?

**Shipping:** Weight (kg), Length (cm), Width (cm), Height (cm), Shipping class

**Reviews:** Allow customer reviews?, Purchase note, Meta: `_wc_average_rating`, Meta: `_wc_review_count`

**Taxonomy:** Categories (nested `Shop > Curiosities`), Tags

**Media:** Images (primary + gallery on featured products)

**Downloads:** Download limit, Download expiry days, Download 1 name/URL, Download 2 name/URL

**Hierarchy & cross-links:** Parent, Grouped products, Upsells, Cross-sells

**External:** External URL, Button text

**Attributes (3 slots):** Attribute 1–3 name / value(s) / visible / global / default

**Virtual/Downloadable flags:** Meta: `_virtual`, Meta: `_downloadable`

**SEO (Yoast):** Meta: `_yoast_wpseo_title`, `_yoast_wpseo_metadesc`, `_yoast_wpseo_focuskw`

**Bookkeeping:** Meta: `_wo_test_batch` (so you can bulk-delete this test data later by meta-key filter), Position

## Cleanup

To bulk-delete the test products later, query by the custom meta key `_wo_test_batch` = `2026-04-16-a` (every row carries it). All SKUs also share the `WO-` prefix, so a SKU-prefix filter works too.

## Notes on the data

- Dimensions are in cm, weight in kg (WooCommerce defaults).
- Sale window runs 2026-04-01 → 2026-05-31 on every on-sale product.
- Grouped products have empty `Regular price` (correct — the bundle's price is the sum of its children).
- Variation rows use `Parent` in `id:####` format (required by the WooCommerce importer).
- External products have `example.com` placeholder URLs — replace with real URLs if you want the CTA to go somewhere.
- The `Purchase note` is the same cheeky line on every product. Tweak if you need per-product notes.

## If you want to regenerate or extend

The asset repo at `github.com/nickhamze/wonders-oddities-assets` is public. All 34 files are committed to `main`. If you add more products later and need more images, the generation scripts live in the Cowork session that built this catalog.
