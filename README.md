# Trending Topic Detection on Social Media

Mini project for **Social Media Analysis**. Detects which hashtags / keywords are *suddenly* becoming popular in a
stream of tweet-like posts, using burst detection on hourly time-series, plus TF-IDF + KMeans topic clustering.

## Pipeline
1. **Load** tweets (`timestamp, user, text`) – synthetic data included, any CSV with `timestamp,text` works
2. **Preprocess** – lowercase, remove URLs/@mentions/punctuation/stopwords
3. **Term extraction** – hashtags, unigrams, bigrams (counted once per tweet)
4. **Time-series** – mentions per hour for every term
5. **Burst detection** – `z = (count_now − mean_prev24h) / (std_prev24h + 1)`; terms with z > 3 and ≥10 mentions are trends
6. **Rank & report** – trending hashtags with related keywords, trending keywords, charts
7. **Topic clustering** – TF-IDF + KMeans groups tweets into themes

## Run
```bash
pip install pandas numpy matplotlib scikit-learn
python generate_dataset.py        # creates data/tweets.csv (9k tweets, 7 days, 4 injected events)
python trend_detector.py          # options: --input file.csv --top 10 --window 24
```
Outputs in `outputs/`: `trending_report.csv`, `trending_keywords.csv`, `topic_clusters.csv`, `trend_timeline.png`, `top_trends.png`.

## Result on the sample data
All 4 injected events were recovered, ranked by burst strength: #indiavsaus (25.5), #cyclonealert (11.8),
#diwalisale (7.2), #airegulation (6.2) – while everyday chatter (#coffee, #traffic ...) was not flagged, because
its volume stays steady (high volume ≠ trending; *sudden growth* = trending).

## Files
`generate_dataset.py` · `trend_detector.py` · `data/tweets.csv` · `outputs/`
