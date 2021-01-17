# GW2API_PythonGuide
A quick guide to using the GW2 API in Python.

I was tinkering around one day and decided to play with the GW2 API. I couldn't find many helpful resources out there, thankfully it was a very intuitive API. With that said, I thought I'd throw a quick and brief guide together to help those who may have never used an API like this before.

### Requirements:
* Python
* Pip
* The "requests" and "JSON" modules. You'll need to install the "requests" module which is easy. Just run `pip install requests`.

That's it! Let's get started.

# Looking at the WIKI:
You're go-to when using GW2's API is going to be the following WIKI page:  
https://wiki.guildwars2.com/wiki/API:Main

If you ever need to know where to get information from the API, the WIKI is your guide.

## Authenticated and Unauthenticated
You need to pay attention to whether you need an API key or not. Most of the API does not require a key, but some does so pay attention! Things like guild and player details will require authentication, whereas general achievement, game, or item information will not. The WIKI will tell you if it needs a key.

### Once you're ready, head to [1-MakingAPICalls](1-MakingAPICalls.md)

# Getting Daily Achievements
For our first endeavor, let's take a look at getting a list of all of your bank items.