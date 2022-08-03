# Weratedogs-twitter-analysis

This project's aim is to gather data from the Weratedogs Twitter page to create analysis about the tweets and the predicted dog’s breed.

The data wrangling process follows the following 3 steps:
1. Data Gathering 
  - The WeRateDogs Twitter archive
  - The tweet image predictions
  - Additional data from the Twitter API
2. Assessing Data 
3. Cleaning Data 

## 1. Data Gathering 

- The WeRateDogs Twitter Enhanced Archive was provided by Udacity. 
The twitter_archive_enhanced.csv file contains basic tweet data (tweet ID, timestamp, dog stage, doge name etc.) with 2356 and 17 columns. This is tweet information as they stood on August 1, 2017

- The tweet image predictions is hosted on Udacity's servers and was downloaded programmatically using the Requests library 

- Additional data (retweet and favourite count of tweets) was scraped from Twitter's API using the Tweepy library. Tweepy is an easy to use Python library for accessing the Twitter API. 
The retweet and favourite counts were gotten by querying the tweet_ids present in twitter_archive_enhanced.csv. The data is stored in tweet_json.text. 

**The gathered data are loaded into 3 different DataFrames**
- twitter_archive : Loaded data from twitter_archive_enhanced.csv
- image_pred : Loaded data from image_predictions.tsv
- retweet_favourite_count : Loaded data from tweet_json.txt


## 2. Data Assessing

The Datasets were assessed using wo types of assessment:
- Visual assessment: each piece of gathered data is displayed in the Jupyter Notebook for visual assessment purposes. Once displayed, data was additionally assessed in Excel and text editor. 
- Programmatic assessment: pandas' functions and methods were used to assess the data.

Summary of issues found after assessing the Datasets 

|Problem Type      | Dataset | Problem|
| :---       | :---: | :---: |
|Quality            |	```twitter_archive, image_pred``` |Incorrect data type issues - tweet_id should be object not integers or floats because they are not numeric and aren't intended to perform calculations,  rating_denominator and rating_numerator should be floats not integers since there is nothing stopping future dog ratings from having a number with a decimal either in the denominator or numerator, tweet_id column in image prediction should be an object (string) not integers or floats because they are not numeric and will not be used for performing calculations, timestamp field should be a datetime not object.
|Quality | ```twitter_archive``` | Too many Missing values in columns: in_reply_to_status_id, in_reply_to_user_id, and expanded_urls. 
|Quality | ```twitter_archive``` | Too many duplicate values in column: source
|Quality | ```twitter_archive``` | +0000 at the end of timestamp is not necessary.
|Qaulity |```image_pred``` | jpg_url column is making the dataset not look neat - unnecessary for analysis. 
|Quality |```twitter_archive``` | Many missing names from the 'name' column: 'None', and random names like 'a', 'an'
|Quality |```twitter_archive``` |  Redundant retweet rows. Remove values in the retweeted_status_id, retweeted_status_user_id, and retweeted_status_timestamp columns 
|Quality |```twitter_archive``` | Some rating_numerators were extracted incorrectly
|Tidiness | ```twitter_archive``` | Each column for the dog stage should be merged into one column as 'dog_stage'
|Tidiness | ```twitter_archive``` | Data does not need to be split over three datasets - the other two datasets should be joined to ```twitter_archive```

## 3. Data Cleaning
Before running any cleaning operations, I have copied all the three DataFrames using .copy() method- 

```twitter_archive_clean = twitter_archive.copy()```

```image_pred_clean = image_pred.copy()```

```retweet_favourite_count_clean = retweet_favourite_count.copy()```

### Cleaning Steps 
- tweet_id should be object, rating_denominator and rating_numerator should be floats not integers, tweet_id column in image prediction should be an object (string), timestamp field should be a datetime not object.
- Dropped all the columns with missing values
- rating_denominator and rating_numerator were changed to float data type 
- Extracted rating_numerators correctly and changed data type to float 
- Source column was dropped because we don't need it for analysis
- Removed +0000 from the timestamp and changed it to datetime format 
- Removed any names with small letters as they are most likely not correct from the 'name' column
- jpg_url column was dropped 
- Dog stages were merged into a single column 

After cleaning, ```twitter_archive_clean, image_pred and retweet_favourite_count ``` were merged to form a master dataset.

The data has been assessed and cleaned so it ready for analysis. I combined the 3 datasets into ```twitter_archive_master.csv```
This master dataset is not completely free of quality issues because data wrangling is an iterative process so it is still possible for more cleaning to be done. 
The cleaned dataset has **1896** rows and **19** columns :

|Column      | Description|
| :---       | :---: |
|tweet_id            |	ID of tweet | 
|timestamp | Time and date the tweet was sent | 
|text | The text information contained in tweet|
|rating_numerator | Rating numerator of tweet |
|rating_denominator  | Rating denominator of tweet |
|name | Name of the dog |
|dog_stage | Dog stage |
|retweet_count | Number of retweets |
|favorite_count | Number of favourites |
|img_num | number of images in the tweet | 
|p1, p2, & p3 | 1st, 2nd and 3rd predictions for the image in the tweet | 
|p1_conf, p2_conf, & p3_conf | Confidence levels of all predictions | 
|p1_dog, p2_dog, & p3_dog | Bool - True or False (True if image prediction is correct, False if image prediction is wrong |


I analyzed and visualized my wrangled data in the ``` wrangle_act.ipynb ``` Jupyter Notebook.
