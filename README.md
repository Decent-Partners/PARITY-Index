# **PARITY Index**

**A Synthetic DOT\:KSM Ratio Asset**

*A tokenized rivalry between Kusama & Polkadot.*

---

## ✨ Summary

**PARITY** is a synthetic ERC-20 token deployed on **Kusama Asset Hub** that tracks the market cap ratio of **KSM (Kusama)** to **DOT (Polkadot)**. It allows users to speculate on the convergence of the two ecosystems.

PARITY's price rises when KSM's market cap gets closer to DOT's, whether KSM or DOT move up or down in the broader market. 

* **Price increases** as Kusama gains ground on Polkadot whether KSM or DOT move up or down in the broader market.
* **Payout prize pot of dUSD unlocks** when DOT\:KSM market cap ratio hits 1:1.
* **All trading occurs** in dUSD (a fully backed stablecoin), with **settlement in KSM**.
* **Initial liquidity** is seeded by the creators, with LP tokens then sent to the treasury ensuring the protocol captures fee revenue.
* **Premium capture** increases the Net Asset Value (NAV) of PARITY over time.

This synthetic asset isolates **relative performance** from **absolute price movement**, creating a **new class of DeFi primitive**.

---

## ⚙️ Mechanics Overview

| **Property**     | **Details**                                 |
| ---------------- | ------------------------------------------- |
| Token            | `PARITY` (ERC-20)                           |
| Price Reference  | Market cap ratio: DOT / KSM                 |
| Settlement Asset | `KSM`                                       |
| Trading Pair     | `PARITY/dUSD`                               |
| Chain            | Kusama Asset Hub (PVM)                      |
| Login            | [Virto Connect](https://demo.virto.dev/) universal login               |
| Market Mechanism | DODO-style Proactive Market Maker (PMM)     |
| Oracle Feed      | Off-chain script + on-chain Oracle contract |
| Prize Trigger    | DOT\:KSM ratio ≤ 1.0                        |
| Prize Pot      | `dUSD`, distributed pro-rata to holders      |
| Frontend         | [WIP here](https://parity.birdbrain.lol)    |

---

## ⚖️ Core Concepts

### What is PARITY?

* Synthetic token that tracks the **DOT\:KSM** market cap ratio.
* Price increases as KSM gains ground against DOT.
* Users profit from **relative convergence**, not just absolute price.
* Designed to be **transparent, non-custodial**, and **unruggable**.
* A **prize pool in KSM** is unlocked at full parity (1:1), redeemable by burning PARITY.

---

## 🧪 Net Asset Value (NAV) Growth

**Buy Pressure = NAV Growth**

* If a user buys PARITY at a **premium**, the excess stablecoin is **retained**, increasing the **NAV** per token.
* The **protocol mints** new tokens based on the oracle price, but retains any extra stablecoin paid above that value.
* This premium is **removed from the pool**, boosting the protocol-owned value.

**Example:**

* Oracle price = \$0.10, Buyer pays \$0.11
* \$0.01 premium is captured → NAV rises to \$0.111

---

## 📈 Market Mechanics

### Proactive Market Maker (PMM)

Based on **DODO’s PMM model**, the pricing curve adjusts dynamically with each trade:

```math
Price(x) = P₀ * (1 + Κ * (x / R))
```

Where:

* `P₀`: Oracle price (DOT/KSM ratio)
* `Κ`: Curvature constant (e.g. 0.8)
* `x`: Trade size
* `R`: Pool reserve

**Key Benefits:**

* Minimizes slippage
* Allows **single-sided liquidity**
* Responsive to **oracle-driven price anchoring**

---

## 💸 Fees & Revenue

### 1. Mint/Redeem Fee

* **0.25%** fee on mint/redemption
* Collected in **dUSD**
* Sent to the **Prize Pot**. 

```solidity
uint256 fee = (amountIn * 25) / 10000;
```

### 2. Slippage Capture

* \~**0.5%** fee on trades that deviate from oracle price
* **Non-inflationary**, scales with usage
* Sent to the **Prize Pot**.

---

## 🏆 Prize Pot Mechanics

* **Denominated in KSM**
* **One-time payout** when DOT\:KSM ratio hits or drops below **1.0**
* Claim process:

  * Burn PARITY tokens
  * Receive pro-rata share of dUSD prize pool

```solidity
payout = (prizePoolBalance * userBalance) / totalSupply;
```

---

## 🟡 Why It Matters for Kusama

* **KSM-native launch** on Asset Hub
* **Stablecoin liquidity**, KSM-based settlement
* **Treasury-seeded LP** aligns public funding with upside
* **PARITY trading activity** increases demand for:

  * **dUSD** → strengthens DeFi layer
  * **KSM** → price support & narrative attention

This is not just a token — it’s a **DeFi ignition mechanism** for the Kusama ecosystem.

---

## 🔗 Oracle & Stablecoin Support

* Oracle uses **CoinGecko API** to fetch live DOT and KSM market caps.
* Feed updates pushed via a script to an on-chain **OracleFeed.sol** contract.
* Quoted in **bridged USDC/T or dUSD**, anticipating future **stablecoin precompiles**.

---

## 🔐 Virto Connect Integration

* Unified signer for **EVM-based Kusama**
* Supports:

  * **MetaMask**
  * **Nova**
  * **Talisman**
  * **Passkeys/WebAuthn**
* Accessible UX for web2 and web3 users

---

## 🧱 Smart Contract Architecture

| Contract        | Role                                   |
| --------------- | -------------------------------------- |
| `PARITYToken`   | ERC-20 token tracking DOT\:KSM         |
| `OracleFeed`    | Fetches & pushes ratio data on-chain   |
| `ParityPMMPool` | Proactive Market Maker stablecoin pool |
| `PrizePool`     | KSM vault unlocked at parity           |

---

## 🧠 Project Goals

* ✅ Launch MVP on **Kusama Asset Hub**
* ✅ Mint/Redeem flow using **dUSD**
* ✅ Prize logic + PMM math fully tested
* ✅ Lightweight **frontend** at `birdbrain.lol`
* 🚧 Governance engagement + LP reclaim motion

---

## 🔮 What’s Next?

* Support for **native asset PMMs** (once precompiles are live)
* New matchups (e.g. **ETH vs BTC**, **SOL vs AVAX**)
* Plug into **decentralized oracles** like **DIA**, **Substrate-native feeds**
* Full **DAO governance** of prize pool and future matchups

---

## 🧑‍💻 Development Directory

```
├── contracts/
│   ├── PARITYToken.sol
│   ├── OracleFeed.sol
│   ├── ParityPMMPool.sol
│   └── PrizePool.sol
├── frontend/
│   ├── public/
│   ├── pages/
│   └── components/
├── scripts/
│   └── updateOracle.ts
└── README.md
```

---

## 🤝 Contribute

Looking for:

* Solidity engineers (esp. with DODO, PMM, or synthetic asset experience)
* UX designers for `birdbrain.lol`
* DeFi meme creators
* Governance tacticians for Kusama OpenGov

---

## 📜 License

MIT — **Memeable. Forkable. Fundable.**

---

Would you like this exported into a markdown file, a PDF, or integrated into a frontend readme format?




