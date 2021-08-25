# BEM이란?

- CSS 클래스 명명 규칙
  - Block-Element-Modifier 의 준말
  - CSS를 쉽게 읽고, 쉽게 이해하고, 확장하기 용이하게 함

## BEM의 장점

1. 목적 또는 기능을 전달
2. 구성 요소의 구조를 전달
3. 선택자 특이성을 항상 낮은 수준으로 유지함

## BEM의 구성

1. 블록(Block) : 구성 요소의 가장 바깥쪽 상위 요소를 블록으로 정의한다.
2. 요소(Element) : 구성 요소 안쪽에는 하나 또는 그 이상의 요소가 있을 수 있다.
3. 변경자(Modifier) : 블록 또는 요소는 변경자를 이용하여 변형을 표시할 수 있다.

`.block__element--modifier`

## 예제

### 1. 요소(Element) 또는 변경자(Modifier)가 없는 단순한 구성 요소

- 간단산 구성 요소는 단일 요소와 단일 클래스를 사용하는 것만으로도 블록이(Block) 될 수 있다.

  ```html
  <!-- btn은 하나의 구성 요소다. -->
  <button class="btn"></button>

  <style>
    .btn {
      display: inline-block;
      color: blue;
    }
  </style>
  ```

### 2. 변경자가 있는 구성 요소

- 구성 요소에 변형이 있을때, 변형은 변경자 클래스로 구현한다.

  ```html
  <!-- --submit 변경자를 추가해서 확장. -->
  <button class="btn btn--submit"></button>

  <style>
    .btn {
      display: inline-block;
      color: blue;
    }

    .btn--submit {
      color: green;
    }
  </style>
  ```

> **주의!** <br>
> 변경자 클래스만 사용하면 안 된다. 변경자 클래스는 기본 클래스를 대체하기 위한 것이 아니라 확장하기 위한 것이기 때문

```html
<!-- 이렇게 작성하지 마세요. -->
<button class="btn--secondary"></button>

<style>
  .btn--secondary {
    display: inline-block;
    color: green;
  }
</style>
```

### 3. 하위 요소가 있는 구성 요소

- 더 복잡한 구성 요소에는 하위 요소가 있다
  - 스타일이 필요한 각 하위 요소에는 명명된 클래스가 있어야 한다.
  - BEM의 목적 중 하나는 특이성을 낮추고 일관성 있게 유지하는 것이다.
    - HTML의 하위 요소에서 클래스 이름을 생략하지 않아야 한다.
    - 클래스 이름을 생략하면 구성 요소 내부에 있는 이름 없는 요소의 스타일을 처리하기 위해 특이성이 더 높은 선택자를 사용해야 한다.
      > 클래스 이름을 생략하면 당장은 간결해보이지만 결국 나중엔 특이성 증가 때문에 위기를 맞게 된다.
      > <br> > <br>
      > BEM의 목표 중 하나는 대부분의 선택자가 단일 클래스 이름만 사용하는 것이다 (!important 같은 끔찍한 규칙을 덜 사용하도록 돕는다.)

```HTML
<!-- 올바른 예 -->
<figure class="photo">
  <img class="photo__img" src="me.jpg">
  <figcaption class="photo__caption">Look at me!</figcaption>
</figure>

<style>
  .photo {} /* 특이성 10 */
  .photo__img {} /* 특이성 10 */
  .photo__caption {} /* 특이성 10 */
</style>

-----------------------------------------------------------

<!-- 이렇게 작성하지 마세요 -->
<figure class="photo">
  <img src="me.jpg">
  <figcaption>Look at me!</figcaption>
</figure>

<style>
  .photo {} /* 특이성 10 */
  .photo img {} /* 특이성 11 */
  .photo figcaption {} /* 특이성 11 */
</style>
```

- 구성 요소에 여러 수준의 하위 요소가 있는 경우 클래스 이름에서 각 수준을 나타내려고 하면 안된다.
  - BEM은 구조의 깊이를 전달하지 않는다.
  - 구성 요소의 하위 요소를 나타내는 BEM 클래스 이름은 오직 기본 블록 이름과 하나의 요소 이름만 허용한다.
  - 아래 예시에서 `photo__caption__quote` 는 BEM을 잘못 사용한 것으로 `photo__quote` 가 더 적합하다.

```html
<!-- 올바른 예 -->
<figure class="photo">
  <img class="photo__img" src="me.jpg" />
  <figcaption class="photo__caption">
    <blockquote class="photo__quote">Look at me!</blockquote>
  </figcaption>
</figure>

<style>
  .photo {
  }
  .photo__img {
  }
  .photo__caption {
  }
  .photo__quote {
  }
</style>

<!-- 이렇게 작성하지 마세요 -->
<figure class="photo">
  <img class="photo__img" src="me.jpg" />
  <figcaption class="photo__caption">
    <blockquote class="photo__caption__quote">
      <!-- 클래스 이름에 여러 하위 요소(또는 구조)를 명명하지 말 것 -->
      Look at me!
    </blockquote>
  </figcaption>
</figure>

<style>
  .photo {
  }
  .photo__img {
  }
  .photo__caption {
  }
  .photo__caption__quote {
  } /* 초심자의 가장 흔한 실수 */
</style>
```

### 4. 변경자가 있는 요소

- 경우에 따라 구성 요소의 하위 단일 요소에 변형을 주려고 할 수 있다.
  - 이런 경우 구성 요소를 따로 추가하는 대신 하위 요소에 변경자를 추가하면 된다.

```html
<figure class="photo">
  <img class="photo__img photo__img--framed" src="me.jpg" />
  <figcaption class="photo__caption photo__caption--large">Loop at me!</figcaption>
</figure>

<style>
  .photo__img--framed {
  }
  .photo__caption--large {
  }
</style>
```

### 5. 구성 요소 변경자 기반으로 하위 요소 스타일 처리하기

- 동일한 구성 요소의 하위 요소를 동일한 방식으로 일관성 있게 수정하려면 기본 구성 요소에 변경자를 추가하고 하나의 변경자를 기반으로 각 하위 요소 스타일을 처리한다.
- 특이성이 높아지지만 구성 요소 수정이 훨씬 수월해진다.

```html
<!-- BEM이 선택자 중첩을 금지하는 건 아니다. 하지만 이것보다 중첩이 더 깊어지면 곤란하다. -->
<figure class="photo photo--highlighted">
  <img class="photo__img" src="me.jpg" />
  <figcaption class="photo__caption">Look at me!</figcaption>
  <figure>
    <style>
      .photo--highlighted .photo__img {
      }
      .photo--highlighted .photo__caption {
      }
    </style>

    -----------------------------------------------------------

    <!-- 이렇게 작성하지 마세요 -->
    <figure class="photo">
      <img class="photo__img photo__img--highlighted" src="me.jpg" />
      <figcaption class="photo__caption photo__caption--highlighted">Look at me!</figcaption>
    </figure>

    <style>
      .photo__img--highlighted {
      }
      .photo__caption--highlighted {
      }
    </style>
  </figure>
</figure>
```

### 6. 여러 단어 명명법(케밥 케이스, 카멜 케이스 접목)

- BEM 이름은 의도적으로 블록\_\_요소--변경자 를 분리하기 위해 단일 밑줄 대신 이중 밑줄과 이중 하이픈을 사용한다.
  - 단일 하이픈을 단어 구분 기호로 사용할 수 있기 때문이다.
  - 클래스 이름은 읽기 쉬워야 하기 때문에 보편적으로 인식할 수 없는 약어는 바람직하지 않다.

```HTML
<!-- 케밥 케이스를 접목 -->
<div class="some-thesis some-thesis--fast-read">
  <div class="some-thesis__some-element"></div>
</div>

<style>
  .some-thesis {}
  .some-thesis--fast-read {}
  .some-thesis__some-element {}
</style>

<!-- 카멜 케이스를 접목 -->
<div class="someThesis someThesis--fastRead">
  <div class="someThesis__someElement"></div>
</div>

<style>
  .someThesis {}
  .someThesis--fastRead {}
  .someThesis__someElemet {}
</style>

-----------------------------------------------------------

<!-- 이렇게 작성하지 마세요 -->
<div class="somethesis somethesis--fastread">
  <div class="somethesis--someelement"></div>
</div>

<style>
  .somethesis {}
  .somethesis--fastread {}
  .somethesis__someelement {}
</style>
```

---

예제로 이해하는 BEM : <https://naradesign.github.io/bem-by-example.html>
