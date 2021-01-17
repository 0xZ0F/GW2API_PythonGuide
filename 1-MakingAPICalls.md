If you haven't done so already, please read [README.md](README.md)

# Part 1, Making API Calls
To start, let's just make a quick API call. Once again, be sure you have installed the "requests" module with `pip install requests`.

Here's what we'll do:

```python
import requests

# Make API request and store result in req:
req = requests.get("https://api.guildwars2.com/v2/achievements?ids=1840,910")
# Print the response:
print(req.text)
```

## The URL
When making a call to the API we'll do it to "https://api.guildwars2.com/v2/". Once again, a list of API calls is on the WIKI. Looking at the URL in the code you can see it's requesting something to do with achievements. The `ids=1840,910` are the specific achievement ID's I want. In this case, it's the ID for the daily PvE achievement and a Tequatl achievement. You can get a list of all achievement ids by simply calling "https://api.guildwars2.com/v2/achievements".


Let's step through this.

* First, import the libraries we'll be using. "JSON" is for parsing JSON later on, and "requests" is for making our API request.
* Next, request the API with `requests.get()`. Here we're passing in the URL of what we want from the API. The API will respond with data in the form of JSON.

That's it! The API will respond with JSON data of our request which will look something like this:

```JSON
[
  {
    "id": 1840,
    "name": "Daily Completionist",
    "description": "",
    "requirement": "Complete any  PvE, WvW, or PvP Daily Achievements.",
    "locked_text": "",
    "type": "Default",
    "flags": [
      "Pvp",
      "CategoryDisplay",
      "Daily"
    ],
    "tiers": [
      {
        "count": 3,
        "points": 10
      }
    ],
    "rewards": [
      {
        "type": "Item",
        "id": 70047,
        "count": 1
      },
      {
        "type": "Coins",
        "count": 20000
      }
    ]
  },
  {
    "id": 910,
    "name": "Tequatl the Sunless",
    "description": "",
    "requirement": "Complete  Tequatl achievement.",
    "locked_text": "",
    "type": "Default",
    "flags": [
      "CategoryDisplay",
      "Permanent"
    ],
    "tiers": [
      {
        "count": 10,
        "points": 50
      }
    ],
    "rewards": [
      {
        "type": "Mastery",
        "region": "Tyria",
        "id": 247
      },
      {
        "type": "Title",
        "id": 175
      }
    ]
  }
]
```

As you can see, we get loads of information back. Now what? Well, time for some data parsing! Don't worry, it's easier than you think due to a nice library called "JSON".

Onwards to [2-ParsingResponses](2-ParsingResponses.md)!