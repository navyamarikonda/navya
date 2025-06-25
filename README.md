pip install tweepy nltk textblob
import tweepy
import nltk
from textblob import TextBlob

# Authenticate to the Twitter API (you need API keys from your Twitter Developer account)
consumer_key = 'your_consumer_key'
consumer_secret = 'your_consumer_secret'
access_token = 'your_access_token'
access_token_secret = 'your_access_token_secret'

# Set up Tweepy authorization
auth = tweepy.OAuth1UserHandler(consumer_key, consumer_secret, access_token, access_token_secret)
api = tweepy.API(auth)

# Fetch tweets containing the keyword "Apple"
public_tweets = api.search_tweets('Apple', count=100, lang='en')

# Step 1: Preprocess the tweets (e.g., remove URLs, mentions)
tweets = [tweet.text for tweet in public_tweets]
cleaned_tweets = [tweet.replace('http', '').replace('@', '') for tweet in tweets]

# Step 2: Perform sentiment analysis using TextBlob
nltk.download('vader_lexicon')  # Optional for more advanced analysis
sentiments = []

for tweet in cleaned_tweets:
    analysis = TextBlob(tweet)
    sentiment = analysis.sentiment.polarity  # Sentiment polarity: -1 (negative) to 1 (positive)
    sentiments.append(sentiment)

# Classify sentiment
sentiment_labels = ['positive' if s > 0 else 'negative' if s < 0 else 'neutral' for s in sentiments]

# Step 3: Display results
for tweet, sentiment in zip(cleaned_tweets, sentiment_labels):
    print(f"Tweet: {tweet}\nSentiment: {sentiment}\n")

# Optionally, create a simple sentiment distribution plot
import matplotlib.pyplot as plt
plt.hist(sentiment_labels, bins=3, edgecolor='black')
plt.title("Sentiment Analysis of Tweets")
plt.xlabel('Sentiment')
plt.ylabel('Frequency')
plt.show()
