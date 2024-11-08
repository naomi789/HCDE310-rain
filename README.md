# Introduction to APIs and JSON
This document gives and overview of the idea of the API. It also includes a short discussion of JSON, and how it fits in with and/or is useful with APIs.

API stands for Application Programming Interface. Just as a User Interface (UI) allows users to interact with a program, a program’s API allows another program to interact with it. For example, just as a user would either click on the “x” icon to close a window, or click on “File menu → Close,” program A interacting with another program B may call a function called `window.close()` to close a window of program B. In this case, `window.close()` is a part of the API of program B.

# An example API
For an example, let’s consider extensions for web browsers. As you may already know, web browser applications such as Chrome, Edge, Firefox, and Safari allows software developers to write smaller programs called extensions that can potentially make the user experience of using a browser better or more customized. For the Firefox browser, you can look at [all the available extensions on this website](https://addons.mozilla.org/en-US/firefox/extensions/), and for Chrome extensions, [the Chrome web store has a section](https://chromewebstore.google.com/category/extensions) dedicated to extensions.

A popular type of extension that exists for all browsers is ad-blockers, which prevent ads from being shown in web pages. So if you have an ad-blocker extension installed, you will not see (or see less) advertisements on the websites that you visit. In this case the ad-blocker extension **program** is communicating with the web browser **application** through an **interface**. Whenever you are loading a new web page in the browser, the ad-blocker extension program is calling a function that is returning the content of the web-page you are visiting, and then changing this content by filtering out ads—most ad-blockers use some kind of pre-defined list to do this.[^1]

An important thing to note here is that extensions are typically developed by individuals or small teams, they are usually not created by the developers of the web browsers. What the developers of these web browsers do create is a set of well-documented functions. A function in this set may return the web-page that the browser’s user might be visiting, another function may allow the content of the web-page to be updated somehow, and so on. This collection of functions for a part of the Application Programming Interface or API of the web-browser. Extension developers read these functions’ documentation, which is often referred to as the API documentation, and then decide which ones they need to use. For example, there is a [function in the extensions API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/windows/remove) called `windows.remove()`. that lets an extension close a browser window. The programming language for the extensions API, including `windows.remove()`, is JavaScript, so do not worry if the syntax looks odd.

Thus, in this case, the API includes a set of (usually well-documented) functions. However, there can be some additional parts that make up an API. For example, in this case, the web-browser API also includes classes that might represent various parts of the browser—such as a browser window, or a tab. Each class will have a set of attributes and methods. For example, a `Window` class may have [attributes such as height and width](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/windows/Window). An instance of `Window` will have these attributes set to the height and width taken up by that particular browser window on the computer’s screen.

The functions, as well as the classes (including the attributes and methods associated with those classes) constitute the full API, and the documentation of all these functions and classes constitute the _API documentation_, which is also sometimes referred to as the _API specification_. When you plan to use a given API, skimming through the API documentation before starting to code is a good idea.

Note: An API does not need to include functions and classes. It can be made out of only functions, or only classes. In the above case, the API contains both functions and classes, but that need not be the case for all APIs.

# Another example API
The “application” part of API is sometimes a little loosely defined/used. For example, consider the `json` module in Python ([documentation for that module](https://docs.python.org/3/library/json.html)). We have seen and used this module before, and we already are familiar with functions such as `json.loads()`. If you look at the `json` module’s documentationLinks to an external site. in the Python website, you will notice the following line:

json exposes an API familiar to users of the standard library marshal and pickle modules.

This is somewhat confusing, as json is not an application in the sense of the term we have seen so far. It is a module that you can import and then use in your Python programs. However, what json does represent is a collection of functions doing related things (working with JSON data). All those functions make up the API of the Python json module. Thus, when you hear someone talk about the API of a particular Python module, they are talking about the functions and classes that are a part of that module.

# Web APIs
The idea of an API also applies to web-sites. Many modern websites are full-fledged software applications; the two major distinctions with applications on your computer being that: 
 - rather that running on your computer, they run on other computers (servers) connected to the Internet
 - you typically interact with them with a web-browser over the Internet.

For example, when you log into Canvas for this course, you are interacting with a software application that is connected to a database that has all the material related to this course, as well as information about you, your assignment submissions, and more.

Building on the idea of an API that we saw earlier, a web API, therefore, lets you write programs that can interact with a website, over the Internet. In the rest of this section, we will look at a few ways in which web APIS work, and how you can work with them from your Python code.

## A simple web API example: Is it raining in Seattle?
“Functions” in web APIs are usually implemented as web-addresses or URLs. Accessing the URL will give you the “return value” of that function. As an example, let’s look at a very simple web API, Is it raining in Seattle? All this API does is return `True` or `False` based on whether it is currently raining in Seattle or not. So, there is only one “function” for this web API, and helpfully, this function is called `is_it_raining_in_seattle()`. Since this is web API, in order to “call” the “function,” you need to access a URL. In this case, the URL is [https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/](https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/). If you open it up with a browser, you will see a `true` or a `false`. Alternatively, you can use the following Python code-snippet (using `urllib` - [documentation for urllib](https://docs.python.org/3/howto/urllib2.html)) to access and use the API function call:

``` import urllib.request

with urllib.request.urlopen('https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/') as response:
    is_it_raining_in_seattle = response.read().decode()

if is_it_raining_in_seattle == "true":
    print("Yes!")
else:
    print("No!")
```

From the example above, we can see that a “function” in a web API is a specific URL (this mapping is usually defined in the web API documentation), and when one accesses the URL from either a web-browser or Python code, the “return value” of the “function” is obtained. It is possible, and common to hide these details. For example, you can write a Python module with a function called `is_it_raining_in_seattle()` that uses the API described above.

```import urllib.request

def is_it_raining_in_seattle():
    with urllib.request.urlopen('https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/') as response:
        is_it_raining_in_seattle = response.read().decode()

    if is_it_raining_in_seattle == "true":
        return True
    else:
        return False
```

If you share this module with your friend or colleague, they will not even have to know anything about the Is it raining in Seattle? API or urllib. All they would need to know is that there is a Python function called `is_it_raining_in_seattle()` which returns `True` if it is currently raining in Seattle, and `False` if not.

The above approach of “hiding away” some details of working with web APIs through a module is quite common. You can download and install Python modules that hide away the details of working with the web APIs for the [Internet Movie Database (IMDB)](https://cinemagoer.github.io/), [Reddit](https://praw.readthedocs.io/en/stable/), [Spotify](https://spotipy.readthedocs.io/en/2.22.1/), and more. This process of “hiding away” is also called “wrapping,” and these modules are often called “wrappers” for the specific web API that they work with.

# A slightly more featureful web API: Astronomy Picture of the Day
A web API with a single function is not very useful. Most web APIs have multiple functions, or even if it is an API with a single function, the function usually accepts some kind of “arguments” to make it more useful.

As an example, let’s look at the API for NASA’s [Astronomy Picture of the Day (APOD)](https://apod.nasa.gov/apod/astropix.html), a web application that shows a picture of our universe along with a description written by an astronomer. The picture changes every day. The most basic way to use this API is to access the URL: [https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY](https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY). This returns information about the current picture of the day, for example:



```
{"copyright":"Lyman Insley","date":"2023-02-06","explanation":"In the heart of the Rosette Nebula lies a bright cluster of stars that lights up the nebula.  The stars of NGC 2244 formed from the surrounding gas only a few million years ago.  The featured image taken in mid-January using multiple exposures and very specific colors of Sulfur (shaded red), Hydrogen (green), and Oxygen (blue), captures the central region in tremendous detail. A hot wind of particles streams away from the cluster stars and contributes to an already complex menagerie of gas and dust filaments while slowly evacuating the cluster center.  The Rosette Nebula's center measures about 50 light-years across, lies about 5,200 light-years away, and is visible with binoculars towards the constellation of the Unicorn (Monoceros).   Your Sky Surprise: What picture did APOD feature on your birthday? (post 1995)","hdurl":"https://apod.nasa.gov/apod/image/2302/Rosette_Insley_3424.jpg","media_type":"image","service_version":"v1","title":"In the Heart of the Rosette Nebula","url":"https://apod.nasa.gov/apod/image/2302/Rosette_Insley_960.jpg"}
```

While the call is very similar to what we used for the Is it raining in Seattle? API, there is a small change. This API call needs an additional argument, called the API key. For many web APIs, these arguments are specified at the end of the URL, in the portion that starts with a `?`. Each argument is specified as a key value pair, each pair being separated by `&`, in the format `?argname1=argvalue1&argname2=argvalue2&argname3=argvalue3`. In this case, `?api_key=DEMO_KEY` tells us that we are calling the APOD API function with an argument name `api_key` whose value is `DEMO_KEY`. The function of the `api_key` is to help the web application recognize who you are, and keep track of how much you are using the API (which can then be used for billing you, or limiting your access, especially if the API is a paid service). In this case, we are using the default “demo” API key (`DEMO_KEY`), but if you plan to use this API for anything other than a tutorial, you should [sign up for an API key](https://api.nasa.gov/#signUp).

The general format of the URL to call the APOD API’s basic function that we saw above is `https://api.nasa.gov/planetary/apod?api_key=<api_key>`. This can be thought of as a Python function called `get_apod(api_key)`. In fact, as we saw with the Is it raining in Seattle? API, we can implement this function ourselves:
```
import urllib.request

def get_apod(api_key):
    url = "https://api.nasa.gov/planetary/apod?api_key={}".format(api_key)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    return response

# Test code
print(get_apod("DEMO_KEY"))
```

## Complex data: JSON to the rescue
Compared to the Is it raining in Seattle?, the “return value” for the APOD API is more complicated. This is where JSON becomes useful.

JSON is a format where objects with attributes-value or key-value pairs can be represented as strings. A lot of programming languages can turn a JSON string into a data structure that supports key-value pairs—in the case of Python, the json module can turn JSON into dictionaries. For example, the JSON string:

```
{"a": 1, "b": 3}
```

can be converted to a dictionary as the following Python code shows:

```
import json

json_string = '{"a": 1, "b": 3}'
parsed_data = json.loads(json_string)

for key in parsed_data:
    print("{}:{}".format(key, parsed_data[key]))

# Prints:  
# a:1
# b:3
```

Note: It is also possible for JSON to represent a collection of objects, rather than a single object, which can then be turned into a Python list of dictionaries by the json module. We will soon see how.

Going back to the APOD API, for our `get_apod()` function, we can parse the returned data with `json.loads()` to make it more usable in our program.

```
import json
import urllib.request

def get_apod(api_key):
    url = "https://api.nasa.gov/planetary/apod?api_key={}".format(api_key)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    # Change to return a dictionary, rather
    # than the "raw" JSON
    return json.loads(response)

# Test code
apod_dict = get_apod("DEMO_KEY")
print(apod_dict.["title"])
# Should print the title of today's photo
```

## From JSON to dictionaries to objects
A common thing that is done with the dictionaries that we get from APIs is to then create objects, with values in the dictionary being the attributes of the object. For example, if we define a class called `AstroPhoto`, the value of the title key of the dictionary returned by `get_apod()` can be the value of the title attribute of an instance, the value of the date key can be value of the date attribute, and so on. Note that the key name of the dictionary does not need to be exactly same the attribute name, nor do all keys need to have corresponding attributes. It is up to us, the programmer, to make those decisions. For example:

```
import json
import urllib.request

class AstroPhoto:
    def __init__(self, data):
        self.title = data["title"]
        self.date = data["date"]
        self.description = data["explanation"]
        self.url = data["hdurl"]

def get_apod(api_key):
    url = "https://api.nasa.gov/planetary/apod?api_key={}".format(api_key)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    # Further change to return an instance of 
    # AstroPhoto, rather than a dictionary
    return AstroPhoto(json.loads(response))

# Test code
apod_instance = get_apod("DEMO_KEY")
print(apod_instance.title)
# Should print the title of today's photo 
# but notice the changed syntax. Instead of
# accessing a key value, we are accessing an
# attribute
```
Of course, a class with a few attributes is not very much different from a dictionary; to use the full potential of object-oriented programming, we need methods. We can add a method called `.get_short_description()` for a shorter description (or explanation) of the photo. The `.get_short_description()` method returns the entire description if it is less than or equal to 200 characters long, and if not, it returns a truncated description that is 197 characters long, followed by “…”.

```
import json
import urllib.request

class AstroPhoto:
    def __init__(self, data):
        self.title = data["title"]
        self.date = data["date"]
        self.description = data["explanation"]
        self.url = data["hdurl"]

    # new method
    def get_short_description(self):
        if len(self.description) <= 200:
            return self.description
        else:
            return (self.description[0:197] + "...")

def get_apod(api_key):
    url = "https://api.nasa.gov/planetary/apod?api_key={}".format(api_key)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    return AstroPhoto(json.loads(response))

# Test code
apod_instance = get_apod("DEMO_KEY")
print("Title: {}".format(apod_instance.title))
print("Description: {}".format(apod_instance.get_short_description()))
```

## Doing more with the APOD API
The APOD API can do more. The table of parameter names and their descriptions is shown below (copied from the [NASA API website](https://api.nasa.gov/#browseAPI)) 

|  Parameter |    Type    |  Default |                                                          Description                                                          |   |
|:----------:|:----------:|:--------:|:-----------------------------------------------------------------------------------------------------------------------------:|---|
| date       | YYYY-MM-DD | today    | The date of the APOD image to retrieve                                                                                        |   |
| start_date | YYYY-MM-DD | none     | The start of a date range, when requesting date for a range of dates. Cannot be used with date.                               |   |
| end_date   | YYYY-MM-DD | today    | The end of the date range, when used with start_date.                                                                         |   |
| count      | int        | none     | If this is specified then count randomly chosen images will be returned. Cannot be used with date or start_date and end_date. |   |
| thumbs     | bool       | False    | Return the URL of video thumbnail. If an APOD is not a video, this parameter is ignored.                                      |   |
| api_key    | string     | DEMO_KEY | api.nasa.gov key for expanded usage                                                                                           |   |

This suggests that, for example, we can get the APOD for any past date, and not just for today by adding a parameter called date to our URL. The format of the date value is specified as `YYYY-MM-DD`, so the URL for getting the APOD for 31st December 2022 would be `https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2022-12-31`. We can extend our previous `get_apod()` function definition to support the new parameter (Note: I removed the rest of the code that we wrote earlier to focus on what’s important here):

```
def get_apod(api_key, date):
    url = "https://api.nasa.gov/planetary/apod?api_key={}&date={}".format(api_key, date)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    return AstroPhoto(json.loads(response))

# Test code
# Should print:
# Title: Moon over Makemake
apod_instance = get_apod("DEMO_KEY", "2022-12-31")
print("Title: {}".format(apod_instance.title))
```

We can even get multiple APODs, either a random selection (with the count parameter), or within a specific date range (with the start_date and end_date parameters). Let’s extend our code to use the count parameter. We can start with figuring out the URL, and then trying it out on the web browser. For the URL `https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&count=2`, we get back the following data (yours might look different as this returns randomly chosen photos):

```
[{"date":"1999-08-12","explanation":"Last October the Space Shuttle Discovery deployed Spartan 201, a spacecraft that monitored the corona of the Sun.  Instruments on Spartan 201 were used to estimate the density of electrons emitted into the solar corona, calibrate data from the Solar and Heliospheric Observatory (SOHO) satellite, and study how the Sun is changing as it reaches maximum activity over the next few years.  Pictured above, the space shuttle's robot arm (top left) releases Spartan (center) into space.  The tail fin of the space shuttle is visible on the right, while the Earth hovers in the background.  Spartan floated near the shuttle for two days before it was picked up again and returned to Earth.","hdurl":"https://apod.nasa.gov/apod/image/9908/spartan201_sts95_big.jpg","media_type":"image","service_version":"v1","title":"Deploying Spartan","url":"https://apod.nasa.gov/apod/image/9908/spartan201_sts95.jpg"},{"copyright":"T. Rector","date":"2007-03-21","explanation":"It may look to some like a duck, but it lays stars instead of eggs.  In the center of the above image lies Barnard 163, a nebula of molecular gas and dust so thick that visible light can't shine through it.   With a wing span measured in light years, Barnard 163's insides are surely colder than its exterior, allowing conditions where gas can clump and eventually form stars.  Barnard 163 lies about 3,000 light years from Earth toward the constellation of Cepheus the King. The red glow in the background results from IC 1396, a large emission nebula that houses the Elephant's Trunk Nebula.  Finding Barnard 163 in an image of its greater emission nebula IC 1396 can be a challenge, but it's possible.    News: Night and Day are Equal: Today is an Equinox","hdurl":"https://apod.nasa.gov/apod/image/0703/barnard163_noao_big.jpg","media_type":"image","service_version":"v1","title":"Molecular Cloud Barnard 163","url":"https://apod.nasa.gov/apod/image/0703/barnard163_noao.jpg"}]
```

If you look carefully at the JSON that get back from the API, you will notice that rather than one dictionary, we get a collection back (indicated by the `[` and `]` pair at the start and end of the string), which json can turn into a list of dictionaries. However, our code so far relies on the assumption that `get_apod()` returns a single dictionary. One approach to address this is to simply create a new function, called `get_random_apods(api_key, count)` that will return count number of randomly chosen AstroPhoto instances (as with earlier, only the function definition is shown below):

```
def get_random_apods(api_key, count):
    url = "https://api.nasa.gov/planetary/apod?api_key={}&count={}".format(api_key, count)
    with urllib.request.urlopen(url) as request:
        response = request.read().decode()

    photos = []
    
    for item in json.loads(response):
        # Each item in the list is an APOD dictionary
        # We create a new instance of AstroPhoto using
        # the dictionary, and then append it to photos
        photos.append(AstroPhoto(item))
    
    return photos

# Test code
# Should print 2
apods = get_random_apods("DEMO_KEY", 2)
print(len(apods))
```

# Exercises
Learning to program takes practice. In order to sharpen your skills, you may want to try out some of the following exercises. These are for your own practice, we will not grade them. Of course, if you have questions, feel free to meet one of us during office hours.

## Exercise 1: Get APODs between two dates
Similar to the `get_random_apods()` function that was defined above, can you create a function called `get_apods_between(api_key, start_date, end_date)` that returns all the APODs between two dates. Once you write the function, try it out for the range between 1st January 2023 and 7th January 2023.

## Exercise 2: Working with a new Web API: The Zippopotamus API
The [Zippopotamus API](https://api.zippopotam.us/) lets you look up information about zip codes. Let’s say we want to look up information about the zip code 98105. The URL for doing that with the Zippopotamus API is [https://api.zippopotam.us/us/98105](https://api.zippopotam.us/us/98105). Note how the parameters in this case are passed slightly different compared to the APOD API. The general format is `https://api.zippopotam.us/<countrycode>/<zipcode>`, rather than `https://api.zippopotam.us/?countrycode=<countrycode>&zipcode=<zipcode>`. This is another way of sending parameter values to API calls.

Exercise 2a: Implement a function to get information about a specific zip code
Accessing a URL of the format `https://api.zippopotam.us/<countrycode>/<zipcode>` can be thought as calling a function called `zipcode_info(countrycode, zipcode)`. Implement this wrapper function in Python. The function should return the JSON string returned by the API.

Exercise 2b: Turning the API data into objects
Once you have implemented the `zipcode_info()` function, calling zipcode_info("US", 98105) would return the following JSON string:

{"post code": "98105", "country": "United States", "country abbreviation": "US", "places": [{"place name": "Seattle", "longitude": "-122.3022", "state": "Washington", "state abbreviation": "WA", "latitude": "47.6633"}]}
The useful information in this JSON string is the value of the places key, which is a list of dictionaries. Each dictionary in this key represents a place.

For the first step in this exercise, implement a class called Place, with the following attributes

 - longitude
 - latitude
 - name
 - state

Next, modify the zipcode_info() function to return a list of instances of `Place` instead of a JSON string. The list for `zipcode_info("US", 98105)` will be only 1 element long, but you can test your code with `zipcode_info("US", 02861)`, which should return two Place instances, one in Massachusetts, and the other one in Rhode Island. This zip code is one of the few that [span multiple states](https://gis.stackexchange.com/questions/53918/determining-which-us-zipcodes-map-to-more-than-one-state-or-more-than-one-city/167333#167333).

# Additional readings on APIs
 - [What is an API?](https://18f.gsa.gov/2016/04/22/what-is-an-api/)
 - [Wikipedia article on an API](https://en.wikipedia.org/wiki/API)



1. Note that this is a somewhat simplified version of what actually happens, but those details are not really relevant here.
