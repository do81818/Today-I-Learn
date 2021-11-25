# 1. 수평 레이아웃 구성

- 레이아웃을 좌우, 수평방향으로 배치하기 위한 방법
  - disply: inline-block
  - table
  - float
  - display: flex 또는 grid
- flex 또는 grid를 사용하는 방법이 가장 최신 방법이지만 오래된 브라우저에서 정상적으로 작동하지 않는 문제가 있음
- table 태그는 표를 그리기 위한 용도이므로, 레이아웃을 구성하는 측면에는 바람직하지 못함
- inline-block 또한 레이아웃을 용도로 만들어진 속성이 아니기 때문에 사소한 문제가 많이 발생

## float + clear 속성으로 수평 레이아웃 구성

```html
<style>
  .container:after {
    clear: both;
    display: block;
    content: "";
  }

  .container .item {
    float: left;
    width: 100px;
    text-align: center;
    border-right: 1px solid #ddd;
  }

  .container .item.last {
    float: right;
    border-right: none;
  }
</style>

<div class="container">
  <div class="item">menu 1</div>
  <div class="item">menu 2</div>
  <div class="item">menu 3</div>
  <div class="item last">login</div>
</div>
```

# 2. 수평 가운데 정렬

- 웹 브라우저의 크기는 고정되어 있지 않고 사용자가 늘였다 줄였다 할 수 있음
  - 이에 대응하여 크기마다 모두 디자인을 하는 것은 불가능함
  - 그러므로, 기본 컨텐츠를 보여줄 컨테이너의 크기는 고정하고 남는 공간은 여백으로 사용하는 것이 보편적인 웹 디자인 방법 중 하나임

## left와 margin-left

- absolute position의 left와 margin-left 속성을 잘 이용하면 이를 구현할 수 있음
  - left 를 50%를 주고, width의 절반 만큼 더 왼쪽으로 이동시키면 된다.
    - **left: 50%, margin-left: -{width / 2}**
    - 단 width를 동적으로 가져와 절반 만큼 사용하는 방법은 일반적으로 존재하지 않으며 width와 padding을 계산하여 적어야 함
    - 또 absolute요소는 부모의 height계산에 반영되지 않으므로 부모의 height를 고정해서 적어주어야 할 수도 있음
  - HTML 요소의 좌표계는 중앙이 아닌 좌상단이기 때문

```html
<style>
  .container {
    position: absolute;
    left: 50%;
    margin-left: -265px; // -(530 / 2)
    width: 500px;
    padding: 15px;
    background: white;
  }
</style>

<div style="height: 300px">
  <div style="container">
    <h4 style="margin: 0; padding: 10px 0; font-size: 26px">Title</h4>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
      commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
      velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
      cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id
      est laborum.
    </p>
  </div>
</div>
```

## margin: auto

- left와 margin-left를 사용하는 방법은 그리 좋은 방법은 아님
- 일반적으로는 margin속성의 auto 값을 주로 사용함,
  - margin-left와 margin-right가 모두 auto라면, 브라우저는 해당 요소를 수평 가운데 정렬하기 때문
  - margin: 0 auto; // 상하 여백은 사용하지 않고 좌우 여백은 auto로 설정

```html
<style>
  .container {
    margin: 0 auto;
    width: 500px;
    padding: 15px;
    background: white;
  }
</style>

<div>
  <div class="container">
    <h4 style="margin: 0; padding: 10px 0; font-size: 26px">Title</h4>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
      commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
      velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
      cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id
      est laborum.
    </p>
  </div>
</div>
```

## 보충

- left: 50%와 margin-left: <값> 은 메인 컨테이너를 가운데 정렬하는데 잘 사용되지는 않지만, absolute인 요소를 가운데 배치할 때는 종종 사용함

# 3. 수직 가운데 정렬

## padding

- 많이 사용되는 방법
- 상하 여백이 얼마인지 아는 경우라면 그에 맞춰서 텍스트에 상하 padding을 주면 된다.
  - 상하 여백이 얼마인지 나와있지 않은 경우 주어진 정보를 통해 유추할 수 있다.
  - 폰트의 크기는 너비가 아닌 높이를 의미
    - 전체 높이가 50px이고 폰트 사이즈가 16px 일 경우 (50 - 16) / 2 로 상하 여백이 17이라고 유추할 수 있음
    - 마찬가지로 전체 높이가 50px이고 폰트 사이즈가 24px일 경우 (50 - 24) / 2 로 13이라고 유추
- box-sizing을 border-box로 사용하지 않는다면 해당 요소들의 height를 50px로 고정해서는 안된다.

## line-height

- line-height는 본래 줄 간격을 나타내는 속성이지만, 인라인 요소의 수직 가운데 정렬을 구현하기 위해서도 널리 이용된다.
- % 나 실수 등 비율로 표현할 수 있지만 px 단위로도 사용이 가능하며, 폰트 크기보다 크게 설정 할 경우 가운데에 요소가 알맞게 그려진다.
  - padding을 따로 계산할 필요 없이 height와 동일한 값을 적어주면 된다는 측면에서 매우 간단하게 가운데 정렬을 구현할 수 있음
  - 오래된 브라우저에서도 잘 작동함
- 다만 본래 줄 간격을 나타내는 속성의 특징상, 여러 줄의 텍스트는 사용이 불가능하다는 한계가 존재

```html
<style>
  .component:after {
    clear: both;
    display: block;
    content: "";
  }
  .component .label {
    float: left;
    width: 200px;
    height: 50px;
    font-size: 16px;
    line-height: 50px;
    text-align: center;
    background-color: #ccc;
  }

  .component .plus {
    float: left;
    width: 50px;
    height: 50px;
    font-size: 24px;
    line-height: 50px;
    text-align: center;
    background-color: #2c77f6;
    color: white;
  }
</style>

<div class="component">
  <div class="label">Add to my cart</div>
  <div class="plus">+</div>
</div>
```
