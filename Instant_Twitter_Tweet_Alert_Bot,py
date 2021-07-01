import os
from dotenv import load_dotenv
load_dotenv()
import requests
import json
import tweepy

# Getting data from .env file
token = os.getenv("token")
consumer_key = os.getenv("consumer_key")
consumer_secret = os.getenv("consumer_secret")
access_token = os.getenv("access_token")
access_token_secret = os.getenv("access_token_secret")
twitter_UserName = os.getenv("twitter_UserName")

botsUrl = "https://api.telegram.org/bot{}".format(token)

# Authenticating Twitter using twitter API
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Sending message to Telegram Bot users using Telegram API
def sendMessage(msg, chat_id):
    url = botsUrl+ "/sendMessage?chat_id={}&text={}".format(chat_id, msg)
    resp = requests.get(url)

while True:
    # Getting the last tweet object of Twitter user
    try:
        temp = [x.id_str for x in tweepy.Cursor(api.user_timeline, id = "@{}".format(twitter_UserName)).items(1)]
        temp = temp[0]
    except:
        print("An error occurred in initiating")

    # Searching New Tweets and if found new tweets sending the link of new tweet to the Bot users
    while True:
        try:
            for x in tweepy.Cursor(api.user_timeline, id = "@{}".format(twitter_UserName), since_id = temp).items():
                # In here I stored the telegram ids of Users and Telegram Channel Manually in the array. You can get the user ids 
                # of all the Telegam users using get method of Telegram api ,and storing in Database, and then retriving it here
                telegram_user_ids = ["1137250020", "1387370461"]
                for chat_id in telegram_user_ids:
                    try:
                        sendMessage("https://twitter.com/{}/status/{}".format(x.user.screen_name, x.id), chat_id)
                    except:
                        print("An exception occurred")
                temp = x.id_str
        except:
            print("An error occurred")
