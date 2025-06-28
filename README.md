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

## ✨ Summary

**PARITY** is a synthetic ERC-20 token deployed on Kusama Asset Hub that tracks the market cap ratio of Kusama (KSM) to Polkadot (DOT). It enables speculation on the relative performance of the two ecosystems, with active backing by wrapped KSM and DOT reserves (v1). PARITY is redeemable at any time for its share of the protocol’s holdings, including wrapped KSM and DOT.

The protocol proactively uses premiums from PARITY purchases to acquire more wrapped KSM and DOT, building a deeper, more liquid reserve aligned with the oracle-reported ratio. DODO-style Proactive Market Maker (PMM) contracts manage liquidity for PARITY/dUSD, KSM:dUSD, and DOT:dUSD pools, while wrapped tokens enable smart contract composability in v1. 

Oracle-driven rebalancing ensures the DOT:KSM ratio is maintained. 

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
| **Redemption**      | PARITY → Wrapped KSM & DOT & dUSD (v1); Native KSM, DOT and dUSD (v2)              |
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

### Smart Contracts

The smart contracts below support the protocol, including proactive premium allocation to wrapped KSM and DOT reserves. These are designed for v1, with v2 expected to adapt to native precompiles. Click to expand each contract. *Note*: These are best viewed on GitHub, where `<details>` tags render as collapsible sections.

<details>
<summary>View WrappedAsset.sol</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract WrappedAsset is Ownable {
    string public name;
    string public symbol;
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    IERC20 public nativeAsset; // Native KSM, DOT, or dUSD
    uint256 public reserve; // Native assets held
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Deposit(address indexed user, uint256 nativeAmount, uint256 wrappedAmount);
    event Withdraw(address indexed user, uint256 wrappedAmount, uint256 nativeAmount);

    constructor(string memory _name, string memory _symbol, address _nativeAsset) {
        name = _name;
        symbol = _symbol;
        nativeAsset = IERC20(_nativeAsset);
    }

    function deposit(uint256 amount) external {
        require(amount > 0, "Invalid amount");
        require(nativeAsset.transferFrom(msg.sender, address(this), amount), "Transfer failed");
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        reserve += amount;
        emit Deposit(msg.sender, amount, amount);
        emit Transfer(address(0), msg.sender, amount);
    }

    function withdraw(uint256 amount) external {
        require(amount > 0, "Invalid amount");
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        reserve -= amount;
        require(nativeAsset.transfer(msg.sender, amount), "Transfer failed");
        emit Withdraw(msg.sender, amount, amount);
        emit Transfer(msg.sender, address(0), amount);
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transfer(address to, uint256 amount) external returns (bool) {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) external returns (bool) {
        require(balanceOf[from] >= amount, "Insufficient balance");
        require(allowance[from][msg.sender] >= amount, "Insufficient allowance");
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        allowance[from][msg.sender] -= amount;
        emit Transfer(from, to, amount);
        return true;
    }
}

```

# PARITY Protocol

## 🔍 Contract Viewer
- [View `BackingVault.sol`](./contracts/BackingVault.sol)
- [View `MintRedeem.sol`](./contracts/MintRedeem.sol)
- [View `ParityPMMPool.sol`](./contracts/ParityPMMPool.sol)

---

## ⚖️ Core Concepts

### What is PARITY?

- A synthetic token tracking the **DOT:KSM market cap ratio**.
- Backed by **wrapped KSM and DOT** (v1), with minimal `wDUSD` from fees or holding; **native KSM and DOT** in v2.
- **Price increases** as KSM outperforms DOT.
- Redeemable at **Net Asset Value (NAV)** for backing assets.
- Transparent, **non-custodial**, and **unruggable** by design.

---

## 🧪 Net Asset Value (NAV)

```

NAV = (Total Wrapped KSM & DOT Held + Vaulted wDUSD) ÷ PARITY Supply (v1)
OR
(Total Native KSM & DOT Held) ÷ PARITY Supply (v2)

```

- In v1: `wDUSD` (including premiums) is used to purchase wrapped KSM/DOT via PMM pools at oracle ratio.
- In v2: native assets used directly.
- Premiums boost NAV by buying more backing without increasing supply.
- Redeeming returns pro-rata share of KSM/DOT (`+wDUSD` in v1).

**Example:**
```

Oracle ratio = 10 (DOT = \$10B, KSM = \$1B)
Oracle prices: KSM = \$20, DOT = \$80
User mints 1 PARITY with \$11 wDUSD
→ \$10 used to buy 0.0833 wDOT + 0.1665 wKSM
→ \$1 premium buys extra backing in same ratio
→ NAV = \$11
→ Redeeming returns \~0.0875 wDOT + \~0.1748 wKSM

```

---

## 🔁 Dynamic Rebalancing

- Maintains ratio by:
  - Buying backing with all `wDUSD` (v1) or native (v2)
  - Redeeming PARITY for backing assets
  - Periodically rebalancing if ratio shifts >1%

**Example:**  
If ratio drops to 8, protocol sells wDOT for wKSM (or DOT for KSM in v2).

---

## 📈 Market Mechanics: PMM

DODO-style PMM contracts manage:
- `PARITY/dUSD`, `KSM/dUSD`, and `DOT/dUSD` with single-sided liquidity

```

Price(x) = P₀ × (1 + Κ × (x / R))
Where:
P₀ = Oracle price
Κ = 0.8 (curvature constant)
x = Trade size
R = Pool reserve (wDUSD)

````

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

| Action                  | Effect on Supply | Effect on Pool Value | Effect on NAV     |
|------------------------|------------------|-----------------------|-------------------|
| Buy at premium         | ↑ Increases      | ↑ Increases           | ↑ Increases       |
| Sell back to pool      | ↓ Decreases      | ↓ Decreases           | Neutral / ↓       |
| Hold (no action)       | -                | -                     | ↑ (if others buy) |

---

## 💸 Fees & Revenue

- **Mint/Redeem Fee**: 0.25% (in wDUSD) → Treasury  
```solidity
uint256 fee = (amountIn * 25) / 10000;
````

* **Slippage Fee**: \~0.5% for off-oracle trades → Treasury
* **LP Fee Capture**: LP tokens go to Treasury, earn fees
* **Premiums**: 100% used to buy backing assets → ↑ NAV

---

## 🔐 Virto Connect Integration

* SSO for Kusama PVM ecosystem
* Wallet support: MetaMask, Nova, Talisman, WebAuthn
* Seamless UX across web2/web3

---

## 🔗 Oracle & Stablecoin Support

* **Oracle**: CoinGecko API → on-chain every 5 min, 24h TWAP

  * DIA integration coming
* **Stablecoin**: `wDUSD` in v1, `dUSD` (native) in v2

  * Bridged USDC fallback
* ⚠️ Oracle reliability is critical — decentralised options in roadmap

---

## 🧠 Project Goals

✅ Launch MVP on Kusama Asset Hub
🚧 Oracle-driven minting with premium allocation
🚧 Dynamic BackingVault rebalancing
🚧 PMM pools for all pairs
🚧 Basic frontend for mint/redeem/trade
🚧 Governance for Treasury LP management

---

## 🔮 Where does PARITY go?

* **PARITY v2**: Native precompile migration post-Kusama upgrade
* Decentralised oracle (DIA)
* * New synthetic matchups (e.g. ETH\:BTC and then other rivalries).

---

## 🧑‍💻 Development Directory

```
contracts/
├── PARITYToken.sol
├── WrappedAsset.sol
├── OracleFeed.sol
├── ParityPMMPool.sol
├── BackingVault.sol
└── MintRedeem.sol

frontend/
├── public/
├── pages/
└── components/

scripts/
└── updateOracle.ts

README.md
```

---

## 📜 License

**MIT** — Memeable. Forkable. Fundable.

```

