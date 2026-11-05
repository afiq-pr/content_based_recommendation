Content-Based Article Recommendation (TF-IDF, XGBoost and Hybrid)

Notebook: content-basedv3_hybrid.ipynb

How to run
- pip install -r requirements.txt
- Run all cells in the notebook
- Change data_folder in the notebook if the data is in another folder

1. Goal

Predict whether a user will view an article or not

The predicted probability is then used to rank articles for each user so the highest-scoring articles become the recommendations.

Three models are built and compared:

Model 1 Baseline: TF-IDF + cosine similarity
Pure content-based. Recommend articles whose text is similar to what the user read before.

Model 2 XGBoost
Learns from user behaviour like reading history, category preference, article popularity and demographics.

Model 3 Hybrid: XGBoost + TF-IDF similarity
Same as Model 2 but add the TF-IDF similarity score from Model 1 as one extra feature.

2. Data

- user_demographics.csv: user info (305 users)
- user_article_history.csv: which user saw which article and is_viewed (target)
- article_list.csv: article title, category and sentiment


3. Data Cleaning

- Missing article id (8 rows): removed because the number of missing is not many
- Missing sentiment: filled with "Unknown" so the articles are not lost
- Article id not found in article list: removed because no title to use
- Duplicates with same user, article and time: 918 rows are removed
- Education level: many spellings binning into simple levels
- Class balance: Arounf 60% viewed, 40% not viewed. Small difference so no balancing needed


4. Features

- Only use data before the current row, so the model cannot see the answer
- Used: article age, user past views, user favourite category and sentiment, article popularity, gender, race, education
- Not used: time, city, occupation, user id, article id


5. Train and Test Split

- For each user, sort by time
- First 80% is train, last 20% is test
- Not random split because the model should learn from the past and predict the future of the user


6. Models

- Baseline: compare article text with what the user read before. Simple starting point
- XGBoost: learn from user and article behaviour. Good for this kind of table data
- Hybrid: XGBoost plus the text similarity from baseline. Same settings as XGBoost so it is fair to compare


7. Results

- Baseline: F1 0.32, ROC-AUC 0.82. Misses most views
- XGBoost: F1 0.95, ROC-AUC 0.985. Much better
- Hybrid: F1 0.96, ROC-AUC 0.989. Best, chosen as final model
