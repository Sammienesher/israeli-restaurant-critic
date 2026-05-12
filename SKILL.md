---
name: israeli-restaurant-critic
version: 1.0.0
description: Israeli restaurant critic — aggregate reviews and recommendations from top Israeli food review sites. Multi-source scraping across rest.co.il, Mako, Ynet, Walla, Globes, and Yuviyam.
author: Sammie
trigger: when user asks for restaurant recommendation, where to eat, best restaurant, מסעדה מומלצת, איפה לאכול, ביקורת מסעדות
---

# Israeli Restaurant Critic

Aggregate restaurant recommendations and reviews from the top Israeli food review sources. When the user asks for a restaurant recommendation, search across ALL sources and compile a curated response.

## Data Sources

| Source | URL | Content |
|--------|-----|---------|
| Rest | https://www.rest.co.il/ | Restaurant directory, ratings, menus, user reviews |
| Mako Food Weekend | https://www.mako.co.il/food-weekend/restaurant-reviews | Professional restaurant reviews |
| Mako Tag | https://www.mako.co.il/Tagit/%D7%91%D7%99%D7%A7%D7%95%D7%A8%D7%AA%20%D7%9E%D7%A1%D7%A2%D7%93%D7%95%D7%AA | All Mako restaurant review tagged articles |
| Ynet Reviews | https://www.ynet.co.il/food/foodreviews | Ynet food review section |
| Ynet Tag | https://www.ynet.co.il/topics/%D7%91%D7%99%D7%A7%D7%95%D7%A8%D7%AA_%D7%9E%D7%A1%D7%A2%D7%93%D7%95%D7%AA | Ynet restaurant review tag |
| Walla Food | https://food.walla.co.il/category/1115 | Walla! restaurant reviews |
| Globes | https://www.globes.co.il/news/%D7%91%D7%99%D7%A7%D7%95%D7%A8%D7%AA_%D7%9E%D7%A1%D7%A2%D7%93%D7%95%D7%AA.tag | Globes business/food restaurant reviews |
| Yuviyam | https://www.yuviyam.com/ | Food blog with restaurant reviews |

## Workflow

When the user asks for a restaurant recommendation:

1. **Extract the query intent:**
   - City/area (תל אביב, ירושלים, חיפה, רמת השרון, etc.)
   - Cuisine type (סושי, איטלקי, בשרים, חומוס, אסייתי, שף, etc.)
   - Price range (זול, יקר, סביר)
   - Occasion (דייט, עסקים, משפחתי, רומנטי)
   - Meal type (ארוחת צהריים, ערב, בוקר)

2. **Search all sources** using Hebrew search queries across every source simultaneously.

3. **Extract content** from the top results — get full review text, ratings, prices, addresses.

4. **Compile recommendations** with restaurant name, location, cuisine, notable dishes, price range, rating, and links.

5. **Cross-reference with Ontopo** (if the user wants to book) — check availability and get booking links.

## Example Output

```
🏆 **Recommended: [Restaurant Name]**
📍 [Address], [City]
🍽️ [Cuisine]
💰 [Price range]
⭐ [Rating / critic quote]

🔗 [Source link]
```

## Notes

- Israeli review sites are in Hebrew — search in Hebrew for best results
- Cross-reference multiple critics — a place praised by 3+ sources is a safe bet
- Always include links back to original reviews so users can read the full critique
- Never rank by rating alone — rank by relevance to what the user asked
