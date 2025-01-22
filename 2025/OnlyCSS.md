
## CSS 코드

### 01. 컨테이너 개념 
```css
.container {
    width: 50%;
    margin: 0 auto;
    padding: 30px;
    background-color: #fff;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    margin-top: 50px;
    border-radius: 8px;
}
```

.container` 클래스를 사용하는 HTML 요소에 다음 스타일을 적용합니다:
- `width`: 요소의 너비를 50%로 설정합니다.
- `margin`: 상하 좌우 중앙 정렬을 위해 `0 auto` 값을 사용합니다.
- `padding`: 내부 여백을 30px로 설정합니다.
- `background-color`: 배경색을 흰색(#fff)으로 설정합니다.
- `box-shadow`: 0 4px 6px 크기의 그림자 효과를 추가합니다.
- `margin-top`: 위쪽 여백을 50px로 설정합니다.
- `border-radius`: 모서리를 8px로 둥글게 만듭니다.

이 스타일을 적용하면 요소가 페이지의 중앙에 위치하고, 흰색 배경과 그림자 효과가 적용되어 더 부드럽고 깔끔한 디자인을 연출할 수 있습니다.


### 02. 데이터 입력 박스 

```css
.input-box {
    margin-bottom: 20px;
}
.input-box label {
    font-size: 16px;
    color: #333;
    display: block; // block=보임, none=안보임 
    margin-bottom: 5px;
}
.input-box textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}
```

.input-box` 클래스를 사용하는 HTML 요소에 다음 스타일을 적용

- `.input-box` 클래스:
    - `margin-bottom`: 요소의 아래쪽 여백을 20px로 설정합니다.

- `.input-box label` 클래스:
    - `font-size`: 글자 크기를 16px로 설정합니다.
    - `color`: 글자 색상을 #333으로 설정합니다.
    - `display`: 블록(display: block)으로 설정하여 레이블이 각 줄에 표시되도록 합니다.
    - `margin-bottom`: 아래쪽 여백을 5px로 설정합니다.

- `.input-box textarea` 클래스:
    - `width`: 너비를 100%로 설정하여 부모 요소의 너비를 가득 채우도록 합니다.
    - `padding`: 내부 여백을 10px로 설정합니다.
    - `border`: 1px 고정 크기의 경계를 #ccc 색상으로 설정합니다.
    - `border-radius`: 모서리를 5px로 둥글게 만듭니다.
    - `font-size`: 글자 크기를 16px로 설정합니다.
  

```css
바닐라 자바스크립트 코드와의 연동 
const element = document.getElementById('myDiv');
element.style.display = 'none'; // 요소 숨김
element.style.display = 'block'; // 요소 보이기
```

### 03 버튼 
```css
.button:hover {
background-color: #154b63;
} // 버튼을 클릭하면, 색깔이 바뀜
```

### 04 결과 화면
```css
#resultContainer {
    display: none;
    margin-top: 20px;
}
#resultTextarea {
    width: 100%;
    height: 300px;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    font-size: 14px;
    color: #555;
    background-color: #f7f7f7;
    white-space: pre-wrap;
    word-wrap: break-word;
    overflow-y: auto;
}
```

- `#resultContainer`와 `#resultTextarea`
   * 요소에 다음과 같은 스타일을 적용합니다:

- `#resultContainer` ID:
    - `display: none`: 요소를 화면에서 숨깁니다.
    - `margin-top`: 위쪽 여백을 20px로 설정합니다.

- `#resultTextarea` ID:
    - `width`: 너비를 100%로 설정하여 부모 요소의 너비를 가득 채웁니다.
    - `height`: 높이를 300px로 설정합니다.
    - `border`: 1px 고정 크기의 경계를 #ccc 색상으로 설정합니다.
    - `border-radius`: 모서리를 5px로 둥글게 만듭니다.
    - `padding`: 내부 여백을 10px로 설정합니다.
    - `font-size`: 글자 크기를 14px로 설정합니다.
    - `color`: 글자 색상을 #555로 설정합니다.
    - `background-color`: 배경색을 #f7f7f7로 설정합니다.
    - `white-space`: 줄 바꿈을 유지하며, 공백을 그대로 표시합니다.
    - `word-wrap`: 단어를 넘치는 경우 줄 바꿈 처리합니다.
    - `overflow-y`: 세로 스크롤바를 자동으로 표시합니다.

이 스타일을 적용하면 텍스트 영역이 깔끔하고 일관된 디자인으로 표시되며, 필요 시 스크롤이 가능해집니다.

### 05 로딩
```css
.loading-spinner {
    border: 4px solid #f3f3f3;
    border-radius: 50%;
    border-top: 4px solid #1c475d;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
    margin: 0 auto;
}
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}
```

이 CSS 코드는 로딩 스피너를 스타일링합니다:

- `.loading-spinner` 클래스:
    - `border`: 4px 고정 크기의 경계를 #f3f3f3 색상으로 설정합니다.
    - `border-radius`: 50%로 설정하여 원형으로 만듭니다.
    - `border-top`: 4px 고정 크기의 상단 경계를 #1c475d 색상으로 설정합니다.
    - `width`: 너비를 40px로 설정합니다.
    - `height`: 높이를 40px로 설정합니다.
    - `animation`: `spin` 애니메이션을 1초 동안 선형적으로 무한히 반복합니다.
    - `margin`: 요소를 중앙에 배치하기 위해 상하좌우 여백을 자동으로 설정합니다.

- `@keyframes spin` 애니메이션:
    - `0%`: 요소를 0도 회전합니다.
    - `100%`: 요소를 360도 회전합니다.

이 스타일을 적용하면 원형 로딩 스피너가 회전하는 애니메이션을 갖게 되며, 페이지 로딩 중 사용자에게 시각적인 피드백을 제공합니다.
