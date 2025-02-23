# Cloud-Based Link Tracker

## Overview

This project is a **minimal, cloud-based resource for tracking links and content from the web**. It is designed to be lightweight, inexpensive, and easily queryable from AI assistants like ChatGPT or Apple Intelligence. The system is **API-first**, with a CLI interface for interaction.

The implementation is **open-source**, hosted on GitHub, and designed for easy deployment using AWS Lambda and S3.

---

## Features

### ✅ Core Functionality

- Track links to blog posts, videos, podcasts, and useful websites.
- Automatic metadata fetching (title, description, thumbnail) via OpenGraph tags.
- Three privacy levels: `private`, `shareable`, `public`.
- CLI for easy interaction (`tracker add`, `tracker search`, `tracker delete`).
- Keyword-based search (searches tags and descriptions, with an optional content search).
- Rate limiting (60 requests per minute) to prevent API abuse.

### 🗄️ Data Storage

- Flat file storage in S3 (each link stored as JSON, with an index for quick lookups).
- Archive system: Deleted links are moved to an `/archive/` folder in S3.
- ISO 8601 timestamps for created/updated dates.
- Automatic duplicate tag cleanup (removes redundant tags).

### 🔐 Security & Access Control

- Per-user API keys stored in DynamoDB (admin-generated, no OAuth for now).
- Public links are fully open (no authentication needed).
- Private links are only accessible by the owner.
- Optional API warnings for duplicate URLs (but duplicates are allowed).

### 🔄 Background Jobs

- Metadata fetching handled asynchronously via AWS Lambda + SQS.
- Automatic retries (up to 3 times) if metadata fetching fails.
- Errors are logged, but failed metadata fetches are not retried indefinitely.

### 🛠️ Developer & AI Readability

- Manual API documentation in Markdown (no auto-generated OpenAPI docs).
- API versioning (`/v1/`) included from the start to handle future updates.
- Structured JSON error responses (e.g., `{ "error": "Invalid API key", "code": 401 }`).

### ⌨️ CLI Features

- Command-line utility (`tracker`) to interact with the API.
- Config file (`~/.trackerconfig`) for API keys, with env var support (`TRACKER_API_KEY`).
- Search results include privacy labels (`[Private]`, `[Public]`).
- Long URLs are truncated for cleaner CLI display.
- JSON export (`tracker export links.json`); CSV support may be added later.
- JSON import (`tracker import links.json`), with error handling.
- Manual updates only (`curl install.sh`), CLI update command may be added later.

---

## API Endpoints (v1)

| Method     | Endpoint                     | Description                          |
| ---------- | ---------------------------- | ------------------------------------ |
| **POST**   | `/v1/links`                  | Add a new link                       |
| **GET**    | `/v1/links/{id}`             | Retrieve a link by ID                |
| **GET**    | `/v1/links`                  | List all links                       |
| **GET**    | `/v1/links/search?q=keyword` | Search links by keyword              |
| **PUT**    | `/v1/links/{id}`             | Update link metadata                 |
| **DELETE** | `/v1/links/{id}`             | Archive a link (move to `/archive/`) |
| **GET**    | `/v1/links/{id}/share`       | Generate a shareable URL             |
| **GET**    | `/v1/links/export`           | Export all links as JSON             |
| **POST**   | `/v1/links/import`           | Import links from JSON               |

---

## Deployment & Setup

### Requirements

- AWS Lambda + API Gateway for API hosting
- AWS S3 for link storage
- AWS SQS for background job queue
- DynamoDB for storing API keys
- Golang or Python for CLI implementation

### Installation (CLI)

```sh
curl -s https://example.com/install.sh | bash
```

### Configuration

Set API key via a config file (`~/.trackerconfig`):

```json
{
  "api_key": "your-api-key",
  "api_url": "https://api.example.com/v1"
}
```

Or, use an environment variable:

```sh
export TRACKER_API_KEY="your-api-key"
```

---

## Future Enhancements

- Bulk delete support (`tracker delete --all` or API batch delete).
- OAuth-based authentication (Google/GitHub login support).
- CLI command to update itself (`tracker update`).
- Optional pagination for search results.
- Automatic link expiration settings (user-configurable).
- Live previews for links in the Web UI.

---

## Contribution Guide

- Fork the repo and submit a pull request.
- Open an issue for feature requests or bugs.
- All API changes should be documented in `README.md`.

---

## License

[MIT License](LICENSE)

---

This document provides **a full implementation roadmap** for developers and AI agents. 🚀

