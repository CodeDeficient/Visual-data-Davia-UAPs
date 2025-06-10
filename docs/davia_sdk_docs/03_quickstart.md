# Quickstart - Davia Starter Kit

Get started with Davia in minutes by following these simple steps.

## Prerequisites

Davia requires Python 3.9 or higher. Make sure you have the correct Python version installed before proceeding.

## 1. Install Davia

First, install Davia using pip:

```bash
pip install davia
```

## 2. Create Your First App

Create a new Python file, for example `welcome.py`, and add the following code:

```python
from davia import Davia

app = Davia()

@app.task
def get_welcome_message(name: str = "there") -> str:
    """
    Return a personalized welcome message.
    """
    return f"Hi {name}! Welcome to Davia !"

if __name__ == "__main__":
    app.run()
```

## 3. Run Your App

Run your Python file:

```bash
python welcome.py
```

Alternatively, you can use the Davia CLI command instead of including `app.run()` in your code:

```python
# Without app.run() in your code
from davia import Davia

app = Davia()

@app.task
def get_welcome_message(name: str = "there") -> str:
    """
    Return a personalized welcome message.
    """
    return f"Hi {name}! Welcome to Davia !"
```

Then run it with:

```bash
davia run welcome.py
```

---
*Navigation links from original page (for reference, may not be active locally):*
*   [Introduction](https://docs.davia.ai/introduction)
*   [Core concepts](https://docs.davia.ai/develop/how-it-works)
