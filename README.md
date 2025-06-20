# PARITY Index — Synthetic DOT\:KSM Ratio Asset on Kusama Asset Hub

**WIP**: [birdbrain.lol](https://birdbrain.lol)

**KSM vs DOT** — a tokenized rivalry between Kusama & Polkadot

**PARITY** is a synthetic ERC-20 token that tracks the market cap ratio between Polkadot (DOT) and Kusama (KSM). It’s designed to reward convergence — i.e., when Kusama gains ground on Polkadot in relative valuation, the PARITY token price increases.

PARITY also includes a one-time Prize Pool for holders if Kusama ever reaches full parity (1:1 market cap with Polkadot).

> **Note:** PARITY is **settled against KSM**, aligning incentives with the Kusama network and ensuring that all value is ultimately expressed in the native chain token.

> **Team Allocation:** A small percentage of the initial PARITY token supply will be reserved for the team to fund ongoing development, maintenance, and ecosystem growth. This allocation will be transparently documented at launch and subject to a vesting schedule.

---

## ✨ Overview

* **Token**: PARITY
* **Mechanism**: Synthetic PMM (Proactive Market Maker)
* **Chain**: Kusama Asset Hub (EVM)
* **Frontend Style**: [birdbrain.lol](https://birdbrain.lol) (lightweight, irreverent, clean)
* **Status**: Prototype stage, Solidity contracts and frontend in development

---

## 🧠 Core Concepts

### ⚖️ What is PARITY?

* A synthetic token that tracks the DOT/KSM market cap ratio
* Isolates **relative** performance from **absolute** performance
* Increases in value when **Kusama outperforms DOT**, regardless of broader market moves
* Rewards holders as Kusama closes the valuation gap
* Prize Pool pays out pro-rata to all holders if **KSM = DOT** in market cap

---

## 📊 Parity Ratio Metrics

| Metric          | Value                          |
| --------------- | ------------------------------ |
| Current Ratio   | 0.04 (KSM is 4% of DOT's mcap) |
| Change (Δ)      | +428.18bps (4.28%)             |
| PMM Sensitivity | ΔPrice ≈ ±10% at Κ=0.8         |

> **Key Insight**: The bonding curve allows significant price movement with small shifts in DOT/KSM ratio, especially as Κ nears 1.

---

## 🧮 How PMM Works

Based on DODO's Proactive Market Maker model:

* Anchors price to the DOT\:KSM ratio via an oracle
* Uses a tunable **Κ** parameter for curve steepness
* Allows **single-sided liquidity** → reduces impermanent loss

**PMM Price Formula**

```
Price(x) = P0 * (1 + Κ * (x / R))
```

Where:

* `P0`: Oracle price (DOT/KSM ratio)
* `Κ`: Curvature sensitivity (e.g., 0.8)
* `x`: Trade amount
* `R`: Reserve size

---

## 💰 The Prize Pool

### 🏆 Reward Mechanism

* Pool is funded in **dUSD or KSM**
* If DOT/KSM ratio hits **1.0 or lower**, pool unlocks
* PARITY holders can **burn** tokens to claim their share

### ✅ Example Claim Logic

```solidity
function claimPrize() external {
    require(isParityReached(), "Not yet at parity");
    uint256 userBalance = parityToken.balanceOf(msg.sender);
    uint256 totalSupply = parityToken.totalSupply();
    uint256 payout = (prizePoolBalance * userBalance) / totalSupply;
    parityToken.burnFrom(msg.sender, userBalance);
    dUSD.transfer(msg.sender, payout);
}
```

---

## 🧱 Smart Contracts

| Contract      | Role                                           |
| ------------- | ---------------------------------------------- |
| PARITYToken   | ERC-20 synthetic token tracking DOT\:KSM ratio |
| dUSDToken     | Stablecoin backing the pool (optional)         |
| OracleFeed    | Push-updated DOT/KSM ratio                     |
| ParityPMMPool | Custom bonding curve, κ-tuned                  |
| PrizePool     | Escrow for endgame prize payout                |

---

## 🧠 Oracle Feed

* Off-chain script fetches DOT & KSM market caps (e.g., CoinGecko API)
* Computes: `KSM_MC / DOT_MC`
* Updates `OracleFeed.sol` contract
* Used by both **PMM** and **PrizePool** logic

---

## 🎨 Design + Frontend

* See [birdbrain.lol](https://birdbrain.lol)

**Guidelines:**

* Minimalist layout
* Easy to understand
* Big numbers, few buttons
* Unserious tone for serious mechanics

**Focus:**

* Real-time DOT/KSM ratio
* Live token price
* Prize Pool balance
* Mint / Redeem UX
* Claim UI once parity is hit

---

## 🧪 Project Layout

```
.
├── contracts/
│   ├── PARITYToken.sol
│   ├── dUSDToken.sol
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

* Deliver MVP on **Kusama Asset Hub EVM**
* Enable mint/redeem with **dUSD** or equivalent
* Fully implement and test **PrizePool** mechanic
* Align with **birdbrain.lol**'s visual tone

---

## 🔮 Future Work

* Upgrade to native Kusama assets once precompiles are live
* Add index products for other network pairs
* Use decentralized oracles (e.g., DIA, Substrate-native relayers)

---

## 📜 License

**MIT** — open to collaboration.

---

## 🤝 Contribute

Open to:

* Solidity developers
* Frontend devs (Next.js + Tailwind)
* Visual and UX designers
* Meme lords



