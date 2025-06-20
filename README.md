# PARITY Index — Synthetic DOT:KSM Ratio Asset on Kusama Asset Hub

> WIP frontend: [birdbrain.lol](https://birdbrain.lol)

A tokenized rivalry between Kusama & Polkadot.

**PARITY** is a synthetic ERC-20 token deployed on Kusama Asset Hub that tracks the **market cap ratio of Polkadot (DOT) to Kusama (KSM)**. It’s designed to reward convergence — when Kusama gains ground on Polkadot, the PARITY token increases in value.

---

## ✨ Overview

| Property      | Detail                         |
|---------------|--------------------------------|
| **Token**     | PARITY                         |
| **Mechanism** | Synthetic PMM (Proactive Market Maker) |
| **Chain**     | Kusama Asset Hub (EVM)         |
| **Settles In**| KSM                            |
| **Frontend**  | [WIP](https://parity.birdbrain.lol) |
| **Signer**    | [Virto connect](https://demo.virto.dev/) |
| **Status**    | Prototype — contracts + UI in development |

---

## 🧠 Core Concepts

### ⚖️ What is PARITY?

- A synthetic token tracking the **DOT/KSM market cap ratio**
- Isolates **relative** performance rather than absolute market movement
- Price increases when **KSM closes the valuation gap** with DOT
- One-time **Prize Pool** payout to holders if KSM reaches 1:1 with DOT
- Settled in KSM, indexed by oracle-fed market cap ratio

---

## 🟡 Why This Benefits KSM Holders

PARITY is built entirely on Kusama Asset Hub, settles in KSM, and directs fees and value to the Kusama ecosystem:

- KSM-Denominated Settlement: All trading, minting, and redemptions are in KSM, increasing utility and demand for the token.
- Treasury-Aligned LPs: Initial liquidity positions are owned by the Kusama Treasury, giving the network direct upside as PARITY adoption grows.
- Fee Capture in KSM: Both mint/redeem fees and PMM slippage are collected in KSM, creating recurring revenue streams for funding governance proposals or further development.
- Ecosystem Narrative: Positions KSM as a serious contender to DOT, while creating a fun, market-driven way for users to speculate on Kusama’s comeback.

The more PARITY is used and traded, the more value accrues to KSM holders and the Kusama Treasury.

---

## 📊 Parity Ratio Metrics

| Metric           | Value                          |
|------------------|-------------------------------|
| Current Ratio    | 0.04 (KSM is 4% of DOT’s mcap) |
| Delta            | +428.18bps (4.28%)             |
| Sensitivity      | ±10% Price Movement @ Κ=0.8    |

> Small changes in the ratio = significant price movements due to bonding curve.

---

## 🧮 How PMM Works

Modeled on [DODO’s Proactive Market Maker](https://dodoex.github.io/docs/docs/protocol/introduction/):

- Anchors price to DOT/KSM ratio via on-chain oracle
- Uses a **tunable Κ parameter** to shape bonding curve steepness
- Supports **single-sided liquidity**
- Allows price drift and arbitrage correction
- **Developer-aligned revenue** through mint/redeem + slippage

### PMM Formula

```

Price(x) = P0 \* (1 + Κ \* (x / R))

````

Where:

- **P0** = Oracle price (DOT/KSM ratio)
- **Κ** = Curvature (e.g., 0.8)
- **x** = Trade size
- **R** = Pool reserve size

At Κ = 0.8, the curve is responsive but allows meaningful arbitrage opportunities.

---

## 💸 Fee Mechanics

PARITY includes two transparent, usage-driven revenue streams:

### 1. Mint/Redeem Fee

- **0.25%** fee on all mints/redemptions
- Collected in KSM
- Sent to dev multisig for maintenance, oracle ops, and frontend upkeep

```solidity
function mint(uint256 amountIn) external {
    uint256 fee = (amountIn * 25) / 10000; // 0.25%
    uint256 netAmount = amountIn - fee;
    // proceed with minting PARITY using netAmount
}
````

### 2. PMM Slippage Capture

* A portion of trade slippage (e.g. **0.5%**) is routed to the dev treasury
* Happens when trading away from oracle price
* Non-inflationary, driven purely by usage

---

## 🏆 The Prize Pool

* One-time **KSM denominated pool**
* Unlocks when **DOT/KSM ≤ 1.0**
* PARITY holders can **burn tokens for pro-rata payout**

### Claim Logic

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

| Contract        | Role                                      |
| --------------- | ----------------------------------------- |
| `PARITYToken`   | ERC-20 token tracking DOT\:KSM ratio      |
| `OracleFeed`    | Fetches and updates market cap ratio      |
| `ParityPMMPool` | Custom PMM pool for trading PARITY <> KSM |
| `PrizePool`     | Holds and distributes prize at 1:1 parity |

---

## 🔗 Oracle Feed

* Off-chain script using CoinGecko API
* Computes `KSM_mcap / DOT_mcap`
* Pushes to `OracleFeed.sol`
* Referenced by both PMM & PrizePool contracts

---

## 🎨 Frontend

Initial guide here [birdbrain.lol](https://birdbrain.lol)

> Simple, light, fast, unserious. Big numbers, few buttons. Clean data UX.

### Features:

* Real-time DOT/KSM ratio
* Live PARITY price
* Mint / Redeem interface
* Prize Pool balance + Claim UI
* Memeable toggle modes
* Social sharing of price updates. 

---

## 💼 Development Model

* **Initial liquidity** seeded by targeting [Kusama burn destination](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama.dotters.network#/extrinsics/decode/0x2e00010100).  
* **LP tokens sent to Kusama Treasury**
* Team submits **funding proposal** via OpenGov. 
* Treasury exposure to upside, community-aligned governance

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

* MVP deployed on Kusama Asset Hub EVM
* Real-time mint/redeem with KSM
* Oracle updates & prize logic tested
* Compliant with [birdbrain.lol](https://birdbrain.lol) style guide

---

## 🔮 Future Work

* Upgrade to native Kusama assets via precompiles
* Expand to new synthetic indexes (e.g. ETH vs BTC, SOL vs AVAX)
* Use decentralized oracles (e.g. DIA, Substrate-native feeds)

---

## 📜 License

MIT — open to forks, memes, and governance proposals.

---

## 🤝 Contribute

We’re looking for:

* Solidity devs (contracts + tests)
* Frontend engineers (Next.js + Tailwind)
* Designers / UX tinkerers
* Oracle runners + data nerds
* Meme Lords & KSM degens

---

**About:**

PARITY is a synthetic tokenized bet on Kusama closing the gap on Polkadot — and rewarding those who back the underdog.





