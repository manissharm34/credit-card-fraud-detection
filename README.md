# credit-card-fraud-detection
End-to-end fraud detection using Logistic Regression, Random Forest and XGBoost | Python | Kaggle

What This Project Is About
Every time you tap your card, the bank has milliseconds to decide — is this you, or a fraudster?
This project builds and compares three machine learning models to automatically detect fraudulent credit card transactions. Using 284,807 real transactions from European cardholders, the analysis finds patterns that separate fraud from legitimate spending — and tests which model catches the most fraud most reliably.
This is not just a model — it's a complete analysis with:

Exploratory Data Analysis (7 charts)
3 machine learning models trained and compared
Business-ready Excel report with KPI dashboard
Real-world context explaining how banks actually use this


The Dataset
DetailValueSourceKaggle — Credit Card Fraud DetectionTotal transactions284,807Fraud cases492 (0.17%)Time period48 hours — European cardholders, September 2013FeaturesTime, Amount, V1–V28, Class

Why V1–V28?
The original transaction details (location, device, merchant type etc.) were anonymised using PCA — Principal Component Analysis — to protect cardholder privacy. V1–V28 are compressed versions of those original features. The fraud patterns are preserved but personal data is hidden.


Real World Use Case
This type of model runs in production at every major bank and payment company in the world.
Customer taps card
        ↓
Transaction data sent to bank's system (milliseconds)
        ↓
Fraud model scores it: probability 0.00 to 1.00
        ↓
Score < 0.30  →  Approved instantly
Score 0.30–0.70  →  Extra verification (OTP / call)
Score > 0.70  →  Blocked, customer alerted
        ↓
Customer confirms or disputes
        ↓
Outcome fed back to retrain and improve model
Who uses models like this:

Banks (HDFC, Barclays, SBI, Axis) — real-time transaction screening
Payment networks (Visa, Mastercard, RuPay) — cross-network fraud monitoring
Payment apps (Paytm, PhonePe, GPay) — UPI fraud detection
E-commerce (Amazon, Flipkart) — blocking fraudulent orders before dispatch
Credit bureaus (Equifax, CIBIL) — monitoring account behaviour patterns


The Three Models — What Each One Does
1. Logistic Regression
The simplest and fastest model. It looks at all features together and draws a straight line to separate fraud from legit. Like a weighing scale — if the combined weight of all signals tips past a threshold, it calls it fraud.
Strength  → Fast, easy to explain, good baseline
Weakness  → Assumes fraud and legit can be separated by a straight line
                (real data is rarely that clean)
Our result → AUC ~0.97
In plain English: Think of it as one expert analyst who looks at all the signals together and gives a single yes/no verdict. Quick but not the most sophisticated.

2. Random Forest
Builds 50 different decision trees — each one asking different yes/no questions about the transaction — then takes a majority vote.
Tree 1: Is V14 below -3? → Fraud
Tree 2: Is Amount below 2 AND Hour between 0-6? → Fraud
Tree 3: Is V17 below -5 AND Amount below 50? → Fraud
...50 trees vote...
Final answer: majority wins
Strength  → Handles complex patterns, resistant to noise
Weakness  → Slower to train than Logistic Regression
Our result → AUC ~0.97
In plain English: Like asking 50 different fraud investigators to independently review the same transaction and vote. Much more reliable than one person.

3. XGBoost (eXtreme Gradient Boosting)
The most powerful of the three. Instead of building trees independently, XGBoost builds them sequentially — each new tree specifically learns from the mistakes the previous trees made.
Round 1: Tree 1 makes predictions → gets some wrong
Round 2: Tree 2 focuses on fixing Round 1's mistakes
Round 3: Tree 3 focuses on fixing Round 2's mistakes
...continues for 50+ rounds...
Final answer: all trees combined
Strength  → Most accurate on tabular data, handles imbalanced data well
Weakness  → More complex, harder to explain to non-technical people
Our result → AUC ~0.98
In plain English: Like a student who reviews every wrong answer before each test and specifically practises those weak areas. Gets smarter with every round.

Why ROC-AUC and Not Accuracy?
This is the most important concept in this project.
If you built a model that predicted every single transaction as legitimate, it would be 99.83% accurate — because 99.83% of transactions actually are legitimate.
But that model would catch zero fraud. It would be completely useless.
This is why we use ROC-AUC (Area Under the ROC Curve):
0.50 = Random guessing (flipping a coin)
0.80 = Decent model
0.95 = Strong model
0.97+ = Our models → excellent
1.00 = Perfect model (doesn't exist in practice)
ROC-AUC measures how well the model separates fraud from legit across all possible thresholds — not just at 50%.

Key Findings
FindingDetailFraud rate0.17% — only 492 out of 284,807Fraud median amount$9.25 — card testing behaviourLegit median amount$22.00Zero-amount fraud5.5% of fraud is €0 — pure card testingPeak fraud hour2 AM — lowest monitoring periodTop fraud signalsV14, V17, V12Best modelXGBoost — AUC ~0.98
Pattern 1 — Card Testing (€0–€10)
Fraudsters make tiny transactions first to check if a stolen card is active. The fraud median of $9.25 reveals this — half of all fraud transactions are below $9.25. Real customers don't make €0 purchases.
Pattern 2 — Late Night Timing
Fraud spikes between midnight and 6 AM when transaction volumes are lowest and real-time monitoring is reduced. Fraudsters deliberately choose these hours.
Pattern 3 — V14 as Key Signal
When V14 drops below -3, the fraud rate jumps from 0.01% to 17% — 170x the average. This feature likely captures device or location anomalies based on how strongly it separates fraud from legit.

Model Comparison Results
ModelROC-AUCBest ForLogistic Regression~0.97Baseline, fast, explainableRandom Forest~0.97Balanced accuracy and speedXGBoost~0.98Highest accuracy
All three significantly outperform random guessing (0.50). XGBoost edges ahead — consistent with published research showing XGBoost achieving 0.989 AUC on this dataset.

Project Structure
credit-card-fraud-detection/
│
├── README.md                         ← You are here
├── fraud_analysis_v2_kaggle.py       ← Main code (3 models + 7 charts)
├── Credit_Fraud_Analysis.xlsx        ← Business Excel report
└── charts/
    ├── chart1_class_distribution.png
    ├── chart2_amount_distribution.png
    ├── chart3_fraud_by_hour.png
    ├── chart4_feature_correlations.png
    ├── chart5_roc_curves.png
    ├── chart6_confusion_matrices.png
    └── chart7_feature_importance.png

How to Run
On Kaggle (Recommended — no setup needed)

Go to kaggle.com → New Notebook
Add the Credit Card Fraud Detection dataset from the right panel
Paste fraud_analysis_v2_kaggle.py into a code cell
Run all → all 7 charts display inline

On Local Machine
bashpip install pandas matplotlib seaborn scikit-learn xgboost openpyxl
python fraud_analysis_v2_kaggle.py
Change the path in pd.read_csv() to your local CSV location.

Tools Used
ToolPurposepandasData loading, cleaning, filtering, groupbymatplotlibAll chart creation — 7 custom dark-theme chartsseabornConfusion matrix heatmapsscikit-learnLogistic Regression, Random Forest, metricsxgboostXGBoost modelopenpyxlBusiness Excel report

Learning aspects

How to handle class imbalance (0.17% fraud) using stratified sampling
Why ROC-AUC beats Accuracy as a metric for imbalanced datasets
How three different model types approach the same problem differently
How to analyse PCA-anonymised features without knowing their original meaning
How to present technical findings as business insights — not just numbers
How fraud patterns translate to real banking decisions
