# tweet

# Gather the Data


```python
import pandas as pd
import requests
import json
import tweepy 
```

Read all the image data.


```python
url = "https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv"
image = requests.get(url)
with open("image.tsv", 'wb') as f:
        f.write(image.content)
image = pd.read_csv("image.tsv",sep="\t")
image.to_csv('image.csv')
```

Read the twitter archieve data


```python
twitter = pd.read_csv("twitter-archive-enhanced.csv")
```

Gather the "like" and "retweet" number


```python
consumer_key = #
consumer_secret = #
access_token = #
access_secret = #

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_secret)

api = tweepy.API(auth, wait_on_rate_limit=True)
```


```python
error=[]
with open('tweet_json.txt', 'a') as outfile:
    for tweet_id in list(image.tweet_id):
        try:
            temp = api.get_status(tweet_id, tweet_mode='extended')
            json.dump(temp._json, outfile,indent=2)
            outfile.write('\n')
        except:
            error.append(tweet_id)
```


```python
import json
status = []
file = open('tweet_json.txt').read()[1:-2].split('}\n{')
for jsonline in file:
    data = json.loads("{"+jsonline+"}") 
    if data.get("retweeted") == False:
        tweet_id = data.get('id')
        retweets = data.get('retweet_count')
        likes = data.get('favorite_count')
        status.append({'tweet_id': tweet_id, 
                'retweets': retweets,
                'likes': likes})
```


```python
status = pd.DataFrame(status, columns = ["tweet_id","retweets","likes"])
status.to_csv("status.csv")
```

Merge all data that has an image


```python
final_data = image.merge(twitter, on = "tweet_id").merge(status, on = "tweet_id")
final_data.to_csv("final_data.csv")
```

# Access and Clean the data


```python
import pandas as pd
twitter = pd.read_csv('final_data.csv',index_col=0)
```


```python
twitter.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2077 entries, 0 to 2076
    Data columns (total 30 columns):
    tweet_id                      2077 non-null object
    jpg_url                       2077 non-null object
    img_num                       2077 non-null int64
    p1                            2077 non-null object
    p1_conf                       2077 non-null float64
    p1_dog                        2077 non-null bool
    p2                            2077 non-null object
    p2_conf                       2077 non-null float64
    p2_dog                        2077 non-null bool
    p3                            2077 non-null object
    p3_conf                       2077 non-null float64
    p3_dog                        2077 non-null bool
    in_reply_to_status_id         2077 non-null object
    in_reply_to_user_id           2077 non-null object
    timestamp                     2077 non-null object
    source                        2077 non-null object
    text                          2077 non-null object
    retweeted_status_id           2077 non-null object
    retweeted_status_user_id      2077 non-null object
    retweeted_status_timestamp    75 non-null object
    expanded_urls                 2077 non-null object
    rating_numerator              2077 non-null object
    rating_denominator            2077 non-null object
    name                          2077 non-null object
    doggo                         2077 non-null object
    floofer                       2077 non-null object
    pupper                        2077 non-null object
    puppo                         2077 non-null object
    retweets                      2077 non-null int64
    likes                         2077 non-null int64
    dtypes: bool(3), float64(3), int64(3), object(21)
    memory usage: 540.4+ KB



```python
twitter.sample(10)
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
      <th>tweet_id</th>
      <th>jpg_url</th>
      <th>img_num</th>
      <th>p1</th>
      <th>p1_conf</th>
      <th>p1_dog</th>
      <th>p2</th>
      <th>p2_conf</th>
      <th>p2_dog</th>
      <th>p3</th>
      <th>...</th>
      <th>expanded_urls</th>
      <th>rating_numerator</th>
      <th>rating_denominator</th>
      <th>name</th>
      <th>doggo</th>
      <th>floofer</th>
      <th>pupper</th>
      <th>puppo</th>
      <th>retweets</th>
      <th>likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>58</th>
      <td>667090893657276420</td>
      <td>https://pbs.twimg.com/media/CUH7oLuUsAELWib.jpg</td>
      <td>1</td>
      <td>Chihuahua</td>
      <td>0.959514</td>
      <td>True</td>
      <td>Italian_greyhound</td>
      <td>0.005370</td>
      <td>True</td>
      <td>Pomeranian</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/667090893...</td>
      <td>7</td>
      <td>10</td>
      <td>Clybe</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>130</td>
      <td>341</td>
    </tr>
    <tr>
      <th>439</th>
      <td>674436901579923456</td>
      <td>https://pbs.twimg.com/media/CVwUyM9WwAAGDjv.jpg</td>
      <td>1</td>
      <td>acorn_squash</td>
      <td>0.375392</td>
      <td>False</td>
      <td>Shih-Tzu</td>
      <td>0.105416</td>
      <td>True</td>
      <td>Lhasa</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/674436901...</td>
      <td>9</td>
      <td>10</td>
      <td>Bailey</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>422</td>
      <td>1175</td>
    </tr>
    <tr>
      <th>924</th>
      <td>702321140488925184</td>
      <td>https://pbs.twimg.com/media/Cb8lWafWEAA2q93.jpg</td>
      <td>3</td>
      <td>West_Highland_white_terrier</td>
      <td>0.769159</td>
      <td>True</td>
      <td>Scotch_terrier</td>
      <td>0.064369</td>
      <td>True</td>
      <td>Old_English_sheepdog</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/702321140...</td>
      <td>12</td>
      <td>10</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>1138</td>
      <td>3544</td>
    </tr>
    <tr>
      <th>1197</th>
      <td>740676976021798912</td>
      <td>https://pbs.twimg.com/media/Ckdpx5KWsAANF6b.jpg</td>
      <td>1</td>
      <td>wombat</td>
      <td>0.462952</td>
      <td>False</td>
      <td>Norwegian_elkhound</td>
      <td>0.275225</td>
      <td>True</td>
      <td>Siamese_cat</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/740676976...</td>
      <td>11</td>
      <td>10</td>
      <td>Baloo</td>
      <td>None</td>
      <td>None</td>
      <td>pupper</td>
      <td>None</td>
      <td>7530</td>
      <td>19559</td>
    </tr>
    <tr>
      <th>1174</th>
      <td>737322739594330112</td>
      <td>https://pbs.twimg.com/media/Cjt_Hm6WsAAjkPG.jpg</td>
      <td>1</td>
      <td>guinea_pig</td>
      <td>0.148526</td>
      <td>False</td>
      <td>solar_dish</td>
      <td>0.097183</td>
      <td>False</td>
      <td>park_bench</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/737322739...</td>
      <td>9</td>
      <td>10</td>
      <td>Lily</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>886</td>
      <td>3881</td>
    </tr>
    <tr>
      <th>1512</th>
      <td>786595970293370880</td>
      <td>https://pbs.twimg.com/media/CuqM0fVWAAAboKR.jpg</td>
      <td>1</td>
      <td>Pembroke</td>
      <td>0.709512</td>
      <td>True</td>
      <td>Cardigan</td>
      <td>0.287178</td>
      <td>True</td>
      <td>chow</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/786595970...</td>
      <td>11</td>
      <td>10</td>
      <td>Dale</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>3524</td>
      <td>10353</td>
    </tr>
    <tr>
      <th>1319</th>
      <td>756275833623502848</td>
      <td>https://pbs.twimg.com/media/Cn7U2xlW8AI9Pqp.jpg</td>
      <td>1</td>
      <td>Airedale</td>
      <td>0.602957</td>
      <td>True</td>
      <td>Irish_terrier</td>
      <td>0.086981</td>
      <td>True</td>
      <td>bloodhound</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/756275833...</td>
      <td>10</td>
      <td>10</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>puppo</td>
      <td>1710</td>
      <td>7007</td>
    </tr>
    <tr>
      <th>1715</th>
      <td>819015331746349057</td>
      <td>https://pbs.twimg.com/media/C12x-JTVIAAzdfl.jpg</td>
      <td>4</td>
      <td>prison</td>
      <td>0.907083</td>
      <td>False</td>
      <td>palace</td>
      <td>0.020089</td>
      <td>False</td>
      <td>umbrella</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/819006400...</td>
      <td>14</td>
      <td>10</td>
      <td>Sunny</td>
      <td>doggo</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>21336</td>
      <td>0</td>
    </tr>
    <tr>
      <th>594</th>
      <td>679503373272485890</td>
      <td>https://pbs.twimg.com/media/CW4UtmYWsAAEjqA.jpg</td>
      <td>1</td>
      <td>porcupine</td>
      <td>0.999846</td>
      <td>False</td>
      <td>meerkat</td>
      <td>0.000072</td>
      <td>False</td>
      <td>echidna</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/679503373...</td>
      <td>8</td>
      <td>10</td>
      <td>Dwight</td>
      <td>None</td>
      <td>None</td>
      <td>pupper</td>
      <td>None</td>
      <td>1640</td>
      <td>3414</td>
    </tr>
    <tr>
      <th>276</th>
      <td>670840546554966016</td>
      <td>https://pbs.twimg.com/media/CU9N6upXAAAbtQe.jpg</td>
      <td>1</td>
      <td>Shih-Tzu</td>
      <td>0.963622</td>
      <td>True</td>
      <td>Lhasa</td>
      <td>0.016017</td>
      <td>True</td>
      <td>guinea_pig</td>
      <td>...</td>
      <td>https://twitter.com/dog_rates/status/670840546...</td>
      <td>10</td>
      <td>10</td>
      <td>Colby</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>210</td>
      <td>619</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 30 columns</p>
</div>




```python
twitter.describe()
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
      <th>img_num</th>
      <th>p1_conf</th>
      <th>p2_conf</th>
      <th>p3_conf</th>
      <th>retweets</th>
      <th>likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2077.000000</td>
      <td>2077.000000</td>
      <td>2.077000e+03</td>
      <td>2.077000e+03</td>
      <td>2077.000000</td>
      <td>2077.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.203659</td>
      <td>0.594462</td>
      <td>1.347630e-01</td>
      <td>6.034094e-02</td>
      <td>2917.017814</td>
      <td>8687.102070</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.561640</td>
      <td>0.270897</td>
      <td>1.008044e-01</td>
      <td>5.090303e-02</td>
      <td>4902.916648</td>
      <td>12625.263774</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.044333</td>
      <td>1.011300e-08</td>
      <td>1.740170e-10</td>
      <td>13.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.364729</td>
      <td>5.390140e-02</td>
      <td>1.624560e-02</td>
      <td>619.000000</td>
      <td>1657.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.000000</td>
      <td>0.587764</td>
      <td>1.186220e-01</td>
      <td>4.944380e-02</td>
      <td>1382.000000</td>
      <td>3838.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>0.843799</td>
      <td>1.955730e-01</td>
      <td>9.193000e-02</td>
      <td>3378.000000</td>
      <td>10943.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.000000</td>
      <td>1.000000</td>
      <td>4.880140e-01</td>
      <td>2.734190e-01</td>
      <td>77988.000000</td>
      <td>144133.000000</td>
    </tr>
  </tbody>
</table>
</div>



After watched the data, we found many potential problems.
* Tweet id/status_id should be character not int/float
* P1,p2,p3 are not formatted.
* Retweeted_status_id has many null values.
* The stage of the dogs are in separate columns.
* Some names are wrong. (this/very/a/an)
* Retweeted pictures should be deleted.
* Some rating denominators are weird. (some taken the wrong number)
* The timestamp is not formatted.

### 1. Change ID Type

##### Define

Change all the ID's to string.

##### Code


```python
names = []
for i in twitter.columns:
    if 'id' in i:
        names.append(i)
twitter[names]= twitter[names].astype(str)
```

##### Test


```python
twitter.dtypes
```




    tweet_id                       object
    jpg_url                        object
    img_num                         int64
    p1                             object
    p1_conf                       float64
    p1_dog                           bool
    p2                             object
    p2_conf                       float64
    p2_dog                           bool
    p3                             object
    p3_conf                       float64
    p3_dog                           bool
    in_reply_to_status_id          object
    in_reply_to_user_id            object
    timestamp                      object
    source                         object
    text                           object
    retweeted_status_id            object
    retweeted_status_user_id       object
    retweeted_status_timestamp     object
    expanded_urls                  object
    rating_numerator               object
    rating_denominator             object
    name                           object
    doggo                          object
    floofer                        object
    pupper                         object
    puppo                          object
    retweets                        int64
    likes                           int64
    dtype: object



### 2. Format the predictions

##### Define

The names of the prediction columns are all in lowercase and separated by "_".

##### Code


```python
def fmat(s):
    return(' '.join(s.split('_')).title())
for i in ['p1','p2','p3']:
    twitter[i] = twitter[i].apply(fmat)
```

##### Test


```python
print(twitter[['p1','p2','p3']].head(10))
```

                           p1                  p2                           p3
    0  Welsh_springer_spaniel              collie            Shetland_sheepdog
    1                 redbone  miniature_pinscher          Rhodesian_ridgeback
    2         German_shepherd            malinois                   bloodhound
    3     Rhodesian_ridgeback             redbone           miniature_pinscher
    4      miniature_pinscher          Rottweiler                     Doberman
    5    Bernese_mountain_dog    English_springer   Greater_Swiss_Mountain_dog
    6              box_turtle          mud_turtle                     terrapin
    7                    chow     Tibetan_mastiff                     fur_coat
    8           shopping_cart     shopping_basket             golden_retriever
    9        miniature_poodle            komondor  soft-coated_wheaten_terrier


### 3. Delete Re-tweets

##### Define

All the rows with a non-null retweets id needs to be deleted since we only want original tweets.

##### Code


```python
unoriginal = ['in_reply_to_status_id','in_reply_to_user_id','retweeted_status_id','retweeted_status_user_id']
for i in unoriginal:
    twitter = twitter.loc[twitter[i] == 'nan']
unoriginal.append('retweeted_status_timestamp')
twitter = twitter.drop(columns = unoriginal)
```

##### Test


```python
twitter.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2077 entries, 0 to 2076
    Data columns (total 30 columns):
    tweet_id                      2077 non-null object
    jpg_url                       2077 non-null object
    img_num                       2077 non-null int64
    p1                            2077 non-null object
    p1_conf                       2077 non-null float64
    p1_dog                        2077 non-null bool
    p2                            2077 non-null object
    p2_conf                       2077 non-null float64
    p2_dog                        2077 non-null bool
    p3                            2077 non-null object
    p3_conf                       2077 non-null float64
    p3_dog                        2077 non-null bool
    in_reply_to_status_id         2077 non-null object
    in_reply_to_user_id           2077 non-null object
    timestamp                     2077 non-null object
    source                        2077 non-null object
    text                          2077 non-null object
    retweeted_status_id           2077 non-null object
    retweeted_status_user_id      2077 non-null object
    retweeted_status_timestamp    75 non-null object
    expanded_urls                 2077 non-null object
    rating_numerator              2077 non-null object
    rating_denominator            2077 non-null object
    name                          2077 non-null object
    doggo                         2077 non-null object
    floofer                       2077 non-null object
    pupper                        2077 non-null object
    puppo                         2077 non-null object
    retweets                      2077 non-null int64
    likes                         2077 non-null int64
    dtypes: bool(3), float64(3), int64(3), object(21)
    memory usage: 540.4+ KB


### 4. Format the Timestamp

##### Define

The timestamp column looks messy, we need to make it look tidy and sort the data by date.

##### Code


```python
from datetime import datetime
def string_to_date(t):
    return(datetime.strptime(t, '%Y-%m-%d %H:%M:%S %z'))
def date_format(t):
    return(t.strftime('%d, %b %Y'))
def time_format(t):
    return(t.strftime('%H:%M:%S'))
twitter.timestamp = twitter.timestamp.apply(string_to_date)
twitter.sort_values(by = 'timestamp')
twitter['Date'] = twitter.timestamp.apply(date_format)
twitter['Time'] = twitter.timestamp.apply(time_format)
```


```python
twitter = twitter.drop(columns = ['timestamp'])
```

##### Test


```python
twitter[['Date','Time']].head(10)
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
      <th>Date</th>
      <th>Time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15, Nov 2015</td>
      <td>22:32:08</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15, Nov 2015</td>
      <td>23:05:30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15, Nov 2015</td>
      <td>23:21:54</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16, Nov 2015</td>
      <td>00:04:52</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16, Nov 2015</td>
      <td>00:24:50</td>
    </tr>
    <tr>
      <th>5</th>
      <td>16, Nov 2015</td>
      <td>00:30:50</td>
    </tr>
    <tr>
      <th>6</th>
      <td>16, Nov 2015</td>
      <td>00:35:11</td>
    </tr>
    <tr>
      <th>7</th>
      <td>16, Nov 2015</td>
      <td>00:49:46</td>
    </tr>
    <tr>
      <th>8</th>
      <td>16, Nov 2015</td>
      <td>00:55:59</td>
    </tr>
    <tr>
      <th>9</th>
      <td>16, Nov 2015</td>
      <td>01:01:59</td>
    </tr>
  </tbody>
</table>
</div>



### 5. Correct Rating Denominator and Number

##### Define

There are some rows have the wrong value, and we need to correct them.


```python
twitter['rating_denominator'].value_counts() 
```




    10     1962
    50     3   
    80     2   
    11     2   
    170    1   
    150    1   
    120    1   
    110    1   
    90     1   
    70     1   
    40     1   
    20     1   
    7      1   
    2      1   
    Name: rating_denominator, dtype: int64



Now let us take a look at some of the text of the twitter that have unusual ratings.


```python
pd.set_option('display.max_colwidth', -1)
display(twitter[['text','rating_denominator']][twitter['rating_denominator'] != 10])
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
      <th>text</th>
      <th>rating_denominator</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>This is an Albanian 3 1/2 legged  Episcopalian. Loves well-polished hardwood flooring. Penis on the collar. 9/10 https://t.co/d9NcXFKwLv</td>
      <td>2</td>
    </tr>
    <tr>
      <th>501</th>
      <td>Here we have an entire platoon of puppers. Total score: 88/80 would pet all at once https://t.co/y93p6FLvVw</td>
      <td>80</td>
    </tr>
    <tr>
      <th>560</th>
      <td>IT'S PUPPERGEDDON. Total of 144/120 ...I think https://t.co/ZanVtAtvIq</td>
      <td>120</td>
    </tr>
    <tr>
      <th>667</th>
      <td>This is Darrel. He just robbed a 7/11 and is in a high speed police chase. Was just spotted by the helicopter 10/10 https://t.co/7EsP8LmSp5</td>
      <td>11</td>
    </tr>
    <tr>
      <th>692</th>
      <td>Someone help the girl is being mugged. Several are distracting her while two steal her shoes. Clever puppers 121/110 https://t.co/1zfnTJLt55</td>
      <td>110</td>
    </tr>
    <tr>
      <th>865</th>
      <td>Happy Wednesday here's a bucket of pups. 44/40 would pet all at once https://t.co/HppvrYuamZ</td>
      <td>40</td>
    </tr>
    <tr>
      <th>941</th>
      <td>Here is a whole flock of puppers.  60/50 I'll take the lot https://t.co/9dpcw6MdWa</td>
      <td>50</td>
    </tr>
    <tr>
      <th>1007</th>
      <td>From left to right:\nCletus, Jerome, Alejandro, Burp, &amp;amp; Titson\nNone know where camera is. 45/50 would hug all at once https://t.co/sedre1ivTK</td>
      <td>50</td>
    </tr>
    <tr>
      <th>1025</th>
      <td>Here's a brigade of puppers. All look very prepared for whatever happens next. 80/80 https://t.co/0eb7R1Om12</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1050</th>
      <td>Happy Saturday here's 9 puppers on a bench. 99/90 good work everybody https://t.co/mpvaVxKmc1</td>
      <td>90</td>
    </tr>
    <tr>
      <th>1071</th>
      <td>This is Bluebert. He just saw that both #FinalFur match ups are split 50/50. Amazed af. 11/10 https://t.co/Kky1DPG4iq</td>
      <td>50</td>
    </tr>
    <tr>
      <th>1105</th>
      <td>Happy 4/20 from the squad! 13/10 for all https://t.co/eV1diwds8a</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1148</th>
      <td>Say hello to this unbelievably well behaved squad of doggos. 204/170 would try to pet all at once https://t.co/yGQI3He3xv</td>
      <td>170</td>
    </tr>
    <tr>
      <th>1196</th>
      <td>After so many requests, this is Bretagne. She was the last surviving 9/11 search dog, and our second ever 14/10. RIP https://t.co/XAVDNDaVgQ</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1338</th>
      <td>Why does this never happen at my front door... 165/150 https://t.co/HmwrdfEfUE</td>
      <td>150</td>
    </tr>
    <tr>
      <th>1656</th>
      <td>Meet Sam. She smiles 24/7 &amp;amp; secretly aspires to be a reindeer. \nKeep Sam smiling by clicking and sharing this link:\nhttps://t.co/98tB8y7y7t https://t.co/LouL5vdvxx</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1726</th>
      <td>The floofs have been released I repeat the floofs have been released. 84/70 https://t.co/NIYC820tmd</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>


As we can see, some of them are wrong. (The line 1656 doesn't have a score)

##### Code


```python
import re
for index, row in twitter.iterrows():
    l = re.findall(r'((?:\d+\.)?\d+)\/(\d+)',row['text'])[-1]
    twitter.loc[index,['rating_numerator','rating_denominator']] = l

```

##### Test

As we can see, except row 1656, other data all look normal and there are data with decimals.


```python
twitter.rating_denominator.value_counts()
```




    10     2064
    80        2
    50        2
    90        1
    110       1
    120       1
    170       1
    70        1
    40        1
    150       1
    7         1
    130       1
    Name: rating_denominator, dtype: int64




```python
twitter.rating_numerator.value_counts()
```




    12       475
    10       422
    11       415
    13       287
    9        150
    8         98
    7         53
    14        40
    6         33
    5         32
    3         19
    4         16
    2         10
    1          5
    0          2
    24         1
    1776       1
    84         1
    143        1
    420        1
    88         1
    121        1
    15         1
    165        1
    13.5       1
    60         1
    45         1
    9.75       1
    99         1
    144        1
    11.27      1
    44         1
    204        1
    80         1
    11.26      1
    Name: rating_numerator, dtype: int64



### 6. Create a New Column
##### Define
We need to make a new column 'score' and drop the original 'rating_numerator','rating_denominator'.

##### Code


```python
import numpy as np
def divide(x,y):
    return(float(float(x)/float(y))*10)
twitter['score'] = np.vectorize(divide)(twitter['rating_numerator'],twitter['rating_denominator'])
```


```python
from statistics import median
twitter.loc[1656,'score'] = median(twitter['score'])
```


```python
twitter = twitter.drop(['rating_numerator','rating_denominator'],axis = 1)
```

##### Test


```python
twitter.score.value_counts()
```




    12.00      479
    10.00      423
    11.00      422
    13.00      287
    9.00       151
    8.00        98
    7.00        53
    14.00       40
    6.00        33
    5.00        32
    3.00        19
    4.00        16
    2.00        10
    1.00         5
    0.00         2
    15.00        1
    13.50        1
    11.26        1
    11.27        1
    420.00       1
    9.75         1
    1776.00      1
    Name: score, dtype: int64



### 7. Separate Links From Text

##### Define

Every text is followed by a "https" link, and we need to remove them.

##### Code


```python
twitter['link'] = 'None'
for index, row in twitter.iterrows():
    twitter.loc[index,'link'] = "https"+row['text'].split("https")[1]
    twitter.loc[index,'text'] = row['text'].split("https")[0]
```

##### Test


```python
twitter.link.head(10)
```




    0    https://t.co/BLDqew2Ijj
    1    https://t.co/r7mOb2m0UI
    2    https://t.co/y671yMhoiR
    3    https://t.co/DWnyCjf2mx
    4    https://t.co/4B7cOc1EDq
    5    https://t.co/fvIbQfHjIe
    6    https://t.co/v5A4vzSDdc
    7    https://t.co/rdivxLiqEt
    8    https://t.co/yWBqbrzy8O
    9    https://t.co/pYAJkAe76p
    Name: link, dtype: object



### 8. Find All the Right Names

##### Define

There are some dogs have name but get a "None" on their "name" column.

##### Code


```python
def name_change(s):
    if s[0] == s[0].lower():
        s = 'None'
    return(s)
twitter.name = twitter.name.apply(name_change)
```


```python
for index, row in twitter.iterrows():
    try:
        l = row['text'].split('named')
        name = l[1].split(".")[0]
        twitter.loc[index,'name'] = name
    except:
        pass
```

##### Test


```python
for index, row in twitter.iterrows():
    if row['name'] == 'None' and 'name' in row['text']:
        print(row['text'])
```

    These are Peruvian Feldspars. Their names are Cupit and Prencer. Both resemble Rand Paul. Sick outfits 10/10 &amp; 10/10 
    This is a Dasani Kingfisher from Maine. His name is Daryl. Daryl doesn't like being swallowed by a panda. 8/10 
    Another topnotch dog. His name is Big Jumpy Rat. Massive ass feet. Superior tail. Jumps high af. 12/10 great pup 
    I would do radical things in the name of Dog God. I'd believe every word in that book. 10/10 
    This pup's name is Sabertooth (parents must be cool). Ears for days. Jumps unannounced. 9/10 would pet diligently 
    We normally don't rate bears but this one seems nice. Her name is Thea. Appears rather fluffy. 10/10 good bear 
    This is my dog. Her name is Zoey. She knows I've been rating other dogs. She's not happy. 13/10 no bias at all 
    Sorry for the lack of posts today. I came home from school and had to spend quality time with my puppo. Her name is Zoey and she's 13/10 


##### Define

There are some dogs are named using "his/her name is".

##### Code


```python
for index, row in twitter.iterrows():
    try:
        l = row['text'].split('name is')
        name = l[1].split(".")[0]
        twitter.loc[index,'name'] = name
    except:
        pass
    try:
        l1 = row['text'].split('names are')
        name1 = l1[1].split(".")[0].split('and')
        twitter.loc[index,'name'] = ','.join(name1)
    except:
        pass
```

##### Test


```python
display(twitter.name.value_counts())
```


    None              653
    Charlie            11
    Tucker             10
    Penny              10
    Lucy               10
    Oliver             10
    Cooper             10
    Lola                8
    Bo                  8
    Winston             8
    Sadie               8
    Jax                 7
    Toby                7
    Daisy               7
    Scout               6
    Milo                6
    Bailey              6
    Dave                6
    Koda                6
    Bella               6
    Stanley             6
    Rusty               6
    Leo                 5
    Archie              5
    Larry               5
    Alfie               5
    Oscar               5
    Louis               5
    Chester             5
    Buddy               5
                     ... 
    Barclay             1
    Creg                1
    Maisey              1
    Billy               1
    Gerbald             1
    Lilli               1
    Crimson             1
    Ralphus             1
    Eriq                1
    Jamesy              1
    Zuzu                1
    Banditt             1
    Raphael             1
    Maxwell             1
    Jazzy               1
    Lulu                1
    Thor                1
    Stuart              1
    Meyer               1
    Michelangelope      1
    Sage                1
    Stewie              1
    Remus               1
    Dale                1
    Obie                1
    Miguel              1
    Snoop               1
    Rhino               1
    Geno                1
    Vince               1
    Name: name, Length: 937, dtype: int64


### 9. The Dogs' Stage

##### Define
There are four stages: doggo, puppo, pupper and floofer. We need to combine the columns.

##### Code


```python
twitter['stage'] = 'None'
for index, row in twitter.iterrows():
    try: 
        l = row['text'].split()
        for i in ['doggo','puppo','pup','pups','pupper','floofer','doggos','puppos','puppers','floofers']:
            if i in l:
                twitter.loc[index,'stage'] = i
    except:
        pass    
```

##### Test


```python
twitter.stage.value_counts()
```




    None       1659
    pupper     140 
    pup        90  
    doggo      44  
    pups       15  
    puppo      14  
    puppers    11  
    doggos     4   
    floofer    2   
    Name: stage, dtype: int64



##### Define

Now we need to merge all the different names of the same groups.

##### Code


```python
def change_stage(s):
    if s in ['doggos','puppos','floofers','puppers','pups']:
        s = s[:-1]
    if s == 'pup':
        s = 'pupper'
    return(s)
twitter.stage = twitter.stage.apply(change_stage)
```


```python
twitter = twitter.drop(['doggo','puppo','pupper','floofer'],axis =1)
```

##### Test


```python
twitter.stage.value_counts()
```




    None       1743
    pupper      269
    doggo        49
    puppo        14
    floofer       2
    Name: stage, dtype: int64



### 10. Make All Texts in One Line

##### Define
Some texts are written in separate lines. (Separated by "\n")
##### Code


```python
def one_line(s):
    return(' '.join(s.split('\n')))
twitter.text = twitter.text.apply(one_line)
```

##### Test


```python
twitter.text.head(10)
```




    0    Here we have a Japanese Irish Setter. Lost eye...
    1    This is a western brown Mitsubishi terrier. Up...
    2    Here is a very happy pup. Big fan of well-main...
    3    This is a purebred Piers Morgan. Loves to Netf...
    4    Here we have a 1949 1st generation vulpix. Enj...
    5    This is a truly beautiful English Wilson Staff...
    6    This is an odd dog. Hard on the outside but lo...
    7    Here is a Siberian heavily armored polar bear ...
    8    My oh my. This is a rare blond Canadian terrie...
    9    Here is the Rand Paul of retrievers folks! He'...
    Name: text, dtype: object



### 11. Extract the Source From the HTML Tags
##### Define
The source is between two tags, which need to be removed.


```python
twitter.source.head(10)
```




    0    <a href="http://twitter.com/download/iphone" r...
    1    <a href="http://twitter.com/download/iphone" r...
    2    <a href="http://twitter.com/download/iphone" r...
    3    <a href="http://twitter.com/download/iphone" r...
    4    <a href="http://twitter.com/download/iphone" r...
    5    <a href="http://twitter.com/download/iphone" r...
    6    <a href="http://twitter.com/download/iphone" r...
    7    <a href="http://twitter.com/download/iphone" r...
    8    <a href="http://twitter.com/download/iphone" r...
    9    <a href="http://twitter.com/download/iphone" r...
    Name: source, dtype: object



##### Code


```python
def extract_from_tags(s):
    return(re.findall(r'>(.*?)<',s)[0])
twitter.source = twitter.source.apply(extract_from_tags)
```

##### Test


```python
twitter.source.head(10)
```




    0    Twitter for iPhone
    1    Twitter for iPhone
    2    Twitter for iPhone
    3    Twitter for iPhone
    4    Twitter for iPhone
    5    Twitter for iPhone
    6    Twitter for iPhone
    7    Twitter for iPhone
    8    Twitter for iPhone
    9    Twitter for iPhone
    Name: source, dtype: object



### 12. Change Column Names

##### Define


```python
twitter.columns 
```




    Index(['tweet_id', 'jpg_url', 'img_num', 'p1', 'p1_conf', 'p1_dog', 'p2',
           'p2_conf', 'p2_dog', 'p3', 'p3_conf', 'p3_dog', 'in_reply_to_status_id',
           'in_reply_to_user_id', 'source', 'text', 'retweeted_status_id',
           'retweeted_status_user_id', 'retweeted_status_timestamp',
           'expanded_urls', 'name', 'retweets', 'likes', 'Date', 'Time', 'score',
           'link', 'stage'],
          dtype='object')



##### Code


```python
twitter.columns = ['Tweet_ID', 'Jpg_url', 'Img_number', 'Prediction_1', 'P1_confidence', 'P1_is_dog', 'Prediction_2',
       'P2_confidence', 'P2_is_dog', 'Prediction_3', 'P3_confidence', 'P3_is_dog', 'Source',
       'Text', 'Expanded_urls', 'Name', 'Retweets_number', 'Likes_number' , 'Date', 'Time', 'Score', 'Link',
       'Stage']
```

##### Test


```python
twitter.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 1979 entries, 0 to 2076
    Data columns (total 23 columns):
    Tweet_ID           1979 non-null object
    Jpg_url            1979 non-null object
    Img_number         1979 non-null int64
    Prediction_1       1979 non-null object
    P1_confidence      1979 non-null float64
    P1_is_dog          1979 non-null bool
    Prediction_2       1979 non-null object
    P2_confidence      1979 non-null float64
    P2_is_dog          1979 non-null bool
    Prediction_3       1979 non-null object
    P3_confidence      1979 non-null float64
    P3_is_dog          1979 non-null bool
    Source             1979 non-null object
    Text               1979 non-null object
    Expanded_urls      1979 non-null object
    Name               1979 non-null object
    Retweets_number    1979 non-null int64
    Likes_number       1979 non-null int64
    Date               1979 non-null object
    Time               1979 non-null object
    Score              1979 non-null float64
    Link               1979 non-null object
    Stage              1979 non-null object
    dtypes: bool(3), float64(4), int64(3), object(13)
    memory usage: 330.5+ KB


# Final File


```python
twitter.to_csv('twitter_archive_master.csv')
```

# Visualization


```python
from bokeh.plotting import figure
from bokeh.io import output_notebook, push_notebook, show
```


```python
output_notebook()
TOOLS="hover,crosshair,pan,wheel_zoom,zoom_in,zoom_out,box_zoom,undo,redo,reset,tap,save,box_select,poly_select,lasso_select,"
plot = figure(tools=TOOLS,
              title="The Trend of the Likes Number", 
            x_axis_label='Time', y_axis_label='Likes Number')
plot.scatter(x = twitter.index, y = twitter['Likes_number'],
          fill_color='green', fill_alpha=0.6,
          line_color=None)
handle = show(plot, notebook_handle=True)
plot.title.text = "New Title"
push_notebook(handle=handle)
```



    <div class="bk-root">
        <a href="https://bokeh.pydata.org" target="_blank" class="bk-logo bk-logo-small bk-logo-notebook"></a>
        <span id="a9be25a7-6db7-4fbf-a7d1-0fca38d887c1">Loading BokehJS ...</span>
    </div>






<div class="bk-root">
    <div class="bk-plotdiv" id="7099e126-8050-4433-b1d0-8fd0ea49da80"></div>
</div>





```python
plot1 = figure(tools=TOOLS,
               title="The Trend of the Retweets Number", 
               x_axis_label='Time', y_axis_label='Retweets Number')
plot1.scatter(x = twitter.index, y = twitter['Retweets_number'],
          fill_color='green', fill_alpha=0.6,
          line_color=None)
handle = show(plot1, notebook_handle=True)
push_notebook(handle=handle)
```



<div class="bk-root">
    <div class="bk-plotdiv" id="480a1ed0-6f9f-4451-b49b-029e04959813"></div>
</div>





```python
plot1 = figure(tools=TOOLS,
               title="The Relations Between Likes and Score", 
               x_axis_label='Score', y_axis_label='Likes Number')
plot1.scatter(x = twitter.Score[twitter.Score <15] , y = twitter.Likes_number[twitter.Score <15],
          fill_color='green', fill_alpha=0.6,
          line_color=None)
handle = show(plot1, notebook_handle=True)
push_notebook(handle=handle)
```



<div class="bk-root">
    <div class="bk-plotdiv" id="71882892-167a-487b-8705-ee7e18071f34"></div>
</div>





```python
with_name = twitter[twitter.Likes_number < 50000].Likes_number[twitter.Name != 'None']
without_name = twitter[twitter.Likes_number < 50000].Likes_number[twitter.Name == 'None']
```


```python
# import matplotlib
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt
# set all the parameters
num_bins = len(without_name)
mu1=without_name.mean()
sigma1=without_name.std()
mu2=with_name.mean()
sigma2=with_name.std()
# the histogram of the number of dogs without name
n, bins, patches = plt.hist(without_name, num_bins, normed=1, facecolor='aqua', alpha=0.5)
# add a curve
y = mlab.normpdf(bins, mu1, sigma1)
plt.plot(bins, y, 'b--')
# the histogram of the number of dogs with name
n, bins, patches = plt.hist(with_name, num_bins, normed=1, facecolor='coral', alpha=0.5)
# add a curve
y = mlab.normpdf(bins, mu2, sigma2)
plt.plot(bins, y, 'r--')
plt.xlabel('Number of Likes')
plt.title("The impact of having a name")
plt.show()
```


![png](output_102_0.png)


# Insights

* The 'likes' number of the tweet is increasing dramatically over the time. From the visualization we can see that it is going up with the 'J' shape.

* The number of retweets is also growing, however, not as fast as the number of 'likes'. This is probably because more and more people start to click 'likes' without retweeting.

* Although we can see a little difference between the likes number of the named dogs and unnamed dogs, it is not significant at all. Thus probably giving your dog a name is not necessary to get more likes.

* The score and the number of likes have a possitive relation, which means the person who give the score has a similar tastes as his or her readers. However, it may also because the score has an impact on the readers.
