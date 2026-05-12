<p align="center">
  <img src="https://raw.githubusercontent.com/Sammienesher/israeli-restaurant-critic/main/assets/logo.png" alt="Israeli Restaurant Critic" width="200"/>
</p>

<h1 align="center">🍽️ Israeli Restaurant Critic</h1>

<p align="center">
  <b>המבקר הקולינרי שלך בישראל — בפקודה אחת</b><br/>
  <i>Your personal Israeli restaurant critic — one command away</i>
</p>

<p align="center">
  <a href="SKILL.md"><img src="https://img.shields.io/badge/skill-v1.0.0-blue" alt="Version"></a>
  <a href="#"><img src="https://img.shields.io/badge/sources-8-ff6b6b" alt="8 Sources"></a>
  <a href="#"><img src="https://img.shields.io/badge/language-Hebrew+English-success" alt="Bilingual"></a>
</p>

---

## 🦊 What Is This?

An AI skill for **Hermes Agent** that turns your assistant into an expert Israeli food critic. Ask "find me a great hummus place in Jerusalem" or "what's the best burger in Tel Aviv" and it:

1. 🔍 **Scrapes 8 review sources** simultaneously — Rest, Mako, Ynet, Walla, Globes, Yuviyam
2. 📊 **Cross-references across critics** — if 3+ sources rave about a place, you know it's legit
3. 💰 **Detects price range, cuisine, location, and ratings**
4. 📍 **Checks Ontopo** for real-time availability and booking links
5. 🧠 **Ranks by relevance** — not just ratings, but what you actually asked for

No more "let me google that for you." This is a thinking partner that knows Israeli food.

---

## ✨ Why You Need This

### 🇮🇱 Israeli Food Scene Is Chaotic
New places open and close every week. This skill tracks 8 sources so you don't have to.

### 🗣️ Hebrew-Native Search
Israeli review sites are in Hebrew. This skill searches in Hebrew, thinks in Hebrew, and gives you results in whatever language you prefer.

### 🧪 Critic Cross-Validation
One blogger's 10/10 is another's "meh." This skill aggregates across Mako, Ynet, Walla, Globes, and more — you see the consensus, not a single hot take.

### 🎯 Context-Aware
Not just "best restaurants" — it understands:
- 🏙️ **City** (תל אביב, ירושלים, חיפה, באר שבע, אילת...)
- 🥘 **Cuisine** (סושי, איטלקי, בשרים, אסייתי, חומוס, שף...)
- 💵 **Budget** (זול, סביר, יקר)
- 💑 **Occasion** (דייט, עסקים, משפחתי, רומנטי)
- 🍳 **Meal** (בוקר, צהריים, ערב)

---

## 🚀 Installation

### Prerequisites
- [Hermes Agent](https://hermes-agent.nousresearch.com) installed and running
- Web search capabilities enabled

### Install from the Skills Hub
```bash
hermes skill install israeli-restaurant-critic
```

### Manual Install
```bash
git clone https://github.com/Sammienesher/israeli-restaurant-critic.git
cp -r israeli-restaurant-critic ~/.hermes/skills/research/
hermes skill reload
```

---

## 🎮 Usage Examples

Just ask naturally:

> *"איפה כדאי לאכול המבורגר בתל אביב?"*
> → Gets you a curated list from critics who've actually been there

> *"Find me a good sushi place in Herzliya that's not crazy expensive"*
> → Searches all sources, filters by budget and cuisine

> *"מה המסעדה הכי טובה בירושלים לארוחה רומנטית?"*
> → Context-aware: romantic dinner in Jerusalem

> *"יש ביקורות על מסעדת תיאו ברחובות?"*
> → Deep-dive on a specific restaurant across all critics

The skill also integrates with **Ontopo** — if you like a suggestion, it can check availability and generate a booking link.

---

## 📡 Data Sources

| Source | Type | Authority |
|--------|------|-----------|
| [Rest](https://www.rest.co.il/) | Directory + User Reviews | 🏛️ Biggest Israeli restaurant database |
| [Mako](https://www.mako.co.il/food-weekend/restaurant-reviews) | Professional Critics | 🎤 High authority, professional reviews |
| [Ynet](https://www.ynet.co.il/food/foodreviews) | Professional Critics | 🎤 Mainstream food journalism |
| [Walla](https://food.walla.co.il/category/1115) | Professional Critics | 🎤 David Rosenthal & team |
| [Globes](https://www.globes.co.il/news/%D7%91%D7%99%D7%A7%D7%95%D7%A8%D7%AA_%D7%9E%D7%A1%D7%A2%D7%93%D7%95%D7%AA.tag) | Analytical Reviews | 📊 Business-oriented, analytical |
| [Yuviyam](https://www.yuviyam.com/) | Food Blog | 🔥 Strong opinions, hot spots |

---

## 🏗️ Architecture

```
User asks for recommendation
        │
        ▼
┌─────────────────────────────┐
│  Extract Intent             │
│  (city, cuisine, price,     │
│   occasion, meal type)      │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Search ALL 8 Sources       │
│  (Hebrew queries)           │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Extract Content            │
│  (ratings, prices, reviews) │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Cross-Reference & Rank     │
│  (consensus ≠ hype)         │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  Compile + Present          │
│  (with links, prices,       │
│   critic quotes)            │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  [Optional] Ontopo Booking  │
│  (check availability)       │
└─────────────────────────────┘
```

---

## 🧪 Real Example

Here's what happened when someone asked for the **best burgers in Tel Aviv**:

> 🥇 **Jagger** — Mako: "can claim the title of best burger in Israel"
> 🥈 **Fat Cow** — A "minimalist carnivorous masterpiece" (Timeout, Globes, Walla)
> 🥉 **Grinberg Burger** — "No mistaking the quality of the meat" (Walla, Haaretz)
> 4️⃣ **HaSimta** — Smash burger from Metula, now in Tel Aviv (Walla, Mako)
> 5️⃣ **OSU** — Japanese-style smash at the Carmel Market (Ynet, Walla)
> 6️⃣ **Prozdor** — "Surprising, weird, wild and wonderful" (Globes)

Each result linked back to the original critic review. Full transparency.

---

## 🛠️ Customization

The skill is designed to be extended. Want to add a source? Fork the repo and add it to the workflow in `SKILL.md`.

Contributions welcome — especially new Israeli food review sources.

---

## 📄 License

MIT — do whatever you want with it.

---

<p align="center">
  Made with 🦊 by <b>Sammie</b>
</p>
