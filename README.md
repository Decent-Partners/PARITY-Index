## PARITY Index

### A Synthetic DOT\:KSM Ratio Asset

A tokenised rivalry between Kusama & Polkadot.

---

### ✨ Summary

PARITY is a synthetic ERC-20 token deployed on **Kusama Asset Hub** that tracks the **market cap ratio of KSM to DOT**. It allows users to speculate on the convergence of the two ecosystems.

Unlike passive tokens, PARITY is **actively backed** by KSM and DOT, and can be **redeemed at any time** for its fair share of the protocol’s holdings. The protocol dynamically adjusts its backing by **purchasing KSM and DOT** based on a live oracle feed.

* 📈 **Price rises** as Kusama gains ground on Polkadot (whether KSM or DOT go up or down).
* 💵 **PARITY is fully backed** by KSM and DOT held by the protocol treasury.
* 🔁 **Redeemable 1:1** against backing assets based on current market cap ratio.
* 🧮 **Oracle-driven rebalancing** ensures the protocol tracks the DOT\:KSM ratio over time.
* 🧰 Initial liquidity is seeded by creators; LP tokens are sent to the treasury to capture trading fees.

This synthetic asset isolates **relative performance** from absolute price movement, offering a novel DeFi primitive for ratio-based speculation.

---

### ⚙️ Mechanics Overview

| Property         | Details                            |
| ---------------- | ---------------------------------- |
| Token            | `PARITY` (ERC-20)                  |
| Price Reference  | Market cap ratio: DOT / KSM        |
| Backing Assets   | KSM + DOT                          |
| Trading Pair     | PARITY/dUSD                        |
| Chain            | Kusama Asset Hub (PVM)             |
| Login            | Virto Connect universal login      |
| Market Mechanism | Proactive Market Maker (PMM)       |
| Oracle Feed      | Off-chain script + on-chain Oracle |
| Redemption       | PARITY → KSM & DOT (on demand)     |
| Frontend         | WIP \[https;//parity.birdbrain.lol]                        |

---

### ⚖️ Core Concepts

#### What is PARITY?

* A synthetic token that tracks the **DOT\:KSM market cap ratio**
* Backed by a basket of **KSM and DOT**
* Price increases as **KSM outperforms DOT**
* Fully **redeemable at NAV** (Net Asset Value)
* Designed to be transparent, non-custodial, and unruggable

---

### 🧪 Net Asset Value (NAV)

**NAV = Total KSM & DOT Held ÷ PARITY Supply**

* When users mint PARITY, dUSD is used by the protocol to **purchase KSM and DOT** in the appropriate proportions.
* The resulting tokens are held in the backing vault.
* When users redeem PARITY, they receive an equivalent share of the backing KSM and DOT.

#### Example:

* Oracle ratio = DOT market cap / KSM market cap = 10
* The protocol will hold \$10 of DOT and \$1 of KSM for every PARITY token worth \$11
* Redeeming 1 PARITY returns approximately \$10 in DOT and \$1 in KSM

---

### 🔁 Dynamic Rebalancing

To maintain alignment with the oracle ratio, the protocol:

1. **Buys KSM and DOT** using incoming dUSD at the current ratio
2. **Allows redemption** of PARITY for backing tokens
3. Periodically **rebalances holdings** by swapping between KSM and DOT if the oracle ratio shifts significantly

---

### 📈 Market Mechanics: PMM

The Proactive Market Maker (PMM) offers low-slippage trading and dynamic pricing:

```math
Price(x) = P₀ × (1 + Κ × (x / R))
```

Where:

* `P₀`: Oracle price (DOT/KSM ratio)
* `Κ`: Curvature constant (e.g. 0.8)
* `x`: Trade size
* `R`: Pool reserve

**Key Benefits:**

* Single-sided liquidity support
* Oracle-anchored dynamic pricing
* Facilitates mint/redemption and arbitrage

---

### 💸 Fees & Revenue

1. **Mint/Redeem Fee**

   * 0.25% collected in dUSD
   * Sent to Kusama Treasury

   ```solidity
   uint256 fee = (amountIn * 25) / 10000;
   ```

2. **Slippage Capture**

   * \~0.5% on trades deviating from the oracle price
   * Non-inflationary, sent to Treasury

3. **LP Fee Capture**

   * All LP tokens from the initial liquidity are sent to the Treasury
   * Treasury earns ongoing trading fees from dUSD/PARITY pair

---

### 🔗 Oracle & Stablecoin Support

* **Oracle Feed**: Uses CoinGecko API to fetch live market cap data
* Ratio (DOT market cap ÷ KSM market cap) pushed on-chain every 5 minutes
* **Stablecoin**: dUSD (fully collateralised), used for minting and trading

---

### 🔐 Virto Connect Integration

* Single-sign-on for Kusama’s EVM ecosystem
* Supports MetaMask, Nova, Talisman, and WebAuthn (passkeys)
* Designed for seamless UX across web2 and web3 users

---

### 🧱 Smart Contract Architecture

| Contract         | Role                                      |
| ---------------- | ----------------------------------------- |
| `PARITYToken`    | ERC-20 token tracking DOT\:KSM            |
| `OracleFeed`     | Off-chain ratio feed pushed to chain      |
| `ParityPMMPool`  | dUSD/PARITY market using PMM              |
| `BackingVault`   | Stores KSM and DOT underlying each PARITY |
| `MintRedeem.sol` | Handles minting/redemption vs underlying  |

---

### 🧠 Project Goals

* ✅ Launch MVP on Kusama Asset Hub
* ✅ Oracle and asset-backed minting flow
* ✅ Dynamic rebalancing logic for backing vault
* ✅ Basic frontend for mint/redeem/trade
* 🚧 Add governance hook for Treasury-owned LP

---

### 🔮 What’s Next?

* Native DOT bridging and redemption
* New synthetic matchups (e.g. ETH\:BTC, SOL\:AVAX)
* Decentralized oracle integration (e.g. DIA, Substrate-native)

---

### 🧑‍💻 Development Directory

```
├── contracts/
│   ├── PARITYToken.sol
│   ├── OracleFeed.sol
│   ├── ParityPMMPool.sol
│   ├── BackingVault.sol
│   └── MintRedeem.sol
├── frontend/
│   ├── public/
│   ├── pages/
│   └── components/
├── scripts/
│   └── updateOracle.ts
└── README.md
```

---

### 📜 License

**MIT** — Memeable. Forkable. Fundable.







