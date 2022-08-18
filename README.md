

# Runway Example python App

This is an example app demonstrating how to deploy a python app
to [runway](https://runway.planetary-quantum.com/).

* clone this repo, and navigate into that directory
* `runway app create`
* `runway app deploy`
* `runway open`

You can then deploy changes by `git commit`ing them, and running `runway app
deploy` again.

# Python

For this example, we built a small API with `fastapi`, `uvicorn` and `poetry`.

> `poetry` is a modern dependency manager for Python projects with deterministic installs (through a lock), run commands, automated virtualenv and much more. The idea of this example is to get you started. A similar setup with django is of course possible, but a bit more involved.

## Setup

The four steps to build a small application.

### Step 1

Create a new project using `poetry`:

```sh
$ poetry new python-runway
Created package python_runway in python-runway
```

### Step 2

Add our framework (`fastapi`) and a web server (`uvicorn`).

To add them we go into the project directory and add `fastapi`:

```sh
$ poetry add fastapi
Creating virtualenv python-runway-9x66e4jD-py3.10 in /Users/till/Library/Caches/pypoetry/virtualenvs
Using version ^0.79.0 for fastapi

Updating dependencies
Resolving dependencies... (18.4s)

Writing lock file

Package operations: 15 installs, 0 updates, 0 removals

  • Installing idna (3.3)
  • Installing sniffio (1.2.0)
  • Installing anyio (3.6.1)
  • Installing pyparsing (3.0.9)
  • Installing typing-extensions (4.3.0)
  • Installing attrs (22.1.0)
  • Installing more-itertools (8.14.0)
  • Installing packaging (21.3)
  • Installing pluggy (0.13.1)
  • Installing py (1.11.0)
  • Installing pydantic (1.9.2)
  • Installing starlette (0.19.1)
  • Installing wcwidth (0.2.5)
  • Installing fastapi (0.79.0)
  • Installing pytest (5.4.3)
```

... and `uvicorn`:

```sh
$ poetry add 'uvicorn[standard]'
Using version ^0.18.2 for uvicorn

Updating dependencies
Resolving dependencies... (11.0s)

Writing lock file

Package operations: 9 installs, 0 updates, 0 removals

  • Installing click (8.1.3)
  • Installing h11 (0.13.0)
  • Installing httptools (0.4.0)
  • Installing python-dotenv (0.20.0)
  • Installing pyyaml (6.0)
  • Installing uvloop (0.16.0)
  • Installing watchfiles (0.16.1)
  • Installing websockets (10.3)
  • Installing uvicorn (0.18.2)
```

### Step 3

> For demo purposes, the application will only respond to requests at the root (`/`).

Create a application: the project's main module is called `python_runway` (as well), so let's create a `main.py` in `./python_runway` (it did only contain `__init__.py` so far):

```python
import uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

def start():
    """Launched with `poetry run start` at root level"""
    uvicorn.run("python_runway.main:app", host="0.0.0.0", port=5000, reload=True)
```

**Note**: we used our default port of `5000`. But you can customize this with `runway app config set PORT=x` and use an environment variable in your code.

### Step 4

Last but not least, add a start script to `poetry`'s `pyproject.toml` which is a meta file which configures all project settings. This gives you an easier way to start your application:

```toml
[tool.poetry.scripts]
start = "python_runway.main:start"
```

To test, run `poetry run start` after you added it. Adding this will also enable our builder to start your application after it's done assembling all the requirements.

## Deployment

The deployment is the usual steps:

- `runway app create`
- `runway app deploy`
- `runway app open`

