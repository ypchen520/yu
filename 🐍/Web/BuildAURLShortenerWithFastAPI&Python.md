# Build a URL Shortener With FastAPI and Python

🐍 Source: [Real Python Tutorial](https://realpython.com/build-a-python-url-shortener-with-fastapi/)
- 2024/11/29
- 2025/01/17

## Overview

- Create a REST API with FastAPI
- Run a development web server with Uvicorn
- Model an SQLite database
- Investigate the auto-generated API documentation
- Interact with the database with CRUD actions
- Optimize your app by refactoring your code

## Prerequisites

- Object-Oriented Programming (OOP) in Python
- Working with JSON data
- Python Type Checking
- Handling HTTP requests

## Environment

- create `shortener_app` folder
- inside the folder, create `__init__.py`
  - You create a package by adding an `__init__.py` file to a directory. The `__init__.py` file will stay empty throughout this tutorial. Its only job is to tell Python that your `shortener_app/` directory is a package.
  - Without an `__init__.py` file, you’d create not a regular package but a [namespace package](https://docs.python.org/3/reference/import.html#namespace-packages). A namespace package comes in handy when splitting the package over multiple directories, but you’re not splitting the package in this project. Check out [Python import: Advanced Techniques and Tips](../PythonImport.md) to learn more about packages.

### Project Dependencies

Dependencies are the Python packages that your project needs to work. Before installing them with pip, it’s a good idea to create a virtual environment. That way, you’re installing the dependencies not system-wide but only in your project’s virtual environment.

Create a virtual environment named `venv` by using Python’s built-in `venv` module. Then you activate it with the `source` command:

```bash
python -m venv venv
source venv/bin/activate
```

Install the dependencies that your URL shortener Python app requires:
- FastAPI web framework 
- Uvicorn
  - A web server implementation for Python that provides an Asynchronous Server Gateway Interface (ASGI).
  - Web Server Gateway Interfaces (WSGI) specify how your web server communicates with your web application.
  - Traditional WSGI implementations like [Gunicorn](https://gunicorn.org/) need to run multiple processes to handle network traffic concurrently.
  - ASGI, in contrast, can handle an asynchronous event loop on a single thread, as long as you can avoid calling any blocking functions.
  - **FastAPI leverages the ASGI standard**, and you use the uvicorn web server, which can handle asynchronous functionality.
- SQLAlchemy
  - A Python SQL tool kit that helps you communicate with your database.
  - Instead of writing raw SQL statements, you can use SQLAlchemy’s Object-Relational Mapper (ORM).
  - The ORM provides a more user-friendly way for you to declare the interactions of your application and the SQLite database that you’ll use.
- `python-dotenv`: environment variables
  - The `python-dotenv` package helps you read key-value pairs from an external file and set them as environment variables.
- `validators`
  - The `validators` library helps you validate values like email addresses, IP addresses, etc.
  - You’ll use validators to validate the URL that a user wants to shorten in your project.

### Define Environment Variables

You’re currently developing your Python URL shortener on your local computer. But once you want to make it available to your users, you may want to deploy it to the web.

It makes sense to use different settings for different environments. Your local development environment may use a differently named database than an online production environment.

**To be flexible, you store this information in special variables that you can adjust for each environment.** While you won’t take steps to host your app online in this tutorial, you’ll build your app in a way that enables you to deploy it to the cloud in the future.

Learn more about deployment: [Python Web Applications: Deploy Your Script as a Flask App](https://realpython.com/python-web-applications/) or [Deploying a Python Flask Example Application Using Heroku](https://realpython.com/flask-by-example-part-1-project-setup/).

```Python
# shortener_app/config.py

# import lru_cache from functools for caching the settings
from functools import lru_cache

# import BaseSettings for defining environment variables
from pydantic import BaseSettings

# define the default settings for env_name, base_url, db_url
class Settings(BaseSettings):
    env_name: str = "Local"
    base_url: str = "http://localhost:8000"
    db_url: str = "sqlite:///./shortener.db" # Three slashes (sqlite:///): means "a file on the local filesystem."

    # add the Config class and assign `env_file` the path to your external .env file
    class Config:
      env_file = ".env"

# Create get_settings()
# - prints the message: `f"Loading settings for: {settings.env_name}"`
# - returns an instance of your `Settings` class and 
# - implement an LRU cache strategy
@lru_cache
def get_settings():
    settings = Settings()
    print(f"Loading settings for: {settings.env_name}")
    return settings
```

`pydantic`
- You installed `pydantic` automatically when you installed `fastapi` with `pip`.
- `pydantic` is a library that uses **type annotation** to validate data and manage settings.
- The `BaseSettings` class comes in handy to **define environment variables** in your application.

Since the values for your current environment, the domain of your app, and the address of your database are **dependent on the environment that you’re working in**, you’ll **load the values from external environment variables**:
- The configuration variables that you used in your `Settings` class are a fallback for your external environment variables.
- Create an external `.env` file in the **root directory** of your project.
- By storing your environment variables **externally**, you’re following the [twelve-factor app methodology](https://12factor.net/). The **twelve-factor app methodology** states twelve principles to enable developers to build **portable and scalable web applications**. One principle is to store the configuration of your app in the environment:

```
An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc.). This includes:
- Resource handles to the database, Memcached, and other backing services
- Credentials to external services such as Amazon S3 or Twitter
- Per-deploy values such as the canonical hostname for the deploy

The twelve-factor principles require strict separation of config from code. Config varies substantially across deploys, and code does not.
```

It’s recommended to have different .env files for different environments. Also, you should **never add the `.env` file to your version control system**, as your environment variables may store sensitive information.

**Note**: If you’re sharing your code with other developers, then you may want to show in your repository what their `.env` files should look like. In that case, you can add `.env_sample` to your version control system. In `.env_sample`, you can store the keys with placeholder values. To help yourself and your fellow developers, don’t forget to write instructions in your `README.md` file on how to rename `.env_sample` and store the correct values in the file.

To load your external `.env` file, add the Config class with the path to your `env_file` to your settings. `pydantic` loads your environment variables from the `.env` file.

The get_settings() function: 
- prints the message: `f"Loading settings for: {settings.env_name}"`
- returns an instance of your `Settings` class and 
- will provide you with the option of caching your settings.
  - implement a `Least Recently Used (LRU)` strategy
  - You don’t change your app’s settings while running the app. Still, you’re loading your settings over and over again every time you call `get_settings()`
  - **When you start your app, it makes sense to load your settings and then cache the data**.
  - You import `lru_cache` from Python’s `functools` module.
  - The `@lru_cache` decorator allows you to cache the result (the returned value, in this case, a `Settings` instance) of `get_settings()` using the LRU strategy.

## URL Shortener

Create a `FastAPI` app with your first **API endpoint**. 

Once the app is running, you’ll define what your app should be able to do. You’ll model the **data schema** into a database.

### Create FastAPI App

This implementation is a **FastAPI app** that has one **endpoint**. 
- Create a file named `main.py` in the `shortener_app/` folder.
- Import `FastAPI`.
- Define the app by instantiating the `FastAPI` class.
- Use a **path operation decorator** (`@app.get()`) to associate your **root path** with `read_root()` by *registering* it in FastAPI.
- FastAPI listens to the root path and delegates all incoming GET requests to your `read_root()` function.
- Return a string that will be displayed when you send a request to the root path of your API.

```Python
# shortener_app/main.py

# import FastAPI
from fastapi import FastAPI

# define the app by instantiating the FastAPI class
# The app variable is the main point of interaction to create the API
app = FastAPI()

@app.get("/")
def read_root():
    return "Hello World"
```

To run your app, you need a **server**.

### Decide What Your App Can Do

| Endpoint | HTTP Verb | Request Body | Action |
|---|---|---|---|
| `/` | GET | N/A | Returns a "Hello, World!" string |
| `/url` | POST | Your target URL | Shows the created `url_key` with additional info, including a `secret_key` |
| `/{url_key}` | GET | N/A | Forwards to your target URL |
| `/admin/{secret_key}` | GET | N/A | Shows administrative info about your shortened URL |
| `/admin/{secret_key}` | DELETE | N/A | Deletes your shortened URL |

Your **schema** states what your API expects as a **request body** and what the client can expect in the **response body**. You’ll implement **type hinting** to verify that the request and the response match the data types that you define.
- Start by creating the base models for your API request and response bodies in a `schemas.py` file.

```Python
# shortener_app/schemas.py

from pydantic import BaseModel

class URLBase(BaseModel):
    target_url: str

class URL(URLBASE):
    
```
