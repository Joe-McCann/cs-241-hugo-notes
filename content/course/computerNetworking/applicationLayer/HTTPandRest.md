---
title: HTTP and Rest APIs with Flask
linktitle: HTTP and APIs
toc: true
type: book
date: 2025-11-17
draft: false
tags:
    - cs356
    - application layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 502
---

## Text and its Hyperactive Cousin - HTTP

When you browse around on the web, you will find that not only do you go to domain names like `google.com`, but many times there will be more additional information after that, such as `wjmccann.com/course/computernetworking/sections/whatisanetwork/`. When you go to that webpage, your browser shows you all sorts of wonderful and pretty pictures and text and snarky comments written by me, but what actually is going on here?

In the late 1980s, engineers were experimenting with the idea of a webbrowser and webpages, and needed a way to transmit not just text information, but information relating to the ability to link that text to other pieces of text. These links were called *hyperlinks*, and text that could contain hyperlinks were called *hypertext*. 

> **Definition** (true definition): **Hypertext** is text that contains links to other pieces of hypertext. 

However, nowadays I find this definition a bit outdated, and would like to propose my own

> **Definition** (Joe's definition): **Hypertext** is text that contains information or code related to its own formatting and functionality.

These pieces of hypertext would be retreived and then be given to a special piece of software called a *web browser* that can render them and have their functionality work. How do we actually get this text though that will make it to our browser? What we will need is a protocol that was designed for this purpose

> **Definition**: **Hypertext Transfer Protocol** (HTTP) is a protocol designed around transfering text (and more generally hypertext) across a network. HTTP servers normally run on port `80` using TCP. 

So when we make an HTTP request to a server to get a webpage, we are going to make a request to the server that contains the *file* that we would like. Note, this means that in the event that you do not know the IP of the domain name you are requesting, you will need to make a DNS request. The part of the URL that comes after the first `/` is called the *path* or *route* of the URL, and represents the literal path to a file on the server that you are asking for. 

For example, if I was to make an HTTP request to get `wjmccann.com/index.html`, that would actually go to the server that hosts `wjmccann.com`, and then retrieve the file at `/index.html`[^1]. 

Originally, HTTP was designed for the usage with files specifically, however, it has kind of become a catch-all type protocol that is used for literally everything now, even if it probably shouldn't be. 

### HTTP Request and Response Format

Unlike our previous nightmare protocols that deal with bits and need those ugly diagrams I steal from wikipedia, HTTP is actually ascii based. As such we can write it out exactly using text which is wonderful. 

When making a request, it follows the format of 
```
Method URL HTTP-Version
Header: Value
Header: Value
Header: Value
...

Body
```

You can also send body parameters in the URL, which are called **query parameters**. These are placed in the actual URL after the route and a question mark, such as `wjmccann.com/index.html?firstname=joe&lastname=mccann`. In this sense, we would have access to the parameters `firstname` and `lastname` which would have the values of `joe` and `mccann` respectively. This will be useful for GET requests.

We providing a response, it follows the format of 
```
HTTP-Version Status-Code Status
Header: Value
Header: Value
Header: Value
...

Body
```

There are a variable number of headers allowed, and there is not a maximum size for the HTTP body (it could be split up over many TCP packets), however often servers and clients can set their own max size limits. 

In the request category, we can see that there is a part called `Method`, which represents the functionality that we want the server to perform upon handling this request. Back in the day, these had rules behind their usage, however that really is no longer the case as developers kind of just do whatever they want. The main methods that will matter for most devs are

1. `GET`: Used to get a resource, traditionally has no body, so any required parameters are done via query parameters
2. `POST`: Used to create a new resource on a server, such as filling out a form
3. `PUT`: Used to update an existing resource
4. `DELETE`: Used to delete an existing resource

These are based off the CRUD (Create, Retreive, Update, Delete) design principle, but to emphasize again, developers often just use them in whatever way they want. These are really just labels, and there is no reason that you need to follow these conventions. Similar to how its not illegal to have this line of code `even_number_example = 3`. 

When you request a webpage on your browser, you are actually making a `GET` request to the URL. 

One very nice way to make HTTP requests when you are coding yourself is to install software such as Postman, or [Bruno](https://www.usebruno.com/). I prefer Bruno as its free and git friendly, but many companies use Postman (stupidly since Bruno is literally the same thing). 

If you're a true fucking sicko though you can use `curl`, but thats real hardcore.

### HTTP Status Codes

In the response format, there was a category called "status codes", these represent known scenarios that the responder might want to inform the client about. As always, these are actually completely up to the developer of the server to implement, so that developer has no obligation to handle status codes in the conventional way. 

IN FACT, there are some stupid ass weirdos nowadays who will return the success status code in literally every situation, and if a failure occurs just describe it in the response body. This is big dumb because we LITERALLY have codes for this exact fucking reason.

Anyway, here are some prominant HTTP status codes. They fall into $4$ categories

1. `2xx`: Successful message
    - `200` SUCCESS: Generic Message success
2. `3xx`: Redirect-ish message implying that you're not getting the requested resource from the server
3. `4xx`: Something fucked up while processing and its your (client) fault
    - `400` BAD REQUEST: Something generic was wrong with your request
    - `403` FORBIDDEN: You do not have permission to do this
    - `404` NOT FOUND: The requested resource does not exist
4. `5xx`: Something fucked up while processing and its my (server) fault
    - `500` INTERNAL SERVER ERROR: Generic crash of the server
    - `502` BAD GATEWAY: Some other service that the service called had an error

## Rest and APIs

The biggest use case that most of y'all are probably going to do with respect to HTTP is the idea of building restful APIs for your job, where REST stands for *Representational State Transfer*. Now restful APIs are from a specific [design framework](https://restfulapi.net/) that most engineers absolutely do not follow, originally invented in 2000[^2]. Personally, I have seen that most people use "REST" to denote an API[^3] that is hosted on a web server. 

When hosting your API or webserver, there are a few things you need to decide: What will be your webframework, and your webserver.

The webframework is the software library that makes your life easy, so that you don't actually need to read and decode the HTTP text data from the sockets. Some popluar ones in Python are

1. Flask
2. Django
3. Falcon
4. FastAPI

and in Java they have Spring 🤮.

As for a webserver, this will handle all the incoming TCP connections, and help make the process of hosting way easier. Popular webservers are `Apache Webserver` or `Nginx`, but for Python we will use `gunicorn` or `uvicorn`. 

Also, if you are returning a webpage, rather than a functional API, you need to decide how the rendering will be done and who will be responsible for that. You have three options

1. Static Webpages: Every webpage is a file that does not change and is exactly the same for every user.
2. Server Side Rendering: When requested, a script runs that generates the webpage on demand and sends it to the user.
3. Client Side Rendering: When requested, code is sent to the user. The user's browser runs this code in order to render the page.

### API Development with Python Flask

In the following block of code, we will go over a simple API in Python Flask. For this you will need to run the following

```bash
pip3 install flask
pip3 install gunicorn
```

**NOTE** gunicorn does not run on Windows, only UNIX, but it does run on WSL

What we will do here is create a simple API that saves a user name in a "database" and then can return back `Hello` to that person. Ignore the obvious stupidity of what we are going to do, its just for the sake of a tutorial. 

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

database = []

@app.post("/user")
def create_user():
    body = request.json
    
    if "username" not in body:
        return jsonify({"status":"failed"}), 400

    database.append(body["username"])

    return jsonify({"userID":len(database)-1}), 200

@app.get("/user")
def say_hello_user():

    ID = request.args.get("userID")

    if ID == None or any([not char.isdigit() for char in ID]):
        return jsonify({"status":"failed"}), 400

    ID = int(ID)

    if ID >= len(database):
        return jsonify({"status":"failed"}), 404
    
    return jsonify({"message":f"Hello {database[ID]}"})
```

First, we import the main `Flask` class so we can create our API, the `request` class so we can deal with the request parameters, and the `jsonify` class which takes Python objects and turns it into JSON so we can return it.  

Next, these lines of code
```python
app = Flask(__name__)
database = []
```
actually create our API application in the `app` object, and we create a list `database` where we will store our user data. Obviously, when you are at work you will use a real database, but for this class you don't need one.

Next we create the method that will actually save the user data.
```python
@app.post("/user")
def create_user():
    body = request.json
    
    if "username" not in body:
        return jsonify({"status":"failed"}), 400

    database.append(body["username"])

    return jsonify({"userID":len(database)-1}), 200
```
First, the annotation `@app.post("/user")` tells Flask that the following function is something we want to execute whenever you call the `/user` route with a POST request. We then grab the JSON from the request body and save it. For this API, we are taking in JSON that contains a body that has the field `username`, so if we don't get that then return a BAD REQUEST error. 

Finally, add the username to the database, and then return the index (ID) of the entry in the database, along with a `200` OK status. 

Next, we write the code that will return "Hello <username>" when requested. 
```python
@app.get("/user")
def say_hello_user():

    ID = request.args.get("userID")

    if ID == None or any([not char.isdigit() for char in ID]):
        return jsonify({"status":"failed"}), 400

    ID = int(ID)

    if ID >= len(database):
        return jsonify({"status":"failed"}), 404
    
    return jsonify({"message":f"Hello {database[ID]}"}), 200
```

Similar to before, we are annotating to create the method for this route, but we are now setting for GET. Unlike in the previous example where we used the body, we now will grab from a URL query parameter via `ID = request.args.get("userID")`. We then check to see if the provided parameter exists, and is an integer, and return BAD REQUEST otherwise. Then we check if that entry even exists in the database and return NOT FOUND if not. 

Finally, we return `Hello <username>` if everything is good.

[^1]: This is a real page on my website and will actually bring you to the homepage
[^2]: https://en.wikipedia.org/wiki/REST
[^3]: Reminder that an API is really just a software library that abstracts away more complicated logic or permissions