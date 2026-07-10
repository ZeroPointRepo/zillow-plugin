---
name: search
description: Search Zillow listings — for sale, for rent, or sold — by area, price, and bedrooms.
---

Search Zillow listings for: $ARGUMENTS

Steps:

1. Parse the request above into: an area, a status (`for_sale` default; use `for_rent` or `sold` when asked), and optional `beds_min` / `price_max`. If no area was given, ask for one — do not guess.
2. Convert the area to a bounding box `"west,south,east,north"` in decimal degrees for the `search_listings` tool on the `zillapi` MCP server. Size it to the area named: neighborhood ≈ ±0.02°, city ≈ ±0.05–0.15°. Keep it tight — **each returned listing costs 1 credit**, so before calling, tell the user roughly how many results to expect and narrow if the request is broad.
3. Call `search_listings` with `bbox`, `status`, and any filters.
4. Present results as a compact table: address, price, beds/baths, sqft, and zpid. Offer to pull the full record for any of them (`lookup_property_by_zpid`, 1 credit).

If the tool call fails with an authentication error, tell the user to complete the Zillapi OAuth sign-in prompt (free account, 100 credits, no card) and retry.
