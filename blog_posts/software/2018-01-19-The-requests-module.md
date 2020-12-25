---
layout: post
title: The requests module and scraping websites [python]
---

## Introduction
Lately a friend and I have started designing a cryptocurrency trading bot. Our idea is to have a bot that takes into account both
quantitative analysis, but also fundamental analysis. The quantitative analysis is quite straight forward, but how to design an
algorithm that analyzes the public view of a good? One way we thought of is to use web scraping to get the source code of websites
containing cryptocurrency information, discussions, etcetera. Then we could parse these sites in order to obtain just the paragraph
text from the webpages. Websites of course use a lot of HTML, CSS, Javascript and so on to make the sites look fancy, but we do not
care about that, we just want the data. One way to parse the source code is to use regular expressions (regex). When the text data is
parsed from the source code it can be used in a natural language processing framework, e.g. in which a sentimental words-database could
be used.

This blog post will focus on getting the source code from a website and the next post will be about parsing it into text.

## Resources

#### Requets module’s documentation:
[Quickstart]( http://docs.python-requests.org/en/master/user/quickstart/)
[Advanced usages]( http://docs.python-requests.org/en/master/user/advanced/#advanced)

For all other documentation such as api doc etc see [link]( http://docs.python-requests.org/en/master/s)

#### Sentdex:
[Python 3 Programming Tutorial - urllib module](https://www.youtube.com/watch?v=5GzVNi0oTxQ&index=37&list=PLQVvvaa0QuDe8XSftW-RAxdo6OmaeL85M)

[Related blogpost]( https://pythonprogramming.net/urllib-tutorial-python-3/)

[Python 3 Programming Tutorial - Parsing Websites with re and urllib]( https://www.youtube.com/watch?v=GEshegZzt3M&index=39&list=PLQVvvaa0QuDe8XSftW-RAxdo6OmaeL85M)

[Related blogpost]( https://pythonprogramming.net/parse-website-using-regular-expressions-urllib/)

#### YouTube:
[HTML Post/Get explained](https://www.youtube.com/watch?v=576WwU7xlWU)

Note: It is usually recommended to use the [requests]( http://docs.python-requests.org/en/master/) library instead of urllib because of
the api differences. Note also that there is a urllib2 at present time. Although Sentdex videos are a bit outdated they do a great job
in explaining the concepts.

#### Wikipedia:
[Web scraping]( https://en.wikipedia.org/wiki/Web_scraping)

[Web crawler]( https://en.wikipedia.org/wiki/Web_crawler)

#### Other resources:
[GET and POST requests using Python ](https://www.geeksforgeeks.org/get-post-requests-using-python/)
[Using the Requests Module in Python](https://code.tutsplus.com/tutorials/using-the-requests-module-in-python--cms-28204)

## What is Hypertext Transfer Protocol?
[Hypertext Transfer Protocol]( https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) also known as HTTP is a
[request-response protocol]( https://en.wikipedia.org/wiki/Request%E2%80%93response) that enables users to communicate with servers,
also known as [client-server model](https://en.wikipedia.org/wiki/Client%E2%80%93server_model). A web browser could be a user and a
website that the browser is trying to access might be a server. When the user wants a response from the server two main methods are
commonly used. [GET]( https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) requests data from the server and
[POST]( https://en.wikipedia.org/wiki/POST_(HTTP)) submits data to be processed to the server.

## The request module

#### To install the requests module:
```
pip install requests
```
If this doesn’t work and you’re on Windows you can try to use the
[unofficial Windows binaries](https://www.lfd.uci.edu/~gohlke/pythonlibs/), here you can download the wheel (.whl) file and run
pip install with it.




#### A simple example:
If you go to your browser and type in “https://www.google.com/search?q=python programming”, and press enter you will come to the Google
search for “python programming”. Notice that the “space” has been changed to “%20” due to url encoding.

Now we will try to visit the same webpage using the requests module in Python.

```python
import requests

# Example 1
q = 'python programming'

url = 'https://google.com/search'

response = requests.get(url + '?' + 'q=' + q)

print(response.text)
```

When printing the response from the server the source code of that website is simply shown.

The requests module has the ability to send data with the url to query. In example 2 where are trying to visit the same webpage but
instead using the params argument of the requests.get() function.

```python
# Example 2
# Passing params in URLs

url = 'https://google.com/search'

query = {'q': 'python programming'}

response = requests.get(url, params = query)

print(response.text)
```

The requests module automatically decodes the response with some educated guess depending on the HTTPS headers. From the requests
module’s documentation, you can see that you can find and change the encoding.

```python
>>> r.encoding
'utf-8'
>>> r.encoding = 'ISO-8859-1'
```

#### Note:

* If you are dealing with JSON data, there is also a built in JSON decoder in the requests module.

* HTTP headers can be passed into the requests.get() function with the header argument. The header should be a dictionary. Example from
the documentation,

```python
>>> url = 'https://api.github.com/some/endpoint'
>>> headers = {'user-agent': 'my-app/0.0.1'}

>>> r = requests.get(url, headers=headers)
```

* We can look at the header of the response by typing response.headers.

* We can also look at what header we sent to the server by typing response.request.headers.

* You can see if the response contains cookies by response.cookies. You can also use the argument ‘cookies’ in the requests.get() function
to send your own.

* The final url might be different from the one that is passed into the requests.get() function due to redirections. To get the final url
type ‘response.url’. (You can disable redirections)

* If you seek non text response, you can access the binary response using response.content.

* Response.raw will give you the raw response from the server, given that the argument stream = True is passed into the requests.get()
function.

* A nice feature of the requests module described in
[this blogpost](https://code.tutsplus.com/tutorials/using-the-requests-module-in-python--cms-28204) is the way one can download files
from websites. In this example a picture is downloaded in chunks. The reason for downloading the image in chunks is to not load the
whole file into the memory.

```python
import requests
req = requests.get('path/to/image.jpg', stream=True)
req.raise_for_status()
with open('Forest.jpg', 'wb') as fd:
    for chunk in req.iter_content(chunk_size=50000):
        print('Received a Chunk')
        fd.write(chunk)
```

#### A good quote from [this blogpost](https://code.tutsplus.com/tutorials/using-the-requests-module-in-python--cms-28204):
“The concepts discussed in this tutorial should help you make basic requests to a server by passing specific headers, cookies, or
query strings. This will be very handy when you are trying to scrape some webpages for information. Now, you should also be able to
automatically download music files and wallpapers from different websites once you have figured out a pattern in the URLs.”


