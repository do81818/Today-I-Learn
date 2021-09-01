# Data 문법

- 표준이 아닌 속성이나 추가적인 DOM 속성, **Node.setUserData()** 와 같은 조작을 하지 않고도, 의미론적 HTML 요소에 추가 정보를 저장할 수 있도록 해줌

```html
<input type="text" name="id" data-name="name" data-value="value" />

HTML 표준 속성인 name 과 value 와는 아무 상관이 없다. 주의할점 data-index-number 일 경우 자바스크립트에서 접근시 카멜케이스로 변환 예) article.dataset.indexNumber
```

## JavaScript에서 접근하기

```jsx
// 1. getAttribute() 메소드
var article = document.getElementById("elem");
article.getAttribute("data-name");

// 2. dataset 속성
var article = document.getElementById("elem");
article.dataset.name;
```

## CSS에서 접근하기

```css
article[data-name] {
  width: 400px;
}
```

# 문제점

- 접근 보조 기술이 접근할 수 없음
- 검색 크롤러가 데이터 속성의 값을 찾지 못함
- IE11+ 이전 버전들은 **dataset**을 지원하지 않음
  - **getAttribute()** 메소드를 통해 접근
- 자바스크립트 안에 저장하는 것과 비교해서 데이터 속성을 읽는 성능이 떨어짐
- Firefox 49.0.2(이전/이후의 버전) 에서는, 1022 데이터를 초과하는 데이터 속성은 자바스크립트(EcmaScript4)가 읽지 못함
