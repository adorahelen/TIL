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

