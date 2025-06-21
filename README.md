≈
**WIP frontend**: [here](https://parity.birdbrain.lol)

A tokenized rivalry between **Kusama & Polkadot**, expressed as a synthetic trading asset.

PARITY is a synthetic ERC-20 token deployed on Kusama Asset Hub that tracks the market cap ratio of Polkadot (DOT) to Kusama (KSM). 
It’s designed to reward convergence — when Kusama gains ground on Polkadot, the PARITY token increases in value.
A prize pool pays out to holders when the the two assets reach parity and the contest ends. 

The design isolates relative value of two assets, from their market prices, creating a novel defi primitive. 

**The project is unruggable**: 

- Devs seed initial liquidity (initial KSM + matching PARITY) and send LP tokens to the Kusama Treasury.

Later, they can propose to:

- Reclaim a small % of LP tokens

Why it works:

    ✅ Rewards proven impact (not promises)

    ✅ Aligns team incentives with adoption

    ✅ Uses earned fees, not upfront treasury funds

---

### ✨ Overview

| Property   | Detail                                       |
| ---------- | -------------------------------------------- |
| Token      | `PARITY`                                     |
| Mechanism  | Synthetic PMM (Proactive Market Maker)       |
| Chain      | Kusama Asset Hub (EVM)                       |
| Settles In | dUSD stablecoin, redeemable to KSM                |
| Frontend   | WIP — [birdbrain.lol](https://birdbrain.lol) |
| Signer     | Virto Connect (EVM support)                  |
| Status     | Prototype — contracts + UIs in development   |

---

## 🧠 Core Concepts

### ⚖️ What is PARITY?

* A synthetic token tracking the **DOT/KSM** market cap ratio
* Price increases when **Kusama closes the gap on Polkadot**
* Isolates **relative performance**, from absolute price movement
* **Prize Pool unlocks at 1:1** — holders can claim KSM-based payout
* **Trades vs stablecoin**, but ultimately **settled in KSM**

---

## 🟡 Why This Benefits KSM Holders

PARITY is deployed on Kusama Asset Hub, settled in KSM, and kickstarts defi on Kusama Asset Hub:

* **Stablecoin-based Liquidity, KSM-based Settlement**: All minting, redemptions, and trading happen against a stablecoin (e.g. dUSD) to isolate DOT\:KSM relative valuation. Behind the scenes, dUSD is backed and redeemable for KSM, ensuring KSM demand and usability.
* **Initial LP Backed by Treasury**: The Kusama Treasury seeds liquidity (using burn redirects made possible by [WFC-437](https://kusama.subsquare.io/referenda/437)), aligning public funds with upside from PARITY adoption.
* **Fee Capture Flows Back to KSM Ecosystem**:

  * Mint/redeem and slippage fees collected in dUSD stablecoin
* **Ecosystem Narrative**: PARITY makes KSM the center of an attention-grabbing synthetic bet — a gamified, memetic, and market-tradable comeback arc for Kusama.

> 💥 The more PARITY is used and traded, the more demand for dUSD — and the more value accrues to KSM holders and the Kusama Treasury.

---

## 📊 PARITY Ratio Metrics

| Metric        | Value                         |
| ------------- | ----------------------------- |
| Current Ratio | 0.04 (KSM = 4% of DOT mcap)   |
| Delta         | +428.18bps (4.28%)            |
| Sensitivity   | ±10% Price Movement @ Κ = 0.8 |

> Even small shifts in DOT\:KSM ratio create outsized movements in PARITY price.

---

## 🧮 How the PMM Works

Modeled on [DODO’s Proactive Market Maker](https://dodoex.github.io/docs/docs/PMM/principle/):

* Anchors price to live DOT/KSM market cap ratio
* Stablecoin-denominated pool (e.g. dUSD) avoids price noise from KSM volatility
* Tunable Κ (curvature) controls how quickly price reacts to trades
* Allows single-sided liquidity and arbitrage correction

### PMM Pricing Formula:

```
Price(x) = P₀ * (1 + Κ * (x / R))
```

Where:

* `P₀` = Oracle price (DOT/KSM)
* `Κ` = Curvature (e.g., 0.8)
* `x` = Trade size
* `R` = Pool reserve

---

## 💸 Fee Mechanics

PARITY generates **two usage-based revenue streams**:

### 1. Mint/Redeem Fee

* **0.25%** on all mint or redeem actions
* **Collected in dUSD**, sent to dev multisig or Treasury
* Used for: oracle operations, UI hosting, contract upkeep

```solidity
function mint(uint256 amountIn) external {
    uint256 fee = (amountIn * 25) / 10000; // 0.25%
    uint256 netAmount = amountIn - fee;
    // mint PARITY using netAmount
}
```

### 2. PMM Slippage Capture

* Small cut (e.g., **0.5%**) of trading slippage collected on off-oracle trades
* **Non-inflationary**, scales with usage
* Goes to development treasury or can fund community proposals

---

## 🏆 The Prize Pool

* One-time payout **denominated in KSM**
* Unlocks when **DOT\:KSM ≤ 1.0**
* PARITY holders burn tokens to **claim KSM pro-rata**

```solidity
function claimPrize() external {
    require(isParityReached(), "Not yet at parity");
    uint256 userBalance = parityToken.balanceOf(msg.sender);
    uint256 totalSupply = parityToken.totalSupply();
    uint256 payout = (prizePoolBalance * userBalance) / totalSupply;
    parityToken.burnFrom(msg.sender, userBalance);
    ksmToken.transfer(msg.sender, payout);
}
```

---

## 🧱 Smart Contracts

| Contract        | Role                                 |
| --------------- | ------------------------------------ |
| `PARITYToken`   | ERC-20 token tracking DOT\:KSM ratio |
| `OracleFeed`    | Pushes market cap data on-chain      |
| `ParityPMMPool` | PMM-style stablecoin pool for PARITY |
| `PrizePool`     | Holds KSM, unlocked at parity        |

---

## 🔗 Oracle Feed

* External script (CoinGecko API)
* Calculates DOT / KSM market cap ratio
* Pushes to Oracle contract on-chain
* Referenced by both PMM and PrizePool

---

## 🎨 Frontend — [birdbrain.lol](https://birdbrain.lol)

* Simple UI, light UX, fast load
* Big ratios, few buttons, fun toggle modes
* Live:

  * DOT/KSM ratio
  * PARITY price
  * Prize pool & claim interface
  * Social sharing

---

## 💼 Development Model

* Treasury-seeded LPs using redirect from burn destination
* dUSD-stable pool enables predictable value flow
* LP tokens held by Treasury — **recoups funding through usage**
* Dev team proposes OpenGov motions for continued funding as adoption grows

---

## 🧪 Project Layout

```
.
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

## 🧠 Goals

* MVP on Kusama Asset Hub EVM
* Stablecoin mint/redeem flow
* Oracle + prize logic fully tested
* Ultra-light frontend per birdbrain.lol design

---

## 🔮 Future Work

* Native asset version via Asset Hub precompiles
* Expand to new synthetic matchups (e.g. ETH vs BTC, SOL vs AVAX)
* Plug into decentralized oracles (e.g. DIA, Substrate-native)

---

## 📜 License

MIT — memeable, forkable, fundable by governance.

---

## 🤝 Contribute

Looking for:

* Solidity devs (with DODO/PMM interest)
* Frontend hackers (Next.js, Tailwind)
* Oracle runners
* dUSD evangelists
* Meme lords & KSM maxis

---

## TL;DR

PARITY is a synthetic, stablecoin-tradable token that lets you bet on **Kusama catching up to Polkadot** — with fees, liquidity, and final value all flowing **back to the KSM ecosystem**.



