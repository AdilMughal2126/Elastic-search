# Document Search with Elasticsearch

A small internal document search app built with **Flask** and **Elasticsearch 8**. It indexes a set of company documents (policies, guides, sales docs from SharePoint, Teams and GitHub) and lets you search them with ranked full-text results, inline filters and live facet counts.

## Features

- **Full-text search** across title, summary and body (`multi_match`), ranked by relevance (BM25)
- **Inline filters** typed into the search box:
  - `category:sharepoint` filters by source
  - `year:2023` filters by last-updated year
  - combine them with free text: `category:sharepoint policy`
- **Facets** in the same request: result counts per category and per year (`terms` and `date_histogram` aggregations)
- **Filter context**: filters narrow results without affecting the relevance score, and can be cached by Elasticsearch
- **Pagination** and a full document view
- **Bulk indexing** of the whole dataset with one command

## Stack

- Python 3.10+, Flask 3
- Elasticsearch 8.x (official Python client)
- Bootstrap 5 for the UI

## Run it locally

1. Start Elasticsearch (single node, security off, for local development only):

   ```bash
   docker run -d --name es -p 9200:9200 \
     -e "discovery.type=single-node" \
     -e "xpack.security.enabled=false" \
     docker.elastic.co/elasticsearch/elasticsearch:8.15.0
   ```

2. Install dependencies:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. Index the documents and start the app:

   ```bash
   flask reindex
   flask run
   ```

4. Open http://localhost:5001 and try:
   - `work from home`
   - `category:sharepoint policy`
   - `year:2023`

## Project structure

```
app.py          Flask routes, query building, filter parsing, aggregations
search.py       Elasticsearch client wrapper (index, bulk insert, search, get)
data.json       Sample dataset: 15 fictional internal documents
templates/      Jinja templates (search page, document page)
```

## Credits

Built on the starter project from Elastic's search tutorial (MIT License, see `LICENSE`).
