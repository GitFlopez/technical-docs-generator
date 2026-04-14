# Prompt 2: API Reference Docs

## Purpose
Generate structured, developer-ready API reference documentation with endpoint details, schemas, code examples, and error handling.

---

## The Prompt

```
You are a senior technical writer who has documented APIs for companies like Stripe, Twilio, and GitHub.

Generate complete API reference documentation for the following API:

**API Name:** [API_NAME]
**Base URL:** [https://api.yourproduct.com/v1]
**Authentication method:** [API Key / Bearer Token / OAuth 2.0 / Basic Auth]
**API style:** [REST / GraphQL / RPC]
**Target developer audience:** [backend devs / full-stack / mobile / etc.]

**Endpoints to document (list each with method and path):**
[METHOD /path — description]
[METHOD /path — description]
[METHOD /path — description]
[METHOD /path — description]
[METHOD /path — description]

**Key data objects/schemas:**
[OBJECT_NAME: field1 (type), field2 (type), field3 (type)]
[OBJECT_NAME: field1 (type), field2 (type), field3 (type)]

**Rate limits (if any):** [e.g., 100 requests/minute per API key]
**Supported languages for SDK examples:** [Python, JavaScript, cURL — pick up to 3]

---

Generate API reference documentation with these sections:

1. **Overview** — H1 "API Reference", base URL, API version, and 2-sentence description of what the API does

2. **Authentication** — Step-by-step instructions for obtaining credentials, how to pass auth (header format), and a code example. Include a warning about not committing API keys.

3. **Request Format** — Content-Type requirements, request headers table (Header | Required | Description), and pagination pattern (if applicable)

4. **Endpoints** — For EACH endpoint listed above, generate a full block with:
   - Endpoint title as H3 (method badge + path): `### POST /tasks`
   - One-sentence description
   - **Path Parameters** table (Param | Type | Required | Description) — if applicable
   - **Query Parameters** table (Param | Type | Required | Default | Description) — if applicable
   - **Request Body** — JSON schema table (Field | Type | Required | Description) + example JSON in code block
   - **Response** — success response schema table + example JSON in code block
   - **cURL Example** — complete working cURL command
   - **[LANGUAGE] SDK Example** — code snippet for the first specified SDK language

5. **Error Codes** — Complete table: Code | HTTP Status | Meaning | Resolution. Include at minimum: 400, 401, 403, 404, 409, 422, 429, 500

6. **Rate Limiting** — Explain limits, the response headers that show remaining quota (X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset), and the 429 retry pattern with exponential backoff code example

7. **Pagination** — If the API returns lists, document the cursor/offset pattern with a full example request+response

8. **SDK Quick-Start** — For each specified language, a complete "from zero to first API call in 5 lines" example

Rules:
- All JSON examples must be valid, properly formatted JSON
- cURL examples must be complete and runnable (use placeholder API key: sk_test_example)
- Error messages must be realistic (not just "error occurred")
- Use consistent field naming (camelCase for JSON, consistent across all endpoints)
- Table columns must be properly aligned in Markdown
- Target length: 600-900 lines of Markdown
```

---

## How to Use

1. List all your endpoints — even if some aren't fully built yet, document the intended interface
2. The more specific your schema descriptions, the better the tables
3. Copy each endpoint block to your docs platform (Notion, GitBook, Mintlify, ReadTheDocs)
4. Replace `sk_test_example` with your test API key in onboarding guides

## Tips

- For GraphQL APIs: replace "endpoints" with "queries/mutations" in your input
- If using Mintlify or Redocly, the output format is compatible — minor adjustments needed for OpenAPI YAML
- Run the cURL examples against your staging environment before publishing
