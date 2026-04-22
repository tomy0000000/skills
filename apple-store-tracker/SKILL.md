---
name: apple-store-tracker
description: >
  Searches for and lists recently opened and upcoming Apple Store locations worldwide,
  covering new stores, renovations, and relocations. Use this skill whenever the user
  asks about new Apple Stores, Apple Store openings, upcoming Apple retail locations,
  Apple Store news, or anything like "what Apple Stores are opening soon", "has Apple
  opened any new stores", "Apple Store near me opening", or "Apple retail expansion".
  Trigger even for casual phrasings like "any new Apple Stores?" or "where is Apple
  opening next". Always use this skill rather than relying on training data, since
  store opening info changes frequently.
---

# Apple Store Tracker Skill

Surfaces recently opened and upcoming Apple Store locations — new builds, renovations, and relocations — with country emoji, links, and clear labelling of confirmed vs. rumoured.

## Scope

- **Recent**: last 3 months from today
- **Upcoming**: any announced or credibly rumoured future openings
- **Types**: new stores, renovations/revamps, and relocations — all included
- **Sources**: Apple Newsroom (confirmed) + MacRumors / 9to5Mac / Bloomberg (rumoured, labelled)

---

## Workflow

### Step 1 — Search

Run these searches in parallel (or sequentially if needed):

1. `Apple Store new opening [current year]`
2. `Apple Store upcoming opening announced`
3. `Apple Newsroom store news` → fetch `https://www.apple.com/newsroom/topics/store-news/`
4. `Apple Store renovation relocation [current year]`
5. `Apple Store rumored upcoming MacRumors` or `9to5Mac Apple Store`

Use `web_search` for queries 1, 2, 4, 5. Use `web_fetch` on the Apple Newsroom URL for query 3.

### Step 2 — Classify each result

For every store event found, classify it as:

| Tag              | Meaning                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------- |
| ✅ **Confirmed** | Announced by Apple Newsroom or Apple.com                                                  |
| 📰 **Reported**  | Covered by credible outlets (MacRumors, 9to5Mac, Bloomberg) but not yet on Apple Newsroom |
| 🔮 **Rumoured**  | Speculation or leaked info, no official reporting yet                                     |

### Step 3 — Determine the link

Priority order:

1. Apple's retail store page: `https://www.apple.com/retail/[store-slug]/` — always use this if the store is already open and the page exists
2. Apple Newsroom press release URL — use for upcoming/not-yet-open stores with an official announcement
3. Best available news article URL (MacRumors, 9to5Mac, Bloomberg, etc.) — fallback only

To find the store slug, try `https://www.apple.com/retail/[lowercase-store-name-with-hyphens]/`. If unsure, use `web_fetch` to verify the URL resolves before linking it.

### Step 4 — Format the output

Output two sections:

---

## 🏪 Recently Opened _(last 3 months)_

- 🇺🇸 **[Store Name]** — [City, State/Country] · [Type: New / Renovation / Relocation] · [Opened: Month DD, YYYY] · [Link]
- 🇯🇵 **[Store Name]** — ...

Sort oldest to newest by opening date within each section.

## 🔜 Coming Soon

- 🇸🇦 **[Store Name]** — [City, Country] · [Type] · [Expected: Q? YYYY or "TBD"] · [📰 Reported / 🔮 Rumoured] · [Link]
- 🇲🇽 **[Store Name]** — ...

---

## Rules

- Always use the country's flag emoji at the start of each bullet
- For **upcoming**, always include the credibility label (✅ / 📰 / 🔮)
- If a store page on apple.com exists, always prefer that as the link
- If no date is known, write "Date TBD"
- If only a region is known (e.g. "somewhere in India"), still include it with a note
- Do not include store closures, temporary closures, or stores reopening after routine maintenance — only events that represent a meaningful new or upgraded presence
- If the search returns nothing recent, say so clearly rather than hallucinating results
- Today's date is always available in your context — use it to determine the 3-month window precisely
