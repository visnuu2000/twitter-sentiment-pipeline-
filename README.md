# Everything-Proof Sentiment Analyzer

A robust, production-ready Python pipeline that cleans and analyzes sentiment in Twitter data using NLP. Designed with **defensive programming** to run seamlessly across all operating systems.

## Key Features
* **Path Independence:** Uses `os.path` to automatically detect file locations.
* **Data Preprocessing:** Implements Regex to strip URLs and noise for higher accuracy.
* **Smart Sampling:** Efficiently handles large datasets without crashing memory.

## How to Run
1. **Clone the repo:** `git clone https://github.com/visnuu2000/twitter-sentiment-pipeline.git`
2. **Install dependencies:** `pip install pandas matplotlib textblob`
3. **Execute:** `python twitter.py`

## Lessons Learned & Technical Challenges
### The "Dirty Data" Problem
Real-world Twitter data is filled with noise. I implemented a custom **Regex** cleaning function to strip non-essential characters, improving the reliability of the TextBlob scores.

### Defensive Programming
I avoided hard-coded file paths by using the `os` module. This ensures the script is "plug-and-play" for any user on any OS.

---
