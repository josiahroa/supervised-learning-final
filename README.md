# supervised-learning-final

My final project for CU Boulder's Introduction to Machine Learning: Supervised Learning course.

## Project Instructions:

1. Gather data, determine the method of data collection and provenance of the data
   Select a data source and a problem

2. Identify a Supervised Machine Learning Problem

3. Exploratory Data Analysis (EDA) — Inspect, Visualize and Clean the Data
   Go through the initial data cleaning and EDA and judge whether you need to collect more or different data.

   EDA Procedure:

   - Describe the factors or components that make up the dataset. For each factor, use a box-plot, scatter plot, histogram, etc., to describe the data distribution as appropiate.
   - Describe correlations between different factors of the dataset and justify your assumption that they are correlated or not correlated.
   - Determine if any data needs to be transformed.
   - Using your hypothesis, indicate if it's likely that you should transform data, such as using a log transform or other transformation of the dataset.
   - Determine if the data has outliers or needs to be cleaned in any way. Are there missing data values for specific factors? How will you handle the data cleaning? Will you discard, interpolate or otherwise substitue data values?
   - If you believe that specific factors will be more important than others in your anaylsis, you should mention which and why.

4. Perform Analysis Using Supervised Machine Learning Models of your Choice, Present Discussion and Conclusions
   Perform classification or regression. Use multiple models if necessary to compare and show understanding of why specific models work better than the other or what limitations or cautions specific models may have.

## Step 1

### Gather Data

Data Source: https://www.kaggle.com/datasets/jakubkrasuski/league-of-legends-match-dataset-2025?resource=download

### Identify Problem

Identify which team wins based on the level, solo rank, w/l history, and selected chamption mastery of the players involved after champion selection.

### Clean Data

- Keep = [summoner_level, lane, solo_rank, solo_lp, solo_wins, solo_losses, champion_master_level, champion_mastery_points]
- Only use game_mode CLASSIC

- There are multiple players per match. I will need to aggregate their data into team-level features.
- solo_tier, solo_rank, and solo_lp should be normalized
