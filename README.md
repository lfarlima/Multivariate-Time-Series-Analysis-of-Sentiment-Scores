# Project-2

# Gathering data from Twitter and NewsApi


## Installs for Twint
pip install nest_asyncio

pip install --upgrade git+https://github.com/twintproject/twint.git@origin/master#egg=twint

## Imports
import twint
import nest_asyncio
nest_asyncio.apply()
c = twint.Config()

from newsapi import NewsApiClient

### Set API and Client
api_key = "0ef1c61926f54984abcca4338225fd66"
newsapi = NewsApiClient(api_key=api_key

## Pull data from NewsAPI
pltr_headlines = newsapi.get_everything(
    q="PLTR",
    language="en",
    page_size=10,
    sort_by="relevancy",
    from_param="2021-02-29"
)

pltr_newsapi_df = pd.DataFrame.from_dict(pltr_headlines["articles"])
pltr_newsapi_df.head()


## Pull Twitter data using Twint and save to CSV file
c.Search= "$DOT" or "DOT.X"
c.Since= "2020-09-29"
c.Until = '2021-04-04'
c.Limit= 1000
c.Lang= "en"
c.Store_csv= True
c.Output= "Search.csv"

twint.run.Search(c)

## Clean data 
df = pd.read_csv('Search.csv', encoding="utf-8-sig")
df= df[["id", "created_at", "tweet", "language"]]
df=df.loc[df["language"]=="en"]
df=df.rename(columns={"id": "ID", "created_at": "Date", "tweet": "Tweet"})
df=df.drop(["language"], axis=1)
df=df.set_index("Date")
df.shape

## Import local csv to colab
from google.colab import files
uploaded = files.upload()

## Create function to calculate sentiment based on compound score
def get_sentiment(score):
    """
    Calculates the sentiment based on the compound score.
    """
    result = 0  # Neutral by default
    if score >= 0.05:  # Positive
        result = 1
    elif score <= -0.05:  # Negative
        result = -1
    return result
    
## Create empty list for Twitter sentiments
pltr_twtr_sentiments = []

## Iterate through rows to analyze tweets 
for index, row in pltr_twitter_df.iterrows():
    try:
        text = row["Tweet"]
        date = row["Date"]
        sentiment = analyzer.polarity_scores(text)
        compound = sentiment["compound"]
        pos = sentiment["pos"]
        neu = sentiment["neu"]
        neg = sentiment["neg"]
        pol=  get_sentiment(compound)
        
        # Append data to list
        pltr_twtr_sentiments.append({
            "text": text,
            "date": date,
            "compound": compound,
            "positive": pos,
            "negative": neg,
            "neutral": neu,
            "Polarity Score":pol
            
        })
        
    except AttributeError:
        pass
        
## import pltr_twitter_sentiment_df csv to colab
pltr_twitter_sentiment_df = pd.DataFrame(pltr_twtr_sentiments)
pltr_twitter_sentiment_df.info()
