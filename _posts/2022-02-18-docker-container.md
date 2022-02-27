---
layout: post
title:  "Containerize your App with Docker"
date:   2022-02-28 13:00:00 +0100
categories: Automation
---

Intro

In this blog post I will do a recap of a lab section from the course 

The idea and the contents of this demo are pretty much taken out from the course, but I made some changes to get it running on my Macbook Pro and also made some additions to bring some own ideas into it. I also tried to add as much additional explanation as needed to follow and understand this demo to help you get it running on your setup if you want to. So let's start!

At first we need to create a simple Flask application which will be our example application. Flask is an open source Python framework for writing web applications. For more information about Flask, please check the link list below. For a better overview and structure let's create a new folder in your preferred working or coding directory named "my-docker-app-" with another new folder named "app". Then we create a new Python file named "my_app.py" containing the Flask web application:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    out = (
        f'Welcome to my app.<br>'
    )
    return out

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

Let's test 

The Flask application will listen for requests on port 5000 by default.

- [Add Github repository here]()

- [Flask, web development, one drop at a time](https://www.fullstackpython.com/flask.html)