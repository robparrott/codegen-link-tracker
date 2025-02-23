# **Cloud-Based Link Tracker: Development Blueprint**

This document outlines a **step-by-step plan** for developing the Cloud-Based Link Tracker. The development process follows **incremental, test-driven implementation**, ensuring security, usability, and scalability from the start.

---

## **Phase 1: Core API Foundation**

### **1. Set Up API Skeleton**
- Create a basic Flask/FastAPI application.
- Implement a `GET /health` endpoint returning `{ "status": "ok" }`.
- Write unit tests to confirm API is responsive.

### **2. User Authentication (API Key System)**
- Implement an API key system using DynamoDB.
- Require API keys for all endpoints except `/health`.
- Write authentication tests.

### **3. Add a Link (`POST /v1/links`)**
- Accept `url`, `title`, `description`, `tags`, and `privacy_level`.
- Store as JSON in AWS S3.
- Validate URLs and reject duplicates.
- Write tests for valid and invalid inputs.

### **4. Retrieve a Link (`GET /v1/links/{id}`)**
- Fetch stored link data from S3.
- Ensure private links are accessible only by the owner.
- Return `404` for non-existent links.
- Write tests for retrieval and access control.

### **5. Archive a Link (`DELETE /v1/links/{id}`)**
- Move deleted links to an `/archive/` folder in S3.
- Ensure archived links are inaccessible via `/v1/links/{id}`.
- Write tests to confirm archiving behavior.

### **6. List All Links (`GET /v1/links`)**
- Return stored links with pagination.
- Implement API rate limiting (60 requests per minute).
- Write tests to validate listing and rate-limiting.

---

## **Phase 2: Metadata Fetching and Search**

### **7. Background Job for Metadata Fetching**
- Implement an AWS Lambda function to fetch OpenGraph metadata.
- Use AWS SQS for background job processing.
- Retry failed fetches up to 3 times before logging errors.
- Write tests to confirm retry logic.

### **8. Enable Keyword Search (`GET /v1/links/search?q=keyword`)**
- Allow searching by title, description, and tags.
- Implement case-insensitive search.
- Write search functionality tests.

---

## **Phase 3: CLI Implementation**

### **9. Develop the CLI (`tracker`)**
- Implement `tracker add <url>` to send API requests.
- Implement `tracker list` to fetch and display links.
- Implement `tracker delete <id>` for archiving.
- Add JSON export (`tracker export links.json`).
- Write unit tests for CLI interactions.

### **10. Enhance CLI Usability**
- Support API key storage in `~/.trackerconfig` and `TRACKER_API_KEY` environment variable.
- Improve error handling and messages.
- Implement colored output and truncated URLs for cleaner display.

---

## **Phase 4: Security & Deployment**

### **11. API Rate Limiting**
- Implement per-user rate limiting (60 requests per minute).
- Exceeding requests should return `429 Too Many Requests`.
- Write load tests to confirm limits.

### **12. Secure API Endpoints**
- Enforce HTTPS.
- Implement logging and monitoring with AWS CloudWatch.
- Write security tests to validate protected routes.

### **13. Deploy to AWS**
- Use Terraform or AWS CDK to provision API Gateway, Lambda, S3, and DynamoDB.
- Automate deployment with GitHub Actions.
- Write integration tests against the live API.

---

## **Final Phase: Enhancements & Maintenance**

### **14. Shareable URLs (`GET /v1/links/{id}/share`)**
- Generate time-limited signed URLs.
- Write tests to confirm share functionality.

### **15. Improve Search with Full-Text Indexing**
- Enhance search to index full web page content.
- Optimize performance.
- Write accuracy tests.

### **16. Extend CLI with Import (`tracker import links.json`)**
- Support bulk import.
- Handle malformed files gracefully.
- Write validation tests.

---

# **Step-by-Step LLM Prompts for Development**

## **Prompt 1: API Skeleton & Health Check**
```text
Write a Flask/FastAPI app with a single `GET /health` endpoint that returns `{"status": "ok"}`. The app should be structured for easy expansion with future endpoints. Include a test that verifies the endpoint returns the expected response.
```

## **Prompt 2: API Key Authentication**
```text
Modify the existing API to require an API key for all endpoints except `/health`. API keys should be stored in AWS DynamoDB. Requests without a valid API key should return `{"error": "Invalid API key", "code": 401}`. Include tests to confirm authentication works.
```

## **Prompt 3: Add a Link (`POST /v1/links`)**
```text
Implement the `POST /v1/links` endpoint to accept a JSON payload with `url`, `title`, `description`, `tags`, and `privacy_level`. Store this data as a JSON file in AWS S3. Validate that `url` is well-formed and reject duplicates. Write tests for valid and invalid inputs.
```

## **Prompt 4: Retrieve a Link (`GET /v1/links/{id}`)**
```text
Implement the `GET /v1/links/{id}` endpoint to retrieve a stored link from S3. Ensure private links are only accessible by their owner. Return `{"error": "Not Found", "code": 404}` if the link does not exist. Write tests to confirm retrieval and access control.
```

## **Prompt 5: Archive a Link (`DELETE /v1/links/{id}`)**
```text
Modify the API to support deleting links. Instead of hard-deleting, move the JSON file to an `/archive/` folder in S3. If the link doesn’t exist, return `{"error": "Not Found", "code": 404}`. Include tests to confirm archiving behavior.
```

## **Prompt 6: CLI Implementation**
```text
Create a CLI tool named `tracker`. Implement `tracker add <url>` that sends a `POST` request to add a link to the API. Support an API key stored in `~/.trackerconfig` or the `TRACKER_API_KEY` environment variable. Include unit tests for CLI interactions.
```

## **Prompt 7: Background Job for Metadata Fetching**
```text
Create an AWS Lambda function that fetches metadata (title, description, thumbnail) for a given URL. The function should be triggered by an SQS message. If fetching fails, retry up to 3 times before logging an error. Write tests to validate retry logic.
```

## **Prompt 8: CLI Search**
```text
Enhance the `tracker` CLI to support `tracker search <query>`, which calls `GET /v1/links/search?q=<query>` and returns results in a readable format. Implement JSON output with a `--json` flag. Include tests to confirm search functionality.
```

## **Prompt 9: Rate Limiting**
```text
Implement API rate limiting (60 requests per minute per API key). Requests exceeding this limit should return `{"error": "Too Many Requests", "code": 429"}`. Write tests to confirm rate-limiting behavior.
```

## **Prompt 10: Deployment**
```text
Write a Terraform or AWS CDK script to deploy the API, S3 storage, and DynamoDB table. Configure AWS Lambda with API Gateway. Ensure deployments are automated via GitHub Actions. Provide an example `.env` file for local testing.
```

