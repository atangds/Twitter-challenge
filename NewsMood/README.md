
# NewsMood Analysis

* The sentiment score for an individual tweet varies with no clear patterns for all media sources.

* There are a considerable amount of tweets that score zero (neutral).

* The average scores by media source show BBC being most negative, and Fox being most positive. Since only 100 most recent tweets were collected for each media source, the result would be very different depending on when the analysis is performed.


```python
import tweepy
from config import consumer_key, consumer_secret, access_token, access_token_secret
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
from datetime import datetime
import pytz
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
analyzer = SentimentIntensityAnalyzer()
```


```python
outlets = ["@BBCWorld", "@CBSNews", "@CNN", "@FoxNews", "@NYTimes"]
```


```python
account = []
source = []
text = []
time = []
comp = []
pos = []
neg = []
neu = []
id = []
for outlet in outlets:
    tweets = api.user_timeline(outlet, count=100)
    counter = 0
    for tweet in tweets:
        account.append(tweet["user"]["name"])
        try:
            source.append(tweet["retweeted_status"]["user"]["name"])
        except:
            source.append("-")
        text.append(tweet["text"])
        time.append(datetime.strptime(tweet["created_at"], "%a %b %d %H:%M:%S %z %Y").astimezone(pytz.timezone("US/Central")))
        scores = analyzer.polarity_scores(tweet["text"])
        comp.append(scores["compound"])
        pos.append(scores["pos"])
        neg.append(scores["neg"])
        neu.append(scores["neu"])
        counter += 1
        id.append(counter)
```


```python
df = pd.DataFrame({"Account": account, "Source Account": source, "Text": text, "Time": time, 
                   "Compound Score": comp, "Positive Score": pos, "Negative Score": neg, "Neutral Score": neu, "Tweets Ago": id
                  })
df = df[["Account", "Source Account", "Text", "Time", 
         "Compound Score", "Positive Score", "Negative Score", "Neutral Score", "Tweets Ago"]]
df.to_csv("news_mood.csv", index=False)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Source Account</th>
      <th>Text</th>
      <th>Time</th>
      <th>Compound Score</th>
      <th>Positive Score</th>
      <th>Negative Score</th>
      <th>Neutral Score</th>
      <th>Tweets Ago</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>The robots taking on your housework https://t....</td>
      <td>2018-06-04 20:51:55-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Early Van Gogh landscape sells for €7m at Fren...</td>
      <td>2018-06-04 20:42:45-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Georgia officer fired for hitting suspect with...</td>
      <td>2018-06-04 20:28:59-05:00</td>
      <td>-0.7003</td>
      <td>0.000</td>
      <td>0.420</td>
      <td>0.580</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Could a text message save thousands of fisherm...</td>
      <td>2018-06-04 20:26:30-05:00</td>
      <td>0.4939</td>
      <td>0.286</td>
      <td>0.000</td>
      <td>0.714</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Henri van Breda: Axe murderer transfixes South...</td>
      <td>2018-06-04 20:21:33-05:00</td>
      <td>-0.7184</td>
      <td>0.000</td>
      <td>0.462</td>
      <td>0.538</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Europe and nationalism: A country-by-country g...</td>
      <td>2018-06-04 20:16:48-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Trump drops Philadelphia Eagles White House in...</td>
      <td>2018-06-04 19:48:55-05:00</td>
      <td>-0.1027</td>
      <td>0.127</td>
      <td>0.159</td>
      <td>0.714</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Swimmer Ben Lecomte begins record Pacific cros...</td>
      <td>2018-06-04 19:33:12-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Guatemala volcano: Deadly Fuego eruption caugh...</td>
      <td>2018-06-04 19:01:36-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>9</td>
    </tr>
    <tr>
      <th>9</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Pakistan's soaring rates of 'diabetic foot' ht...</td>
      <td>2018-06-04 18:59:22-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>10</td>
    </tr>
    <tr>
      <th>10</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Is Kyary Pamyu Pamyu Japan's Lady Gaga? https:...</td>
      <td>2018-06-04 18:43:23-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>11</td>
    </tr>
    <tr>
      <th>11</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Qatar cash and cows help buck Gulf boycott htt...</td>
      <td>2018-06-04 18:24:51-05:00</td>
      <td>0.1027</td>
      <td>0.225</td>
      <td>0.192</td>
      <td>0.583</td>
      <td>12</td>
    </tr>
    <tr>
      <th>12</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Robert Kennedy: What if US presidential hopefu...</td>
      <td>2018-06-04 18:08:49-05:00</td>
      <td>0.7839</td>
      <td>0.408</td>
      <td>0.000</td>
      <td>0.592</td>
      <td>13</td>
    </tr>
    <tr>
      <th>13</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Why do we love to dance with each other? https...</td>
      <td>2018-06-04 18:08:49-05:00</td>
      <td>0.6369</td>
      <td>0.318</td>
      <td>0.000</td>
      <td>0.682</td>
      <td>14</td>
    </tr>
    <tr>
      <th>14</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Are presidential pardons Trump's secret weapon...</td>
      <td>2018-06-04 18:06:29-05:00</td>
      <td>0.0000</td>
      <td>0.234</td>
      <td>0.234</td>
      <td>0.532</td>
      <td>15</td>
    </tr>
    <tr>
      <th>15</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Ron Rockwell Hansen: US arrests man for trying...</td>
      <td>2018-06-04 17:10:13-05:00</td>
      <td>-0.4404</td>
      <td>0.000</td>
      <td>0.195</td>
      <td>0.805</td>
      <td>16</td>
    </tr>
    <tr>
      <th>16</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Putin says Russia not aiming to divide EU http...</td>
      <td>2018-06-04 16:51:42-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>17</td>
    </tr>
    <tr>
      <th>17</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Apple jams Facebook's web-tracking tools https...</td>
      <td>2018-06-04 16:25:57-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>18</td>
    </tr>
    <tr>
      <th>18</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Starbucks boss leaves firm after 36 years http...</td>
      <td>2018-06-04 16:21:20-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>19</td>
    </tr>
    <tr>
      <th>19</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Dinosaur skeleton sold to private buyer for €1...</td>
      <td>2018-06-04 16:06:55-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>20</td>
    </tr>
    <tr>
      <th>20</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Guatemala Fuego: Search after deadly volcano e...</td>
      <td>2018-06-04 16:04:15-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>21</td>
    </tr>
    <tr>
      <th>21</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Maine man killed in hit-and-run ran over a gir...</td>
      <td>2018-06-04 14:16:00-05:00</td>
      <td>-0.6705</td>
      <td>0.000</td>
      <td>0.310</td>
      <td>0.690</td>
      <td>22</td>
    </tr>
    <tr>
      <th>22</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Russia puts a smile on Kim Jong-un's face http...</td>
      <td>2018-06-04 13:59:41-05:00</td>
      <td>0.3612</td>
      <td>0.263</td>
      <td>0.000</td>
      <td>0.737</td>
      <td>23</td>
    </tr>
    <tr>
      <th>23</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Denmark backs fence on German border to keep o...</td>
      <td>2018-06-04 13:59:41-05:00</td>
      <td>-0.0516</td>
      <td>0.000</td>
      <td>0.098</td>
      <td>0.902</td>
      <td>24</td>
    </tr>
    <tr>
      <th>24</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Tunisia migrant shipwreck death toll reaches 1...</td>
      <td>2018-06-04 13:47:47-05:00</td>
      <td>-0.5719</td>
      <td>0.108</td>
      <td>0.351</td>
      <td>0.541</td>
      <td>25</td>
    </tr>
    <tr>
      <th>25</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Italians shocked by man's selfie after train a...</td>
      <td>2018-06-04 11:32:02-05:00</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.375</td>
      <td>0.625</td>
      <td>26</td>
    </tr>
    <tr>
      <th>26</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>'Remarkable' therapy beats terminal breast can...</td>
      <td>2018-06-04 11:07:02-05:00</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.423</td>
      <td>0.577</td>
      <td>27</td>
    </tr>
    <tr>
      <th>27</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Bill Clinton: 'Monica Lewinsky affair was deal...</td>
      <td>2018-06-04 10:23:48-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>28</td>
    </tr>
    <tr>
      <th>28</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Salah tackle turns up in Syrian law school exa...</td>
      <td>2018-06-04 10:09:54-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>29</td>
    </tr>
    <tr>
      <th>29</th>
      <td>BBC News (World)</td>
      <td>-</td>
      <td>Texas woman shot her husband for 'beating the ...</td>
      <td>2018-06-04 09:57:19-05:00</td>
      <td>-0.4588</td>
      <td>0.000</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>30</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>470</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Serena Williams: "I gave up so much, from the ...</td>
      <td>2018-06-04 09:14:00-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>71</td>
    </tr>
    <tr>
      <th>471</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Democrats hope an Asian influx will help turn ...</td>
      <td>2018-06-04 09:05:04-05:00</td>
      <td>0.6808</td>
      <td>0.337</td>
      <td>0.000</td>
      <td>0.663</td>
      <td>72</td>
    </tr>
    <tr>
      <th>472</th>
      <td>The New York Times</td>
      <td>gabriel dance</td>
      <td>RT @gabrieldance: for our story today on faceb...</td>
      <td>2018-06-04 08:51:36-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>73</td>
    </tr>
    <tr>
      <th>473</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Serena Williams is out of the French Open. No,...</td>
      <td>2018-06-04 08:51:05-05:00</td>
      <td>-0.4102</td>
      <td>0.093</td>
      <td>0.206</td>
      <td>0.701</td>
      <td>74</td>
    </tr>
    <tr>
      <th>474</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"We got him," the officer says on the video, w...</td>
      <td>2018-06-04 08:43:04-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>75</td>
    </tr>
    <tr>
      <th>475</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Steven Mnuchin has so far managed to stay in P...</td>
      <td>2018-06-04 08:30:08-05:00</td>
      <td>0.6705</td>
      <td>0.244</td>
      <td>0.000</td>
      <td>0.756</td>
      <td>76</td>
    </tr>
    <tr>
      <th>476</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Breaking News: President Trump said he had the...</td>
      <td>2018-06-04 08:29:20-05:00</td>
      <td>0.3182</td>
      <td>0.103</td>
      <td>0.000</td>
      <td>0.897</td>
      <td>77</td>
    </tr>
    <tr>
      <th>477</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>The whale washed ashore with nearly 18 pounds ...</td>
      <td>2018-06-04 08:28:04-05:00</td>
      <td>0.4939</td>
      <td>0.132</td>
      <td>0.000</td>
      <td>0.868</td>
      <td>78</td>
    </tr>
    <tr>
      <th>478</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Bashar al-Assad plans to visit Kim Jong-un, No...</td>
      <td>2018-06-04 08:16:03-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>79</td>
    </tr>
    <tr>
      <th>479</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Morning Briefing: Here's what you need to know...</td>
      <td>2018-06-04 08:00:15-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>80</td>
    </tr>
    <tr>
      <th>480</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>A volcano erupted near Guatemala's capital, ki...</td>
      <td>2018-06-04 07:45:09-05:00</td>
      <td>-0.7841</td>
      <td>0.000</td>
      <td>0.330</td>
      <td>0.670</td>
      <td>81</td>
    </tr>
    <tr>
      <th>481</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Nearly 50 years after it was stolen, possible ...</td>
      <td>2018-06-04 07:15:04-05:00</td>
      <td>-0.6486</td>
      <td>0.000</td>
      <td>0.227</td>
      <td>0.773</td>
      <td>82</td>
    </tr>
    <tr>
      <th>482</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>The London police have blamed a bleak rap genr...</td>
      <td>2018-06-04 07:10:07-05:00</td>
      <td>-0.8020</td>
      <td>0.000</td>
      <td>0.286</td>
      <td>0.714</td>
      <td>83</td>
    </tr>
    <tr>
      <th>483</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Your daily @DealBook Briefing:\n\n• Facebook g...</td>
      <td>2018-06-04 07:00:08-05:00</td>
      <td>-0.5994</td>
      <td>0.000</td>
      <td>0.196</td>
      <td>0.804</td>
      <td>84</td>
    </tr>
    <tr>
      <th>484</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>A rare, brain-damaging virus that experts cons...</td>
      <td>2018-06-04 06:44:05-05:00</td>
      <td>-0.7579</td>
      <td>0.000</td>
      <td>0.289</td>
      <td>0.711</td>
      <td>85</td>
    </tr>
    <tr>
      <th>485</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>A suicide bomber targeted a gathering of Afgha...</td>
      <td>2018-06-04 06:31:06-05:00</td>
      <td>-0.8750</td>
      <td>0.000</td>
      <td>0.417</td>
      <td>0.583</td>
      <td>86</td>
    </tr>
    <tr>
      <th>486</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>A year after the killing of Freddie Gray, a te...</td>
      <td>2018-06-04 06:14:03-05:00</td>
      <td>-0.8625</td>
      <td>0.000</td>
      <td>0.312</td>
      <td>0.688</td>
      <td>87</td>
    </tr>
    <tr>
      <th>487</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"Westworld" has been pushing its viewers to th...</td>
      <td>2018-06-04 06:00:07-05:00</td>
      <td>-0.2732</td>
      <td>0.000</td>
      <td>0.104</td>
      <td>0.896</td>
      <td>88</td>
    </tr>
    <tr>
      <th>488</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>The French interior minister called her appear...</td>
      <td>2018-06-04 05:43:04-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>89</td>
    </tr>
    <tr>
      <th>489</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Voters in Slovenia gave victory to a populist ...</td>
      <td>2018-06-04 05:31:03-05:00</td>
      <td>0.4019</td>
      <td>0.130</td>
      <td>0.000</td>
      <td>0.870</td>
      <td>90</td>
    </tr>
    <tr>
      <th>490</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Razan al-Najjar was trying to help an injured ...</td>
      <td>2018-06-04 05:17:03-05:00</td>
      <td>-0.6369</td>
      <td>0.102</td>
      <td>0.259</td>
      <td>0.639</td>
      <td>91</td>
    </tr>
    <tr>
      <th>491</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Morning Briefing: Here's what you need to know...</td>
      <td>2018-06-04 05:00:06-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>92</td>
    </tr>
    <tr>
      <th>492</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Many women with early-stage breast cancer who ...</td>
      <td>2018-06-04 04:45:08-05:00</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.206</td>
      <td>0.794</td>
      <td>93</td>
    </tr>
    <tr>
      <th>493</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>An Australian politician's partner says his pa...</td>
      <td>2018-06-04 04:30:03-05:00</td>
      <td>0.4019</td>
      <td>0.124</td>
      <td>0.000</td>
      <td>0.876</td>
      <td>94</td>
    </tr>
    <tr>
      <th>494</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"You're not going to be able to read" his next...</td>
      <td>2018-06-04 04:15:05-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>95</td>
    </tr>
    <tr>
      <th>495</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"They tried to hit him but he defended himself...</td>
      <td>2018-06-04 04:00:04-05:00</td>
      <td>0.4380</td>
      <td>0.121</td>
      <td>0.000</td>
      <td>0.879</td>
      <td>96</td>
    </tr>
    <tr>
      <th>496</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"If it was 5,000 dogs, there would be outrage....</td>
      <td>2018-06-04 03:44:03-05:00</td>
      <td>-0.5106</td>
      <td>0.000</td>
      <td>0.142</td>
      <td>0.858</td>
      <td>97</td>
    </tr>
    <tr>
      <th>497</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>"Until now they don't understand they can't wa...</td>
      <td>2018-06-04 03:29:05-05:00</td>
      <td>-0.0572</td>
      <td>0.000</td>
      <td>0.075</td>
      <td>0.925</td>
      <td>98</td>
    </tr>
    <tr>
      <th>498</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>Facebook let Apple, Samsung and other device m...</td>
      <td>2018-06-04 03:16:05-05:00</td>
      <td>0.4767</td>
      <td>0.134</td>
      <td>0.000</td>
      <td>0.866</td>
      <td>99</td>
    </tr>
    <tr>
      <th>499</th>
      <td>The New York Times</td>
      <td>-</td>
      <td>How to finally just make a decision https://t....</td>
      <td>2018-06-04 03:00:09-05:00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 9 columns</p>
</div>




```python
cats = [df.iloc[_, 0] for _ in range(0,500,100)]
colors = ["skyblue", "g", "r", "b", "yellow"]
sns.set_style("darkgrid")
sns.lmplot(x="Tweets Ago", y="Compound Score", data=df, hue="Account", palette={cat:color for cat,color in zip(cats,colors)}, 
           scatter_kws={"alpha": 0.8, "s": 200, "lw": 1, "edgecolors": "k"}, size=7.5, aspect=4/3, legend=False, fit_reg=False)
plt.title(f"Sentiment Analysis of Media Tweets ({datetime.today().strftime('%m/%d/%y')})", size=20)
plt.xlabel("Tweets Ago", size=20)
plt.ylabel("Tweet Polarity", size=20)
plt.xticks(size=15)
plt.yticks(size=15)
plt.xlim(102, -1)
leg = plt.legend(fontsize=15, bbox_to_anchor=(1, 1))
leg.set_title("Media Sources", prop={"size": 20})
plt.savefig("plot1", bbox_inches="tight")
plt.show()
```


![png](output_6_0.png)



```python
overall = df.groupby("Account")[["Compound Score"]].mean().reset_index()
overall
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Compound Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BBC News (World)</td>
      <td>-0.086637</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CBS News</td>
      <td>-0.059035</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CNN</td>
      <td>-0.049955</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fox News</td>
      <td>0.051031</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The New York Times</td>
      <td>0.006645</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(12,9))
sns.set_style("dark")
bp = sns.barplot(x="Account", y="Compound Score", data=overall, palette=colors)
[patch.set_width(1) for patch in bp.patches]
# Recenter the bars.
[patch.set_x(patch.get_x()-0.1) for patch in bp.patches]
plt.title(f"Overall Media Sentiment based on Twitter ({datetime.today().strftime('%m/%d/%y')})", size=20)
plt.xlabel("")
plt.ylabel("Tweet Polarity", size=20)
plt.xticks(size=15)
plt.yticks(size=15)
avg_score = list(overall["Compound Score"])
for _ in range(5):
    if avg_score[_] > 0:
        plt.annotate(s=round(avg_score[_],2), xy=(_,avg_score[_]+.0025), size=15, ha="center")
    else:
        plt.annotate(s=round(avg_score[_],2), xy=(_,avg_score[_]-.005), size=15, ha="center")
plt.savefig("plot2", bbox_inches="tight")
plt.show()
```


![png](output_8_0.png)

