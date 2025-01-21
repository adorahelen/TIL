# 서버 사이드 랜더링  
## vs 
# 클라이언트 사이드 랜더링 
## (JSON, Rest API)

- 타임리프(Thymeleaf)는 서버사이드 렌더링(SSR) 방식을 사용하는 자바 템플릿 엔진


## 01 서버 사이드 랜더링 

```markdown
### Thymeleaf 기반 SSR

서버에서 HTML을 렌더링한 후 클라이언트에 전달합니다.

#### Controller (Spring Boot)

```java
@Controller
public class ArticleController {

    @GetMapping("/articles")
    public String getArticles(Model model) {
        List<Article> articles = articleService.getAllArticles();
        model.addAttribute("articles", articles);
        return "articles"; // Thymeleaf 템플릿 이름
    }
}
```

#### Thymeleaf Template (HTML)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Articles</title>
</head>
<body>
    <h1>Article List</h1>
    <ul>
        <li th:each="article : ${articles}">
            <h2 th:text="${article.title}">Article Title</h2>
            <p th:text="${article.content}">Article Content</p>
        </li>
    </ul>
</body>
</html>
```

## 02 클라이언트 사이드 랜더링 

### JSON, REST API 기반 CSR

서버는 데이터를 JSON으로 반환하며, 클라이언트에서 데이터를 가져와 동적으로 렌더링합니다.

#### REST API Controller (Spring Boot)

```java
@RestController
@RequestMapping("/api/articles")
public class ArticleRestController {

    @GetMapping
    public List<Article> getArticles() {
        return articleService.getAllArticles();
    }
}
```

#### HTML + JavaScript (CSR)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Articles</title>
</head>
<body>
    <h1>Article List</h1>
    <ul id="article-list"></ul>

    <script>
        async function fetchArticles() {
            const response = await fetch('/api/articles');
            const articles = await response.json();
            const articleList = document.getElementById('article-list');

            articles.forEach(article => {
                const listItem = document.createElement('li');
                listItem.innerHTML = `
                    <h2>${article.title}</h2>
                    <p>${article.content}</p>
                `;
                articleList.appendChild(listItem);
            });
        }

        fetchArticles();
    </script>
</body>
</html>
```

### 주요 차이점

| 항목            | Thymeleaf (SSR)                        | JSON, REST API (CSR)                   |
|-----------------|----------------------------------------|----------------------------------------|
| **렌더링 방식** | 서버에서 HTML 생성 후 클라이언트에 전달 | 서버에서 JSON 데이터 제공, 클라이언트에서 렌더링 |
| **초기 로드 속도** | 느림 (전체 HTML 전송)                  | 빠름 (JSON만 전송)                     |
| **인터랙션**    | 새로고침 필요                          | 비동기 처리로 새로고침 없이 업데이트 가능   |
| **SEO 지원**   | 우수 (HTML에 데이터 포함)               | 제한적 (추가 설정 필요)                 |
| **유지보수**   | 서버와 클라이언트 코드가 강하게 결합      | 서버와 클라이언트가 분리되어 유지보수 용이   |


## 03 융합형 (ssr + csr)

- 몇개는 ssr 방식으로 서버에서 그려진 상태로 받아오고 
- 몇개는 csr 방식으로 쏴서(서버), 클라이언트가 그리게 한다

```markdown
## Thymeleaf를 사용한 서버 사이드 렌더링(SSR)과 REST API를 통한 클라이언트 사이드 렌더링(CSR)의 조합

### 코드의 방식

#### 1. Thymeleaf 기반 SSR
- HTML 템플릿에서 동적으로 데이터를 렌더링합니다.
- 서버에서 데이터를 조회하거나 처리한 후, Model 객체에 데이터를 담아 클라이언트로 전달합니다.
- Thymeleaf의 `${}` 구문을 통해 서버 데이터를 HTML에 직접 바인딩합니다.

**예시:**

```html
<h1 th:text="${blog.title}"></h1> <!-- 서버에서 전달된 데이터를 렌더링 -->
<span th:text="${#temporals.format(blog.createdAt, 'yyyy-MM-dd HH:mm')}"></span>
```

#### 2. JavaScript와 REST API 기반 CSR
- JavaScript를 사용해 REST API와 통신하며 데이터를 처리합니다.
- 클라이언트에서 이벤트(예: 버튼 클릭)가 발생하면 JavaScript로 서버 API를 호출해 데이터를 조회, 생성, 수정, 삭제합니다.
- REST API는 JSON 데이터를 반환하며, 클라이언트에서 동적으로 DOM을 업데이트합니다.

**예시:**

```javascript
fetch(`/api/blogs/${id}`, { method: 'DELETE' })  // 삭제 요청
    .then(() => {
        alert('삭제 완료');
        location.replace(`/`);
    });
```

### 구체적인 특징

#### 1. Thymeleaf (SSR)
- 서버에서 HTML과 데이터를 함께 생성하여 클라이언트에 전달.
- 주로 초기 페이지 로드 시 사용.

**장점:**
- SEO 친화적 (HTML에 모든 데이터 포함).
- 서버에서 완성된 페이지를 전달하므로 초기 렌더링이 빠름.

**단점:**
- 동적인 데이터 업데이트에는 한계가 있음.
- 페이지 이동이 많아질 경우 사용자 경험이 저하될 수 있음.

#### 2. JavaScript + REST API (CSR)
- 클라이언트에서 데이터를 가져와 화면을 동적으로 렌더링.
- 주로 페이지 내에서 비동기적으로 데이터를 갱신하거나 조작할 때 사용.

**장점:**
- 새로고침 없이 페이지 일부를 업데이트 가능.
- REST API를 통해 서버와 클라이언트 코드 분리 가능.

**단점:**
- 초기 로드 시 JavaScript와 API 호출로 인해 속도가 느려질 수 있음.
- SEO 설정이 추가적으로 필요함.

## 결합 방식의 의도
- 초기 페이지는 Thymeleaf로 SSR 방식으로 렌더링해 빠르게 사용자에게 제공.
- 이후 사용자의 상호작용(예: 글 수정, 삭제 등)은 JavaScript와 REST API로 처리해 CSR 방식으로 동작.
- 이 조합은 SSR의 SEO 장점과 CSR의 동적 인터랙션을 동시에 활용하려는 설계.

# 04 . 종류

템플릿 엔진은 HTML, XML, 텍스트 문서 등을 동적으로 생성하기 위해 사용되는 도구입니다. 주로 서버에서 데이터를 바인딩하여 클라이언트에 전달하거나, 클라이언트에서 데이터 기반 UI를 생성하는 데 사용됩니다. 아래는 주요 템플릿 엔진의 종류와 특징입니다.

### 1. 서버 사이드 템플릿 엔진

#### (1) Thymeleaf
- **언어:** Java
- **특징:**
  - HTML 파일 자체를 템플릿으로 사용하며, 자연스러운 HTML 형태 유지.
  - Spring Boot와 통합이 쉬움.
  - 동적 콘텐츠 생성과 데이터 바인딩에 강력함.
  - SEO 친화적.
- **사용 예:**
  ```html
  <h1 th:text="${title}">Title</h1>
  ```

#### (2) JSP (Java Server Pages)
- **언어:** Java
- **특징:**
    - Java 코드와 HTML을 혼합하여 작성.
    - Servlet 기반으로 작동.
    - 오래된 기술로 현대적인 대안(Thymeleaf, Mustache 등)에 비해 인기가 줄어듦.
- **사용 예:**
  ```html
  <h1><%= title %></h1>
  ```

#### (3) Mustache
- **언어:** 다수 지원 (Java, JavaScript, Python 등)
- **특징:**
    - 논리 없이 데이터만 바인딩하는 간단한 템플릿 엔진.
    - 다양한 언어에서 사용 가능.
    - 빠르고 경량화된 템플릿 엔진.
- **사용 예:**
  ```html
  <h1>{{title}}</h1>
  ```

#### (4) Freemarker
- **언어:** Java
- **특징:**
    - 강력한 데이터 표현식 지원.
    - HTML 및 XML 문서를 템플릿으로 사용 가능.
    - 유연한 커스터마이징 옵션 제공.
- **사용 예:**
  ```html
  <h1>${title}</h1>
  ```

#### (5) EJS (Embedded JavaScript)
- **언어:** JavaScript
- **특징:**
    - Node.js 환경에서 사용되는 템플릿 엔진.
    - HTML 내에서 JavaScript 코드를 삽입 가능.
    - 직관적이고 간단한 문법.
- **사용 예:**
  ```html
  <h1><%= title %></h1>
  ```

### 2. 클라이언트 사이드 템플릿 엔진

#### (1) Handlebars
- **언어:** JavaScript
- **특징:**
    - Mustache 기반 확장 템플릿 엔진.
    - 블록 헬퍼 기능으로 조건문과 반복문 지원.
    - 주로 브라우저와 Node.js에서 사용.
- **사용 예:**
  ```html
  <h1>{{title}}</h1>
  {{#if isPublished}}
    <p>Published</p>
  {{/if}}
  ```

#### (2) React (JSX)
- **언어:** JavaScript/TypeScript
- **특징:**
    - UI를 컴포넌트로 나누어 재사용 가능.
    - HTML과 JavaScript를 결합한 JSX 문법 사용.
    - 데이터 바인딩과 상태 관리를 쉽게 처리.
- **사용 예:**
  ```jsx
  const element = <h1>{title}</h1>;
  ```

#### (3) Vue (Vue.js 템플릿)
- **언어:** JavaScript
- **특징:**
    - HTML 템플릿과 JavaScript 로직을 결합.
    - v-bind와 같은 디렉티브를 통해 데이터와 DOM 바인딩.
    - 간단하고 직관적인 문법.
- **사용 예:**
  ```html
  <h1>{{ title }}</h1>
  ```

#### (4) Angular (Angular Template)
- **언어:** TypeScript
- **특징:**
    - 양방향 데이터 바인딩과 강력한 템플릿 문법 지원.
    - 구조 지시자(*ngIf, *ngFor)를 통해 동적 UI 구성.
    - 대규모 애플리케이션 개발에 적합.
- **사용 예:**
  ```html
  <h1>{{ title }}</h1>
  ```

### 3. 정적 사이트 생성용 템플릿 엔진

#### (1) Jekyll
- **언어:** Ruby
- **특징:**
    - 정적 사이트 생성을 위한 템플릿 엔진.
    - Markdown 및 Liquid 템플릿 사용.
- **사용 예:**
  ```html
  <h1>{{ page.title }}</h1>
  ```

#### (2) Hugo
- **언어:** Go
- **특징:**
    - 빠르고 간단한 정적 사이트 생성기.
    - 템플릿 언어로 Go 템플릿 사용.
- **사용 예:**
  ```html
  <h1>{{ .Title }}</h1>
  ```

### 템플릿 엔진 선택 기준
- **사용 환경:** 서버 사이드(Java, Node.js 등)인지 클라이언트 사이드(JavaScript)인지.
- **프로젝트 규모:** 간단한 블로그부터 대규모 애플리케이션까지.
- **성능 요구사항:** 렌더링 속도, SEO 요구사항.
- **개발자 친화성:** 문법의 간단함과 학습 곡선.

템플릿 엔진은 프로젝트의 요구 사항에 맞게 선택하는 것이 중요

