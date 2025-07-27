# DeFi-Wallet-Risk-Scoring-Compound-Protocol-V2-V3-
This project builds a risk scoring model for Ethereum wallets interacting with the Compound V2/V3 DeFi lending protocols. It analyzes historical borrowing, repaying, and liquidation events using data from the Covalent API, and outputs a normalized risk score and level for each wallet.
📌 Problem Statement
Given a list of Ethereum wallet addresses, the goal is to:

Collect transaction data from Compound V2/V3 for each wallet.

Engineer risk-related features based on historical DeFi activity.

Assign a risk score (0–1000) and classify wallets into Low, Medium, or High risk categories.

Deliver a CSV output:


| wallet_id                              | risk_score | risk_level |
|----------------------------------------|------------|------------|
| 0xfaa0768bde629806739c3a4620656c5d26f44ef2 |   732      | Medium     |


🔧 Features Engineered
Feature	Description
total_borrow	Total borrowed amount
total_repay	Total repaid amount
borrow_repay_ratio	Ratio of borrowed vs. repaid amounts
num_borrows	Number of borrow transactions
num_repays	Number of repay transactions
num_liquidations	Number of liquidation events (high risk indicator)

🧪 Scoring Methods
Approach 1: Rule-Based Scoring (Raw Values)
Points assigned for:

Each liquidation event

High borrow-repay ratio

Borrowing without repayment

Score range: small integers (0–5+)

Risk levels:

High: ≥5

Medium: ≥3

Low: <3

Approach 2: Weighted Scoring with Normalization (Final Approach)
Originally attempted min-max normalization, but it failed to differentiate wallets (scores close to 0).

Final method uses log1p transformation to normalize skewed data.

Weighted score formula:

makefile
Copy
Edit
score = 0.4 * log_norm(total_borrow) + 
        0.4 * log_norm(borrow_repay_ratio) + 
        0.2 * log_norm(num_liquidations)
Final score scaled to 0–1000

🚀 How to Run
1. Clone the repository
bash
Copy
Edit
git clone https://github.com/your-username/defi-wallet-risk.git
cd defi-wallet-risk
2. Install requirements
bash
Copy
Edit
pip install -r requirements.txt

4. Add your Covalent API Key
Edit covalent_fetch.py and insert your API key:
COVALENT_API_KEY = "your_api_key_here"

6. Run the full pipeline

python main.py
The final output will be saved as wallet_risk_scores_1.csv.

📊 Sample Output
wallet_id	risk_score	risk_level
0xabc...	712	Medium
0xdef...	951	High
0xghi...	238	Low

📚 Justification for Risk Indicators
Borrow-repay ratio flags repayment discipline.

Liquidation count is a critical signal of failed collateral management.

Total borrow captures credit exposure.

This feature set balances interpretability, generalizability, and protocol-level risk coverage.

📈 Future Extensions
Apply the same model to other lending protocols (e.g., Aave, MakerDAO)

Add time-weighted event tracking

Use clustering or ML classifiers to group risk profiles

📄 License
This project is under the MIT License.

