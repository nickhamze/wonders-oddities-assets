# Wonders & Oddities — Theme Test Dataset

A self-contained, fictional e-commerce dataset for stress-testing WordPress and WooCommerce themes. Everything here is fake: the shop, the products (*Bottled Monday Morning*, *Portable Hole*, *Moon Dust of Certified Lunar Origin*), the authors, the customers who write comments. The data is designed to exercise every template a theme is likely to render.

## What's in this repo

| File | What it does |
| --- | --- |
| `wonders-oddities-products.csv` | WooCommerce product import — 30 parent products + 23 variations = 53 rows, 67 columns. Covers simple, variable, grouped, external/affiliate, downloadable, and virtual product types. |
| `wonders-oddities-content.xml` | WordPress WXR import — 8 pages, 20 posts (2 sticky), 3 authors, 7 nested categories, 20 tags, 35 threaded comments, 2 nav menus. |
| `SHOP-README.md` | Step-by-step WooCommerce import instructions + column reference. |
| `CONTENT-README.md` | Step-by-step WordPress content import instructions. |
| `wonders-*.png` (30 files) | Product featured images. |
| `wonders-page-*.png` (8 files) | Page featured images. |
| `wonders-post-*.png` (20 files) | Blog post featured images. |
| `one-hand.wav`, `lost-recipe.pdf`, `imaginary-deed.pdf`, `imaginary-seal.png` | Download artifacts referenced by the 3 downloadable WooCommerce products. |

## Quick start

1. **Products** (WooCommerce): `WooCommerce → Products → Import` → upload `wonders-oddities-products.csv`.
2. **Content** (WordPress): `Tools → Import → WordPress → Run Importer` → upload `wonders-oddities-content.xml`. Check "Import Attachments" to pull images into the Media Library.
3. **Menus** (after content import): `Appearance → Menus` → assign *Main Navigation* and *Footer Navigation* to theme locations.

Image URLs in both files point at this repo:

```
https://raw.githubusercontent.com/nickhamze/wonders-oddities-assets/main/<filename>
```

No find-and-replace or upload-to-media-library step required.

## What this dataset exercises

### Product / shop templates
Simple, variable (size/color variations), grouped (bundles), external (affiliate CTA), downloadable, and virtual product types. Featured vs. non-featured. On-sale vs. regular. In stock / low stock / out of stock / backorder. Upsells, cross-sells, and grouped-product relationships (all internally consistent). Up to 3 attributes per variable product. Gallery images on featured products. Reviews (rating + count as meta). GTIN, dimensions, shipping class.

### Content / blog templates
Multiple authors with bios. Nested categories (parent → child) and a flat tag list. Sticky posts. Post formats from short utility posts to long editorial essays. Threaded comments (1–2 levels deep). Featured images on every page and post. Yoast SEO meta. Custom fields (`_wo_reading_time`). Backdated publish times across multiple months for archive/pagination testing.

### Site-wide
Two navigation menus (header + footer) with a mix of page links and a custom link (`/shop/`). Privacy Policy, FAQ, Contact, Shipping & Returns pages. A home page with a hero section, featured products block reference, and customer quote. A Lookbook page with a gallery block.

## Cleanup

Every imported product, page, post, and attachment carries the custom meta `_wo_test_batch` = `2026-04-16-a`. Use that to bulk-delete later:

- WP-CLI: `wp post delete $(wp post list --post_type=any --meta_key=_wo_test_batch --meta_value=2026-04-16-a --format=ids) --force`
- Or use a plugin like "WP Bulk Delete" filtered by that meta key.

All WooCommerce SKUs share a `WO-` prefix. The 3 imported authors must be removed manually under **Users**.

## Regenerating

The generation scripts that built this dataset live in the Cowork session that produced it. They use only Python stdlib + Pillow + reportlab, and are deterministic — re-running them produces identical files (same IDs, same image bytes).

## License / use

The contents of this repo are intentionally fictional and exist solely for theme QA. Use however you like; attribution appreciated but not required. No warranty, expressed or implied, regarding the actual production of time, the authenticity of Carl's Moon Dust, or the safety of opening a Portable Hole in your ceiling.
