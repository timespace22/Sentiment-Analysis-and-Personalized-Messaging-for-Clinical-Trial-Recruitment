# Sentiment Analysis and Personalized Messaging for Clinical Trial Recruitment
# Name: Ajeesh Ajayan Nayaruparambil

## Objective:
The aim of the project is to:
1. Ethically scrape and analyze reddit posts and comments relevant to clinical trials.
2. Analyze the Sentiment of the post/comment by the user.
3. Generate a personalized message using AI to convince/motivate the user to participate in the clinical trials.

## Setup:
This code is written in Python (v3.11.9).
Installation of the packages and their corresponding versions are needed:

 praw  = 7.7.1;
 nltk  = 3.8.1;
 openai = 1.28.1

VADER is used for Sentiment Analysis.
Download the VADER lexicon using the following snippet: 
>> nltk.download('vader_lexicon')  

We are using PRAW for scraping Reddit. Link for Setup:
https://praw.readthedocs.io/en/stable/getting_started/quick_start.html

Steps to setup PRAW in brief:
1. Create a Reddit account.
2. Go to app preferences (https://www.reddit.com/prefs/apps) and create an app.
3. Get all the required credentials: client_id, client_secret, password, user_agent, user_name. These credentials are needed for initializing PRAW.

We are using OpenAI API for generating personalized messages: 
Steps for setting up the API:
1. Create an OpenAI account on OpenAI platform (https://platform.openai.com/docs/overview).
2. Create an API key. This will be used as the key for our API calls. *Paid OpenAI account is required*

## Methodology:
### Scraping the Posts and Comments from Reddit using PRAW about clinical trials:
1. Access the subreddit "all" having posts and comments from all of Reddit.
2. Search the subreddit for posts having keyword "clinical trials".
3. Sort it by the number of upvotes the post received in the descending order.
4. For each post, there would be some comments and for each comments there would be some replies and so on. Create a dictionary of key = posts and values = list of comments and replies with each comment/reply referencing the parent post/comment.

Data is in abundance. Even for a single post, we get around 30-40 comments on average. 

Eg:
Post: "Paid Clinical Trials Just wondering if anyone has had any experience with these and if they’re good value for money. Definitely sounds enticing earning a few thousand dollars for a few weeks stay in a hospital"
Comment: "Have done one got 3k for 2 days. \n\nHave to be healthy to do it. They took a metric tonne of blood. I did not enjoy the amount of blood letting. You get different amount of money fir how long and risky the trial is. Spent two night in inner city Auckland. No coffee for about a month and no sex or alcohol. So it's quite a big commitment, plus usually a few months if follow ups. \n\nAsk me anything else\n\nEdit. \n\nAlso no coffee drinking. They had sn issue with people having withdrawals from caffeine. Made the trials difficult due to them not knowing if dude affects were from drugs or symptoms. \n\nCompletely forgot. You get 3 meals and a snack, you get exact calories for food. No deviation and must piss in a special bucket that they give you. I drank loads of water so blood was easy to give. Pretty much couldn't get any blood on the third day."


### Analyzing Sentiment of the post/comment using VADER Sentiment Intensity Analyzer
1. For each post/comment, use VADER to get their polarity scores. 
2.  Segmenting the scores according to the following:

    less than -0.5 = Very Negative;
    between -0.5 and -0.05 = Negative;
    between -0.05 and 0.05 = Neutral;
    between 0.05 and 0.5 = Positive;
    greater than 0.5 = Very Positive

Eg:
For the above example Post: 
{'neg': 0.0, 'neu': 0.789, 'pos': 0.211, 'compound': 0.7906}
For the above example Comment:
{'neg': 0.086, 'neu': 0.807, 'pos': 0.106, 'compound': 0.7363}

These values are the sentiment intensity of the user.

### Generating personalized reply to the users based on the corresponding segments to motivate participation in the clinical trial
1. For each post/comment, we ask ChatGPT to convince them based on a prompt prepared for the corresponding segment. Eg: For "Very Negative" comments, we ask ChatGPT to: "Write a personalized message for this user convincing them to consider the clinical trial, addressing their concerns."
2. We append the replies to the post/comments and the user name so that it would be easier to track and send the reply to the accurate user.

Eg:
I didn't have a paid OpenAI account and hence whenever I was trying to run the code, it threw the "RateLimitError: Error code: 429". Please run through a paid OpenAI account. 

### Ethical Considerations and User Engagement:
1. Ensured compliance with Reddit's API terms of service and adhered to their guidelines on data usage and privacy.
2. Used user_agent string while scraping to maintain transparency.
3. PRAW only extracts from public subreddits ensuring privacy.
4. Encouraged meaningful engagement by offering valuable and relevant messages that aligns with users' interests and needs.
5. Provided transparent and accurate information about the trials being promoted.



