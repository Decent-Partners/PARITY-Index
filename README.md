# PARITY Index — Synthetic DOT:KSM Ratio Asset
### Kickstarting defi on Kusama Asset Hub through a tokenized rivalry between **Kusama & Polkadot**, expressed as a synthetic trading asset.

**WIP frontend**: [here](https://parity.birdbrain.lol)

PARITY is a synthetic ERC-20 token deployed on Kusama Asset Hub that tracks the market cap ratio of Polkadot (DOT) to Kusama (KSM). 
It’s designed to reward convergence — when Kusama gains ground on Polkadot, the PARITY token increases in value irrespective of absolute market performance from relative performance. 
A prize pool pays out to holders when the the two assets reach parity and the contest ends. 

If somebody buys PARITY at a premium it will sell PARITY tokens to the purchaser, it will take it out of the supply ensuring that the NAV of the project goes up. 

The design isolates relative value of two assets, from their market prices, creating a novel defi primitive. 

**The project is designe to be unruggable**: 

- Launch team seed initial liquidity (initial KSM + matching PARITY) and send LP tokens to the Kusama Treasury.

Later, they put proposal to the treasury to:

- Reclaim a small % of LP tokens and therefore KSM. 

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
| Quoted in  | bridged USDC/T from Polkadot AH              | 
| Frontend   | WIP — [birdbrain.lol](https://birdbrain.lol) |
| Signer     | Virto Connect (wallet linking & passkey support) |
| Status     | Prototype — contracts + UIs in development   |

---

## 🧠 Core Concepts

### ⚖️ What is PARITY?

* A synthetic token tracking the **DOT/KSM** market cap ratio
* Price increases when **Kusama closes the gap on Polkadot**
* Isolates **relative performance**, from absolute price movement
* **Prize Pool unlocks at 1:1** — holders can claim KSM-based payout
* **Trades vs stablecoin**, but ultimately **settled in KSM on Kusama AH**

---

## 🟡 Why This Benefits KSM Holders

PARITY is deployed on Kusama Asset Hub, settled in KSM, priced using the native dUSD stablecoin and is designed to kickstarts DeFi on Kusama Asset Hub:

* **Stablecoin-based Liquidity, KSM-based Settlement**: All minting, redemptions, and trading happen against the dUSD stablecoin to isolate DOT:KSM relative valuation. Behind the scenes, dUSD is a fully backed and regulated stablecoin issued by [Brale](https://brale.io). 
* **Initial LP seeded by KSM protocol**: The Kusama protocol seeds liquidity (using the [burn redirect](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama.dotters.network#/extrinsics/decode/0x2e00010100) made possible by [WFC-437](https://kusama.subsquare.io/referenda/437), aligning public funds with upside from PARITY adoption.
* **Fee Capture Flows Back to KSM Ecosystem**:

  * Mint/redeem fees collected in dUSD stablecoin
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

## 🧮 Net Asset Value (NAV) Mechanism

The PARITY token uses a synthetic supply model combined with DODO's Proactive Market Maker (PMM) to dynamically adjust its Net Asset Value (NAV) based on user demand.
⚙️ How It Works

    When users buy PARITY tokens above the oracle-defined price, the PMM contract mints new tokens to fulfill the order.

    The price premium paid by the user is removed from active circulating liquidity, rather than retained in the pool.

    This results in:

        📈 An increase in circulating token supply

        💧 A rise in protocol-owned value due to premium capture

        🧮 An effective increase in NAV per token, since more value is retained relative to the number of tokens in circulation

## 📈 Why This Design Matters

This mechanism creates a self-reinforcing dynamic:

    More demand → more premium captured → higher NAV → stronger incentive to hold

    Speculators who believe Kusama (KSM) will converge with Polkadot (DOT) in market cap can gain leveraged exposure to this outcome via PARITY

🪙 Example

    Let’s say 1 PARITY = $0.10 (based on the DOT:KSM ratio).

    A new buyer comes in and pays $0.11 per PARITY (a 10% premium).

    The protocol mints 1 PARITY and sends it to the buyer.

    The $0.11 in stablecoin is captured, but only $0.10 is needed to match the oracle price.

    The $0.01 premium is retained by the protocol, not paired with new PARITY.

    Result: there’s now $1.11 backing 10 PARITY tokens (instead of $1.00).

    → NAV = $0.111, up from $0.10.
    
---

## User Flow: KSM → PARITY Exposure via Stablecoin

╭────────────---╮
│   Kusama User │
╰─────┬──────---╯
      │
      ▼
╭────────────────────────────╮
│ Transfer KSM to Asset Hub │
│    → becomes xcKSM        │
╰────────┬──────────────────╯
         │
         ▼
╭────────────────────────────╮
│  Swap xcKSM for dUSD/USDC  │
│ (via local DEX on Asset Hub│
╰────────┬───────────────────╯
         │
         ▼
╭────────────────────────────╮
│  Interact with DODO PMM    │
│  PARITY / stablecoin pool  │
╰────────┬───────────────────╯
         │
         ▼
╭──────────────────────────────────────────────╮
│ User buys PARITY at premium vs oracle price  │
│ → New PARITY minted                          │
│ → Premium removed from pool       │
│ → PARITY sent to user                        │
╰────────┬─────────────────────────────────────╯
         │
         ▼
╭─────────────────────────────────────╮
│ Premium captured → NAV increases    │
│ → Protocol vault / prize pool grows │
╰─────────────────────────────────────╯

🧾 Summary

    User never needs to see PMM mechanics — just a clean buy/sell interface.

    Premiums are used to increase value per PARITY token, creating a reflexive incentive structure.

    KSM → Stablecoin → PARITY is the full onboarding arc.

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

🔄 Stablecoin Quotation and Bridging

    PARITY is quoted in bridged USDC or USDT (originating from Polkadot Asset Hub and bridged to Kusama Asset Hub) using a derivative token that anticipates native support via precompiles.

    This provides early access to trusted dollar-based pricing without waiting for stablecoin precompiles to be live.

---

🔐 Universal Signing with Virto Connect

    Virto Connect is the universal signer layer for PARITY on EVM-based Kusama Asset Hub.

    Users can link existing wallets (MetaMask, Nova, Talisman) or authenticate with passkeys — offering the most flexible UX for web2 and web3 users alike.

---

## 🎨 Frontend — [WIP](https://parity.birdbrain.lol)

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
* bridged USDC/T quoted against PARITY enables predictable value flow
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



