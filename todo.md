# **Cloud-Based Link Tracker: To-Do Checklist**

This checklist outlines all tasks required to build the Cloud-Based Link Tracker. Use this as a **progress tracker** to ensure each component is completed step by step.

---

## **Phase 1: Core API Foundation**

### ✅ Set Up API Skeleton
- [ ] Create a Flask/FastAPI application.
- [ ] Implement a `GET /health` endpoint returning `{ "status": "ok" }`.
- [ ] Write unit tests to confirm API is responsive.

### 🔑 User Authentication (API Key System)
- [ ] Implement an API key system using DynamoDB.
- [ ] Require API keys for all endpoints except `/health`.
- [ ] Write authentication tests.

### 🔗 Add a Link (`POST /v1/links`)
- [ ] Accept `url`, `title`, `description`, `tags`, and `privacy_level`.
- [ ] Store as JSON in AWS S3.
- [ ] Validate URLs and reject duplicates.
- [ ] Write tests for valid and invalid inputs.

### 🔍 Retrieve a Link (`GET /v1/links/{id}`)
- [ ] Fetch stored link data from S3.
- [ ] Ensure private links are accessible only by the owner.
- [ ] Return `404` for non-existent links.
- [ ] Write tests for retrieval and access control.

### 🗑️ Archive a Link (`DELETE /v1/links/{id}`)
- [ ] Move deleted links to an `/archive/` folder in S3.
- [ ] Ensure archived links are inaccessible via `/v1/links/{id}`.
- [ ] Write tests to confirm archiving behavior.

### 📜 List All Links (`GET /v1/links`)
- [ ] Return stored links with pagination.
- [ ] Implement API rate limiting (60 requests per minute).
- [ ] Write tests to validate listing and rate-limiting.

---

## **Phase 2: Metadata Fetching and Search**

### ⚙️ Background Job for Metadata Fetching
- [ ] Implement an AWS Lambda function to fetch OpenGraph metadata.
- [ ] Use AWS SQS for background job processing.
- [ ] Retry failed fetches up to 3 times before logging errors.
- [ ] Write tests to confirm retry logic.

### 🔎 Enable Keyword Search (`GET /v1/links/search?q=keyword`)
- [ ] Allow searching by title, description, and tags.
- [ ] Implement case-insensitive search.
- [ ] Write search functionality tests.

---

## **Phase 3: CLI Implementation**

### 🖥️ Develop the CLI (`tracker`)
- [ ] Implement `tracker add <url>` to send API requests.
- [ ] Implement `tracker list` to fetch and display links.
- [ ] Implement `tracker delete <id>` for archiving.
- [ ] Add JSON export (`tracker export links.json`).
- [ ] Write unit tests for CLI interactions.

### 💡 Enhance CLI Usability
- [ ] Support API key storage in `~/.trackerconfig` and `TRACKER_API_KEY` environment variable.
- [ ] Improve error handling and messages.
- [ ] Implement colored output and truncated URLs for cleaner display.

---

## **Phase 4: Security & Deployment**

### ⏳ API Rate Limiting
- [ ] Implement per-user rate limiting (60 requests per minute).
- [ ] Exceeding requests should return `429 Too Many Requests`.
- [ ] Write load tests to confirm limits.

### 🔒 Secure API Endpoints
- [ ] Enforce HTTPS.
- [ ] Implement logging and monitoring with AWS CloudWatch.
- [ ] Write security tests to validate protected routes.

### 🚀 Deploy to AWS
- [ ] Use Terraform or AWS CDK to provision API Gateway, Lambda, S3, and DynamoDB.
- [ ] Automate deployment with GitHub Actions.
- [ ] Write integration tests against the live API.

---

## **Final Phase: Enhancements & Maintenance**

### 🔗 Shareable URLs (`GET /v1/links/{id}/share`)
- [ ] Generate time-limited signed URLs.
- [ ] Write tests to confirm share functionality.

### 📄 Improve Search with Full-Text Indexing
- [ ] Enhance search to index full web page content.
- [ ] Optimize performance.
- [ ] Write accuracy tests.

### 📥 Extend CLI with Import (`tracker import links.json`)
- [ ] Support bulk import.
- [ ] Handle malformed files gracefully.
- [ ] Write validation tests.

---

### ✅ **Final Review and Documentation**
- [ ] Ensure all API endpoints are well-documented in `README.md`.
- [ ] Verify CI/CD pipeline for automated testing and deployment.
- [ ] Final round of testing and bug fixes before launch.

---

This checklist serves as a **structured roadmap** to ensure the project is completed **incrementally and efficiently**. Tick off each item as you progress! 🚀

