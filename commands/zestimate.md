---
name: zestimate
description: Get the Zestimate (home value) and rent Zestimate for a property.
---

Get the current Zestimate for: $ARGUMENTS

Steps:

1. Identify the property from the input above (or from conversation context). If none was given, ask for a U.S. address or a zillow.com link — do not guess.
2. Pick the cheapest correct tool from the `zillapi` MCP server:
   - A `zillow.com/homedetails/...` URL or a known zpid → `get_zestimate` with that zpid (1 credit; the zpid is the number right before `_zpid` in the URL).
   - A street address → `lookup_property_by_address` (3 credits; the full record already includes the Zestimate — do NOT also call `get_zestimate`).
3. Report: Zestimate, rent Zestimate, and — when the full record is available — tax-assessed value and last sold price for context. One line each, with a reminder that a Zestimate is an automated estimate, not an appraisal.

If the tool call fails with an authentication error, tell the user to complete the Zillapi OAuth sign-in prompt (free account, 100 credits, no card) and retry.
