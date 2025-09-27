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
