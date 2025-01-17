# RIZ Reward System Documentation

## Overview

The RIZ Reward System is designed to incentivize providers who contribute in-house rAgent (RX, RC, RD, RE) or borrow rAgent on our platform. Providers are rewarded with **RIZ tokens**, our native cryptocurrency, which has a total supply of **5,000,000,000 RIZ** and an initial valuation of **$0.10 per RIZ**, equating to a total valuation of **$500,000,000**.

This system aims to:

1. **Encourage rAgent Contribution**: Providers can mint rAgent tokens (RC, RX, RD, RE) and be rewarded with RIZ tokens based on the action cost and fixed ETH price.
2. **Promote rAgent Utilization**: Borrowers who utilize rAgent are also rewarded in RIZ tokens, ensuring active participation in the ecosystem.
3. **Establish Market Liquidity**: An initial set of 1000 fake RC, RD, and RE tokens will be minted to bootstrap the lending market and foster liquidity.

The reward calculation mechanism ensures fairness and scalability, aligning incentives for both contributors and users. Below, we describe the reward structure, formulas, and examples in detail to provide a clear understanding of how rewards are calculated and distributed.

---

## RIZ Tokenomics

### Token Supply and Valuation
- **Total Supply**: 5,000,000,000 RIZ
- **Initial Price**: $0.10 per RIZ
- **Valuation**: $500,000,000
- **ETH Price**: $3300

---

## Reward Distribution Mechanism

### Mainnet Actions and Rewards

| Frequency     | Action                     | Action Cost | Profit | Fixed ETH Price | Reward Type | RIZ Reward Amount | # of Users | Total Tokens | Token %   | Description               |
|---------------|----------------------------|-------------|--------|-----------------|-------------|-------------------|------------|--------------|-----------|---------------------------|
| 1-time        | Mint RC                    | $0.40       | $0.40  | 0.00012         | RIZ         | 4                 | 100,000    | 400,000      | 0.008%    | One-time action           |
| 1-time        | Mint RX                    | $0.40       | $0.40  | 0.00012         | RIZ         | 4                 |            |              |           | One-time action           |
| 1-time        | Mint RD                    | $0.40       | $0.40  | 0.00012         | RIZ         | 4                 |            |              |           |                           |
| 1-time        | Mint RE                    | $0.40       | $0.40  | 0.00012         | RIZ         | 4                 |            |              |           |                           |
| Infinite      | Create & Name Agent (PAG) | $20.00      | $15.00 | 0.006           | RIZ         | 150               |            |              |           | Multiple                  |
| 1-time        | Borrow 1 month RC         | $10.00      | $5.00  | IN USDC         | RIZ         | 50                |            |              |           | Limited availability: 1000|
| 1-time        | Borrow 1 month RD         | $10.00      | $5.00  | IN USDC         | RIZ         | 50                |            |              |           | Limited availability: 1000|
| 1-time        | Borrow 1 month RE         | $20.00      | $10.00 | IN USDC         | RIZ         | 100               |            |              |           | Limited availability: 1000|

---

## Reward Calculation

### Reward Formula
#### For Minting Actions:
The reward amount for minting rAgent tokens (RC, RX, RD, RE) is calculated as follows:
```
Reward Amount (RIZ) = Reward Multiplier * Action Cost (USD) / Fixed ETH Price (ETH)
```

- **Action Cost**: The cost paid by the provider to mint a rAgent token, denominated in USD.
- **Fixed ETH Price**: The fixed conversion rate for 1 USD to ETH (e.g., 0.00012 ETH).
- The formula ensures that higher costs (in USD) result in proportionally higher rewards in RIZ tokens.

#### For Borrowing Actions:
The reward amount for borrowing rAgent is calculated based on a fixed reward structure:
```
Reward Amount (RIZ) = Fixed Reward (RIZ per Month) x Borrowing Duration (Months)
```

- **Fixed Reward**: A predefined amount of RIZ tokens awarded for each month of borrowing.
- **Borrowing Duration**: The number of months the rAgent is borrowed.

### Explanation of Reward Mechanism
1. **Minting Actions**:
   - Providers who mint tokens (RC, RX, RD, RE) are rewarded with RIZ tokens based on their contribution. The reward ensures fairness by directly correlating with the action cost in USD.

2. **Borrowing Actions**:
   - Borrowers are incentivized to utilize rAgent by receiving fixed RIZ rewards. This encourages long-term participation and rAgent usage.

### Example Calculations

#### Example 1: Minting RC
- **Action Cost**: $0.40
- **Fixed ETH Price**: 0.00012 ETH
\[
\text{Reward Amount (RIZ)} = \frac{0.40}{0.00012} = 4 \, \text{RIZ}
\]
The provider is rewarded with 4 RIZ tokens for minting one RC token.

#### Example 2: Borrowing RC for 1 Month
- **Fixed Reward**: 50 RIZ
- **Borrowing Duration**: 1 month
\[
\text{Reward Amount (RIZ)} = 50 \times 1 = 50 \, \text{RIZ}
\]
The borrower is rewarded with 50 RIZ tokens for borrowing one RC token for one month.

#### Example 3: Creating & Naming an Agent (PAG)
- **Action Cost**: $20.00
- **Fixed ETH Price**: 0.006 ETH
\[
\text{Reward Amount (RIZ)} = \frac{20.00}{0.006} = 150 \, \text{RIZ}
\]
The provider is rewarded with 150 RIZ tokens for creating and naming an agent.

---

## Additional Notes

1. **Reward Types**:
   - Actions such as minting RC, RX, RD, and RE are **one-time actions** rewarded with a fixed amount of RIZ.
   - Borrowing rewards are dependent on the duration and rAgent type.

2. **Action Cost**:
   - Action costs are additional payments in ETH or USDC, excluding transaction fees.

3. **Market Initialization**:
   - During the launch of the system, 1000 fake RC/RD/RE tokens will be created and distributed to the lending market for liquidity.

4. **Token Allocation**:
   - A total of 400,000 RIZ tokens (0.008% of supply) will be allocated for minting actions.

---

## Summary Table for Token Allocation

| Action Type          | Total RIZ Allocated | Token Percentage |
|----------------------|---------------------|------------------|
| Mint RC, RX, RD, RE | 400,000             | 0.008%          |
| Borrowing Actions    | Based on Availability | TBD             |