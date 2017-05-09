---
date: 2017-01-16
title: Intro to Flask
categories:
  - Python
description: Your guide to installation
type: Document
---
Flask is a light-weight python based microframework for developing web applications. 

```
pip install flask
```

Lets create a basic Hello World website using Flask. Well start by creating a folder on our desktop, and creating a file 'app.py'

## Hello World

First, we'll start by importing the Flask into our application and setting our varibale 'app' to Flask

```
from flask import Flask
app = Flask(__name__)
```


Next, we'll create our first route. Well set the route to "/" or the main page of our application, and we'll create a function that returns the words "hello world" we can name our function anything. For this tutorial, I have named it 'hello'.

```
@app.route("/")
def hello():
    return "Hello World!"
```

Lastly, well close our app and tell it to run.

```
if __name__ == "__main__":
    app.run()
```

What is if __name__ = "main"?

I think 'gsamaras' [explained it best](http://stackoverflow.com/questions/419163/what-does-if-name-main-do)

When the Python interpreter reads a source file, it executes all of the code found in it.

Before executing the code, it will define a few special variables. For example, if the python interpreter is running that module (the source file) as the main program, it sets the special __name__ variable to have a value "__main__". If this file is being imported from another module, __name__ will be set to the module's name.

In your browser, navigate to localhost:5000, and you should have a basic webpage with the words "hello world" in the upper left!

Next, we'll expand on this hello world example to create basic templates for our application and start to create some interactions with the database.