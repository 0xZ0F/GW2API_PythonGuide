# Part 3, Getting Bank Items
This is going to be one of the more involved pieces of code, but I'll try to break it down.

First of all, you will need your API token for this. You can generate one here:  
[https://account.arena.net/applications](https://account.arena.net/applications)

Let's take a look at the account/bank section of the WIKI which can be found here:  
[https://wiki.guildwars2.com/wiki/API:2/account/bank](https://wiki.guildwars2.com/wiki/API:2/account/bank)

At first, this seems trivial, but in fact, it's not going to be super straight forward. Unfortunately, the account/bank API request returns a list of items in the bank but doesn't give us their names, only their ID's. This means we'll have to find a way to resolve the ids to the name. Thankfully for us, there's a portion of the API specifically for getting item information via its ID which can be found here:  
[https://wiki.guildwars2.com/wiki/API:2/items](https://wiki.guildwars2.com/wiki/API:2/items)

So what we could do is request the items in our bank, then for every item lookup, it's ID using the above API call. However, looking up every item will be very slow, noticeably slow. Making web requests like this will result in a very bad user experience and an overall slow operation. So instead, we need should think of something else. Looking at the items API we can see a parameter called `ids` which will allow us to pass loads of IDs at once! Now, be careful, there is a limit of 200 IDs per request, but that won't cause any issues here.

So here are our steps:

* Get a list of items in our bank, this will return IDs which we must resolve.
  * To do this, pass your API key to "https://wiki.guildwars2.com/wiki/API:2/account/bank?access_token=".
* Resolve the IDs to their names 200 at a time.
  * To do this, request "https://wiki.guildwars2.com/wiki/API:2/items?ids=".
* Print out all item names, and any additional info we want.
  * To do this, loop over the response given by the previous API call and print out the "name" value for every item.

Alright, let's start. First, the request:

```python
req = requests.get("https://api.guildwars2.com/v2/account/bank", params=payload)
```

Then what we'll do is loop over all of the items and save the ID's to a string so we can send one big API request.

```python
# Loop over every item in the bank's JSON response for all item IDs. Collect them into one string:
reqJSON = json.loads(req.text)
idStr = ""
for item in reqJSON:        
    try:
        # Must be in #,#,# format for API call.
        idStr += str(item["id"]) + ","
    except:
        continue
```

That code loops over the returned JSON data of items extracting all of the IDs and appending them to one long comma delimited string which will be passed to the items API call.

Notice the try-except statement. This is because empty bank slots return null, therefore they don't have an "id" value. To prevent the program from crashing when it can't find "id", I threw it into a try-except statement. There are other ways to do this, but this is how I'm doing it.

Because a comma is being appended to every ID to create the comma-delimited list, the final ID will have a comma after it, but that will cause issues when we pass the list to the API. So we need to remove that.

```python
# Strip leading or trailing ","'s for API call syntax:
if(idStr[len(idStr) - 1] == ","):
    idStr = idStr.strip(",")
```

Now that we have the list of IDs, we need to pass them to the item API request.

```python
# Pass all item IDs to one API call:
print(idStr)
req = requests.get("https://api.guildwars2.com/v2/items?ids=" + idStr)
```

That will return the item data for every item in our bank! Let's print out the name and chat link (because why not) of every item.

```python
# Print out the name and chat link of every item resolved (the req var is now the items API request):
reqJSON = json.loads(req.text)
for item in reqJSON:
    try:
        print(item["name"] + " - " + item["chat_link"])
    except:
        continue
```

This is once again thrown into a try-except statement because for some reason I've had issues with it now and then.

That's it! Here's the full code:

```python
# Get bank items:
req = requests.get("https://api.guildwars2.com/v2/account/bank?access_token=ACCESS_TOKEN_HERE")

# Loop over every item in the bank's JSON response for all item IDs. Collect them into one string:
reqJSON = json.loads(req.text)
idStr = ""
for item in reqJSON:        
    try:
        # Must be in #,#,# format for API call.
        idStr += str(item["id"]) + ","
    except:
        continue

# Strip leading or trailing ","'s for API call syntax:
if(idStr[len(idStr) - 1] == ","):
    idStr = idStr.strip(",")

# Pass all item IDs to one API call (super fast!):
print(idStr)
req = requests.get("https://api.guildwars2.com/v2/items?ids=" + idStr)

# Print out the name and chat link of every item resolved:
reqJSON = json.loads(req.text)
for item in reqJSON:
    try:
        print(item["name"] + " - " + item["chat_link"])
    except:
        continue
```

There you go! If it's confusing, I assure you that if you write the code yourself it will make sense. It's never easy making your code make sense to others :)

# Final Notes
That's it for this short guide, it should get you started with the API. It's pretty straight forward, just takes some JSON parsing skills and looking through the WIKI. 

Have fun, happy programming!