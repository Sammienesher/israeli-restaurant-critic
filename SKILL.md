---
name: israeli-restaurant-critic
version: 1.1.0
description: Israeli restaurant critic — aggregate reviews and recommendations from top Israeli food review sites. Multi-source scraping across rest.co.il, Timeout, Mako, Ynet, Walla, Globes, and Yuviyam.
author: Sammie
trigger: when user asks for restaurant recommendation, where to eat, best restaurant, מסעדה מומלצת, איפה לאכול, ביקורת מסעדות
---

# Israeli Restaurant Critic

Aggregate restaurant recommendations and reviews from the top Israeli food review sources. When the user asks for a restaurant recommendation, search across ALL sources and compile a curated response.

## Data Sources

| Source | URL | Content |
|--------|-----|---------|
| Timeout | https://timeout.co.il/topic/ביקורת-מסעדות/ | Professional critic reviews (Oded Kramer / Yuval Goldberg) |
| Rest | https://www.rest.co.il/ | Restaurant directory, ratings, menus, user reviews |
| Mako Food Weekend | https://www.mako.co.il/food-weekend/restaurant-reviews | Professional restaurant reviews |
| Mako Tag | https://www.mako.co.il/Tagit/ביקורת%20מסעדות | All Mako restaurant review tagged articles |
| Ynet Reviews | https://www.ynet.co.il/food/foodreviews | Ynet food review section |
| Ynet Tag | https://www.ynet.co.il/topics/ביקורת_מסעדות | Ynet restaurant review tag |
| Walla Food | https://food.walla.co.il/category/1115 | Walla! restaurant reviews |
| Globes | https://www.globes.co.il/news/ביקורת_מסעדות.tag | Globes business/food restaurant reviews |
| Yuviyam | https://www.yuviyam.com/ | Food blog with restaurant reviews |

## Workflow

When the user asks for a restaurant recommendation:

### Phase 1: Extract Intent
   - City/area (תל אביב, ירושלים, חיפה, רמת השרון, etc.)
   - Cuisine type (סושי, איטלקי, בשרים, חומוס, אסייתי, שף, etc.)
   - Price range (זול, יקר, סביר)
   - Occasion (דייט, עסקים, משפחתי, רומנטי)
   - Meal type (ארוחת צהריים, ערב, בוקר)

### Phase 2: Search All Sources
Do NOT rely on `web_extract` alone — Israeli news sites (Mako, Ynet, Walla, Globes) block web scraping. They return 403s, timeouts, or Next.js shells with no content. **`web_search` rich snippets are your primary data source.**

Fire **6-8 concurrent searches** with site-specific Hebrew queries:
   ```
   site:mako.co.il המבורגר תל אביב ביקורת
   site:ynet.co.il המבורגר תל אביב ביקורת מסעדה
   site:food.walla.co.il המבורגר תל אביב
   site:globes.co.il המבורגר תל אביב ביקורת מסעדה
   site:rest.co.il המבורגר תל אביב
   site:timeout.co.il המבורגר תל אביב ביקורת
   ```
   Generic queries without site filter also work for discovery:
   ```
   "ביקורת מסעדה [name]"
   "מסעדה מומלצת [city]"
   "מסעדת [cuisine] [city]"
   ```

### Phase 3: Cross-Reference & Extract Details
   - Read the `description` fields from search results — they often contain the review's key takeaway, price, and rating
   - Identify restaurants that appear in **3+ sources** — those are safe bets
   - For key restaurants, run **additional targeted searches** to fill in prices, addresses, and specific dishes:
     ```
     "[restaurant name]" תל אביב ביקורת
     "[restaurant name]" תפריט מחיר
     ```
   - Try `web_extract` on a few results, but expect failures on Mako/Ynet/Walla/Globes. When it fails, the search snippet IS your review content.

### Phase 4: Compile Recommendations
   - Summarize **3-10** recommendations (user may ask for a specific number) with:
     - Restaurant name and location
     - Cuisine type
     - Notable dishes mentioned
     - Price range (if available)
     - Rating/summary from the review
     - Link to the review source
   - If the user asked about a specific restaurant by name, search for it directly across all sources.

### Phase 5: Ontopo Cross-Reference (if user wants to book)
   - Use the `ontopo` skill to search and check availability
   - **Expect most places NOT to be on Ontopo** — Israeli burger joints, hole-in-the-wall spots, and newer places often aren't listed. Only ~20% of places in a typical list will have Ontopo entries.
   - For places that are found, check availability and generate booking links.

## ⚠️ Known Pitfalls

### Scraping Blocks
**Mako (Next.js):** Returns 404 or empty shell with `web_extract`. The actual article content loads client-side via React. Search snippets are the only reliable source. `curl` also returns garbage (Next.js build data).

**Ynet:** `web_extract` times out after 60s. Site is slow and heavily ad-loaded. Search snippets work.

**Walla Food:** Returns 403 IP block on `web_extract`. Rich search snippets carry detailed review summaries.

**Globes:** `web_extract` times out. Search snippets are minimal — often just the headline. Use additional targeted searches to find secondary coverage elsewhere.

**Timeout:** `web_extract` returns 403 IP block, but `curl` with a browser User-Agent extracts clean article text. Works well for targeted reading of specific reviews.

### Browser Automation
`browser_navigate` (Camofox) is not available on this machine. If the user has it configured, it may work for these sites — but the default fallback is search + snippet aggregation.

### Missing Data
- Rest.co.il has structured data (ratings, price levels, tags) but requires pagination through category pages
- Many trendy/new places appear only on one source, not across all 8
- Prices in reviews may be outdated — always check the review date
- Some review sites use tags inconsistently — try both the category URL and the generic search

### Ontopo Coverage Gap
Most Israeli burger places, hole-in-the-wall spots, and casual dining are NOT on Ontopo. Only about 1 in 5 restaurants in a typical list will be found. Don't oversell this integration — present it as a bonus when it works.

## Best Practices

### Search Efficiency
- Fire **6-8 concurrent searches** per recommendation request (one per source)
- Each search returns 10 results = 60-80 data points to cross-reference
- The first search batch usually identifies the main candidates; follow-up searches fill specific details

### Cross-Referencing
- A place mentioned in 3+ sources = reliable recommendation
- A place mentioned in 1-2 sources but with strong critic language ("הכי טוב בארץ") = worth including
- If 2 sources disagree (one raves, one pans), note both perspectives

### Output Quality
- Always include source links — the user should be able to read the full review
- Rank by relevance to the query, not by rating score
- For "top N" requests, aim for N results with solid coverage. If you only have solid data for 6 of 10, be honest about it
- Price notation: use ₪ symbols, not vague descriptors. Multiple prices from one place = range. Single price = note what it's for (e.g. "בורגר 58 ₪")

## Notes

- Israeli review sites are in Hebrew — search in Hebrew for best results
- Rest.co.il has structured data (ratings, price levels, tags) — prefer extracting from it when possible
- Mako and Ynet reviews are professional critic reviews (not user-generated) — higher authority
- Timeout reviews are by Oded Kramer and Yuval Goldberg — sharp, opinionated, highly respected in the Israeli food scene
- Globes reviews are from the business/food section — often more analytical
- Yuviyam is a personal food blog with strong opinions — good for specific hot spots
- Always provide links back to the original review source
- If no results found, try broader queries (remove city filter, try different cuisine terms)
- Verify restaurant is still open — the pandemic caused many closures

## Example Response Format

```
🏆 **מסעדה מומלצת: [Name]**
📍 [Address], [City]
🍽️ [Cuisine type]
💰 [Price range]
⭐ [Rating / summary]

> "[Excerpt from the review — most notable takeaway]"

🔗 [Source link]

---
```

For multiple recommendations, rank by relevance to the user's query, not by rating score.
