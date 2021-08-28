## 1. 와일드 카드 꽉 채우기

- 와일드 카드 선택자는 모든 엘리먼트에 영향을 준다. 한 두 둦 정도는 괜찮지만 많은 스타일을 넣어선 안된다.
  - HTML 엘리먼트에 따라 연산이 많아진다.
  - 개발하면서 생기는 CSS 이슈들로 인해서 지우는 경우가 많다. (라이브러리를 사용하면 더더욱 사용할 필요가 없다)

## 2. 묶음으로 스타일링하기

- 묶음으로 CSS를 부여하는 것은 특별한 기능 없이 귀찮게 하는 경우가 자주 생긴다. (특히 라이브러리를 도입하거나, 새로운 컴포넌트를 생성할때)
  - 전역 엘리먼트 속성에 영향을 받는 모든 엘리먼트는 다 수정해줘야 하는 끔찍한 일이 펼쳐진다.
- 공통된 스타일을 주고 싶다면 `Class` 를 사용한다.
- `!important` 는 절대 사용하지 않는다.

## 3. HTML 기본 속성 무시하기

- block 요소는 기본적으로 한 줄을 모두 차지한다(width: 100%)
- inline 요소는 자신의 컨텐츠만큼의 크기를 가지기 때문에 width와 height를 부여해도 값이 설정되지 않는다.
  - `<span>` 은 텍스트에만 사용한다
  - `<li>` 은 `<ul>`,`<ol>` 안에 넣어서 사용한다.
  - `<main>` 은 페이지 당 한 번 이하로 사용한다.
  - HTML 태그 기본 속성을 지우면서 사용하지 않는다.

## 4. 마음대로 헤딩 엘리먼트 숫자 넣기

- 제목 엘리먼트는 잘 설계된 웹사이트를 구별해준다. (텍스트의 중요도, 디자인을 쉽게 만들어줌, 웹사이트 전체에 일관성을 부여함)
  - 또한 검색엔진은 헤딩 엘리먼트의 중요도를 다르게 판단하고 텍스트를 처리한다.
- 핵심이 되는 메시지는 `<h1>`, `<h2>`에 넣는다.
- `<h1>`, `<h2>`에 대한 스타일을 페이지 별로 변화를 주기보단 통일한다.
- margin이나 padding을 변화해야하는 상황에선 클래스를 추가해서 관리한다.

## 5. 라이브러리 사용 안하기

- UI 라이브러리는 생산성을 몇 배는 항상시켜주며, 스타일에 통일성을 주고, 귀찮은 일들을 대신해준다
- 또한 모바일 최적화가 된 라이브러리가 대부분이다. 그러므로 이런 혜택을 거부하고 라이브러리를 사용하지 않는다면 그에 합당한 이유가 있어야 한다.
  - 라이브러리를 사용할 수 있는 부분은 무조건 사용한다.

## 6. !important 즐겨쓰기

- !important는 CSS 선택자 규칙(명시도)를 모두 무시해버린다.
  - !important 밖에 답이 없는 상황이라면 잘못 만든 것이다.
  - !important 를 사용하기 시작하면 계속 사용해야 한다.
    - 결국 다른 !important와 충돌하게 될 확률이 높다.

## 7. :before, :after 사용 안하기

- 제목 엘리먼트에 포인트를 주거나, 반복적으로 사용하는 텍스트, 스타일링에 도움이 되며 생산성에 도움이 된다.

## 8. Flex, Grid 안쓰기