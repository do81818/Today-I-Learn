# Semantic 이란?

- 사전적인 의미로는 의미의, 의미론적인 이라는 뜻을 가지고 있음

```HTML
<!-- 표현을 기준으로 이름 지어진 태그들은 더 이상 사용하지 않는다. -->
<b> : 굵은 글씨             <tt> : 타이프체
<i> : 이탤릭체              <u> : 밑줄
<big> : 큰 글씨             <center> : 중앙 정렬
<small> : 작은 글씨         <nobr> : 줄바꿈 안함
<blink> : 깜빡임            <font> : 글씨 모양
<s> : 가로줄                <marquee> : 흐르는 글씨
```

> HTML5 버전에서 `<i>` 태그가 단순 표현용 태그에서 특정 이유(기술적인 용어, 외국어 문구, 소설속 인물의 생각 등)로 다른 글자와 구분하기 위해 사용되는 태그로 변경되었다.

```HTML
<!-- 컨텐츠가 가진 의미를 나타내는 태그들을 가능한 지켜 사용한다. -->
<strong> : 강한 강조        <samp> : 시스템 출력
<em> : 강조                 <code> : 개발 코드
<p> : 문단                  <ins> : 추가된 내용
<q> : 짧은 인용             <address> : 주소
<cite> : 작품 제목          <blockquote> : 인용문
<del> : 삭제된 내용         <abbr> : 약자표기
```

## Heading과 웹페이지의 정보구조

- 웹 페이지에서는 헤딩이 목차와 같은 역할을 한다.
- 웹 페이지의 내용이 어떤 흐름을 가지고 있는지 한 눈에 알 수 있다.
- 헤딩이 잘못 제공되면 사용자에게 잘못된 정보구조를 전달하게 되므로 헤딩은 정보구조를 반영하여 올바른 순서로 작성되어야 한다.

```HTML
<h1>네이버</h1>
  <h2>뉴스스탠드</h2>
    <h3>구독목록</h3>
    <h3>전체언론사</h3>
  <h2>로그인</h2>
  <h2>타임스퀘어</h2>
  <h2>주제형캐스트</h2>
  .. 등등
```

## 잘못된 시맨틱 마크업은 사용자에게 혼란을 준다.

```HTML
<!-- 잘못된 시멘틱 마크업의 예 -->
<img src="img/tiger.jpg" alt="코끼리"> <!-- 호랑이 사진 -->
```

## 섹셔닝 요소는 outline을 만든다.

- 섹셔닝 요소란?
  - HTML5 컨텐츠 모델에서의 Sectioning 태그 (article, aside, nav, section 등)
  - header와 footer는 섹셔닝 요소에 포함되지 않는다.
- HTML4 웹 페이지에서는 헤딩이 목차와 같은 역할을 했지만 HTML5 에서는 섹션이 만드는 구역과 헤딩을 조합하여 아웃라인을 만들고, 이 아웃라인이 목차의 역할을 한다.
  - 아웃라인은 3가지 요소(섹션, 헤딩, 섹셔닝 루츠)에 의해 결정된다.
    - 섹셔닝 루츠(Sectioning Roots)란?
      - body, blockquote, details, fieldset, figure, td 등
      - 섹셔닝 루츠는 별개의 새로운 문서로 취급하기 때문에 그 하위에 있는 내용은 아웃라인에 포함시키지 않는다.

## 코드로 살펴보기

### 1. 각 섹션 요소는 헤딩에 의해 이름이 지어진다.

- 이름이 없을 경우 Untitled Section(무제) 섹션이 된다.

```HTML
<h1>예제</h1>
<section>
  <h2>멋진 섹션</h2>
</section>
<article>
  <h2>아주 좋은 글</h2>
</article>
<section>
  여긴 헤딩이 없어요
</section>

---

1. 예제
  1. 멋진 섹션
  2. 아주 좋은 글
  3. Untitled Section
```

### 2. 한 섹션 안에 헤딩이 여러 개일 때는, 헤딩이 섹션을 만든다.

```HTML
<h1>책 팝니다</h1>
<section>
  <h1>중고 책</h1>
  <h2>HTML 공부하기</h2>
  <h2>CSS 공부하기</h2>
  <h2>Java 공부하기</h2>
</section>

---

1. 책 팝니다.
  1. 중고책
    1. HTML 공부하기
    2. CSS 공부하기
    3. Java 공부하기
```

### 3. body는 섹셔닝 루츠이기 때문에 헤딩이 없으면 문서의 제목이 없는 무제 문서가 된다.

```HTML
<!-- <h1>헤딩이 없네?</h1> -->
<section>
  <h1>중고 책</h1>
  <h2>HTML 공부하기</h2>
  <h2>CSS 공부하기</h2>
  <h2>Java 공부하기</h2>
</section>

---

1. Untitled Section
  1. 중고 책
    1. HTML 공부하기
    2. CSS 공부하기
    3. Java 공부하기
```

### 4. 헤딩과 섹셔닝 요소가 형제 요소일때

- h2 와 article이 형제 요소이므로 같은 단계에 위치하는 아웃라인이 만들어진다.

```HTML
<h1>물건 팝니다</h1>
<h2>쓰던 물건</h2>
<article>
  <h3>볼펜</h3>
  <p>아주 잘 나와요</p>
</article>
<article>
  <h3>지갑</h3>
  <p>중후한 멋이 있어요</p>
</article>
<article>
  <h3>신발</h3>
  <p>냄새 안나고 깨끗합니다</p>
</article>

---

1. 물건 팝니다
  1. 쓰던 물건
  2. 볼펜
  3. 지갑
  4. 신발
```

### 5. 섹셔닝 루츠 안에 또 다른 섹셔닝 루츠가 있는 경우

- 그 안의 내용은 아웃라인에 포함시키지 않는다.

```HTML
<h1>책팝니다</h1>
<blockquote>
  <h1>중고책</h1>
  <h2>HTML 공부하기</h2>
  <h2>CSS 공부하기</h2>
  <h2>Java 공부하기</h2>
</blockquote>

---

1. 책팝니다
```

### 아웃라인 설계를 돕는 도구

- HTML5 Outliner : <http://gsnedders.html5.org/outliner/>

---

널리세미나 시멘틱한 HTML5 마크업 구조 설계, 어떻게 할까? : <https://nuli.navercorp.com/seminar/s03th>
