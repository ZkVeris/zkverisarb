# zkverisarb

**zkverisarb** is a high-frequency **Solana DEX arbitrage bot**.  
It monitors multiple tokens you whitelist, finds price gaps via the **Jupiter aggregator** across major Solana DEXs (Raydium, Orca, Phoenix, Lifinity, Meteora, …), simulates **round-trip** trades (after slippage & fees), and executes only if profit is guaranteed.

- Author / Contact: [Twitter/X](https://x.com/ZkVeris) • [Telegram](https://t.me/ZkVeris)  
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

## 📂 Repository Structure

zkverisarb/
├── README.md # This file
├── LICENSE # MIT open source license
├── .gitignore
├── .env.example # Copy to .env and fill in
├── package.json # Node.js project file
└── src/
├── index.js # Main loop: scan → simulate → (optional) trade
├── config.js # Loads & validates .env
├── dexScanner.js # Round-trip quotes via Jupiter
├── jupiter.js # Jupiter API helpers
├── risk.js # Profitability checks
└── utils.js # Helpers
├── scripts/
│ └── keypair-json-to-b58.js # Convert Solana keypair JSON → base58 secret
└── service/
└── zkverisarb.service # systemd unit (optional)

## 🛠 Installation Guide

### 0. Requirements
- Ubuntu 24.04 VPS (2 vCPU / 4 GB RAM+)
- Node.js 18+, git
- Solana RPC (private/paid recommended)
- Fresh Solana wallet (base58 secret, small funds)

---

### 1. Install prerequisites
```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs git build-essential
node -v && npm -v && git --version
