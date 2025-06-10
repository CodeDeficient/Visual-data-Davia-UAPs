# Defining Tasks

Use @app.task to expose Python functions to the frontend.

In Davia, a **task** is a Python function that becomes available from the frontend.

You define the logic in Python — and Davia takes care of generating the UI: input fields, buttons, and result areas.

This is the main way to let users interact with your backend: they can trigger actions, pass data, and get responses, without you writing a single line of frontend code.

## Create your first task

To create a task, use the `@app.task` decorator. Davia will generate a UI based on the function’s inputs and output type.

```python
from davia import Davia

app = Davia()

@app.task
def action_process_chart_data(
 start: float,
 end: float
) -> dict:
 """
 Generate chart data from start to end values.
 """
 step = (end - start) / 10
 return {
 "x": [start + i * step for i in range(11)],
 "y": [x * x for x in range(11)],
 "range": [start, end]
 }

@app.task
def display_current_time() -> str:
 """
 Returns the current time.
 """
 from datetime import datetime
 return datetime.now().strftime("%H:%M:%S")

if __name__ == "__main__":
 app.run()
```

When you run this file, Davia will open the editor at:

🔗 <https://dev.davia.ai/dashboard>

## Davia and FastAPI Integration

Davia is built on top of FastAPI. When you create your app with `app = Davia()`, you are actually creating a FastAPI application. This means you can use all FastAPI features and add your own endpoints alongside Davia tasks.

`app = Davia()`

### Example: Adding a FastAPI Endpoint

```python
from davia import Davia
from pydantic import BaseModel

app = Davia()

@app.task
def say_hello(name: str) -> str:
 return f"Hello, {name}!"

# Define a Pydantic model for request body
class Item(BaseModel):
 value: int

# Add a custom FastAPI endpoint
@app.put("/custom-endpoint")
def custom_put_endpoint(item: Item):
 return {"received": item.value}
```

## Best Practices

Follow these essential guidelines to ensure your Davia tasks are robust, maintainable, and user-friendly:

*   **Prefix task names:** Use `action_` for tasks that perform an action and `display_` for tasks that return data to be displayed.
    *   Example: `action_add`, `display_user_profile`
*   **Type hints:** Always use type hints for function parameters and return values. Davia uses these to generate the UI.
*   **Docstrings:** Write clear docstrings for your tasks. Davia may use these for UI labels or tooltips.
    *   Example:
        ```python
        @app.task
        def action_add(a: int, b: int) -> int:
         """Add two numbers and return the result."""
         return a + b
        ```

If you want to extend your app with custom endpoints or advanced features, see the [FastAPI documentation](https://fastapi.tiangolo.com/).
