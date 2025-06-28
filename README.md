# PARITY Index

A Synthetic DOT:KSM Ratio Asset

A tokenized rivalry between Kusama & Polkadot.

## 🚀 Introduction

**PARITY** is a synthetic ERC-20 token on [Kusama Asset Hub](https://kusama.network/) that tracks the market cap ratio of Kusama (KSM) to Polkadot (DOT), enabling speculation on their convergence. Unlike passive tokens, PARITY is actively backed by wrapped KSM and DOT (v1), redeemable at any time for its share of the protocol’s reserves. Premiums from PARITY purchases are used to buy more KSM and DOT, building deeper reserves. Initial liquidity for the PARITY:dUSD pool is seeded by creators, with LP tokens sent to the Treasury upon pool creation to capture trading fees.

- **PARITY v1 and v2 Roadmap**:
  - *PARITY v1*: Uses wrapped ERC-20 tokens for KSM, DOT, dUSD, and PARITY due to missing precompiles on Kusama Asset Hub.
  - *PARITY v2*: Will transition to native KSM, DOT, and dUSD after a future runtime upgrade enables precompiles, improving efficiency.

- **Benefits for Kusama and KSM Holders**:
  - *Increased KSM Demand*: Premiums buy wrapped KSM, boosting demand.
  - *Enhanced Ecosystem Growth*: A DeFi primitive attracts developers and users, increasing network activity.
  - *Improved Liquidity*: [DODO-style](https://docs.dodoex.io/en/product/pmm-algorithm) PMM pools deepen KSM:dUSD and DOT:dUSD liquidity.
  - *Greater Composability*: Wrapped KSM (v1) and native KSM (v2) enable DeFi integration.

- **Problems Addressed**:
  - *Thin Liquidity*: KSM:dUSD pool was illiquid and mispriced.
  - *Limited Asset Interaction*: No precompiles restricted KSM’s DeFi use.
  - *Stablecoin Over-Reliance*: Excess dUSD weakened reserves.
  - *Centralized Management*: Manual pool management by Brale was inefficient.

## ✨ Summary

**PARITY** is a synthetic ERC-20 token deployed on Kusama Asset Hub that tracks the market cap ratio of Kusama (KSM) to Polkadot (DOT). It enables speculation on the relative performance of the two ecosystems, with active backing by wrapped KSM and DOT reserves (v1). PARITY is redeemable at any time for its share of the protocol’s holdings, including wrapped KSM, DOT, and any residual wDUSD.

The protocol proactively uses premiums from PARITY purchases to acquire more wrapped KSM and DOT, building a deeper, more liquid reserve aligned with the oracle-reported ratio. DODO-style Proactive Market Maker (PMM) contracts manage liquidity for PARITY/dUSD, KSM:dUSD, and DOT:dUSD pools, while wrapped tokens enable smart contract composability in v1. Oracle-driven rebalancing ensures the DOT:KSM ratio is maintained. In v2, native precompiles will replace wrapped tokens for improved efficiency.

- 📈 Price rises as Kusama gains ground on Polkadot.
- 💵 Fully backed by wrapped KSM and DOT (v1), with minimal wDUSD.
- 🔁 Redeemable 1:1 against backing assets based on the market cap ratio.
- 🧮 Oracle-driven rebalancing maintains the DOT:KSM ratio.
- 🧰 Liquidity seeded by creators; LP tokens for PARITY:dUSD sent to Treasury for fee capture.

## ⚙️ Mechanics Overview

| **Property**        | **Details**                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **Token**           | PARITY (ERC-20, wrapped in v1)                                              |
| **Price Reference** | Market cap ratio: DOT / KSM                                                 |
| **Backing Assets**  | Wrapped KSM + DOT + minimal wDUSD (v1); native KSM + DOT (v2)               |
| **Trading Pairs**   | PARITY/dUSD, KSM:dUSD, DOT:dUSD                                             |
| **Chain**           | Kusama Asset Hub (PVM)                                                      |
| **Login**           | [Virto Connect](https://demo.virto.dev/) universal login                    |
| **Market Mechanism**| DODO-style Proactive Market Maker (PMM) with premium allocation to reserves |
| **Oracle Feed**     | [CoinGecko API](https://www.coingecko.com/) (off-chain) + on-chain Oracle, 24-hour TWAP |
| **Redemption**      | PARITY → Wrapped KSM & DOT & wDUSD (v1); Native KSM & DOT (v2)              |
| **Frontend**        | [WIP](https://parity.birdbrain.lol/)                                       |

## 🧱 Smart Contract Architecture

| **Contract**          | **Role**                                                              |
|-----------------------|----------------------------------------------------------------------|
| `PARITYToken.sol`     | ERC-20 token tracking DOT:KSM ratio                                  |
| `WrappedAsset.sol`    | Wraps KSM, DOT, dUSD as ERC-20 tokens (v1 only)                      |
| `OracleFeed.sol`      | Pushes off-chain ratio data on-chain                                  |
| `ParityPMMPool.sol`   | Manages PARITY/dUSD, KSM:dUSD, DOT:dUSD pools with PMM               |
| `BackingVault.sol`    | Stores wrapped KSM, DOT, and minimal wDUSD (v1); native assets (v2)   |
| `MintRedeem.sol`      | Handles minting/redemption of PARITY                                 |

## ⚖️ Core Concepts

What is PARITY?

- A synthetic token tracking the DOT:KSM market cap ratio.
- Backed by wrapped KSM and DOT (v1), with minimal wDUSD from fees or temporary holding; native KSM and DOT in v2.
- Price increases as KSM outperforms DOT.
- Redeemable at Net Asset Value (NAV) for wrapped KSM, DOT, and any wDUSD (v1); native assets in v2.
- Transparent, non-custodial, and designed to be unruggable.

## 🧪 Net Asset Value (NAV)

**v1:**  
- NAV = (Total Wrapped KSM & DOT Held + Vaulted wDUSD) ÷ PARITY Supply  

**v2:**  
- NAV = (Total Native KSM & DOT Held) ÷ PARITY Supply

- In v1: `wDUSD` (including premiums) is used to purchase wrapped KSM/DOT via PMM pools at oracle ratio.
- In v2: native assets are used directly.
- Premiums boost NAV by buying more backing without increasing supply.
- Redeeming returns pro-rata share of KSM/DOT (plus `wDUSD` in v1).

**Example:**

- Oracle ratio = 10 (DOT = $10B, KSM = $1B)  
- Oracle prices: KSM = $20, DOT = $80  
- User mints 1 PARITY with $11 wDUSD  
  → $10 used to buy 0.0833 wDOT + 0.1665 wKSM  
  → $1 premium buys extra backing in same ratio  
  → NAV = $11  
  → Redeeming returns ~0.0875 wDOT + ~0.1748 wKSM

---

## 🔁 Dynamic Rebalancing

- Maintains backing ratio by:
  - Buying backing with all `wDUSD` (v1) or native assets (v2)
  - Redeeming PARITY for backing assets
  - Periodic rebalancing if ratio shifts >1%

**Example:**  
If ratio drops to 8, protocol sells wDOT for wKSM (or DOT for KSM in v2).

---

## 📈 Market Mechanics: PMM

DODO-style PMM contracts manage:
- `PARITY/dUSD`, `KSM/dUSD`, and `DOT/dUSD` with single-sided liquidity

**Price(x)** = P₀ × (1 + Κ × (x / R))  
Where:  
- P₀ = Oracle price  
- Κ = 0.8 (curvature constant)  
- x = Trade size  
- R = Pool reserve (wDUSD)

### Premium Allocation:
- PARITY always minted at oracle price
- Full payment (including premium) buys backing via PMM
- NAV increases — no extra PARITY minted

### Benefits:
- Single-sided liquidity
- Oracle-anchored pricing
- Low slippage
- Capital efficient
- Reserve-growing protocol design

### Liquidity Provision:
- Seeded by creators
- LP tokens go to Treasury to earn fees

---

## 📊 NAV Dynamics

| Action            | Effect on Supply | Effect on Pool Value | Effect on NAV     |
|-------------------|------------------|-----------------------|-------------------|
| Buy at premium    | ↑ Increases      | ↑ Increases           | ↑ Increases       |
| Sell back to pool | ↓ Decreases      | ↓ Decreases           | Neutral / ↓       |
| Hold (no action)  | -                | -                     | ↑ (if others buy) |

---
## 🔐 Virto Connect Integration

    Single-sign-on for Kusama’s EVM ecosystem.
    Supports MetaMask, Nova, Talisman, and WebAuthn (passkeys).
    Ensures seamless UX for web2 and web3 users.

## 🔗 Oracle & Stablecoin Support

    Oracle Feed: CoinGecko API, pushed on-chain every 5 minutes, with 24-hour TWAP smoothing. Future DIA integration.
    Stablecoin: wDUSD (collateralized) in v1, native dUSD in v2; bridged USDC as fallback.
    Note: Oracle reliability is critical; decentralized alternatives are planned.

## 🔮 What’s Next?
   
    PARITY v2: Migration to native KSM, DOT, and dUSD precompiles after Kusama Asset Hub runtime upgrade.
    Decentralized oracle integration (e.g., DIA).
    New synthetic matchups (e.g., ETH:BTC).

## 🧑‍💻 Development Directory
text
├── contracts/
│   ├── PARITYToken.sol
│   ├── WrappedAsset.sol
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

## 📜 License

MIT — Memeable. Forkable. Fundable.
