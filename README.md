# 🎭 Identity Crisis — Multiplayer Web App

A full multiplayer implementation of the Identity Crisis card game, built with vanilla HTML/CSS/JS and Supabase for real-time sync.

---

## 🚀 Quick Start

### 1. Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) and create a free account
2. Create a new project
3. Go to **Settings → API** and copy:
   - **Project URL** (e.g. `https://xxxx.supabase.co`)
   - **Anon public key** (starts with `eyJ...`)

### 2. Set Up the Database

Open the **SQL Editor** in Supabase and run:

```sql
CREATE TABLE IF NOT EXISTS games (
  id TEXT PRIMARY KEY,
  state JSONB NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE games ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Allow all" ON games
  FOR ALL USING (true) WITH CHECK (true);

-- Enable realtime
ALTER PUBLICATION supabase_realtime ADD TABLE games;
```

### 3. Host the HTML File

Just serve `identity-crisis.html` anywhere:

- **Locally**: Open the file directly in a browser or use `npx serve .`
- **Netlify Drop**: Drag the file to [netlify.com/drop](https://netlify.com/drop)
- **GitHub Pages**: Put the file in a repo and enable Pages
- **Any static host**: Cloudflare Pages, Vercel, etc.

### 4. Open & Configure

When you first open the app, enter your Supabase URL and Anon Key. They're saved in `localStorage` — you only need to do this once per device.

---

## 🎮 How to Play

### Setup
1. **Host** creates a room (gets a 4-letter room code)
2. **Players** join using the room code (4–15 players)
3. Host starts the game when everyone is in

### Roles
- **Catcher** (1 player): Tries to steal cards during exchanges
- **Identity Seekers** (everyone else): Try to find their own name card

### Gameplay
- Cards are dealt randomly — nobody starts with their own card
- 3 Extra cards are mixed in and placed in the Identity Bank at start
- Players freely exchange cards with each other
- The Catcher can **Snatch** cards during exchanges (player loses a wristband)
- Use the Identity Bank to get mystery cards
- Use Extra Cards for special effects

### Winning
- When the timer ends, everyone reveals their card
- **Any player holding their own name card wins!**
- If nobody has their own card: add 5 minutes and continue

---

## ⚡ Features

- ✅ Real-time multiplayer (Supabase Realtime)
- ✅ 4–15 players
- ✅ Random Catcher assignment
- ✅ Card exchanges with 5-second snatch window
- ✅ Identity Bank (visit or take specific cards)
- ✅ Extra Cards: Roll the Dice / Swap with Bank / Lose a Band
- ✅ Wristband system (5 wristbands → eliminated)
- ✅ Live event log
- ✅ Configurable timer (5–30 min)
- ✅ Play Again without re-joining
- ✅ Mobile-friendly layout

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML/CSS/JS |
| Realtime | Supabase Realtime (Postgres Changes) |
| Database | Supabase (PostgreSQL) |
| Fonts | Google Fonts (Bebas Neue, Special Elite, Courier Prime) |
| Hosting | Any static host |

---

## 📝 Notes

- The entire game state is stored as a single JSONB blob in Supabase and synced via Postgres Changes
- All players subscribe to the same channel; the host drives game progression (timer end, game start)
- Supabase free tier supports 500MB database and 2GB realtime traffic — more than enough for many games
- Credentials are stored in `localStorage` client-side only
