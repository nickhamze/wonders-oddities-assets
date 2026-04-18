# Wonders & Oddities — WordPress Content Import

A WXR (WordPress eXtended RSS) file that imports pages, posts, authors, categories, tags, comments, and menus into any WordPress install. Pairs with the product CSV to give a theme a full site's worth of content for QA.

## What's inside

| File | Purpose |
| --- | --- |
| `wonders-oddities-content.xml` | The WXR import file. 196 KB, 3,389 lines. |
| Images are hosted on GitHub — already referenced by absolute URL in the WXR. No upload step needed for this import. |

## What gets created

| Entity | Count |
| --- | --- |
| Authors | 3 (Brenda Ash, Carl Lunar, Percival Aftermath) |
| Pages | 8 (Home, About, Journal, Lookbook, Shipping & Returns, FAQ, Contact, Privacy Policy) |
| Posts | 20 (2 sticky, 18 regular — mix of how-tos, editorial, interviews, reviews, seasonal) |
| Categories | 7 (nested: Curiosities → Hole-ology, Curiosities → Sock Affairs, Moods & Feelings, Forbidden Snacks, Behind the Counter, Guides & How-Tos) |
| Tags | 20 |
| Comments | 35 (threaded, 3–5 per post) |
| Attachments | 28 (one featured image per page/post, fetched from GitHub during import) |
| Nav menus | 2 (Main Navigation, Footer Navigation — 9 items total) |

## Import steps

1. **Tools → Import** in wp-admin.
2. Scroll to **WordPress** → click **Install Now** if you don't have the importer plugin, then **Run Importer**.
3. Click **Choose File** → pick `wonders-oddities-content.xml`.
4. Click **Upload file and import**.
5. **Author mapping screen**: you'll see the 3 authors (brenda-ash, carl-lunar, percival-aftermath). Either:
   - Leave "create new user" in the dropdown — WordPress creates fresh accounts with the Author role
   - Or map each to an existing WP user
6. **Import Attachments checkbox**: check this if you want WordPress to download the 28 featured images from GitHub into your Media Library. If unchecked, the URLs will still work but images stay remote (fine for testing).
7. Click **Submit**.
8. Wait ~30–60 seconds. WordPress imports 8 pages + 20 posts + 28 attachments + 35 comments + menu items.

## After import: enabling the menus

WordPress imports the menus as data, but you still need to assign them to theme locations:

1. **Appearance → Menus**.
2. Top dropdown: select **Main Navigation** → scroll to **Menu Settings** → check your theme's primary location (commonly "Primary" or "Header") → **Save Menu**.
3. Repeat for **Footer Navigation**, assigning it to the footer location.

In modern block themes this is done via **Appearance → Editor → Navigation** instead.

## Verifying the import

- **Posts → All Posts** should show 20 posts. Filter by "Sticky" — you should see 2.
- **Pages → All Pages** should show 8 pages.
- **Users → All Users** should show 3 new authors with bios filled in.
- **Posts → Categories** should show the 7 categories with correct nesting.
- **Comments** should show 35 approved comments.
- Front-end: visit `/`, `/about/`, `/journal/`, individual post pages. Images should load from GitHub.

## Matching the product import

If you want the full shop experience, also import `wonders-oddities-products.csv` via **WooCommerce → Products → Import** (see `README.md` in this folder). The menus link to `/shop/` which WooCommerce will create on activation.

## Cleanup

Every imported page, post, and attachment carries the custom field `_wo_test_batch` = `2026-04-16-a`. To bulk-delete the test content later:

- Install a plugin like **WP Bulk Delete** or **Advanced WP Reset**, filter by that meta key, delete.
- Or use WP-CLI: `wp post delete $(wp post list --meta_key=_wo_test_batch --meta_value=2026-04-16-a --format=ids) --force`

The 3 authors will remain — delete them manually under **Users** if you want a fully clean slate.

## Notes

- **Site URL in content**: links within post bodies use relative paths (`/journal/`, `/about/`) so they resolve on whatever domain you import into.
- **Image URLs**: point at `raw.githubusercontent.com/RegionallyFamous/wonders-oddities/main/wonders-{page,post}-{slug}.png`. All 28 verified as 200-OK at the time of delivery.
- **Yoast SEO**: every page/post includes Yoast meta (`_yoast_wpseo_title`, `_yoast_wpseo_metadesc`, `_yoast_wpseo_focuskw`). If Yoast isn't installed, these fields are harmless stored custom fields.
- **Sticky posts**: 2 posts are marked sticky (`Welcome to Wonders & Oddities`, `Year One, Abridged`). Any theme that respects the sticky flag should pin them at the top of the blog.
- **Publish dates**: posts are backdated across Jan–April 2026 so the blog archive has entries in multiple months. The most recent post is the sticky Welcome (2026-04-15).
- **WordPress Importer plugin bug**: on very old versions, the importer can silently drop menu items. If menus come through empty, update the importer plugin to v0.8+ and re-run.
