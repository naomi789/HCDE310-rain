# Introduction to APIs and JSON
This document gives and overview of the idea of the API. It also includes a short discussion of JSON, and how it fits in with and/or is useful with APIs.

API stands for Application Programming Interface. Just as a User Interface (UI) allows users to interact with a program, a program’s API allows another program to interact with it. For example, just as a user would either click on the “x” icon to close a window, or click on “File menu → Close,” program A interacting with another program B may call a function called `window.close()` to close a window of program B. In this case, `window.close()` is a part of the API of program B.

# An example API
For an example, let’s consider extensions for web browsers. As you may already know, web browser applications such as Chrome, Edge, Firefox, and Safari allows software developers to write smaller programs called extensions that can potentially make the user experience of using a browser better or more customized. For the Firefox browser, you can look at [all the available extensions on this website](https://addons.mozilla.org/en-US/firefox/extensions/), and for Chrome extensions, [the Chrome web store has a section](https://chromewebstore.google.com/category/extensions) dedicated to extensions.

A popular type of extension that exists for all browsers is ad-blockers, which prevent ads from being shown in web pages. So if you have an ad-blocker extension installed, you will not see (or see less) advertisements on the websites that you visit. In this case the ad-blocker extension **program** is communicating with the web browser **application** through an **interface**. Whenever you are loading a new web page in the browser, the ad-blocker extension program is calling a function that is returning the content of the web-page you are visiting, and then changing this content by filtering out ads—most ad-blockers use some kind of pre-defined list to do this.1

An important thing to note here is that extensions are typically developed by individuals or small teams, they are usually not created by the developers of the web browsers. What the developers of these web browsers do create is a set of well-documented functions. A function in this set may return the web-page that the browser’s user might be visiting, another function may allow the content of the web-page to be updated somehow, and so on. This collection of functions for a part of the Application Programming Interface or API of the web-browser. Extension developers read these functions’ documentation, which is often referred to as the API documentation, and then decide which ones they need to use. For example, there is a [function in the extensions API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/windows/remove) called `windows.remove()`. that lets an extension close a browser window. The programming language for the extensions API, including `windows.remove()`, is JavaScript, so do not worry if the syntax looks odd.

Thus, in this case, the API includes a set of (usually well-documented) functions. However, there can be some additional parts that make up an API. For example, in this case, the web-browser API also includes classes that might represent various parts of the browser—such as a browser window, or a tab. Each class will have a set of attributes and methods. For example, a `Window` class may have [attributes such as height and width](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/windows/Window). An instance of `Window` will have these attributes set to the height and width taken up by that particular browser window on the computer’s screen.

The functions, as well as the classes (including the attributes and methods associated with those classes) constitute the full API, and the documentation of all these functions and classes constitute the _API documentation_, which is also sometimes referred to as the _API specification_. When you plan to use a given API, skimming through the API documentation before starting to code is a good idea.

Note: An API does not need to include functions and classes. It can be made out of only functions, or only classes. In the above case, the API contains both functions and classes, but that need not be the case for all APIs.

# Another example API
The “application” part of API is sometimes a little loosely defined/used. For example, consider the `json` module in Python ([documentation for that module](https://docs.python.org/3/library/json.html)). We have seen and used this module before, and we already are familiar with functions such as `json.loads()`. If you look at the `json` module’s documentationLinks to an external site. in the Python website, you will notice the following line:

json exposes an API familiar to users of the standard library marshal and pickle modules.

This is somewhat confusing, as json is not an application in the sense of the term we have seen so far. It is a module that you can import and then use in your Python programs. However, what json does represent is a collection of functions doing related things (working with JSON data). All those functions make up the API of the Python json module. Thus, when you hear someone talk about the API of a particular Python module, they are talking about the functions and classes that are a part of that module.

# Web APIs
The idea of an API also applies to web-sites. Many modern websites are full-fledged software applications; the two major distinctions with applications on your computer being that (i) rather that running on your computer, they run on other computers (servers) connected to the Internet, and (ii) you typically interact with them with a web-browser over the Internet. For example, when you log into Canvas for this course, you are interacting with a software application that is connected to a database that has all the material related to this course, as well as information about you, your assignment submissions, and more.

Building on the idea of an API that we saw earlier, a web API, therefore, lets you write programs that can interact with a website, over the Internet. In the rest of this section, we will look at a few ways in which web APIS work, and how you can work with them from your Python code.

## A simple web API example: Is it raining in Seattle?
“Functions” in web APIs are usually implemented as web-addresses or URLs. Accessing the URL will give you the “return value” of that function. As an example, let’s look at a very simple web API, Is it raining in Seattle? All this API does is return `True` or `False` based on whether it is currently raining in Seattle or not. So, there is only one “function” for this web API, and helpfully, this function is called `is_it_raining_in_seattle()`. Since this is web API, in order to “call” the “function,” you need to access a URL. In this case, the URL is [https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/](https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/). If you open it up with a browser, you will see a `true` or a `false`. Alternatively, you can use the following Python code-snippet (using `urllib` - [documentation for urllib](https://docs.python.org/3/howto/urllib2.html)) to access and use the API function call:

``` import urllib.request

with urllib.request.urlopen('https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/') as response:
    is_it_raining_in_seattle = response.read().decode()

if is_it_raining_in_seattle == "true":
    print("Yes!")
else:
    print("No!") ```

From the example above, we can see that a “function” in a web API is a specific URL (this mapping is usually defined in the web API documentation), and when one accesses the URL from either a web-browser or Python code, the “return value” of the “function” is obtained. It is possible, and common to hide these details. For example, you can write a Python module with a function called `is_it_raining_in_seattle()` that uses the API described above.

```import urllib.request

def is_it_raining_in_seattle():
    with urllib.request.urlopen('https://depts.washington.edu/ledlab/teaching/is-it-raining-in-seattle/') as response:
        is_it_raining_in_seattle = response.read().decode()

    if is_it_raining_in_seattle == "true":
        return True
    else:
        return False ```

If you share this module with your friend or colleague, they will not even have to know anything about the Is it raining in Seattle? API or urllib. All they would need to know is that there is a Python function called `is_it_raining_in_seattle()` which returns `True` if it is currently raining in Seattle, and `False` if not.

The above approach of “hiding away” some details of working with web APIs through a module is quite common. You can download and install Python modules that hide away the details of working with the web APIs for the [Internet Movie Database (IMDB)](https://cinemagoer.github.io/), [Reddit](https://praw.readthedocs.io/en/stable/), [Spotify](https://spotipy.readthedocs.io/en/2.22.1/), and more. This process of “hiding away” is also called “wrapping,” and these modules are often called “wrappers” for the specific web API that they work with.

# A slightly more featureful web API: Astronomy Picture of the Day
A web API with a single function is not very useful. Most web APIs have multiple functions, or even if it is an API with a single function, the function usually accepts some kind of “arguments” to make it more useful.

As an example, let’s look at the API for NASA’s [Astronomy Picture of the Day (APOD)](https://apod.nasa.gov/apod/astropix.html), a web application that shows a picture of our universe along with a description written by an astronomer. The picture changes every day. The most basic way to use this API is to access the URL: [https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY](https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY). This returns information about the current picture of the day, for example:



``` {"copyright":"Lyman Insley","date":"2023-02-06","explanation":"In the heart of the Rosette Nebula lies a bright cluster of stars that lights up the nebula.  The stars of NGC 2244 formed from the surrounding gas only a few million years ago.  The featured image taken in mid-January using multiple exposures and very specific colors of Sulfur (shaded red), Hydrogen (green), and Oxygen (blue), captures the central region in tremendous detail. A hot wind of particles streams away from the cluster stars and contributes to an already complex menagerie of gas and dust filaments while slowly evacuating the cluster center.  The Rosette Nebula's center measures about 50 light-years across, lies about 5,200 light-years away, and is visible with binoculars towards the constellation of the Unicorn (Monoceros).   Your Sky Surprise: What picture did APOD feature on your birthday? (post 1995)","hdurl":"https://apod.nasa.gov/apod/image/2302/Rosette_Insley_3424.jpg","media_type":"image","service_version":"v1","title":"In the Heart of the Rosette Nebula","url":"https://apod.nasa.gov/apod/image/2302/Rosette_Insley_960.jpg"} ```
