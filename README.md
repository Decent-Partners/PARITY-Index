## PARITY Index

### A Synthetic DOT:KSM Ratio Asset

A tokenised rivalry between Kusama & Polkadot.

---

### ✨ Summary

PARITY is a synthetic ERC-20 token deployed on **Kusama Asset Hub** that tracks the **market cap ratio of KSM to DOT**. It allows users to speculate on the convergence of the two ecosystems.

Unlike passive tokens, PARITY is **actively backed** by KSM and DOT, and can be **redeemed at any time** for its fair share of the protocol’s holdings. The protocol dynamically adjusts its backing by **purchasing KSM and DOT** based on a live oracle feed. When users buy PARITY at a premium, the excess dUSD is captured by the protocol `Vault`, increasing the Net Asset Value (NAV) per PARITY token.

- 📈 **Price rises** as Kusama gains ground on Polkadot (whether KSM or DOT go up or down).
- 💵 **PARITY is fully backed** by KSM and DOT held by the protocol Reserve.
- 🔁 **Redeemable 1:1** against backing assets based on current market cap ratio and vaulted dUSD.
- 🧮 **Oracle-driven rebalancing** ensures the protocol tracks the DOT:KSM ratio over time.
- 🧰 Initial stablecoin liquidity is seeded by creators; LP tokens are sent to the Treasury to capture trading fees.

This synthetic asset isolates **relative performance** from absolute price movement, offering a novel DeFi primitive for ratio-based speculation.

---

### ⚙️ Mechanics Overview

| Property         | Details                            |
| ---------------- | ---------------------------------- |
| Token            | `PARITY` (ERC-20)                  |
| Price Reference  | Market cap ratio: DOT / KSM        |
| Backing Assets   | KSM + DOT + vaulted dUSD           |
| Trading Pair     | PARITY/dUSD                        |
| Chain            | Kusama Asset Hub (PVM)             |
| Login            | [Virto Connect](https://demo.virto.dev/) universal login      |
| Market Mechanism | Proactive Market Maker (PMM) with premium capture       |
| Oracle Feed      | Off-chain script (CoinGecko API) + on-chain Oracle |
| Redemption       | PARITY → KSM & DOT & dUSD (on demand)     |
| Frontend         | [WIP](https://parity.birdbrain.lol)                        |

---

### ⚖️ Core Concepts

#### What is PARITY?

- A synthetic token that tracks the **DOT:KSM market cap ratio**.
- Backed by a basket of **KSM and DOT**, plus vaulted dUSD from premium purchases.
- Price increases as **KSM outperforms DOT**.
- Fully **redeemable at NAV** (Net Asset Value), including KSM, DOT, and dUSD.
- Designed to be transparent, non-custodial, and unruggable.

---

### 🧪 Net Asset Value (NAV)

**NAV = (Total KSM & DOT Held + Vaulted dUSD) ÷ PARITY Supply**

- When users mint PARITY, dUSD is used by the protocol to **purchase KSM and DOT** in the appropriate proportions based on the oracle-reported DOT:KSM ratio.
- If PARITY is purchased at a premium (e.g., $0.11 vs. $0.10 oracle price), the excess ($0.01) is sent to a protocol owned reserve account, increasing NAV without minting additional PARITY.
- The resulting KSM and DOT are held in the `BackingVault`, and vaulted dUSD is stored separately.
- When users redeem PARITY, they receive an equivalent share of the backing KSM, DOT, and vaulted dUSD.

#### Example:

- Oracle ratio = DOT market cap / KSM market cap = 10 (e.g., DOT = $10B, KSM = $1B).
- Oracle prices: KSM = $20, DOT = $80.
- User mints 1 PARITY with $11 dUSD, but oracle price is $10.
- Protocol uses $10 to buy $6.67 DOT (0.0833 DOT) and $3.33 KSM (0.1665 KSM), sends $1 to the protocol.
- BackingVault holds 0.0833 DOT and 0.1665 KSM, protocol holds $1 dUSD.
- NAV = ($6.67 DOT + $3.33 KSM + $1 dUSD) ÷ 1 PARITY = $11.
- Redeeming 1 PARITY returns 0.0833 DOT, 0.1665 KSM, and $1 dUSD.

---

### 🔁 Dynamic Rebalancing

To maintain alignment with the oracle ratio, the protocol:

1. **Buys KSM and DOT** using incoming dUSD at the current ratio, via a Kusama DEX (e.g., Karura).
2. **Allows redemption** of PARITY for backing KSM, DOT, and vaulted dUSD.
3. Periodically **rebalances holdings** by swapping between KSM and DOT if the oracle ratio shifts significantly (e.g., >1% deviation).
   - Example: If ratio drops to 8, protocol sells DOT for KSM to adjust reserve to 8:1 value ratio.
   - Funded by vaulted dUSD or fees.

---

### 📈 Market Mechanics: PMM

The Proactive Market Maker (PMM) offers low-slippage trading and dynamic pricing:

```math
Price(x) = P₀ × (1 + Κ × (x / R))
```

Where:
- `P₀`: Oracle price (DOT/KSM ratio, normalized to dUSD).
- `Κ`: Curvature constant (0.8).
- `x`: Trade size.
- `R`: Pool reserve (dUSD).

**Premium Capture**:
- When a user buys PARITY at a premium (e.g., $0.11 vs. $0.10 oracle price), the PMM:
  - Mints PARITY at the oracle price ($0.10).
  - Captures the full $0.11, sending $0.01 to the Treasury.
  - Uses $0.10 to buy KSM and DOT for the BackingVault.
- Result: NAV increases as vaulted dUSD grows without inflating PARITY supply.

**Key Benefits**:
- Single-sided liquidity support.
- Oracle-anchored dynamic pricing.
- Facilitates mint/redemption and arbitrage.
- Premium capture rewards early holders.

#### NAV Dynamics
| Action                     | Effect on Supply | Effect on Pool Value | Effect on NAV |
|----------------------------|------------------|----------------------|---------------|
| Buying PARITY at premium   | ↑ Increases      | ↑ Increases          | ↑ Increases   |
| Selling PARITY back to pool| ↓ Decreases      | ↓ Decreases          | Neutral/↓     |
| No trade (HODLing)         | -                | -                    | ↑ (if others buy) |

---

### 💸 Fees & Revenue

1. **Mint/Redeem Fee**
   - 0.25% collected in dUSD.
   - Sent to Kusama Treasury.
   ```solidity
   uint256 fee = (amountIn * 25) / 10000;
   ```

2. **Slippage Capture**
   - ~0.5% on trades deviating from the oracle price.
   - Non-inflationary, sent to Treasury.

3. **LP Fee Capture**
   - All LP tokens from initial liquidity are sent to the Treasury.
   - Treasury earns ongoing trading fees from dUSD/PARITY pair.

4. **Premium Capture**
   - Excess dUSD from premium purchases sent to Treasury, increasing NAV.

---

### 🔗 Oracle & Stablecoin Support

- **Oracle Feed**: Uses CoinGecko API to fetch live market cap data, pushed on-chain every 5 minutes. Future integration with decentralized oracles (e.g., DIA).
- **Stablecoin**: dUSD (fully collateralized), used for minting and trading. Supports bridged USDC as fallback.
- **Smoothing**: 24-hour TWAP to reduce volatility.

---

### 🔐 Virto Connect Integration

- Single-sign-on for Kusama’s EVM ecosystem.
- Supports MetaMask, Nova, Talisman, and WebAuthn (passkeys).
- Designed for seamless UX across web2 and web3 users.

---

### 🧱 Smart Contract Architecture

| Contract         | Role                                      |
| ---------------- | ----------------------------------------- |
| `PARITYToken.sol`    | ERC-20 token tracking DOT:KSM            |
| `OracleFeed.sol`     | Off-chain ratio feed pushed to chain      |
| `ParityPMMPool.sol`  | dUSD/PARITY market using PMM with premium capture |
| `BackingVault.sol`   | Stores KSM, DOT, and vaulted dUSD underlying each PARITY |
| `MintRedeem.sol`     | Handles minting/redemption vs underlying  |

---

### 🧠 Project Goals

- ✅ Launch MVP on Kusama Asset Hub.
- ✅ Oracle and asset-backed minting flow with premium capture.
- ✅ Dynamic rebalancing logic for BackingVault.
- ✅ Basic frontend for mint/redeem/trade.
- 🚧 Add governance hook for Treasury-owned LP.

---

### 🔮 What’s Next?

- Native DOT bridging and redemption.
- New synthetic matchups (e.g., ETH:BTC, SOL:AVAX).
- Decentralized oracle integration (e.g., DIA, Substrate-native).

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

---

### Technical Implementation
Below are the updated smart contracts for `BackingVault.sol` and `MintRedeem.sol`, aligning with the premium capture, reserve backing, and redemption mechanics. `ParityPMMPool.sol` is also included to handle PMM trading and premium capture.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract BackingVault is Ownable {
    IERC20 public ksmToken;
    IERC20 public dotToken;
    IERC20 public dUsd;
    address public mintRedeem;
    address public treasury;
    
    uint256 public vaultedDusd; // Premiums stored here

    event AssetsAdded(uint256 ksmAmount, uint256 dotAmount, uint256 dusdAmount);
    event AssetsWithdrawn(uint256 ksmAmount, uint256 dotAmount, uint256 dusdAmount);

    constructor(
        address _ksmToken,
        address _dotToken,
        address _dUsd,
        address _treasury
    ) {
        ksmToken = IERC20(_ksmToken);
        dotToken = IERC20(_dotToken);
        dUsd = IERC20(_dUsd);
        treasury = _treasury;
    }

    modifier onlyMintRedeem() {
        require(msg.sender == mintRedeem, "Only MintRedeem");
        _;
    }

    function setMintRedeem(address _mintRedeem) external onlyOwner {
        mintRedeem = _mintRedeem;
    }

    // Add KSM, DOT, and dUSD to vault
    function addAssets(uint256 ksmAmount, uint256 dotAmount, uint256 dusdAmount) external onlyMintRedeem {
        if (ksmAmount > 0) {
            require(ksmToken.transferFrom(msg.sender, address(this), ksmAmount), "KSM transfer failed");
        }
        if (dotAmount > 0) {
            require(dotToken.transferFrom(msg.sender, address(this), dotAmount), "DOT transfer failed");
        }
        if (dusdAmount > 0) {
            vaultedDusd += dusdAmount;
            require(dUsd.transferFrom(msg.sender, address(this), dusdAmount), "dUSD transfer failed");
            require(dUsd.transfer(treasury, dusdAmount / 2), "Treasury transfer failed"); // 50% to Treasury
        }
        emit AssetsAdded(ksmAmount, dotAmount, dusdAmount);
    }

    // Withdraw KSM, DOT, and dUSD for redemption
    function withdrawAssets(
        address to,
        uint256 ksmAmount,
        uint256 dotAmount,
        uint256 dusdAmount
    ) external onlyMintRedeem {
        if (ksmAmount > 0) {
            require(ksmToken.transfer(to, ksmAmount), "KSM transfer failed");
        }
        if (dotAmount > 0) {
            require(dotToken.transfer(to, dotAmount), "DOT transfer failed");
        }
        if (dusdAmount > 0) {
            require(vaultedDusd >= dusdAmount, "Insufficient dUSD");
            vaultedDusd -= dusdAmount;
            require(dUsd.transfer(to, dusdAmount), "dUSD transfer failed");
        }
        emit AssetsWithdrawn(ksmAmount, dotAmount, dusdAmount);
    }

    // Get vault balances
    function getBalances() external view returns (uint256 ksmBalance, uint256 dotBalance, uint256 dusdBalance) {
        return (ksmToken.balanceOf(address(this)), dotToken.balanceOf(address(this)), vaultedDusd);
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "./PARITYToken.sol";
import "./OracleFeed.sol";
import "./BackingVault.sol";

contract MintRedeem is Ownable {
    PARITYToken public parityToken;
    IERC20 public ksmToken;
    IERC20 public dotToken;
    IERC20 public dUsd;
    OracleFeed public oracle;
    BackingVault public vault;
    address public treasury;
    
    uint256 public constant FEE_BASIS_POINTS = 25; // 0.25%
    uint256 public constant BPS_DENOMINATOR = 10000;

    event Mint(address indexed user, uint256 parityAmount, uint256 dUsdAmount, uint256 premium);
    event Redeem(address indexed user, uint256 parityAmount, uint256 ksmAmount, uint256 dotAmount, uint256 dusdAmount);

    constructor(
        address _parityToken,
        address _ksmToken,
        address _dotToken,
        address _dUsd,
        address _oracle,
        address _vault,
        address _treasury
    ) {
        parityToken = PARITYToken(_parityToken);
        ksmToken = IERC20(_ksmToken);
        dotToken = IERC20(_dotToken);
        dUsd = IERC20(_dUsd);
        oracle = OracleFeed(_oracle);
        vault = BackingVault(_vault);
        treasury = _treasury;
    }

    // Mint PARITY with dUSD, handle premium
    function mint(uint256 dUsdAmount, uint256 marketPrice) external {
        require(dUsdAmount > 0, "Invalid amount");
        (uint256 ratio, uint256 ksmPrice, uint256 dotPrice, uint256 oraclePrice) = oracle.getRatioAndPrices();
        
        // Calculate PARITY to mint at oracle price
        uint256 parityAmount = (dUsdAmount * 1e18) / marketPrice;
        uint256 oracleValue = (parityAmount * oraclePrice) / 1e18;
        uint256 premium = dUsdAmount > oracleValue ? dUsdAmount - oracleValue : 0;
        
        // Apply mint fee
        uint256 fee = (oracleValue * FEE_BASIS_POINTS) / BPS_DENOMINATOR;
        uint256 netValue = oracleValue - fee;

        // Calculate KSM and DOT to buy
        uint256 ksmValue = netValue * 100 / (100 + ratio);
        uint256 dotValue = netValue - ksmValue;
        uint256 ksmAmount = (ksmValue * 1e18) / ksmPrice;
        uint256 dotAmount = (dotValue * 1e18) / dotPrice;

        // Transfer dUSD from user
        require(dUsd.transferFrom(msg.sender, address(this), dUsdAmount), "dUSD transfer failed");
        require(dUsd.transfer(treasury, fee), "Fee transfer failed");
        
        // Add assets to vault (simplified: assume DEX purchase by ParityPMMPool)
        require(ksmToken.transferFrom(msg.sender, address(vault), ksmAmount), "KSM transfer failed");
        require(dotToken.transferFrom(msg.sender, address(vault), dotAmount), "DOT transfer failed");
        if (premium > 0) {
            vault.addAssets(0, 0, premium);
        }

        // Mint PARITY
        parityToken.mint(msg.sender, parityAmount);

        emit Mint(msg.sender, parityAmount, dUsdAmount, premium);
    }

    // Redeem PARITY for KSM, DOT, and dUSD
    function redeem(uint256 parityAmount) external {
        require(parityAmount > 0, "Invalid amount");
        require(parityToken.balanceOf(msg.sender) >= parityAmount, "Insufficient PARITY");

        uint256 totalSupply = parityToken.totalSupply();
        (uint256 ksmBalance, uint256 dotBalance, uint256 dusdBalance) = vault.getBalances();

        // Calculate pro-rata payouts
        uint256 ksmAmount = (ksmBalance * parityAmount) / totalSupply;
        uint256 dotAmount = (dotBalance * parityAmount) / totalSupply;
        uint256 dusdAmount = (dusdBalance * parityAmount) / totalSupply;

        // Apply redemption fee
        uint256 fee = (parityAmount * FEE_BASIS_POINTS) / BPS_DENOMINATOR;
        require(dUsd.transferFrom(msg.sender, treasury, fee), "Fee transfer failed");

        // Burn PARITY
        parityToken.burnFrom(msg.sender, parityAmount);

        // Withdraw assets
        vault.withdrawAssets(msg.sender, ksmAmount, dotAmount, dusdAmount);

        emit Redeem(msg.sender, parityAmount, ksmAmount, dotAmount, dusdAmount);
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "./PARITYToken.sol";
import "./OracleFeed.sol";
import "./BackingVault.sol";
import "./MintRedeem.sol";

contract ParityPMMPool is Ownable {
    PARITYToken public parityToken;
    IERC20 public dUsd;
    OracleFeed public oracle;
    BackingVault public vault;
    MintRedeem public mintRedeem;
    address public treasury;
    
    uint256 public constant KAPPA = 8e17; // 0.8
    uint256 public constant SLIPPAGE_FEE_BPS = 50; // 0.5%
    uint256 public constant BPS_DENOMINATOR = 10000;
    uint256 public reserve; // dUSD reserve

    event Trade(address indexed user, bool isBuy, uint256 parityAmount, uint256 dUsdAmount, uint256 premium);

    constructor(
        address _parityToken,
        address _dUsd,
        address _oracle,
        address _vault,
        address _mintRedeem,
        address _treasury
    ) {
        parityToken = PARITYToken(_parityToken);
        dUsd = IERC20(_dUsd);
        oracle = OracleFeed(_oracle);
        vault = BackingVault(_vault);
        mintRedeem = MintRedeem(_mintRedeem);
        treasury = _treasury;
    }

    // Buy PARITY with dUSD
    function buy(uint256 dUsdAmount) external {
        require(dUsdAmount > 0, "Invalid amount");
        (, , , uint256 oraclePrice) = oracle.getRatioAndPrices();

        // Calculate market price using PMM
        uint256 marketPrice = calculatePrice(dUsdAmount);
        uint256 parityAmount = (dUsdAmount * 1e18) / marketPrice;
        uint256 oracleValue = (parityAmount * oraclePrice) / 1e18;
        uint256 premium = dUsdAmount > oracleValue ? dUsdAmount - oracleValue : 0;
        
        // Apply slippage fee
        uint256 slippageFee = (dUsdAmount * SLIPPAGE_FEE_BPS) / BPS_DENOMINATOR;
        uint256 netAmount = dUsdAmount - slippageFee;

        // Transfer dUSD
        require(dUsd.transferFrom(msg.sender, address(this), dUsdAmount), "dUSD transfer failed");
        require(dUsd.transfer(treasury, slippageFee), "Fee transfer failed");

        // Mint PARITY via MintRedeem
        mintRedeem.mint(netAmount, marketPrice);

        emit Trade(msg.sender, true, parityAmount, dUsdAmount, premium);
    }

    // Sell PARITY for dUSD
    function sell(uint256 parityAmount) external {
        require(parityAmount > 0, "Invalid amount");
        (, , , uint256 oraclePrice) = oracle.getRatioAndPrices();

        // Calculate market price
        uint256 marketPrice = calculatePrice(parityAmount);
        uint256 dUsdAmount = (parityAmount * marketPrice) / 1e18;
        
        // Apply slippage fee
        uint256 slippageFee = (dUsdAmount * SLIPPAGE_FEE_BPS) / BPS_DENOMINATOR;
        uint256 netAmount = dUsdAmount - slippageFee;

        // Transfer PARITY and dUSD
        require(parityToken.transferFrom(msg.sender, address(this), parityAmount), "PARITY transfer failed");
        require(dUsd.transfer(msg.sender, netAmount), "dUSD transfer failed");
        require(dUsd.transfer(treasury, slippageFee), "Fee transfer failed");

        // Burn PARITY
        parityToken.burn(parityAmount);

        emit Trade(msg.sender, false, parityAmount, dUsdAmount, 0);
    }

    // PMM pricing function
    function calculatePrice(uint256 tradeSize) public view returns (uint256) {
        (, , , uint256 oraclePrice) = oracle.getRatioAndPrices();
        return oraclePrice * (1e18 + (KAPPA * tradeSize) / reserve) / 1e18;
    }

    // Add liquidity (for initial seeding)
    function addLiquidity(uint256 dUsdAmount) external onlyOwner {
        require(dUsd.transferFrom(msg.sender, address(this), dUsdAmount), "dUSD transfer failed");
        reserve += dUsdAmount;
    }
}
```
