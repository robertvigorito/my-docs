# UV - Fast Python Package Manager

UV is an extremely fast Python package installer and resolver written in Rust.

## Installation

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# With pip
pip install uv
```

## Creating a New Project

```bash
# Create a new Python project
uv init my-project
cd my-project

# Create with specific Python version
uv init my-project --python 3.12
```

## Managing Dependencies

```bash
# Add a dependency
uv add requests

# Add a dev dependency
uv add --dev pytest

# Install all dependencies from pyproject.toml
uv sync

# Update all dependencies
uv lock --upgrade
```

## Running Python

```bash
# Run a script with uv
uv run script.py

# Run a command in the project environment
uv run python -m pytest

# Start a Python REPL
uv run python
```

## Virtual Environments

```bash
# Create a virtual environment
uv venv

# Create with specific Python version
uv venv --python 3.11

# Activate the environment
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows
```

## Package Management

```bash
# Install a package
uv pip install requests

# Install from requirements.txt
uv pip install -r requirements.txt

# Compile requirements
uv pip compile pyproject.toml -o requirements.txt

# Show installed packages
uv pip list
```

## Useful Tips

!!! tip "Speed"
    UV is 10-100x faster than pip for most operations. It achieves this through:
    - Parallel downloads
    - Better caching
    - Written in Rust

!!! note "Compatibility"
    UV is designed to be a drop-in replacement for pip and pip-tools. Most pip commands work with `uv pip`.

## Common Workflows

### Starting a New Project
```bash
uv init my-app
cd my-app
uv add fastapi uvicorn
uv run python main.py
```

### Working with Existing Projects
```bash
# Clone and setup
git clone <repo>
cd <repo>
uv sync
uv run python main.py
```

## Configuration

UV can be configured via `pyproject.toml`:

```toml
[tool.uv]
index-url = "https://pypi.org/simple"
extra-index-url = ["https://my-private-index.com/simple"]
```

## References

- [Official Documentation](https://docs.astral.sh/uv/)
- [GitHub Repository](https://github.com/astral-sh/uv)
