# The Reading Nook — Adobe Target MVT Demo

Updated version of the supplied `launch-target-demo` GitHub Pages files.

## Deployment

Copy `index.html`, `about.html`, and `README.md` into the root of the GitHub repository that publishes `https://govindapatale.github.io/launch-target-demo/`. Preserve the repository's Pages settings and publish branch. GitHub Pages normally deploys changes after a commit, but wait for its build/deploy workflow to succeed before testing.

No build step, package manager, image hosting, or JavaScript framework is required.

## Adobe Tags / Web SDK

The **existing development Adobe Tags embed URL** from the uploaded `index.html` has been preserved unchanged:

`https://assets.adobedtm.com/666644a00be2/36b179fb4819/launch-dfcd6a67c6d6-development.min.js`

The original pre-hiding snippet is also retained on `index.html`. The `about.html` page also includes the same development embed to allow second-page demonstrations.

**Important:** The script loading does not, by itself, prove that Target is enabled for the GitHub Pages hostname. Verify Adobe Tags build, Web SDK configuration, datastream Target service, assigned Target Property, the `__view__` request, and successful Target decisions on this domain. Keep a development configuration until verified.

## Native MVT test proposal (Adobe Target VEC)

**Activity name:** `Reading_Nook_MVT_Hero_Copy`

**Activity URL:** `https://govindapatale.github.io/launch-target-demo/`

**Activity type:** Multivariate Test (Visual Experience Composer)

**Audience:** All Visitors (for demonstration)

**Design:** 3 independent locations, 2 versions in each = **8 total combinations**.

| VEC location | DOM selector | Original content | Alternative content |
|---|---|---|---|
| 1 — Hero headline | `#mvt-headline` | Find your next favorite book | Your next great read starts here |
| 2 — Supporting message | `#mvt-subheading` | Discover handpicked stories, thoughtful nonfiction, and fresh ideas for every kind of reader. | Explore new voices, unforgettable adventures, and the books everyone will be talking about. |
| 3 — Hero CTA text | `#mvt-cta` | Browse featured books | Discover the collection |

Use **Edit Text** on each element in the VEC; keep the original HTML tags, IDs, and CTA `href` unchanged. Do **not** replace the whole hero `<section>` or modify the book illustration, since the intention is to test three separate factors rather than one bundled change.

1. In Adobe Target, choose **Create Activity > Multivariate Test** and load the HTTPS activity URL in VEC.
2. Check that `#mvt-headline`, `#mvt-subheading`, and `#mvt-cta` can each be selected separately.
3. Add **one alternative content variation** for each location using the text in the table.
4. Review all eight combinations via **Show Experiences**, QA previews, and mobile viewport.
5. Use **All Visitors** for initial QA, then configure an audience as needed.
6. Set Goals & Settings. For an initial Target-only test you can measure CTA interactions once instrumented.
7. Keep activity inactive until approval and the required reporting/QA checks pass.

### CTA tracking / CJA setup

- The CTA is a working anchor link: `<a id="mvt-cta" href="#featured-books">`.
- It carries `data-analytics="mvt-hero-cta-click"`, which can be used in a Tags click rule.
- **No custom click event is sent automatically by this HTML.** To measure CTA clicks in CJA, create an appropriate Web SDK `Send Event` action for that click and ensure its data reaches an AEP event dataset, a CJA connection, and data view.
- Selecting CJA in Target alone does not ingest click data or configure conversion metrics automatically.

### Preview / troubleshooting

Open the deployed page and run:

```js
["mvt-headline", "mvt-subheading", "mvt-cta"].map(id => ({
  id,
  count: document.querySelectorAll("#" + id).length,
  text: document.getElementById(id)?.textContent.trim()
}));
```

Expected counts: `1`, `1`, and `1`.

For Target delivery, inspect the Web SDK `interact` request and ensure default personalization (`__view__`) is requested and that your MVT activity is eligible for the new GitHub Pages hostname and property.

## Training note

The book titles are fictional. This page is for demonstrating **MVT authoring and delivery**, not generating statistically significant experiment results. Adobe advises checking the Traffic Estimator before running a genuine MVT.
