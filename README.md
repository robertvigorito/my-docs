# Dev Documentation

Personal development documentation and cheat sheets built with MkDocs Material.

## Setup

### Prerequisites
- Python 3.8+
- pip or uv

### Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd dev-docs

# Install dependencies
pip install mkdocs mkdocs-material
# or with uv
uv pip install mkdocs mkdocs-material
```

## Usage

### Local Development

Start the development server with live reload:

```bash
mkdocs serve
```

Then open http://127.0.0.1:8000 in your browser.

### Build

Build the static site:

```bash
mkdocs build
```

This creates a `site/` directory with the static HTML files.

### Deploy

Deploy to GitHub Pages:

```bash
mkdocs gh-deploy
```

## Project Structure

```
dev-docs/
├── docs/
│   ├── index.md          # Home page
│   ├── python/
│   │   ├── uv.md
│   │   ├── typer.md
│   │   └── requests.md
│   ├── docker/
│   │   └── index.md
│   └── git/
│       └── index.md
├── mkdocs.yml            # Configuration file
├── .gitignore
└── README.md
```

## Adding New Documentation

1. Create a new `.md` file in the appropriate directory under `docs/`
2. Add the page to the navigation in `mkdocs.yml`
3. Use Markdown with the supported extensions (code blocks, admonitions, tables, etc.)

### Example Page

```markdown
# My Tool

Brief description

## Installation

\`\`\`bash
pip install my-tool
\`\`\`

## Usage

\`\`\`python
import my_tool
my_tool.do_something()
\`\`\`

!!! tip
    This is a helpful tip!
```

## Features

- 🎨 Material Design theme with dark mode
- 🔍 Full-text search
- 📱 Responsive design
- 📋 Code block copy buttons
- 🎯 Navigation tabs and sections
- 📝 Markdown extensions (admonitions, code highlighting, tables)

## License

MIT
