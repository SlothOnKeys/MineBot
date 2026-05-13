# MineBot

A production-ready AI-powered Minecraft bot using **Mineflayer** and free AI APIs. The bot understands natural language, executes tasks autonomously, and behaves like an intelligent in-game assistant.

---

##  Features

-  **Natural Language Chat** — Understands commands like "bot mine 20 iron" or "bot follow me"
-  **Mining System** — Finds and mines ores, collects drops, avoids lava
-  **Wood Chopping** — Locates and chops trees
-  **Crafting** — Crafts items using inventory or crafting table
-  **Smelting** — Uses nearby furnaces to smelt ores
-  **Chest Management** — Deposit/withdraw items from chests
-  **Pathfinding** — Safe navigation using mineflayer-pathfinder
-  **Exploration** — Autonomously explores the world
-  **Combat** — Attacks hostile mobs, flees on low health
-  **Farming** — Harvests mature crops
-  **Task Queue** — Sequential task execution with retry logic
-  **Memory** — Remembers chest locations, home position, chat history
-  **Multiple AI Providers** — OpenRouter, Gemini, Ollama, or Mock (offline)
-  **Web Viewer** — Optional 3D browser view of the bot

---

##  Requirements

- **Node.js 18+** ([Download](https://nodejs.org/))
- **Minecraft Java Edition** (server version 1.16–1.20.x)
- **A running Minecraft server** (local or remote)
- One of: OpenRouter key, Gemini key, Ollama, or no key (mock mode)

---

##  Installation

```bash
# 1. Clone or download this project
git clone https://github.com/SlothOnKeys/MineBot/tree/main
cd minecraft-ai-bot

# 2. Install dependencies
npm install

# 3. Copy the example environment file
cp .env.example .env

# 4. Edit .env with your settings (see below)
nano .env   # or use any text editor

# 5. Start the bot
npm start
```

---

## ⚙️ Configuration

### `.env` file (required)

```env
# Minecraft server
MINECRAFT_HOST=localhost
MINECRAFT_PORT=25565
BOT_USERNAME=AI_Bot
MINECRAFT_VERSION=1.20.1

# AI Provider: openrouter | gemini | ollama | mock
AI_PROVIDER=mock
```

### `config.json` (optional tweaks)

Key settings you may want to change:

| Setting | Default | Description |
|---|---|---|
| `chat.triggerWords` | `["bot", "ai_bot"]` | Words that make the bot respond |
| `chat.respondToAll` | `false` | Respond to all chat (not just mentions) |
| `mining.radius` | `32` | How far to search for ores |
| `movement.followDistance` | `3` | Distance to keep when following |
| `autoEat.startAt` | `14` | Food level to start eating |
| `safety.minHealthToWork` | `6` | Stop tasks if health drops below this |

---

##  AI Provider Setup (Free Options)

### Option 1: Mock Mode (No API Key — Offline)
```env
AI_PROVIDER=mock
```
Uses pattern matching. Works great for basic commands without any API.

---

### Option 2: OpenRouter (Free Models Available)
1. Sign up at [openrouter.ai](https://openrouter.ai/)
2. Create a free API key
3. Free models available: `mistralai/mistral-7b-instruct:free`, `meta-llama/llama-3-8b-instruct:free`

```env
AI_PROVIDER=openrouter
OPENROUTER_API_KEY=sk-or-v1-your-key-here
OPENROUTER_MODEL=mistralai/mistral-7b-instruct:free
```

---

### Option 3: Google Gemini (Free Tier)
1. Go to [aistudio.google.com](https://aistudio.google.com/)
2. Click "Get API Key" → Create a free key
3. Free tier: 15 requests/minute on gemini-1.5-flash

```env
AI_PROVIDER=gemini
GEMINI_API_KEY=AIza-your-key-here
GEMINI_MODEL=gemini-1.5-flash
```

---

### Option 4: Ollama (Local, Completely Free)
1. Install Ollama: [ollama.ai](https://ollama.ai/)
2. Pull a model: `ollama pull llama3`
3. Start server: `ollama serve`

```env
AI_PROVIDER=ollama
OLLAMA_HOST=http://localhost:11434
OLLAMA_MODEL=llama3
```

---

## 🎮 Running the Bot

### Local Server
```bash
# Start a Minecraft server first, then:
npm start
```

### Public Server
```env
MINECRAFT_HOST=play.example.com
MINECRAFT_PORT=25565
BOT_USERNAME=MyAIBot
```

> ⚠️ **Note**: Many public servers ban bots. Always get permission before running on public servers!

### With Web Viewer
```env
VIEWER_ENABLED=true
VIEWER_PORT=3007
```
Then open `http://localhost:3007` in your browser.

---

## 💬 In-Game Commands

Type these in Minecraft chat (replace "bot" with your bot's username if changed):

### Movement
```
bot follow me          - Follow you around
bot come here          - Walk to your position
bot stop               - Stop all tasks
bot explore            - Explore the area
bot go home            - Return to home position
bot set home           - Mark current position as home
```

### Resource Gathering
```
bot mine iron          - Mine iron ore (5 by default)
bot mine 20 diamond    - Mine 20 diamond ore
bot get wood           - Chop trees (5 logs)
bot chop 10 logs       - Chop 10 logs
bot harvest            - Harvest nearby crops
```

### Crafting & Smelting
```
bot craft torch        - Craft torches
bot craft 4 torch      - Craft 4 torches
bot craft pickaxe      - Craft a pickaxe
bot smelt iron ore     - Smelt iron ore in furnace
```

### Chest & Inventory
```
bot deposit            - Put items in nearest chest
bot withdraw iron ingot - Take iron ingots from chest
bot chest              - List chest contents
bot status             - Show health, food, position, inventory
```

### Combat
```
bot attack zombie      - Attack a specific mob type
bot attack             - Attack nearest hostile mob
```

### Natural Language (with real AI provider)
```
Hey bot, can you mine 20 iron and bring it back?
Bot, go chop some wood and craft me a pickaxe
AI_Bot follow me and protect me from mobs
Bot, put everything in the chest then come back
```

---

## 📁 Project Structure

```
minecraft-ai-bot/
├── src/
│   ├── index.js              # Entry point
│   ├── bot/
│   │   ├── createBot.js      # Bot creation & plugins
│   │   ├── events.js         # Chat, spawn, health events
│   │   ├── movement.js       # Follow, pathfind, explore
│   │   ├── mining.js         # Mining system
│   │   ├── inventory.js      # Inventory helpers
│   │   ├── crafting.js       # Crafting & smelting
│   │   ├── chest.js          # Chest interaction
│   │   ├── combat.js         # Combat system
│   │   └── farming.js        # Crop harvesting
│   ├── ai/
│   │   ├── aiRouter.js       # AI provider abstraction
│   │   ├── memory.js         # Persistent memory
│   │   └── providers/
│   │       ├── openrouter.js
│   │       ├── gemini.js
│   │       ├── ollama.js
│   │       └── mock.js       # Offline fallback
│   ├── commands/
│   │   ├── commandParser.js  # Natural language → tasks
│   │   ├── executor.js       # Task runner
│   │   └── tasks.js          # Task queue
│   ├── utils/
│   │   ├── logger.js         # Colored logging
│   │   ├── helpers.js        # Utilities
│   │   └── world.js          # World state helpers
│   └── web/
│       └── viewer.js         # 3D web viewer
├── data/
│   ├── memory.json           # Persisted memory
│   └── logs/                 # Log files
├── config.json               # Bot configuration
├── .env                      # Your secrets (never commit!)
├── .env.example              # Template for .env
└── package.json
```

---

## 🛠️ Troubleshooting

### Bot can't connect to server
- Make sure the server is running: `MINECRAFT_HOST=localhost`
- Check the port matches (`default: 25565`)
- Check `MINECRAFT_VERSION` matches your server version
- Check if the server has `online-mode=true` (bots need offline mode or proper auth)

### Bot joins but doesn't respond to chat
- Make sure you're using a trigger word: `bot`, `ai_bot`, or whatever is in `config.json`
- Check `chat.triggerWords` in config.json
- Set `chat.respondToAll: true` to respond to everyone

### AI provider errors
- Try `AI_PROVIDER=mock` first to verify the bot itself works
- Check your API key in `.env`
- OpenRouter free models sometimes have rate limits — wait a minute
- For Ollama: make sure `ollama serve` is running and the model is pulled

### Bot gets stuck mining
- Reduce `mining.radius` in config.json
- Check for lava blocking the path (bot should auto-avoid)
- The bot will timeout tasks after `safety.taskTimeoutMs` milliseconds

### "Cannot find module" errors
```bash
npm install
```

### Bot dies immediately
- Raise `safety.minHealthToWork` 
- Enable `autoEat` in config.json
- Make sure the bot has food in inventory

---

## 🔧 Adding Custom Commands

1. Add a case to `src/commands/executor.js` in the `executeTask` switch:

```javascript
case 'mycommand': {
  const target = task.target;
  bot.chat(`Running my custom command: ${target}`);
  // ... your code here
  break;
}
```

2. Add a quick-parse rule to `src/commands/commandParser.js`:

```javascript
if (/^mycommand/.test(msg)) {
  return {
    reply: "Running my command!",
    tasks: [{ action: 'mycommand', target: 'something' }],
  };
}
```

3. Add it to the mock AI's patterns in `src/ai/providers/mock.js` and the AI prompt in `src/ai/aiRouter.js`.

---

## 🗺️ Supported Minecraft Versions

Tested with:
- 1.20.1 ✅
- 1.19.4 ✅  
- 1.18.2 ✅
- 1.17.1 ✅
- 1.16.5 ✅

Set `MINECRAFT_VERSION` in `.env` to match your server.

---

## 🚀 Future Improvements

- [ ] Discord bridge (relay chat to/from Discord)
- [ ] Multi-bot swarm support
- [ ] Long-term persistent goals
- [ ] Autonomous base-building mode
- [ ] Web dashboard with live inventory view
- [ ] Voice command support
- [ ] Auto-trader with villagers
- [ ] Nether/End dimension support
- [ ] Enchanting system
- [ ] Map generation and waypoints

---

## 📜 License

MIT — Use freely, modify as needed, star the repo if it helped you!
