# Typer - CLI Framework for Python

Typer is a library for building CLI applications with Python type hints.

## Installation

```bash
pip install typer
# or
uv add typer
```

## Basic Usage

```python
import typer

app = typer.Typer()

@app.command()
def hello(name: str):
    """Say hello to NAME."""
    typer.echo(f"Hello {name}!")

if __name__ == "__main__":
    app()
```

Run with: `python script.py hello World`

## Arguments and Options

```python
import typer
from typing import Optional

app = typer.Typer()

@app.command()
def greet(
    name: str,                                    # Required argument
    lastname: str = typer.Argument(...),          # Required argument (explicit)
    age: Optional[int] = None,                    # Optional argument
    formal: bool = typer.Option(False, "--formal", "-f"),  # Boolean flag
    count: int = typer.Option(1, "--count", "-c"),         # Option with default
):
    """Greet someone with their NAME and optionally LASTNAME."""
    greeting = "Good day" if formal else "Hello"
    full_name = f"{name} {lastname}" if lastname else name
    
    for _ in range(count):
        typer.echo(f"{greeting} {full_name}!")
        if age:
            typer.echo(f"You are {age} years old.")
```

## Multiple Commands

```python
import typer

app = typer.Typer()

@app.command()
def create(name: str):
    """Create a new item."""
    typer.echo(f"Creating {name}")

@app.command()
def delete(name: str):
    """Delete an item."""
    typer.echo(f"Deleting {name}")

@app.command()
def list():
    """List all items."""
    typer.echo("Listing items...")

if __name__ == "__main__":
    app()
```

Usage: `python app.py create myitem`

## Colors and Styling

```python
import typer

def show_status():
    typer.secho("Success!", fg=typer.colors.GREEN, bold=True)
    typer.secho("Warning!", fg=typer.colors.YELLOW)
    typer.secho("Error!", fg=typer.colors.RED, bg=typer.colors.WHITE)
```

## Progress Bars

```python
import typer
import time

def process_items():
    items = list(range(100))
    with typer.progressbar(items, label="Processing") as progress:
        for item in progress:
            time.sleep(0.01)  # Simulate work
```

## Prompts and Confirmation

```python
import typer

def interactive():
    # Simple prompt
    name = typer.prompt("What's your name?")
    
    # Password prompt (hidden input)
    password = typer.prompt("Password", hide_input=True)
    
    # Confirmation
    delete = typer.confirm("Are you sure you want to delete?")
    if not delete:
        raise typer.Abort()
    
    # Prompt with default
    city = typer.prompt("City", default="NYC")
```

## File Paths

```python
from pathlib import Path
import typer

@app.command()
def process_file(
    input_file: Path = typer.Argument(..., exists=True),
    output_file: Path = typer.Argument(...),
):
    """Process INPUT_FILE and save to OUTPUT_FILE."""
    content = input_file.read_text()
    output_file.write_text(content.upper())
    typer.echo(f"Processed {input_file} -> {output_file}")
```

## Choices (Enums)

```python
from enum import Enum
import typer

class Format(str, Enum):
    json = "json"
    yaml = "yaml"
    toml = "toml"

@app.command()
def export(format: Format):
    """Export data in specified FORMAT."""
    typer.echo(f"Exporting as {format.value}")
```

## Error Handling

```python
import typer

@app.command()
def risky_operation():
    try:
        # Some operation
        result = 10 / 0
    except ZeroDivisionError:
        typer.secho("Error: Division by zero!", fg=typer.colors.RED)
        raise typer.Exit(code=1)
```

## Callback (Global Options)

```python
import typer

app = typer.Typer()

@app.callback()
def main(
    verbose: bool = typer.Option(False, "--verbose", "-v"),
    config: str = typer.Option("config.json", "--config", "-c"),
):
    """
    My awesome CLI application.
    
    Global options are set here.
    """
    if verbose:
        typer.echo("Verbose mode enabled")

@app.command()
def run():
    """Run the application."""
    typer.echo("Running...")
```

## Complete Example

```python
import typer
from pathlib import Path
from typing import Optional
from enum import Enum

app = typer.Typer(help="File processing CLI")

class OutputFormat(str, Enum):
    json = "json"
    csv = "csv"
    txt = "txt"

@app.command()
def convert(
    input_file: Path = typer.Argument(..., exists=True, help="Input file path"),
    output_format: OutputFormat = typer.Option(
        OutputFormat.json, 
        "--format", "-f",
        help="Output format"
    ),
    output_file: Optional[Path] = typer.Option(
        None, 
        "--output", "-o",
        help="Output file path"
    ),
    verbose: bool = typer.Option(False, "--verbose", "-v"),
):
    """
    Convert INPUT_FILE to specified format.
    """
    if verbose:
        typer.echo(f"Reading {input_file}")
    
    # Process file
    content = input_file.read_text()
    
    if not output_file:
        output_file = input_file.with_suffix(f".{output_format.value}")
    
    if verbose:
        typer.echo(f"Writing to {output_file}")
    
    output_file.write_text(content)
    typer.secho(f"✓ Converted to {output_format.value}", fg=typer.colors.GREEN)

if __name__ == "__main__":
    app()
```

## Tips

!!! tip "Auto-completion"
    Typer supports shell auto-completion. Install with:
    ```bash
    # For bash
    _MY_APP_COMPLETE=bash_source python my_app.py > ~/.my-app-complete.bash
    source ~/.my-app-complete.bash
    ```

!!! note "Type Hints"
    Typer uses Python type hints to automatically generate CLI arguments and options. The type determines validation!

## Common Patterns

### Version Flag
```python
def version_callback(value: bool):
    if value:
        typer.echo("My App v1.0.0")
        raise typer.Exit()

@app.callback()
def main(
    version: bool = typer.Option(
        None, 
        "--version",
        callback=version_callback,
        is_eager=True
    )
):
    pass
```

### Subcommands with Groups
```python
# users.py
users_app = typer.Typer()

@users_app.command("create")
def create_user(name: str):
    typer.echo(f"Creating user: {name}")

# main.py
app = typer.Typer()
app.add_typer(users_app, name="users")
```

## References

- [Official Documentation](https://typer.tiangolo.com/)
- [GitHub Repository](https://github.com/tiangolo/typer)
