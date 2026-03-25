# docdidog CLI

Command-line tool for rendering Markdown with diagrams and LaTeX math.
Requires a running docdidog server.

## Installation

### Homebrew (macOS/Linux)

```bash
brew install yaccob/tap/docdidog
```

### Docker

No CLI installation needed — use the container directly:

```bash
docker run --rm -v "$PWD:/data" yaccob/docdidog -t html -o output.html myfile.md
```

## Usage

```
docdidog [OPTIONS] <input.md>

Options:
  -t, --to FORMAT       Output format: pdf, html (default: html)
  -o, --output FILE     Write output to FILE (default: stdout)
  --watch               Watch for changes and regenerate (requires -o)
  --server URL          Server URL (default: http://localhost:3300)
  --maxwidth WIDTH      Max content width for HTML output (default: 60em)
  --center-math         Center block formulas (default: left-aligned)
  -v, --version         Show version
  -h, --help            Show this help
```

### Output Modes

Without `-o`, docdidog writes to **stdout** — ideal for piping:

```bash
docdidog myfile.md > output.html
docdidog -t pdf myfile.md > output.pdf
docdidog myfile.md | less
```

With `-o FILE`, docdidog writes to a file and shows progress on stderr:

```bash
docdidog -o report.html myfile.md
docdidog -t pdf -o report.pdf myfile.md
```

### Specifying a Server

By default, docdidog connects to `http://localhost:3300`. Use `--server` for a different URL:

```bash
docdidog --server http://myserver:3300 myfile.md > output.html
```

### Watch Mode

```bash
docdidog --watch -o report.html myfile.md
```

Regenerates automatically whenever the input file changes. Requires `-o`.

### Examples

```bash
# HTML to stdout (default)
docdidog myfile.md > output.html

# PDF to file
docdidog -t pdf -o report.pdf myfile.md

# Custom max width
docdidog --maxwidth 40em myfile.md > output.html

# Center block formulas
docdidog --center-math -t pdf -o report.pdf myfile.md
```

## docdidog-serve

Convenience wrapper to start a docdidog server.

```
docdidog-serve [PORT]

Options:
  -h, --help            Show this help

Environment:
  DOCDIDOG_PORT           Alternative to PORT argument (default: 3300)
```

```bash
# Start server on default port 3300
docdidog-serve

# Custom port
docdidog-serve 8080

# Via Docker
docker run -d -p 3300:3300 yaccob/docdidog docdidog-serve
```
