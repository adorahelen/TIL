# HTML에서 id와 class의 사용

HTML에서 `id`와 `class`는 요소를 식별하고 스타일(CSS) 또는 동작(JavaScript)을 적용하기 위해 사용됩니다. 이 둘은 서로 다른 목적과 특징을 가지고 있으며, 특정 상황에서 각각 적합하게 사용됩니다. 아래에서 차이점과 함께 자세히 설명하겠습니다.

## 1. id (고유 식별자)

### 특징
- **고유함**: HTML 문서에서 **각 요소마다 하나의 고유한 id**만 사용할 수 있습니다. 중복된 id를 사용하면 HTML 문서가 비표준이 되며, 예기치 않은 동작이 발생할 수 있습니다.
- **식별용**: 특정 요소를 정확히 가리켜야 할 때 사용합니다.
- **JavaScript와 연결**: 자바스크립트에서 특정 요소를 조작할 때 주로 사용됩니다.

### 예제

```html
<div id="header">헤더 섹션</div>
<div id="content">본문 내용</div>
<div id="footer">푸터 섹션</div>

<script>
    // 특정 id를 가진 요소 선택 및 조작
    document.getElementById("header").style.color = "blue";
</script>
```

## 2. class (공통 스타일 또는 그룹 지정)

### 특징
- **공유 가능**: 여러 요소에 동일한 class를 적용할 수 있습니다.
- **그룹화**: 비슷한 역할을 하는 요소들을 묶어 스타일을 적용하거나, 자바스크립트에서 한꺼번에 조작할 때 유용합니다.
- **CSS 스타일링**: CSS에서 스타일을 적용할 때 주로 사용됩니다.

### 예제

```html
<div class="box">박스 1</div>
<div class="box">박스 2</div>
<div class="box highlight">박스 3 (강조)</div>

<style>
    .box {
        border: 1px solid black;
        padding: 10px;
        margin: 5px;
    }
    .highlight {
        background-color: yellow;
    }
</style>
```

## 3. id와 class의 차이점 요약

| 특징         | id                           | class                               |
|--------------|------------------------------|-------------------------------------|
| 중복 여부    | 문서에서 고유해야 함         | 여러 요소에서 공유 가능             |
| 사용 목적    | 특정 요소를 식별하거나 조작할 때 | 여러 요소를 그룹화하거나 스타일링할 때 |
| 선택자 우선순위 | id는 class보다 CSS 우선순위가 높음 | id보다 우선순위가 낮음              |
| 예시         | #header                      | .box                                |

## 4. 혼용해도 될까?

규칙 없이 혼용하면 문제가 될 수 있습니다:
- **혼란 유발**: 코드 가독성이 떨어지고 유지보수가 어려워집니다.
- **우선순위 문제**: CSS에서 id는 class보다 우선순위가 높으므로, 의도하지 않은 스타일 충돌이 발생할 수 있습니다.

### 추천 방법
1. **고유한 요소**: 특정한 한 요소만 조작해야 한다면 id를 사용하세요.
2. **여러 요소**: 비슷한 스타일이나 동작을 적용해야 한다면 class를 사용하세요.
3. **혼합 사용**: 필요하다면 id와 class를 혼합해서 사용할 수 있습니다.

## 5. 혼합 사용 예제

```html
<div id="main-content" class="content highlight">
    메인 콘텐츠
</div>
<div class="content">
    서브 콘텐츠 1
</div>
<div class="content">
    서브 콘텐츠 2
</div>

<style>
    /* id로 스타일 지정 */
    #main-content {
        font-weight: bold;
        color: blue;
    }

    /* class로 스타일 지정 */
    .content {
        padding: 10px;
        border: 1px solid black;
    }

    /* 특정 class에 추가 스타일 */
    .highlight {
        background-color: yellow;
    }
</style>

<script>
    // id로 요소 조작
    const mainContent = document.getElementById("main-content");
    mainContent.innerText = "업데이트된 메인 콘텐츠";

    // class로 요소 조작
    const contentBoxes = document.getElementsByClassName("content");
    for (let box of contentBoxes) {
        box.style.margin = "10px";
    }
</script>
```

## 6. 요약
- **id**는 고유한 요소를 정확히 식별할 때 사용합니다.
- **class**는 여러 요소를 그룹화하거나 공통 스타일을 적용할 때 사용합니다.
- 혼합 사용이 가능하지만, 목적에 맞게 적절히 사용하는 것이 중요합니다.
