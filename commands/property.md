---
name: property
description: Pull the full Zillow record for a property — price, Zestimate, taxes, schools, price history, photos, agent contact.
---

Pull the full Zillow property record for: $ARGUMENTS

Steps:

1. Identify the property from the input above (or from conversation context). If none was given, ask for a U.S. address or a zillow.com link — do not guess.
2. Pick the cheapest correct tool from the `zillapi` MCP server:
   - A `zillow.com/homedetails/...` URL → extract the zpid (the number right before `_zpid`) and call `lookup_property_by_zpid` (1 credit).
   - A bare zpid → `lookup_property_by_zpid` (1 credit).
   - A street address → `lookup_property_by_address` (3 credits).
3. Present a concise summary: address, status, price, beds/baths/sqft, year built, Zestimate + rent Zestimate, tax-assessed value, last sale (date + price), schools with ratings, and listing agent if active. Offer to go deeper (full price history, photos) on request rather than dumping everything.

If the tool call fails with an authentication error, tell the user to complete the Zillapi OAuth sign-in prompt (free account, 100 credits, no card) and retry.
