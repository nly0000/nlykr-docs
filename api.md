# nly.kr Developer API

nly.kr provides APIs for URL shortening and PDF document saving.

This GitHub document provides a quick overview and basic examples.

For the latest specifications, parameters, and examples, always refer to the official nly.kr API documentation.

## Official Documentation

- URL Shortening API: https://nly.kr/en/api
- PDF Save API: https://nly.kr/en/api/pdf-save

Korean documentation:

- URL Shortening API: https://nly.kr/api
- PDF Save API: https://nly.kr/api/pdf-save

---

# 1. URL Shortening API

Create short nly.kr URLs programmatically from websites, applications, scripts, or automation tools.

## Endpoint

```http
POST https://nly.kr/api/shorten
```

Content type:

```http
Content-Type: application/x-www-form-urlencoded
```

## Authentication

An API key is required.

Recommended:

```http
X-API-Key: your_API_KEY
```

Alternative:

```http
Authorization: Bearer your_API_KEY
```

For JSON responses:

```http
Accept: application/json
```

> Never commit your real API key to a public GitHub repository.

## Parameters

| Field | Type | Required | Description |
|---|---|---:|---|
| `url` | String | Yes | Original URL to shorten. HTTP and HTTPS URLs are supported. |
| `category` | String | Yes | Category key for the URL. |

See the official documentation for the current category list:

https://nly.kr/en/api

## cURL Example

```bash
curl -X POST "https://nly.kr/api/shorten" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "url=https://example.com" \
  -d "category=ai"
```

## Success Response

```json
{
  "status": "success",
  "short_url": "https://nly.kr/Ab3XkQ",
  "original_url": "https://example.com",
  "code": "Ab3XkQ",
  "member_idx": 123,
  "category": "ai"
}
```

## Error Response

```json
{
  "status": "error",
  "message": "Category is required."
}
```

Possible HTTP status codes:

| Status | Description |
|---:|---|
| `400` | Bad Request |
| `401` | Unauthorized |
| `405` | Method Not Allowed |
| `500` | Server Error |

---

# 2. PDF Save API

Save HTML or plain text content through the nly.kr PDF service and receive a shareable document URL.

The API supports:

- HTML content
- Plain text content
- Public documents
- Private documents
- Password-protected private documents

## Endpoint

```http
POST https://nly.kr/api/pdf_save
```

Content type:

```http
Content-Type: application/x-www-form-urlencoded
```

## Authentication

The API key can be supplied using one of the following methods.

Recommended:

```http
X-API-Key: your_API_KEY
```

Alternative:

```http
Authorization: Bearer your_API_KEY
```

The API key can also be supplied as a form field:

```text
api_key=your_API_KEY
```

For JSON responses:

```http
Accept: application/json
```

## Parameters

| Field | Type | Required | Description |
|---|---|---:|---|
| `api_key` | String | Conditional | API key. Not required as a form field when authentication is provided in a header. |
| `doc_type` | String | Yes | Must be `html` or `text`. |
| `doc_title` | String | No | Document title. A default title is used when omitted. |
| `html_content` | String | If HTML | Required when `doc_type=html`. |
| `text_content` | String | If text | Required when `doc_type=text`. |
| `is_private` | `0` or `1` | No | `0` = public, `1` = private. Default is `0`. |
| `password` | String | If private | Required when `is_private=1`. |

## Save HTML Example

```bash
curl -X POST "https://nly.kr/api/pdf_save" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "doc_type=html" \
  -d "doc_title=API HTML Test" \
  -d "is_private=0" \
  --data-urlencode 'html_content=<h1>Hello</h1><p>API save test</p>'
```

## Save Text Example

```bash
curl -X POST "https://nly.kr/api/pdf_save" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "doc_type=text" \
  -d "doc_title=API TEXT Test" \
  -d "is_private=0" \
  --data-urlencode 'text_content=Hello. This text will be saved as a PDF.'
```

## Private Document Example

```bash
curl -X POST "https://nly.kr/api/pdf_save" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "doc_type=html" \
  -d "doc_title=Private document" \
  -d "is_private=1" \
  -d "password=1234" \
  --data-urlencode 'html_content=<h1>Private</h1><p>Private document</p>'
```

## Success Response

```json
{
  "status": "success",
  "message": "Document saved.",
  "share_url": "https://nly.kr/pdf/view/Ab3XkQ9Z",
  "code": "Ab3XkQ9Z"
}
```

## Error Response

```json
{
  "status": "error",
  "message": "The API allows only html or text for doc_type."
}
```

Possible HTTP status codes:

| Status | Description |
|---:|---|
| `400` | Bad Request / invalid parameter |
| `401` | Missing or invalid API key |
| `405` | Method Not Allowed |
| `500` | Server Error |

---

# API Key Security

Do not expose API keys in:

- Public GitHub repositories
- Client-side source code
- Screenshots
- Public documents

Use environment variables or another secure server-side configuration method when integrating the API.

---

# Documentation Policy

The API documentation hosted on nly.kr is the canonical and most up-to-date reference.

If this GitHub document differs from the official documentation, follow the official documentation:

- https://nly.kr/en/api
- https://nly.kr/en/api/pdf-save

---

# Related nly.kr Services

- Website: https://nly.kr/
- URL Shortener: https://nly.kr/create-short-url
- PDF Creator: https://nly.kr/pdf-product-new
- Chrome Extension: https://nly.kr/chrome-extension
