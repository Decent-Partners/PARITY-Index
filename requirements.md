## PARITY - Requirements

PARITY is an asset backed meme token that tracks the relative market caps of Kusama and Polkadot expressed as a ratio.

The Net Asset Value (NAV) at any moment = Total KSM & DOT held ÷ PARITY supply.

## User journeys 

Users buy newly minted PARITY from the protocol with a stablecoin (dUSD). 

An additional premium is added to the purchase price e.g. 10%.

This increases the supply of PARITY.

The protocol swaps dUSD income for KSM and DOT to maintain PARITY at the targeted market cap price ratio. 

The premium is used to buy additional KSM or DOT whilst still maintaining the peg to the market cap ratio of Kusama and Polkadot.

This means that as more PARITY is bought by users and minted by the protocol, the NAV of the token gradually increases rewarding early holders.

When a user wants to sell PARITY this also includes a premium that is again added to the NAV. 

The protocol burns PARITY reducing the supply of tokens and the user redeems the underlying KSM and DOT in proportion to their holdings.

Over time as PARITY is bought/minted and sold/burned the NAV increases rewarding early holders.

Protocol fees generated are used to burn KSM by sending to the new [Dark Hole Pallet](https://kusama.subsquare.io/referenda/539) reducing the circulating supply of KSM.

This burning of KSM feeds back into the Kusama vs Polkadot narrative and increases demand for PARITY which buys more KSM and sells DOT (if market cap ratio reduces) and also increases the NAV, which then creates more fomo, this then generates more fees, which burns more KSM which increases the meme of Kusama catching Polkadot in a recursive design.  

---

Note the original version of PARITY was an entirely synthetic token, but the asset backed iteration creates a more powerful incentive mechanism and adds trust to the system rather than being purely algorithmic. 

---

## Prioritisation

The following is prioritised using the [MoSCoW method](https://en.wikipedia.org/wiki/MoSCoW_method). Must have, Should have, Could have, Won't have. 

### Must have

- Smart contracts on Kusama Asset Hub
- PARITY token (ERC20)
- Trust minimised oracle to track market cap ratio of Kusama and Polkadot.
- Web3 login via Polkadot JS, Nova, Talisman, Metamask.  
- dUSD stablecoin for purchases of PARITY.
- PARITY:dUSD, KSM:dUSD, DOT:dUSD pairs.
- Protocol owned reserve of KSM + DOT. 
- Fees sent to [Dark Hole pallet](https://kusama.subsquare.io/referenda/539) to burn KSM and reduce supply. 
- Trust minimised oracle to track market cap ratio of Kusama and Polkadot.
- Retro style interface [see](https://parity.birdbrain.lol).
- UI to enable users to buy (mint) PARITY and sell (burn) PARITY.
 - Selling PARITY enables the holder to redeem the underling KSM+DOT in proportion to their holdings.
- Basic UI with chart showing current and historical ratio.
- Documentation

### Should have 

- Login with Virto (universal login).
- Basic analytics / data
 - Token supply
 - Minted/Burned
 - NAV
 - Protocol income
 - KSM burned  

### Could have 

- Rivalry related media + publishing within UI 
- User generated + PARITY protocol indexed via [Feather Protocol](https://github.com/Decent-Partners/Feather-Protocol)  
 - Bullish posts
 - Data comparisons
 - NAV
 - Protocol income
 - KSM burned 

### Won't have 

- Social links
- Developer contact



