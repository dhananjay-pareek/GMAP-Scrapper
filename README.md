# GMAP Scraper

A powerful and open-source Google Maps scraper for extracting business data at scale. Available as CLI, Web UI, REST API.

## Why Use This Scraper?

| | |
|---|---|
| **Multiple Interfaces** | CLI, Web UI, REST API - use what fits your workflow |
| **High Performance** | ~120 places/minute with optimized concurrency |
| **33+ Data Points** | Business details, reviews, emails, coordinates, and more |
| **Production Ready** | Scale from a single machine to Kubernetes clusters |
| **Flexible Output** | CSV, JSON, PostgreSQL, S3, or custom plugins |
| **Proxy Support** | Built-in SOCKS5/HTTP/HTTPS proxy rotation |

## Quick Start

### Web UI

Start the web interface with a single command:

```bash
mkdir -p gmapsdata && docker run -v $PWD/gmapsdata:/gmapsdata -p 8080:8080 gosom/google-maps-scraper -data-folder /gmapsdata
```

Then open http://localhost:8080 in your browser.

> **Note:** Results take at least 3 minutes to appear (minimum configured runtime).
> 
> **macOS Users:** Docker command may not work. See [MacOS Instructions](MacOS%20instructions.md).

### Command Line

```bash
touch results.csv && docker run \
  -v $PWD/example-queries.txt:/example-queries \
  -v $PWD/results.csv:/results.csv \
  gosom/google-maps-scraper \
  -depth 1 \
  -input /example-queries \
  -results /results.csv \
  -exit-on-inactivity 3m
```

**Want emails?** Add the `-email` flag.

**Want all reviews (up to ~300)?** Add `--extra-reviews` and use `-json` output.

## Installation

### Using Docker (Recommended)

Two Docker image variants are available:

| Image | Tag | Browser Engine | Best For |
|-------|-----|----------------|----------|
| Playwright (default) | `latest`, `vX.X.X` | Playwright | Most users, better stability |
| Rod | `latest-rod`, `vX.X.X-rod` | Rod/Chromium | Lightweight, faster startup |

```bash
# Playwright version (default)
docker pull gosom/google-maps-scraper

# Rod version (alternative)
docker pull gosom/google-maps-scraper:latest-rod
```

### Build from Source

Requirements: Go 1.25.6+

```bash
git clone https://github.com/dhananjay-pareek/GMAP-Scrapper.git
cd GMAP-Scrapper
go mod download

# Playwright version (default)
go build
./google-maps-scraper -input example-queries.txt -results results.csv -exit-on-inactivity 3m

# Rod version (alternative)
go build -tags rod
./google-maps-scraper -input example-queries.txt -results results.csv -exit-on-inactivity 3m
```

> First run downloads required browser libraries (Playwright or Chromium depending on version).