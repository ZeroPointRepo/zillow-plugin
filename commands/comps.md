---
name: comps
description: Run sold comparables for a subject property and estimate value from $/sqft.
---

Run sold comps for the subject property: $ARGUMENTS

Steps (uses the `zillapi` MCP server; ~10–15 credits total for a normal run):

1. Identify the subject property from the input above. If none was given, ask for a U.S. address or zillow.com link — do not guess.
2. Pull the subject's full record — `lookup_property_by_zpid` if you have a zpid from a Zillow URL (1 credit), otherwise `lookup_property_by_address` (3 credits). Note its lat/lon, beds, sqft, and Zestimate.
3. Call `search_listings` with `status: "sold"`, a tight bounding box around the subject's lat/lon (±0.01–0.02°, roughly a 1–2 km radius), and `beds_min` one below the subject's bedroom count. Each result costs 1 credit, so keep the box tight; widen only if fewer than 3 comps come back.
4. Build the comp table: address, sold price, beds/baths, sqft, $/sqft, distance from subject. Drop obvious non-comparables (wrong home type, sqft off by more than ~40%).
5. Conclude with: median $/sqft of the comps × subject sqft = indicated value, shown next to the Zestimate and (if listed) the asking price. Note this is an estimate from public data, not an appraisal.

If a tool call fails with an authentication error, tell the user to complete the Zillapi OAuth sign-in prompt (free account, 100 credits, no card) and retry.
