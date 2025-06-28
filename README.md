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

    A synthetic token tracking the DOT:KSM market cap ratio.
    Backed by wrapped KSM and DOT (v1), with minimal wDUSD from fees or temporary holding; native KSM and DOT in v2.
    Price increases as KSM outperforms DOT.
    Redeemable at Net Asset Value (NAV) for wrapped KSM, DOT, and any wDUSD (v1); native assets in v2.
    Transparent, non-custodial, and designed to be unruggable.

## 🧪 Net Asset Value (NAV)

NAV = (Total Wrapped KSM & DOT Held + Vaulted wDUSD) ÷ PARITY Supply (v1); (Total Native KSM & DOT Held) ÷ PARITY Supply (v2)

    In v1, wDUSD (including premiums) is used to purchase wrapped KSM and DOT via KSM:dUSD and DOT:dUSD PMM pools, matching the oracle-reported ratio. In v2, native KSM and DOT will be used directly.
    Premiums are fully allocated to buy additional wrapped KSM and DOT (v1), increasing NAV without minting more PARITY or holding excess wDUSD.
    Redeeming PARITY returns a pro-rata share of wrapped KSM, DOT, and any residual wDUSD (v1); native assets in v2.

Example:

    Oracle ratio = DOT market cap / KSM market cap = 10 (DOT = $10B, KSM = $1B).
    Oracle prices: KSM = $20, DOT = $80.
    User mints 1 PARITY with $11 wDUSD (oracle price = $10).
    Protocol uses $10 to buy $6.67 wDOT (0.0833 wDOT) and $3.33 wKSM (0.1665 wKSM), and $1 premium to buy additional wKSM/wDOT in the same ratio.
    BackingVault holds ~0.0875 wDOT, ~0.1748 wKSM; NAV = $11.
    Redeeming 1 PARITY returns ~0.0875 wDOT, ~0.1748 wKSM, and $0 wDUSD (if none held). In v2, native KSM/DOT will be returned.

## 🔁 Dynamic Rebalancing

The protocol maintains the oracle ratio by:

    Buying wrapped KSM and DOT with all incoming wDUSD (including premiums) via PMM pools (v1); native assets in v2.
    Allowing redemption of PARITY for wrapped KSM, DOT, and any wDUSD (v1); native assets in v2.
    Periodically rebalancing by swapping wrapped KSM/DOT (v1) or native KSM/DOT (v2) if the oracle ratio shifts >1% (funded by fees or minimal vaulted wDUSD).

Example: If the ratio drops to 8, the protocol sells wDOT for wKSM (v1) or native DOT for KSM (v2) to adjust reserves to an 8:1 value ratio.
📈 Market Mechanics: PMM

DODO-style PMM contracts manage PARITY/dUSD, KSM:dUSD, and DOT:dUSD pools with single-sided liquidity:
text
Price(x) = P₀ × (1 + Κ × (x / R))

    P₀: Oracle price (DOT:KSM ratio, normalized to dUSD).
    Κ: Curvature constant (0.8).
    x: Trade size.
    R: Pool reserve (wDUSD).

Premium Allocation:

  Buying PARITY at a premium (e.g., $0.11 vs. $0.10 oracle price):
  Mints PARITY at oracle price ($0.10).
  Uses full $0.11 (including $0.01 premium) to buy wrapped KSM and DOT via PMM pools (v1); native assets in v2.
  Increases NAV by deepening reserves without inflating PARITY supply.

Benefits:

  Single-sided liquidity (wDUSD, wKSM, or wDOT in v1; native assets in v2).
  Oracle-anchored pricing.
  Low slippage, high capital efficiency.
  Transforms protocol into a reserve-building agent.

Liquidity Provision:

  Initial liquidity for PARITY:dUSD is seeded by creators, with LP tokens sent to the Treasury upon pool creation to capture trading fees.

## 📊 NAV Dynamics

| Action                  | Effect on Supply | Effect on Pool Value | Effect on NAV     |
|------------------------|------------------|-----------------------|-------------------|
| Buy at premium         | ↑ Increases      | ↑ Increases           | ↑ Increases       |
| Sell back to pool      | ↓ Decreases      | ↓ Decreases           | Neutral / ↓       |
| Hold (no action)       | -                | -                     | ↑ (if others buy) |

## 🔐 Virto Connect Integration

    Single-sign-on for Kusama’s EVM ecosystem.
    Supports MetaMask, Nova, Talisman, and WebAuthn (passkeys).
    Ensures seamless UX for web2 and web3 users.

## 🔗 Oracle & Stablecoin Support

    Oracle Feed: CoinGecko API, pushed on-chain every 5 minutes, with 24-hour TWAP smoothing. Future DIA integration.
    Stablecoin: wDUSD (collateralized) in v1, native dUSD in v2; bridged USDC as fallback.
    Note: Oracle reliability is critical; decentralized alternatives are planned.

## 🧠 Project Goals

    ✅ Launch MVP on Kusama Asset Hub (v1).
    ✅ Oracle-driven minting with premium allocation to reserves.
    ✅ Dynamic rebalancing for BackingVault.
    ✅ PMM pools for PARITY/dUSD, KSM:dUSD, DOT:dUSD.
    🚧 Basic frontend for mint/redeem/trade.
    🚧 Governance for Treasury-owned LP.

## 🔮 What’s Next?

    Native DOT bridging and redemption.
    New synthetic matchups (e.g., ETH:BTC).
    Decentralized oracle integration (e.g., DIA).
    PARITY v2: Migration to native KSM, DOT, and dUSD precompiles after Kusama Asset Hub runtime upgrade.

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
