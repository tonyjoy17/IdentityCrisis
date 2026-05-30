# Identity Crisis - Multiplayer Web App

A multiplayer implementation of the Identity Crisis card game, built with vanilla HTML, CSS, JavaScript, and Supabase Realtime.

## Quick Start

### 1. Create a Supabase Project

1. Go to https://supabase.com and create a free account.
2. Create a new project.
3. Go to Settings > API and copy:
   - Project URL, for example `https://xxxx.supabase.co`
   - Anon public key, which starts with `eyJ...`

### 2. Set Up the Database

Open the SQL Editor in Supabase and run:

```sql
CREATE TABLE IF NOT EXISTS games (
  id TEXT PRIMARY KEY,
  state JSONB NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE games ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Allow all" ON games
  FOR ALL USING (true) WITH CHECK (true);

ALTER PUBLICATION supabase_realtime ADD TABLE games;
```

### 3. Add Supabase Config

Open `index.html` and fill in these constants near the top of the script:

```js
const SUPABASE_URL = 'https://xxxx.supabase.co';
const SUPABASE_ANON_KEY = 'your-anon-public-key';
```

The anon key is safe to publish in a browser app when Row Level Security policies are set correctly.

### 4. Host the Static Site

Serve the repository root anywhere:

- Render Static Site: build command blank or `echo "No build needed"`, publish directory `./`
- Local test: use `python -m http.server 5173` or another static file server
- Netlify Drop, GitHub Pages, Cloudflare Pages, or Vercel: publish the repo root

### 5. Open and Play

When `SUPABASE_URL` and `SUPABASE_ANON_KEY` are filled in, players go straight to the lobby and only need a name plus room code.

If those constants are left blank, the app falls back to the setup screen and stores credentials in `localStorage` for that device.

## How to Play

### Setup

1. The host creates a room and gets a 4-character room code.
2. Players join using the room code.
3. The host starts the game when everyone is in.

### Roles

- Catcher: tries to steal cards during exchanges.
- Identity Seekers: try to find their own name card.

### Gameplay

- Cards are dealt randomly so nobody starts with their own card.
- Three Extra cards are mixed in and placed in the Identity Bank at start.
- Players exchange cards with each other.
- The Catcher can snatch during the 3-second window after an exchange, causing that player to lose a wristband.
- Players can use the Identity Bank to get mystery cards.
- Extra Cards trigger special effects.

### Winning

When the timer ends, everyone reveals their card. Any player holding their own name card wins.

## Features

- Real-time multiplayer with Supabase Realtime
- 4-15 players
- Random Catcher assignment
- Card exchanges with a 3-second snatch window
- Catcher snatch cooldown after a successful snatch
- Identity Bank
- Extra Cards: Roll the Dice, Swap with Bank, Lose a Band
- Wristband system
- Live event log
- Configurable timer
- Play Again without rejoining
- Mobile-friendly layout

## Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | Vanilla HTML/CSS/JS |
| Realtime | Supabase Realtime |
| Database | Supabase PostgreSQL |
| Fonts | Google Fonts |
| Hosting | Any static host |

## Notes

- The game state is stored as a single JSONB blob in Supabase and synced through Postgres changes.
- The host drives game progression, including start and timer end.
- Supabase free tier is enough for many casual games.
- Supabase URL and anon key can be built into `index.html` for deployed games.
- If built-in config is blank, credentials are stored in `localStorage` client-side only.
