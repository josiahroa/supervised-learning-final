# Problem Description

## Context

League of Legends (Summoner's Rift) is a competitive team-based strategy game where two teams of five players compete to destroy the opposing team's base. Match outcomes depend on various factors including player skill, team composition, strategy, and in-game performance. While post-game statistics clearly correlate with victory, predicting match outcomes using only pre-game information presents a significant challenge.

## Problem Statement

Is it possible to predict which team will win a League of Legends match using only pre-game player statistics available before the game starts? This is a binary classification problem where the target variable is the match outcome (Win/Loss) for a given team.

## Available Features

The following are available stats from the data that are visible before a game begins.

- **Summoner Level**: Player account level indicating overall experience (games played). It's important to note that this level can be increased outside of the normal Summoner's Rift game mode.

- **Solo Queue Rank**: A visble competitive rank (Iron through Challenger) with LP (League Points)

- **Historical Performance**: Win/loss records for previous games

- **Champion Mastery**: Experience and familiarity with their selected champion for the game.

## Key Challenges

1. Missing Post-Game Information: Win factors like team coordination, strategic exection, champion composition synergy, and actual in-game performance are unavailable.

2. Matchmaking balance: The game's matchmaking system intentionally creates balances matches, making both teams statistically similar.

3. Individual vs Team Performance: Player statistics don't capture team dynamics or how well players work together.

## Expected Outcome

Due to these inherint limitations, model accuracy could be in the range of 55-70%, which is slightly better than random guessing (50%). This reflects the reality that pre-game statistics provide limited insight when matchmaking has already balanced the teams.

## Research Objective

Determine wheter a machine learning model can extract meaningful predictive patterns from pre-game player statistics and identify which features contribute most to match outcomes. The project will compare multiple supervised learning approaches (Logistic Regression, Random Forest, and SVC) to find the optimal model.

## Success Criteria

- Model accuracy consistently above 60%

- Identification of most predictive pre-game features

- Understanding of why certain models perform better for this problem

# EDA

## Data Quality Checks

1. Defined by our problem description, we only want to look at data for games that happened in the Summoners Rift game mode. This is classified in this data by the game_mode column - where `CLASSIC` represents the Summoners Rift mode.

2. In League of Legends, each game has 10 players, split between 2 teams of 5 players. It is important to our problem description that each row's game_id (player in a game) can be grouped with exactly 9 other rows (10 total) to make up a complete game. It is also important that we check that in the data that each team for a game has exactly 5 players.

3. The problem description states that the player's rank will be used to help predict a Win or Loss. In order to clean the data up to support this, we will want to remove any games where ALL players for that game are unranked (solo_rank = NaN). Later, we can look at solutions for how we want to handle games where 9 or less players are unranked.

4. Convert solo_tier and solo_rank into numerical values.

## Conclusions from Feature Distribution Analysis

### Feature Correlation Heatmap:

- Note that solo_wins and solo_losses are highly correlated. This is expected and should consider merging into a single data point that makes more sense.

### Overall Distribution Patterns (Histograms):

- **Summoner Level**: Displays a consistent distribution of summoner levels up to 500 before quickly dropping.

- **Solo Wins & Losses**: Both features show similar distributions with most players having 10-40 games played in the current data. The similar shapes between wins and losses indicate balanced matchmaking creating ~50% win rates across the player base. This also shows that using wins vs losses alone is not useful and that we should later create a W/L ratio to per player.

- **Champion Mastery Points**: Displays an extreme trend towards low champion mastery. This indicates that a lot of players aren't playing champions that they have a lot of experience in for most games.

### Win/Loss Comparison Analysis:

Individual player statistics show almost no impact for game outcomes. The blue (Win) and orange (Loss) distributions are nearly identical across all features:

- **Summoner Level**: Perfect overlap - players of similar experience levels are matched together regardless of outcome.

- **Solo Wins/Losses**: Overlapping patterns confirm matchmaking creates balanced games based solely off player stats.

- **Champion Mastery**: No difference - champion familiarity alone doesn't determine wins at the individual level.

### Implications for Model Development:

This analysis shows that some additional feature engineering is required before modeling. Player stats individually have almost no effect on Win/Loss, but aggregating the players composing a team and creating comparative features should provide a stronger prediction.

# Feature Engineering

1. The data defines solo_tiers and solo_ranks by their name in the game. This should be normalized to numeric values so that our model can access them. The total rank of the player is defined by their solo_rank, solo_tier, and solo_lp. Each tier is offset by 400lp and each rank is offset by 100lp. For example: Diamond IV 50LP is 400LP ahead of Emerald IV 50LP. After normalizing all ranks, verify that all rows have a solo_rank_normalized so that we can drop the unecessary rank columns from the data.

2. Create a performance indicator utilizing solo_wins and solo_losses that captures historical player success independent of games played. Players who haven't played any games before their first match should be assigned a 50% win rate to avoid division by zero and represent unknown performance.

3. Transform individual player statistics into team-level metrics - this is because game outcomes are more dependent on team performance and not individuals.

4. Create more predictive features by capturing relative team strength differences rather than absolute values. These comparative features capture competitive imbalance that the matchmaking system couldn't perfectly equalize. This comparative features become a great candidate for far more predictive functionality in the model.

# Conclusion

This project successfully demonstrated that pre-game player statistics can predict League of Legends match outcomes with an accuracy of 65%-70%. This was achieved with the Support Vector Machine model.

## Model Performance

### SVC (65% - 70%)

The Support Vector Machine model was the best performer due to its ability to handle the non-linear decision boundaries that appear in team-based matchmaking data. The RBF kernel enabled the model to capture complex interactions between features that linear models can't detect.

### Logistic Regression (55% - 60%)

Logistic Regression performed reasonably well, and in some cases, met the success criteria. Its performance was lmited by linear decision boundary assumptions with non-linear data. However, further investigation with polynomial feature engineering could have a a substantial enough improvement.

### Random Forest (50% - 59%)

While Random Forest is supposed to handle complex relationships, it underperformed for this problem. This occurred because of high feature correlation with mean statistics vs differences. The ensemble averaging may have reduced the impact of subtle patterns that individual strong learners capture. This dataset only had 7 continuous numerical features with overlapping information, which limits the model from excelling.

## Key Insights

Feature engineering was critial. Transforming individual players stats into team-level aggregates and relative differences greatly improved model accuracy from EDA.

Matchmaking creates a ceiling. The games' matchmaking system intentionally creates balanced matches, making the resulting accuracy of 60% or greater a strong result. The remaining 34% error is likely due to factors unavailable preg-game.

Non-linearity matters: SVC's kernel based approach was essentioal for this problem due to the slight feature advantages that don't linearly translate to wins but interact in complex ways.

## Success Critera Achievement

We achieved the success criteria by creating a model with an accuracy above 60%, identified most predictive features, and understood model performance for the problem.
