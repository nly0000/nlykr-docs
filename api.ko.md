# nly.kr 개발자 API

English documentation: [api.md](api.md)

nly.kr는 URL 단축과 PDF 문서 저장 기능을 외부 웹사이트, 애플리케이션, 스크립트 및 자동화 서비스에서 사용할 수 있도록 API를 제공합니다.

이 문서는 GitHub에서 빠르게 확인할 수 있는 API 요약 문서입니다.

최신 사양, 파라미터 및 상세 예제는 항상 nly.kr 공식 API 문서를 기준으로 확인하세요.

## 공식 API 문서

한국어:

- URL 단축 API: https://nly.kr/api
- PDF 저장 API: https://nly.kr/api/pdf-save

English:

- URL Shortening API: https://nly.kr/en/api
- PDF Save API: https://nly.kr/en/api/pdf-save

---

# 1. URL 단축 API

웹사이트, 애플리케이션, 스크립트 또는 자동화 도구에서 긴 URL을 짧은 nly.kr 링크로 생성할 수 있습니다.

## 엔드포인트

```http
POST https://nly.kr/api/shorten
```

Content-Type:

```http
Content-Type: application/x-www-form-urlencoded
```

## 인증

API Key가 필요합니다.

권장 방식:

```http
X-API-Key: your_API_KEY
```

대체 방식:

```http
Authorization: Bearer your_API_KEY
```

JSON 응답을 받으려면 다음 헤더를 사용할 수 있습니다.

```http
Accept: application/json
```

> 실제 API Key를 공개 GitHub 저장소에 올리지 마세요.

## 요청 파라미터

| 필드 | 타입 | 필수 | 설명 |
|---|---|---:|---|
| `url` | String | 예 | 단축할 원본 URL입니다. HTTP 및 HTTPS URL을 지원합니다. |
| `category` | String | 예 | URL에 지정할 카테고리 키입니다. |

사용 가능한 카테고리는 변경될 수 있으므로 최신 목록은 공식 문서를 확인하세요.

https://nly.kr/api

## cURL 예제

```bash
curl -X POST "https://nly.kr/api/shorten" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "url=https://example.com" \
  -d "category=ai"
```

## 성공 응답

예:

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

## 오류 응답

예:

```json
{
  "status": "error",
  "message": "Category is required."
}
```

주요 HTTP 상태 코드는 다음과 같습니다.

| 상태 코드 | 설명 |
|---:|---|
| `400` | 잘못된 요청 |
| `401` | 인증 실패 |
| `405` | 허용되지 않은 HTTP 메서드 |
| `500` | 서버 오류 |

---

# 2. PDF 저장 API

HTML 또는 일반 텍스트 콘텐츠를 nly.kr 문서 서비스에 저장하고 공유 가능한 문서 URL을 생성할 수 있습니다.

PDF 저장 API는 다음 기능을 지원합니다.

- HTML 콘텐츠 저장
- 일반 텍스트 콘텐츠 저장
- 공개 문서
- 비공개 문서
- 비밀번호가 설정된 비공개 문서
- 공유 가능한 문서 URL 반환

## 엔드포인트

```http
POST https://nly.kr/api/pdf_save
```

Content-Type:

```http
Content-Type: application/x-www-form-urlencoded
```

## 인증

API Key는 다음 방식 중 하나로 전달할 수 있습니다.

권장 방식:

```http
X-API-Key: your_API_KEY
```

대체 방식:

```http
Authorization: Bearer your_API_KEY
```

POST Form Field로도 전달할 수 있습니다.

```text
api_key=your_API_KEY
```

JSON 응답:

```http
Accept: application/json
```

## 요청 파라미터

| 필드 | 타입 | 필수 | 설명 |
|---|---|---:|---|
| `api_key` | String | 조건부 | API Key입니다. Header로 인증한 경우 Form Field에서는 생략할 수 있습니다. |
| `doc_type` | String | 예 | 문서 타입입니다. `html` 또는 `text`만 사용할 수 있습니다. |
| `doc_title` | String | 아니오 | 문서 제목입니다. 생략하면 기본 제목이 사용됩니다. |
| `html_content` | String | HTML 사용 시 | `doc_type=html`인 경우 필요합니다. |
| `text_content` | String | Text 사용 시 | `doc_type=text`인 경우 필요합니다. |
| `is_private` | `0` 또는 `1` | 아니오 | `0`은 공개, `1`은 비공개입니다. 기본값은 `0`입니다. |
| `password` | String | 비공개 사용 시 | `is_private=1`인 경우 필요합니다. |

## HTML 문서 저장 예제

```bash
curl -X POST "https://nly.kr/api/pdf_save" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "doc_type=html" \
  -d "doc_title=API HTML Test" \
  -d "is_private=0" \
  --data-urlencode 'html_content=<h1>Hello</h1><p>API save test</p>'
```

## 텍스트 문서 저장 예제

```bash
curl -X POST "https://nly.kr/api/pdf_save" \
  -H "X-API-Key: your_API_KEY" \
  -H "Accept: application/json" \
  -d "doc_type=text" \
  -d "doc_title=API TEXT Test" \
  -d "is_private=0" \
  --data-urlencode 'text_content=Hello. This text will be saved as a PDF.'
```

## 비공개 문서 예제

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

## 성공 응답

예:

```json
{
  "status": "success",
  "message": "Document saved.",
  "share_url": "https://nly.kr/pdf/view/Ab3XkQ9Z",
  "code": "Ab3XkQ9Z"
}
```

## 오류 응답

예:

```json
{
  "status": "error",
  "message": "The API allows only html or text for doc_type."
}
```

주요 HTTP 상태 코드는 다음과 같습니다.

| 상태 코드 | 설명 |
|---:|---|
| `400` | 필수 파라미터 누락 또는 잘못된 요청 |
| `401` | API Key 누락 또는 인증 실패 |
| `405` | 허용되지 않은 HTTP 메서드 |
| `500` | 서버 오류 |

---

# API Key 보안

API Key는 계정 인증 정보이므로 외부에 공개하지 마세요.

특히 다음 위치에 실제 API Key를 넣지 않는 것을 권장합니다.

- 공개 GitHub 저장소
- 클라이언트 측 JavaScript 소스
- 공개된 HTML
- 스크린샷
- 블로그 예제
- 공개 문서

애플리케이션에서는 환경 변수 또는 서버 측의 안전한 설정 방식을 이용해 API Key를 관리하세요.

예:

```text
NLY_API_KEY=your_API_KEY
```

공개 예제 코드에서는 항상 다음과 같은 가상 값을 사용하세요.

```text
your_API_KEY
```

---

# API 문서 기준

GitHub의 이 문서는 빠르게 확인하기 위한 요약 문서입니다.

실제 API 동작, 파라미터, 인증 방식 또는 응답 형식이 변경된 경우 nly.kr 공식 API 문서를 최우선 기준으로 합니다.

한국어 공식 문서:

- https://nly.kr/api
- https://nly.kr/api/pdf-save

영문 공식 문서:

- https://nly.kr/en/api
- https://nly.kr/en/api/pdf-save

---

# 관련 nly.kr 서비스

- nly.kr: https://nly.kr/
- URL 단축기: https://nly.kr/create-short-url
- URL 단축 API: https://nly.kr/api
- PDF Creator: https://nly.kr/pdf-product-new
- PDF 저장 API: https://nly.kr/api/pdf-save
- Chrome 확장프로그램: https://nly.kr/chrome-extension
