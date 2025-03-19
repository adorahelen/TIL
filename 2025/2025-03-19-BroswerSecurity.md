
---

# **📌 브라우저 보안 모델 (Browser Security Model) 상세 정리**

브라우저는 악의적인 웹사이트로부터 사용자의 데이터를 보호하기 위해 다양한 보안 모델을 적용합니다. 아래는 주요 보안 원칙과 대응 방법에 대한 상세 설명입니다.

---

## **🔹 1. 동일 출처 정책 (Same-Origin Policy, SOP)**

### **✅ 개념**
- 웹 페이지의 스크립트가 **다른 출처(Origin)**의 리소스에 접근하지 못하도록 제한하는 정책.
- **출처(Origin)** = 프로토콜 + 도메인 + 포트 조합.

### **🚀 예제: SOP 적용 사례**
```javascript
// https://example.com에서 실행되는 JavaScript 코드
fetch("https://another-site.com/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Same-Origin Policy 제한으로 요청 실패:", error));
```

🔹 **SOP 위반**: `https://example.com`이 `https://another-site.com`의 데이터에 접근하려 하면 차단됨.

### **✔ 해결 방법**
1️⃣ **CORS (Cross-Origin Resource Sharing)**: 서버에서 `Access-Control-Allow-Origin` 헤더를 추가.  
2️⃣ **JSONP (과거 방식)**: `<script>` 태그를 활용해 우회.  
3️⃣ **프록시 서버 사용**: 동일 도메인에서 API 요청을 중계.

---

## **🔹 2. 교차 출처 리소스 공유 (CORS)**

### **✅ 개념**
- 서버가 특정 출처(Origin)에서의 요청을 허용하도록 설정.
- HTTP 응답 헤더에 `Access-Control-Allow-Origin` 포함.

### **🚀 예제: Express.js 서버에서 CORS 설정**
```javascript
const express = require('express');
const app = express();
const cors = require('cors');

// 특정 출처 허용
app.use(cors({ origin: "https://example.com" }));

app.get("/data", (req, res) => {
  res.json({ message: "Cross-Origin 요청 허용됨" });
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

🔹 설정 설명:
- `https://example.com`에서만 요청 가능.
- `Access-Control-Allow-Origin: *` 설정은 보안 문제를 유발할 수 있으므로 주의.

---

## **🔹 3. 콘텐츠 보안 정책 (Content Security Policy, CSP)**

### **✅ 개념**
- **XSS(크로스 사이트 스크립팅)** 공격을 방어하기 위한 보안 정책.
- 서버가 `Content-Security-Policy` 헤더를 설정해 허용된 콘텐츠만 로드하도록 제한.

### **🚀 예제: CSP 적용**
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' https://apis.google.com; style-src 'self'">
```

🔹 설정 설명:
- `default-src 'self'`: 같은 출처의 리소스만 허용.
- `script-src 'self' https://apis.google.com`: 특정 스크립트만 허용.
- `style-src 'self'`: 외부 스타일시트 제한.

### **🚀 예제: CSP 위반 코드**
```javascript
eval("alert('XSS 공격')"); // CSP 설정 시 차단됨
```

🔹 **효과**: `eval()` 등의 동적 코드 실행을 방지하여 XSS 공격 차단.

---

## **🔹 4. 쿠키 보안 (Secure Cookies)**

### **✅ 개념**
- 쿠키를 통한 공격(예: 세션 하이재킹)을 방지하기 위한 보안 정책.
- **HttpOnly**, **Secure**, **SameSite** 속성을 활용.

### **🚀 예제: 보안 쿠키 설정**
```
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict
```

🔹 속성 설명:
- **HttpOnly**: JavaScript에서 쿠키 접근 불가 (XSS 방지).
- **Secure**: HTTPS 연결에서만 전송.
- **SameSite=Strict**: 크로스 사이트 요청 시 쿠키 자동 전송 차단.

### **🚀 예제: HttpOnly로 JavaScript 접근 차단**
```javascript
console.log(document.cookie); // 출력되지 않음 (HttpOnly 때문)
```

---

## **🔹 5. 클릭재킹 방지 (Clickjacking Protection)**

### **✅ 개념**
- 공격자가 `iframe`을 이용해 사용자가 의도치 않은 클릭을 하도록 유도하는 공격 방지.

### **🚀 예제: X-Frame-Options 적용**
```
X-Frame-Options: DENY
```
🔹 **DENY**: `iframe` 내에서 페이지 로드 불가.

### **🚀 예제: CSP로 클릭재킹 차단**
```
Content-Security-Policy: frame-ancestors 'none';
```
🔹 **frame-ancestors 'none'**: 모든 `iframe` 포함 금지.

---

## **🔹 6. 웹 취약점 보호 (OWASP Top 10)**

### **✅ 주요 위협 및 대응 방법**

| **보안 위협**                | **설명**                         | **대응 방법**                              |
|-----------------------------|---------------------------------|------------------------------------------|
| **XSS**                    | 악성 JavaScript 삽입 공격         | CSP, HttpOnly 쿠키, 입력 검증              |
| **CSRF**                   | 사용자의 세션을 이용한 요청 위조  | CSRF 토큰, SameSite 쿠키                  |
| **SQL 인젝션**              | SQL 쿼리 조작으로 DB 접근         | Prepared Statements, ORM 사용             |
| **인증/세션 관리 취약점**    | 세션 하이재킹, 비밀번호 유출      | 2FA, 강력한 비밀번호 정책 적용             |
| **보안 설정 오류**           | 잘못된 보안 설정으로 인한 취약점  | 보안 헤더 적용, 정기적 보안 점검           |

---

## **📌 결론**

브라우저 보안 모델은 사용자의 데이터를 보호하고, 웹 애플리케이션의 안전성을 확보하기 위한 필수 메커니즘입니다.

### **✅ 핵심 요약**
1. **SOP (동일 출처 정책)**: 기본적인 보안 원칙.
2. **CORS (교차 출처 리소스 공유)**: 외부 API 접근 허용.
3. **CSP (콘텐츠 보안 정책)**: XSS 공격 방지.
4. **쿠키 보안 (Secure, HttpOnly, SameSite)**: 세션 하이재킹 방지.
5. **클릭재킹 방지 (X-Frame-Options, CSP)**: 악성 iframe 차단.
6. **OWASP Top 10 대응**: XSS, CSRF, SQL Injection 등 방어.

---


