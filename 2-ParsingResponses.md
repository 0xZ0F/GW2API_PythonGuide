# Part 2, Parsing Responses
Now let's take the data from part 1 and get what we want from it. To do this, we'll use a library called "JSON" which comes with Python.

Here's a portion of the JSON data from part 1:

```JSON
[
  {
    "id": 1840,
    "name": "Daily Completionist",
    "requirement": "Complete any  PvE, WvW, or PvP Daily Achievements.",
    "flags": [
      "Pvp",
      "CategoryDisplay",
      "Daily"
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
    "requirement": "Complete  Tequatl achievement.",
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

Square brackets are for arrays, curly brackets are for objects, and data is in pairs. Since the request is for 2 achievements we get a response that is an array of 2 achievements, and both of them are classes that contain their data and achievements.

Let's start by getting the name of both of the achievements. Both of these achievements are in one big array, let's just call it the top-level array. This top-level array has 2 elements since it has 2 achievements. So if we wanted to access the first achievement we'd just access it similar to: `jsonData[0]`. The second would be `jsonData[1]`. It can't be that simple! Well, it sure is! Take a look at the following code:

```python
import JSON
import requests

# Make API request and store result in req:
req = requests.get("https://api.guildwars2.com/v2/achievements?ids=1840,910")
# Print the response:
print(req.text)

# Load the req text as JSON so we can parse it:
data = json.loads(req.text)
# Print the "name" value of the first achievement in the returned JSON.
print(data[0]["name"])
```

Here all that's happening is we're getting the data stored as "name" in the first achievement which is the PvE daily. When the code runs it prints "Daily Completionist".

* First we make a variable that will hold the JSON data. Using the JSON library we call `JSON.loads()` which loads JSON from a string. The `JSON.load()` (no "s") function loads JSON from a file.
* Print the first achievement's "name".

Easy! It's just an array!

What if we wanted to print the name of all returned achievements? Sure, we could write a print statement for every one of them, but what if there are hundreds? The answer is a simple little loop:

```python
import JSON
import requests

req = requests.get("https://api.guildwars2.com/v2/achievements?ids=1840,910")
print(req.text)

# Load the req text as JSON so we can parse it:
data = json.loads(req.text)

# For every achievement in the data, do:
for achievement in data:
    # Print the "name" value of the first element in the returned JSON.
    print("Name: " + achievement["name"])
```

There you go! Now we're printing the name of every achievement returned by the API!

Next up we'll take a look at getting a list of all of your bank items! Head on over to [3-GettingBankItems](3-GettingBankItems.md)