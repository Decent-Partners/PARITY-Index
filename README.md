# PARITY Index

A fully backed DOT:KSM Ratio Asset

A tokenised rivalry between Kusama & Polkadot.

## 🚀 Introduction

PARITY INDEX is a proof of concept built on top of PARITY PROTOCOL a novel decentralised finance (DeFi) system enabling the tokenisation of on chain rivalries through ratio markets, using financial engineering to empower David vs Goliath competitions.

The first ratio market to launch on the protocol is Kusama vs Polkadot and the PARITY token (this will likely be $KVP ticker when launched on mainnet). The rivalry is captured in real-time through their relative market caps, which is the current ‘price’ to mint or burn the token.

Users can mint tokens using KSM, DOT, or dUSD, creating a synthetic asset that tracks relative performance — isolated from broader market movements. The protocol features a dynamic fee mechanism that funds a NAV Vault for holder bonuses to reward early adopters and hodlers and a multi-oracle consensus system for ingesting data feeds.

The project evolves prediction markets, using economic incentives to align collective interest, with the potential to create any ratio market based on independent and publicly verifiable data feeds:

- Crypto centric: Bitcoin vs Gold, Ethereum vs Bitcoin, Solana vs Ethereum
- Traditional markets: Microsoft vs Apple market cap, Intel vs Nvidia market cap
- Sports: Team A’s performance vs Team B’s performance
- Film: Movie A ticket sales vs Movie B’s ticket sales
- Music: Artist A streams vs Artist B’s streams
- Platform: Streaming platform A subscribers vs Streaming platform B subscribers
- Politics: Political party A members vs Political party B members

| Feature | Traditional Prediction Markets (Polymarket, Kalshi, Azuro, Omen, etc.) | Perpetual Synthetics (GMX, Gains Network, Synthetix) | Ratio / Index Tokens (Index Coop, BasketDAO, old Set Protocol) | PARITY Ratio Markets (Kusama vs Polkadot, etc.) |
|--------|--------------------------------------------------------------------------|-------------------------------------------------------|------------------------------------------------------------------|---------------------------------------------------|
| **What you are long/short** | Probability of a discrete event (Yes/No or scalar outcome) | Absolute price direction of an asset | Basket of assets (absolute performance) | Relative performance of two rivals only |
| **Market-direction neutrality** | No (crypto-wide moves affect outcome prices) | No | No | Yes – 100% isolated from broader market |
| **Expiry / settlement** | Fixed event end date → one-time payout | None (perpetual) | None | None – perpetual, real-time price |
| **Resolution mechanism** | Oracle or jury at event close | Price feed | Rebalancing + price feeds | Continuous oracle consensus on ratio |
| **Holding incentive** | None (pure speculation) | Funding rate arbitrage | Sometimes small streaming fees | Strong – NAV Vault bonuses that can exceed entry cost |
| **Early-adopter / loyalty advantage** | None | None | Rare | Yes – dynamic burn rate, lower NAV take the longer you hold |
| **Composability** | Low (PM tokens rarely used elsewhere) | High | High | Very high – ERC-20 ratio token usable anywhere |
| **Scope of what can be tokenized** | Any event with clear resolution | Any price feed | Any basket of tokens | Any two publicly verifiable metrics (crypto, stocks, sports, music, politics…) |
| **Economic flywheel** | Volume → fees → nothing for holders | Volume → fees → liquidity providers | Volume → tiny streaming fees | Mint/burn volume → NAV Vault → rewards loyal holders |
| **Primary user emotion** | “Will this event happen?” | “Will price go up or down?” | “I want diversified exposure” | “I believe the underdog will close the gap” |
| **Closest mental model** | Bet on election, Super Bowl, etc. | Perpetual futures | Crypto index fund | Tokenized rivalry / “David vs Goliath” certificate |

## 🚀 How PARITY INDEX works

Unlike passive tokens, PARITY is actively backed by wrapped KSM and DOT (v1), redeemable at any time for its share of the protocol’s reserves plus any bonuses. 

Premiums from PARITY purchases are used to buy more KSM and DOT, building deeper reserves. 

Transparent, non-custodial, and designed to be unruggable.

- **PARITY v1 and v2 Roadmap**:
  - *PARITY v1*: Uses wrapped ERC-20 tokens for KSM, DOT, dUSD, and PARITY due to missing precompiles on Kusama Asset Hub.
  - *PARITY v2*: Will transition to native KSM, DOT, and dUSD after a future runtime upgrade enables precompiles, improving efficiency.

- **Benefits for KSM and DOT Holders**:
  - *Increased Demand*: Premiums buy wrapped KSM/DOT, boosting demand.
  - *Enhanced Ecosystem Growth*: A DeFi primitive attracts developers and users, increasing network activity.
  - *Improved Liquidity*: [DODO-style](https://docs.dodoex.io/en/product/pmm-algorithm) PMM pools deepen KSM:dUSD and DOT:dUSD liquidity and address 'AssetConversionPallet' limitations.
  - *Greater Composability*: Wrapped KSM (v1) and native KSM (v2) enable DeFi integration.

DODO-style Proactive Market Maker (PMM) contracts manage liquidity for PARITY/dUSD, KSM:dUSD, and DOT:dUSD pools, while wrapped tokens enable smart contract composability in v1. Oracle-driven rebalancing ensures the DOT:KSM ratio is maintained. 

In v2, native precompiles will replace wrapped tokens for improved efficiency.

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

- Single-sign-on for Kusama’s EVM ecosystem.
- Supports MetaMask, Nova, Talisman, and WebAuthn (passkeys).
- Ensures seamless UX for web2 and web3 users.

## 🔮 What’s Next?
   
- PARITY v2: Migration to native KSM, DOT, and dUSD precompiles after Kusama Asset Hub runtime upgrade.
- Decentralized oracle integration (e.g., DIA).
- New synthetic matchups (e.g., ETH:BTC).

## 🧑‍💻 Development Directory

```markdown

├── contracts/
│   ├── PARITYToken.sol        # ERC-20 PARITY token logic
│   ├── WrappedAsset.sol       # Wrapper contracts for native KSM/DOT
│   ├── OracleFeed.sol         # Oracle price feed integration
│   ├── ParityPMMPool.sol      # DODO-style PMM for backing/assets
│   ├── BackingVault.sol       # Vault to hold wrapped/native assets
│   └── MintRedeem.sol         # Minting and redeeming logic for PARITY
├── frontend/
│   ├── public/                # Static assets
│   ├── pages/                 # Next.js or similar frontend routing
│   └── components/            # Reusable UI components
├── scripts/
│   └── updateOracle.ts        # Script to update oracle prices on-chain
└── README.md                  # Project documentation

```

## 📜 License

MIT — Memeable. Forkable. Fundable.
