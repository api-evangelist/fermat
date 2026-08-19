---
name: fermat-pixel-install-and-verify
description: Install FERMÀT Pixel v2 on a non-Shopify storefront and implement the six documented commerce events, then verify the install is emitting them correctly.
api: FERMAT Pixel v2
endpoint: https://static.clairedefermat.com/pixel/v2/claire.mjs
operations:
  - init
  - track
  - status
generated: '2026-08-13'
method: generated
source: https://help.fermatcommerce.com/en/articles/14280269-fermat-pixel-v2-installation-guide-direct-script-google-tag-manager
---

# FERMÀT Pixel v2 install and verify

Client-side. This is a browser integration, not a server API.

## Stop first
- **If the store runs on Shopify, do not follow this.** FERMÀT explicitly warns that Shopify stores
  use a separate native install path and that manual installation causes duplicate or missing
  events. Route the request to the FERMÀT representative instead.
- You need a **Pixel ID**, issued by a FERMÀT representative. It cannot be self-served.

## Steps
1. **Load the pixel.** Add the command-queue shim and dynamically import
   `https://static.clairedefermat.com/pixel/v2/claire.mjs` from `<head>` on **every** page, or add
   it as a GTM Custom HTML tag firing on All Pages.
2. **Initialize.** Call `window.fermat({ method: "init", config: { id: "<PIXEL_ID>", recording: {
   enabled: <bool>, samplingRate: <0..1> } } })`. Session recording only loads when enabled.
3. **Implement all six events** via `window.fermat({ method: "track", eventName, properties })`:
   `page_view`, `view_product`, `add_to_cart`, `remove_from_cart`, `begin_checkout`, `purchase`.
   Field requiredness per event is in `asyncapi/fermat-pixel-asyncapi.yml` — `quantity` is required
   on both cart events, and `transaction_id` is required on `purchase` only.
4. **Verify.** Run `window.fermat({ method: "status" })` in the console and confirm
   `initialized: true`. GTM users can also confirm the tag under "Tags Fired" in Preview mode.
5. **Handle SPAs.** Route changes do not fire `page_view` automatically — re-fire it on navigation.

## Rules
- If sequencing through GTM, set the base tag to fire **before** every event tag (Tag Sequencing).
- The documented loader URL is **unpinned** — it floats to the current build. If build determinism
  matters to the brand, raise it with FERMÀT; there is no published pinned URL.
- The Pixel ID is a public client-side identifier, not a secret. Do not treat it as a credential,
  and do not put any real secret in the pixel config.
