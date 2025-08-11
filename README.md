# Using machine learning in goals market
### Introduction
This project evaluates a machine learning-driven betting strategy for football matches in the Over/Under 2.5 goals market using data found in https://www.football-data.co.uk/. The goal is to determine whether a predictive model, when combined with bookmaker odds, can identify profitable value bets using expected value (EV) in Premier League matches.

Two main strategies are tested:

*A baseline approach that bets based solely on predicted probabilities.
*A refined value betting strategy that filters bets using EV thresholds to focus on situations with theoretical edge.

### Technical Overview
The model was trained using historical football match data up to the end of the 2023–24 season, including engineered features such as:

Team form (last 5 matches); Goal averages scored/conceded; Win rates
All features were carefully calculated to avoid data leakage so a walk-forward validation approach was implemented:
At each point in time, the model is retrained on all prior data, then used to predict the probability of Over 2.5 goals for the next match. The classifier used was XGBoost (XGBClassifier) with feature scaling applied via StandardScaler. The result is a file (walk_forward_test_set.csv) containing out-of-sample predictions for the entire 2024–25 season.

### 1. Baseline Strategy (No EV Filter)
In the initial approach:

*A bet was placed on Over if ProbOver2.5 > 0.5, and Under otherwise.
*A fixed stake of €100 per match was used.
*Every prediction was converted into a bet regardless of expected value.

Limitations:

Many bets were placed without edge. This led to inconsistent returns and validated the need to include market odds and value filtering. This version served as a baseline to compare against more refined strategies.

### 2. Value Betting with EV Thresholds
To improve results, I added expected value (EV) filtering.

For each match:

![EV Over formula](https://latex.codecogs.com/svg.image?\text{EV}_{\text{Over}}=\text{ProbOver2.5}\times\text{Odds}_{\text{Over2.5}})  

![EV Under formula](https://latex.codecogs.com/svg.image?\text{EV}_{\text{Under}}=(1-\text{ProbOver2.5})\times\text{Odds}_{\text{Under2.5}})



### The strategy:

Places a bet only if the EV of one outcome is above a given threshold (e.g., 1.05), and Chooses the side with the higher EV. We tested multiple thresholds and we recorded the performance metrics of the best one.
The threshold with the highest ROI was selected as the optimal strategy.

### Conclusion
After applying a machine learning-based approach to model the probability of Over 2.5 goals in football matches, and combining it with bookmaker odds, we evaluated the profitability of a value betting strategy based on expected value (EV). The use of a high EV threshold (1.50) allowed the strategy to avoid marginal bets and focus only on the most mispriced opportunities in the market.
