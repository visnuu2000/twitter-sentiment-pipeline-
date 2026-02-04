import pandas as pd
import os
import re
import matplotlib.pyplot as plt
from textblob import TextBlob

script_dir = os.path.dirname(os.path.abspath(__file__))
file_path = os.path.join(script_dir, "twitter_training.csv")

def run_project():
    if not os.path.exists(file_path):
        print(f"ERROR: 'twitter_training.csv' not found in {script_dir}")
        print("Please ensure the CSV file is in the same folder as this script.")
        return

    print("Loading dataset...")
    cols = ['id', 'topic', 'sentiment_label', 'text']
    df = pd.read_csv(file_path, names=cols)

    print("Cleaning data...")
    df = df.dropna(subset=['text'])
    df = df.sample(1000, random_state=42)

    def clean_text(text):
        text = re.sub(r'@[A-Za-z0-9_]+', '', str(text))
        text = re.sub(r'https?://[A-Za-z0-9./]+', '', text)
        text = re.sub(r'[^a-zA-Z\s]', '', text)
        return text.strip().lower()

    df['clean_text'] = df['text'].apply(clean_text)

    print("Analyzing sentiment...")
    def get_blob_sentiment(text):
        score = TextBlob(text).sentiment.polarity
        if score > 0:
            return 'Positive'
        elif score < 0:
            return 'Negative'
        else:
            return 'Neutral'

    df['AI_Sentiment'] = df['clean_text'].apply(get_blob_sentiment)

    output_file = os.path.join(script_dir, "analyzed_results.csv")
    df.to_csv(output_file, index=False)
    print(f"Results saved to: {output_file}")

    plt.figure(figsize=(10, 6))
    sentiment_counts = df['AI_Sentiment'].value_counts()
    colors = ['#66b3ff', '#99ff99', '#ff9999']

    sentiment_counts.plot(kind='pie', autopct='%1.1f%%', colors=colors, startangle=140)
    plt.title('Public Sentiment Analysis (Twitter Entity Dataset)')
    plt.ylabel('')
    plt.show()

if __name__ == "__main__":
    run_project()
