# zkverisarb

**zkverisarb** is a high-frequency **Solana DEX arbitrage bot**.  
It monitors multiple tokens you whitelist, finds price gaps via the **Jupiter aggregator** across major Solana DEXs (Raydium, Orca, Phoenix, Lifinity, Meteora, …), simulates **round-trip** trades (after slippage & fees), and executes only if profit is guaranteed.

- Author / Contact: [Twitter/X](https://x.com/zkveris) • [Telegram](https://t.me/ZkVeris)  
- Target OS: **Ubuntu 24.04 LTS**  
- Engine: **Node.js 18+**, **Solana Web3**, **Jupiter v6**  
- Safe testing: **DRY_RUN=true** prevents any trades (observe only).

⚠️ **Disclaimer**: Educational use only. No profit guarantees. Use a fresh wallet with limited funds. Never commit secrets.

---

## ✨ Features
- **Multi-token monitoring** (manual whitelist of SPL mints)
- **DEX coverage** via Jupiter (smart routing, split routes, all major venues)
- **Profitability guard** — trades only if round-trip ≥ `MIN_PROFIT_BPS`
- **Execution modes**:
  - `best_price`: maximize output (multi-hop routes)
  - `fastest`: prefer single-hop + aggressive confirmation (priority fee, skip preflight)
- **DRY_RUN** mode — log opportunities without trading
- **24/7 ready** — run with PM2 or systemd

---

## 📂 Repository Structure

zkverisarb/
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
├── package.json
└── src/
   ├── index.js
   ├── config.js
   ├── dexScanner.js
   ├── jupiter.js
   ├── risk.js
   └── utils.js
├── scripts/
│   └── keypair-json-to-b58.js
└── service/
    └── zkverisarb.service

---

## 🛠 Installation Guide

### 0. Requirements
- Ubuntu 24.04 VPS (2 vCPU / 4 GB RAM+)
- Node.js 18+, git
- Solana RPC (private/paid recommended)
- Fresh Solana wallet (base58 secret, small funds)

---

### 1. Install prerequisites
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs git build-essential
node -v && npm -v && git --version

---

### 2. Clone & install
git clone https://github.com/zkveris/zkverisarb.git
cd zkverisarb
npm install

---

### 3. Wallet setup
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"

mkdir -p ~/.config/solana
solana-keygen new --outfile ~/.config/solana/arb.json
solana address

node scripts/keypair-json-to-b58.js ~/.config/solana/arb.json

Copy the base58 output into `.env`.

---

### 4. Configure `.env`
cp .env.example .env
nano .env

Example `.env`:
RPC_URL=https://your-rpc:8899
WALLET_SECRET_KEY_B58=BASE58_SECRET
TOKENS=DoggZcWcYNVnDsFHhU5QbNEB2c9PzpjzwgyYMvZx7feY
BASE_MINT=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1
NOTIONAL_PER_TRADE=250
MIN_PROFIT_BPS=30
MAX_SLIPPAGE_BPS=50
EXECUTION_MODE=fastest
PRIORITY_FEE_LAMPORTS=
LOOP_INTERVAL_MS=800
JUP_API_BASE=https://quote-api.jup.ag/v6
DRY_RUN=true

---

### 5. Run the bot
npm start

Example logs:
[zkverisarb] Watching 2 tokens | mode=fastest | minProfit=0.30% | dryRun=true
[WIF] spread=0.46% ✓ DRY_RUN=true (not sending trade)

---

### 6. Keep alive (systemd)
sudo cp service/zkverisarb.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable zkverisarb
sudo systemctl start zkverisarb
journalctl -u zkverisarb -f

---

## 🔍 Troubleshooting

- “WALLET_SECRET_KEY_B58 required” → set it in `.env`.  
- “TOKENS list is empty” → add at least one mint.  
- Jupiter quote failed → network hiccup or token not tradable; retry; use faster RPC.  
- No trades happen → keep `DRY_RUN=true` to observe; lower `MIN_PROFIT_BPS` when ready.  
- Few opportunities → add more tokens; better RPC; use `fastest` mode.

---

## 🛡 Security
- Always use fresh wallets  
- Never commit `.env`  
- Prefer private RPC  

---

## 📜 License
MIT © 2025 zkveris
