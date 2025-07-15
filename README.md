# 🏦 DeFi Wallet Credit Scoring — Aave V2 Protocol

This project assigns a **credit score (0–1000)** to DeFi wallets using transaction-level data from the Aave V2 protocol. It analyzes the historical behavior of users — such as deposits, borrows, repayments, liquidations — and generates a transparent, logic-based scoring system without using any machine learning models.

---

## 📌 Problem Statement

Given 100K+ Aave V2 transactions, build a robust, reproducible scoring logic to:

- Assign credit scores to each wallet (0 = risky, 1000 = highly responsible)
- Identify trustworthy vs exploitative users based on behavior
- Explain the scoring model for full transparency

---

## 📁 Dataset

- Format: `user-wallet-transactions.json` (~87MB)
- Each row contains:
  - `userWallet`
  - `timestamp`
  - `action` (deposit, borrow, repay, redeemunderlying, liquidationcall)
  - `actionData` (includes token, amount, price)

---

## ⚙️ Approach

### Step 1: Data Parsing
- Extracted `amount`, `token`, and `assetPriceUSD` from the nested `actionData`
- Normalized token values using appropriate decimals (e.g. 6 for USDC, 18 for DAI)

### Step 2: Feature Engineering per Wallet
- Total deposit, borrow, repay, redeem, liquidation (in USD)
- Number of transactions & unique actions
- Active days and transactions per day
- Repay-to-borrow and borrow-to-deposit ratios

### Step 3: Score Computation (0–1000)
Scoring logic based on normalized features:

```python
credit_score = 1000 * (
    0.4 * total_deposit_norm +
    0.4 * repay_ratio_norm +
    0.2 * tx_per_day_norm
)
