# Astro-ph Radar

**Version:** 1.1.0  
**Description:** Searches for recent papers specifically in the astro-ph repository on arXiv, preserving academic rigor.

## Capabilities

- **`search-astroph`**: Queries arXiv filtering by `astro-ph`, extracting full abstracts, authors, and PDF links.

## Runbook

When a user requests a literature search for a specific topic, execute these steps strictly:

### 1. Construct the Query
- **Base URL:** `http://export.arxiv.org/api/query`
- **Category:** FIXED to astrophysics. Use `cat:astro-ph` (or `cat:astro-ph.GA` for galactic structures).
- **Cost-Control Limit:** Identify the number of papers requested by the user. If the user does not specify a number, default strictly to `max_results=2`. **NEVER** exceed the requested limit.
- **Query Construction:** Combine parameters:
  ```
  ?search_query=cat:astro-ph.GA+AND+all:"<user_topic>"&sortBy=submittedDate&sortOrder=desc&max_results=<number>
  ```
- **Encoding:** Ensure spaces and quotes are properly URL-encoded.

### 2. Execute Fetch
- Use `curl -s` to retrieve the XML data.

### 3. Format the Output (Strict Academic Format)
- **Title:** **Bold** the title.
- **Authors:** List the authors (use "et al." if there are more than 4).
- **Date:** Show the `published` date.
- **Abstract (CRITICAL REQUIREMENT):** Extract the full `<summary>`. DO NOT summarize, rephrase, or truncate. Output the EXACT text inside a blockquote (`> `).
- **Highlighting:** Scan the abstract and automatically **bold** any mentions of specific catalogs, surveys, instruments (e.g., Gaia, VVV, VVVX) and key methodologies (e.g., metallicity, decontamination).
- **Links:** Provide both the arXiv abstract page link and the direct PDF link.
