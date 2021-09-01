# Ten modern layouts in one line of CSS

## 1. Super Centered

- place-items : center;
  - align-items 와 justify-items 의 단축 속성
- contenteditable
  - 사용자가 요소를 편집할 수 있는지 나타내는 열거형 특성

```HTML
  <style>
    .parent {
      display: grid;
      place-items: center;
    }
  </style>

  <body>

    <div class="parent blue">
      <div class="box coral" contenteditable>
    </div>

  </body>
```

## 2. The Deconstructed Pancake

- flex : flex-grow | flex-shrink | flex-basis

```HTML
  <style>
    .parent {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }

    .box {

      flex: 1 1 150px; /*  Stretching: */
      flex: 0 1 150px; /*  No stretching: */

      margin: 5px;
    }
  </style>

  <body>

    <div class="parent white">
      <div class="box green">1</div>
      <div class="box green">2</div>
      <div class="box green">3</div>
    </div>

  </body>
```

## 3. Sidebar Says

- minmax()
  - '최소/최대 크기'를 정의

```HTML
  <style>
    .parent {
      display: grid;
      grid-template-columns: minmax(150px, 25%) 1fr;
    }
  </style>

  <body>

    <div class="parent">
      <div class="section yellow" contenteditable>
      Min: 150px / Max: 25%
      </div>
      <div class="section purple" contenteditable>
        This element takes the second grid position (1fr), meaning
        it takes up the rest of the remaining space.
      </div>
    </div>

  </body>
```

## 4. Pancake Stack

```HTML
  <style>
    .parent {
      display: grid;
      grid-template-rows: auto 1fr auto;
    }
  </style>

  <body>

    <div class="parent">
      <header class="blue section" contenteditable>Header</header>
      <main class="coral section" contenteditable>Main</main>
      <footer class="purple section" contenteditable>Footer Content</footer>
    </div>

  </body>
```

## 5. Classic Holy Grail Layout

- grid-template
  - 행 / 열

* grid-column
  - grid-column-start 와 grid-column-end 속성을 한번에 쓰는 축약형

```HTML
  <style>
    .parent {
      display: grid;
      grid-template: auto 1fr auto / auto 1fr auto;
    }

    header {
      padding: 2rem;
      grid-column: 1 / 4;
    }

    .left-side {
      grid-column: 1 / 2;
    }

    main {
      grid-column: 2 / 3;
    }

    .right-side {
      grid-column: 3 / 4;
    }

    footer {
      grid-column: 1 / 4;
    }
  </style>

  <body>

    <div class="parent">
      <header class="pink section">Header</header>
      <div class="left-side blue section" contenteditable>Left Sidebar</div>
      <main class="section coral" contenteditable> Main Content</main>
      <div class="right-side yellow section" contenteditable>Right Sidebar</div>
      <footer class="green section">Footer</footer>
    </div>

  </body>
```

## 6. 12-Span Grid

```HTML
  <style>
    .parent {
      display: grid;
      grid-template-columns: repeat(12, 1fr);
    }

    .span-12 {
      grid-column: 1 / span 12;
      /* grid-column: 1 / 13; 과 같음 */
    }

    .span-6 {
      grid-column: 1 / span 6;
    }

    .span-4 {
      grid-column: 4 / span 4;
    }

    .span-2 {
      grid-column: 3 / span 2;
    }

    /* centering text */
    .section {
      display: grid;
      place-items: center;
      text-align: center
    }
  </style>

  <body>

    <div class="parent white">
      <div class="span-12 green section">Span 12</div>
      <div class="span-6 coral section">Span 6</div>
      <div class="span-4 blue section">Span 4</div>
      <div class="span-2 yellow section">Span 2</div>
    </div>

  </body>
```

## 7. RAM (Repeat, Auto, Minmax)

- auto-fit
  - minmax 함수의 기본 크기를 초과할 때 나머지 공간을 모두 채움
- auto-fill
  - minmax 함수의 기본 크기를 초과할 때 늘어나지 않음

```HTML
  <style>
    .parent {
      display: grid;
      grid-gap: 1rem;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    }
  </style>

  <body>

    <div class="parent white">
      <div class="box pink">1</div>
      <div class="box purple">2</div>
      <div class="box blue">3</div>
      <div class="box green">4</div>
    </div>

  </body>
```

## 8. Line Up

```HTML
  <style>
    .parent {
      height: auto;
      display: grid;
      grid-gap: 1rem;
      grid-template-columns: repeat(3, 1fr);
    }

    .visual {
      height: 100px;
      width: 100%;
    }

    .card {
      display: flex;
      flex-direction: column;
      padding: 1rem;
      justify-content: space-between;
    }

    h3 {
      margin: 0
    }
  </style>

  <body>

    <div class="parent white">
      <div class="card yellow">
        <h3>Title - Card 1</h3>
        <p contenteditable>Medium length description with a few more words here.</p>
        <div class="visual pink"></div>
      </div>
      <div class="card yellow">
        <h3>Title - Card 2</h3>
        <p contenteditable>Long Description. Lorem ipsum dolor sit, amet consectetur adipisicing elit.</p>
        <div class="visual blue"></div>
      </div>
      <div class="card yellow">
        <h3>Title - Card 3</h3>
        <p contenteditable>Short Description.</p>
        <div class="visual green"></div>
      </div>
    </div>

  </body>
```

## 9. Line Up

- clamp(min, actual, max)
  - 항상 actual로 유지하되 min 이하로는 줄어들지 않고, max 이상으로는 커지지 않음

```HTML
  <style>
    .parent {
      display: grid;
      place-items: center;
    }

    .card {
      width: clamp(23ch, 50%, 46ch);
      display: flex;
      flex-direction: column;
      padding: 1rem;
    }

    .visual {
      height: 125px;
      width: 100%;
    }
  </style>

  <body>

    <div class="parent white">
      <div class="card purple">
        <h1>Title Here</h1>
        <div class="visual yellow"></div>
        <p>Descriptive Text. Lorem ipsum dolor sit, amet consectetur adipisicing elit. Sed est error repellat veritatis.</p>
      </div>
    </div>

  </body>
```

## 10. Respect for Aspect

- aspect-ratio : 종횡비
  - width / height
  - 16 / 9

```HTML
  <style>
    .parent {
      display: grid;
      place-items: center;
    }

    .visual {
      aspect-ratio: 16 / 9;
    }

    .card {
      width: 50%;
      display: flex;
      flex-direction: column;
      padding: 1rem;
    }
  </style>

  <body>

    <div class="parent white">
      <div class="card blue">
        <h1>Video Title</h1>
        <div class="visual green"></div>
        <p>Descriptive Text. This demo works in Chromium 84+.</p>
      </div>
    </div>

  </body>
```

---

Ten modern layouts in one line of CSS : https://web.dev/one-line-layouts/
