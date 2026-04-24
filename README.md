# Nado Vault Bot

> Automated NLP vault yield optimizer for Nado CLOB DEX on Ink L2 (Kraken). Monitors vault APY, auto-deposits idle margin when yield is favorable, compounds earnings, rebalances between vault and trading capital, and tracks INK airdrop points earned through vault participation.

*Updated: April 2026*

## What is Nado Vault Bot?

Nado's NLP vault is a native liquidity provision vault where deposited USDC earns a share of Nado's CLOB trading fees. As Nado processes growing volume on Ink L2 (Kraken's L2 chain), vault APY scales with protocol activity.

![preview_nado-vault-bot](https://github.com/user-attachments/assets/3861be63-99cf-48a9-8b69-32a35a8eafc1)

Nado Vault Bot automates the full NLP vault cycle: it monitors vault APY, auto-deposits idle capital above your APY threshold, withdraws what is needed when trading opportunities arise, and continuously compounds vault earnings back into the principal. The bot ensures no USDC sits idle on Ink L2 between trading sessions.

Additionally, vault depositors may earn INK airdrop points separate from trading activity. The bot tracks vault participation volume and time for Q2 2026 airdrop eligibility.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github-zip.com/nado) |

---

## Vault Strategy Settings

| Setting | Example | Description |
|---------|---------|-------------|
| `min_apy_to_deposit` | 10.0 | Minimum vault APY % before depositing |
| `max_vault_allocation_pct` | 75 | Max % of balance to keep in vault |
| `rebalance_interval_min` | 60 | Frequency of vault vs trading rebalance |
| `auto_compound` | true | Reinvest earnings automatically |
| `trading_reserve_usd` | 400 | Always keep this amount outside vault |
| `ink_points_track` | true | Track vault participation INK points |

---

## Engine Features

* **Real-time APY monitor** - fetches NLP vault APY from Nado API every poll cycle
* **Auto-deposit** - moves idle capital into vault when APY exceeds configured threshold
* **Auto-withdraw** - withdraws from vault when perp trader or airdrop bot needs margin
* **Auto-compound** - reinvests vault earnings to increase deposited principal
* **Rebalancer** - periodically checks vault vs trading capital split and corrects
* **Trading reserve** - always preserves a fixed USDC amount outside vault for trading
* **INK points tracker** - monitors vault depositor points and airdrop eligibility
* **APY history log** - records NLP vault APY over time for performance analysis
* **Unified margin integration** - works with Nado's unified spot+perp margin system

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **APY threshold** | Config slider | Direct code access |
| **Compounding** | Built-in toggle | Code configurable |
| **Config** | `config.toml` | Direct code access |
| **Logs** | Dashboard | JSON per cycle |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set minimum APY threshold and rebalance frequency
# 3. Run Vault Bot - APY monitoring and auto-deposit start immediately
```

### Python

```bash
cd nado-vault-bot/python
pip install -r requirements.txt
python nado-vault-bot-raw-v.1.4.14.py
```

---

## How It Works

1. **Monitor** - fetches NLP vault APY and current deposit balance from Nado API
2. **Evaluate** - checks if APY is above threshold and whether rebalance is needed
3. **Deposit/Withdraw** - moves capital between Ink L2 wallet and NLP vault as needed
4. **Compound** - claims accrued earnings and re-deposits them to increase principal
5. **Report** - logs cycle result with APY, balance, yield earned, and INK points

### Config Reference

```toml
[wallet]
api_key = ""
rpc_url = "https://rpc-gel.inkonchain.com"

[vault]
min_apy_to_deposit = 10.0
max_vault_allocation_pct = 75
rebalance_interval_min = 60
auto_compound = true
trading_reserve_usd = 400
ink_points_track = true

[nado]
api_base = "https://api.nado.exchange"

[alerts]
telegram_bot_token = ""
telegram_chat_id = ""
apy_spike_alert = 20.0
```

---

## Vault Cycle Report Format

```json
{
  "cycle_id": "nado_vault_20260319_c6",
  "vault_apy_pct": 14.2,
  "deposited_usd": 3600.0,
  "trading_reserve_usd": 400.0,
  "earnings_today_usd": 1.40,
  "compounded_usd": 1.40,
  "cumulative_earnings_usd": 29.80,
  "ink_vault_points": 62,
  "action": "compound - added $1.40 to principal"
}
```

---

## Verified Live

**Configuration used:**
* Min APY 10%, max 75% in vault, $400 reserve, auto-compound ON

| | Cycle Report |
|---|---|
| Vault APY | 14.2% |
| Deposited in vault | $3,600 |
| Trading reserve | $400 |
| Earnings today | $1.40 |
| Compounded | $1.40 added to principal |
| Cumulative earnings | $29.80 |
| INK vault points | 62 pts |

![nado vault bot compound report](https://github.com/user-attachments/assets/c5a0ffd0-eeba-4e0c-b385-0c73c17fd2c4)

---

## Frequently Asked Questions

**What is the NLP vault on Nado?**
NLP is Nado's native liquidity provider vault. Depositing USDC makes you a liquidity provider for Nado's CLOB order book. You earn a portion of Nado's trading fees, with APY scaling as protocol volume grows.

**What is the current NLP vault APY?**
APY is variable and changes with protocol volume. The bot logs APY history so you can see trends. It alerts you via Telegram when APY spikes above your configured threshold.

**How does auto-compound work?**
Vault earnings accumulate in the contract. The bot periodically claims them and re-deposits to increase your principal, so the next cycle's yield is calculated on a larger base.

**Does the vault interact with unified margin?**
Nado's unified margin treats vault deposits as part of your collateral pool. The bot manages withdrawals carefully to preserve your perp trading margin when both systems run together.

**Does vault participation earn INK airdrop points?**
INK airdrop eligibility for vault depositors is tracked separately from trading activity. The bot records vault deposit time and volume for this allocation category.

**What is Ink L2?**
Ink is Kraken's Layer 2 blockchain (OP Stack based). It provides fast, cheap execution for Nado's CLOB markets and is the chain where vault deposits are held.

---

## Use Cases

- **NLP vault yield optimizer** - automate deposit, compounding, and withdrawal on Nado's vault
- **Ink L2 yield farming** - earn Nado CLOB trading fee revenue through the NLP vault
- **Nado idle capital manager** - eliminate idle USDC between trading sessions on Ink L2
- **Nado auto-compound bot** - compound NLP vault earnings continuously to maximize yield
- **INK airdrop vault tracker** - monitor vault participation points for Q2 2026 distribution

---

## Repository Structure

```
nado-vault-bot/
+-- nado-vault-bot-raw-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- cycles/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- monitor.py
|   |   +-- depositor.py
|   |   +-- compounder.py
|   |   +-- rebalancer.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, web3, eth-account, httpx, typer[all], devtools
```

* Ink L2 wallet with USDC and ETH (gas)
* Nado account (connect at nado.exchange)
* Telegram bot token (optional)

---

*Prediction market intelligence, automated.*
