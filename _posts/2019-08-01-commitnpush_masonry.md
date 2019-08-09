---
layout: post
title: 레이아웃의 꽃 - Masonly Layout
tags: [html layout, masonry, pinterest]
author-id: commitnpush
excerpt_separator: <!--more-->
---

#### 들어가며

웹페이지에서 정보를 잘 표현해 주기 위해서 효율적인 엘리먼트의 배치는 매우 중요합니다. 중요한 정보는 위에 덜 중요한 정보는 밑으로 배치를 하며 비슷한 중요도를 가지는 내용은 가로로 나란히 배치하기도 합니다. 이러한 배치를 하는 작업 또는 배치된 상태를 레이아웃이라고 부릅니다. <!--more-->

일반적으로 UI/UX 디자이너에 의해서 레이아웃이 정해지면 퍼블리셔나 웹 개발자가 해당 레이아웃을 코드로 구현하게 됩니다. 요즘 웹은 워낙 다양한 정보를 표현해 줘야 하기 때문에 상황에 맞는 적절한 레이아웃을 UI 디자인에 있어서 굉장히 중요한 작업입니다. 여러 가지 레이아웃 중 이번 포스팅에서는 flex, grid를 활용하여 masonry 레이아웃을 구현해 보도록 하겠습니다.

<br>

#### Masonry Layout?

masonry이라는 단어가 다소 생소하게 느껴질 수도 있는데 '석조', '돌쌓기'라는 뜻을 가지고 있습니다. 

<div style="text-align:center; margin-bottom: 16px;">
  <img style="display: inline; padding:0 10px;" src="/assets/img/commitnpush/masonry/1.jpg" alt="벽돌쌓기1">
  <img style="display: inline; padding:0 10px; height: 176px;" src="/assets/img/commitnpush/masonry/2.jpg" alt="벽돌쌓기2">
</div>

벽돌을 가지고 담을 만들때 각각의 돌의 크기가 일정하지 않을 때에는 그림들 처럼 쌓을 수 밖에 없습니다. 이와 마찬가지로 화면에 표시해야 할 엘리먼트들의 가로 또는 세로의 길이가 제각각이라면 격자형의 float 속성을 이용한 카드 레이아웃을 사용했을 때는 다음 그림과 같이 중간마다 빈 공간이 생기게 됩니다.

<img src="/assets/img/commitnpush/masonry/3.png" style="width: 600px;" alt="카드 레이아웃">

이처럼 가로길이는 고정이지만 세로길이가 각양각색일때 벽돌을 쌓듯이 빈 공간을 없애주는 masonry 레이아웃을 함께 만들어 보도록 하겠습니다.

<br>

#### display: flex 속성 이용한 masonry layout

flex 속성값은 display 속성에서 사용할 수 있는 속성값으로 이름 그대로 신축성 있는 엘리먼트를 만들 수 있게 해줍니다. 

```html
<div class="wrapper">
  <div class="item">1</div>
  <div class="item">2</div>
</div>
```

```css
div{
  box-sizing: border-box;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
}

.wrapper > .item {
  width: 150px;
  height: 150px;
  color: white;
  background-color: #000;
  padding: 10px;
  margin: 10px;
}
```

<img src="/assets/img/commitnpush/masonry/4.png" style="width: 600px;" alt="flex속성">

위 그림처림 부모 엘리먼트에 display: flex 속성이 적용되면 자식 엘리먼트는 display 속성에 상관없이 무조건 display: inline-block 속성이 적용된 것 마냥 가로로 배치됩니다. 여기서 '신축성'이라는 느낌을 보여주려면 다음 코드를 추가하시면 됩니다.

```css
.wrapper > .item:first-child{
  flex-grow: 1;
}
```

<img src="/assets/img/commitnpush/masonry/5.png" style="width: 600px;" alt="flex-grow속성">

첫 번째 자식엘리먼트의 너비는 width 속성에 상관없이 길어졌습니다. flex-grow: 1 속성에 의해서 남은 영역을 차지해 버린것이죠. 만약 두 번째 엘리먼트에도 똑같이 flex-grow: 1의 속성을 적용해줬다면 남은 영역을 똑같이 차지해서 두 엘리먼트의 너비는 1:1의 비율을 유지할 겁니다. 이처럼 flex 속성을 이용하면 원하는 레이아웃을 쉽게 만들어 낼 수 있습니다.

flex-direction이라는 속성을 이용해서 자식 엘리먼트들이 배치되는 방향을 지정할 수 있습니다. 기본값은 row(행)이기 때문에 가로로 배치되지만 column(열)으로 지정하면 세로로 배치됩니다.

```css
.wrapper {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  height: 500px;
}
```

<img src="/assets/img/commitnpush/masonry/6.png" style="width: 120px; padding: 0;" alt="세로 배치">

이를 응용해서 위의 카드 레이아웃에서의 중간중간의 빈 공간을 메꿔줄 수 있습니다.

```html
<div class="card-wrapper">
  <div class="card-wrapper__card card-wrapper__card--small">1</div>
  <div class="card-wrapper__card">2</div>
  <div class="card-wrapper__card card-wrapper__card--large">3</div>
  <div class="card-wrapper__card">4</div>
  <div class="card-wrapper__card card-wrapper__card--small">5</div>
  <div class="card-wrapper__card">6</div>
  <div class="card-wrapper__card card-wrapper__card--large">7</div>
  <div class="card-wrapper__card">8</div>
  <div class="card-wrapper__card">9</div>
  <div class="card-wrapper__card card-wrapper__card--small">10</div>
</div>
```

```css
div{
  box-sizing: border-box;
}

.card-wrapper{
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-content: flex-start;
  height: 600px;
  
}

.card-wrapper .card-wrapper__card{
  width: 150px;
  height: 150px;
  background-color: #000;
  color: #fff;
  padding: 10px;
  margin: 10px;
}

.card-wrapper .card-wrapper__card.card-wrapper__card--small{
  height: 100px;
}

.card-wrapper .card-wrapper__card.card-wrapper__card--large{
  height: 200px;
}
```

<img src="/assets/img/commitnpush/masonry/7.png" style="width: 600px;" alt="masonry(flex)">

보기엔 그럴듯해 보이지만 자세히 보면 배치의 순서가 달라졌다는 것을 알 수 있습니다. 위의 카드 레이아웃에서의 자식 엘리먼트들의 순서는 왼쪽부터 오른쪽으로 채워져 나가며 더이상 공간이 없으면 다음 줄로 이동합니다. 하지만 flex를 사용하게 되면서 배치의 순서가 세로로 변경이 되어버렸습니다. 

엘리먼트의 순서가 중요하지 않거나 배치를 세로로 해야 하는 경우에는 상관없겠지만 엘리먼트의 순서가 중요할 경우에는 우리가 글을 읽을 때처럼 왼쪽에서 오른쪽으로의 배치가 더 자연스럽습니다. 그리고 한 가지 문제가 또 있는데 만약 무한스크롤 또는 더보기 버튼을 활용해서 엘리먼트를 추가한다면 어떻게 될까요? 

<img src="/assets/img/commitnpush/masonry/8.png" style="width: 600px;" alt="masonry(flex) added">

이상한 점을 예상하셨거나 느끼셨나요? 두 번째 줄 위에 있는 4번 엘리먼트가 첫 번째 줄로 이동해 버리고 이어서 쭉 같은 현상이 발생합니다. 만약 숫자가 없었다면 3줄에서 4줄이 됐다고 느꼈겠지만 4번째 줄에 새로운 데이터들이 추가된 것이 아닙니다. 이런 방식으로 추가가 된다면 사용자는 혼란스러움을 겪게 될 것입니다.

flex를 고집한다면 이를 해결하기 위해서 순서를 조작해주는 자바스크립트 코드가 필요합니다. 자바스크립트 코드가 많이 복잡하진 않지만 정적인 UI는 CSS만으로 해결하는 것이 좋아 보입니다.

<br>

#### display: grid 속성 이용한 masonry layout

저는 특별한 경우(IE 하위버전 대응 같은)가 아니라면 flex를 이용해서 레이아웃을 구성하는 편이기 때문에 grid보다 flex를 선호하는 편입니다. 이름에서 느껴지듯이 flex가 하나의 행(1차원)에서 엘리먼트들의 flexible 한 배치를 다룬다면 grid는 여러 개의 행(2차원)에서 엘리먼트들의 배치를 다룹니다. 물론 flex도 여러개의 행을 다루기 위한 속성들을 지원하지만, grid가 더 많은 기능을 가지고 있습니다. 많은 기능을 사용할 수 있다는 말은 그만큼 사용법이 복잡할 수 있다는 뜻도 포함하고 있습니다.

```css
.card-wrapper{
  display: grid;
  grid-template-columns: 10% 20% 30% 40%;
  grid-template-rows: 50% 30% 20%;
}
```

<img src="/assets/img/commitnpush/masonry/9.png" style="width: 600px;" alt="card(grid)">

grid 속성은 grid-template-columns, grid-template-rows 속성을 사용하여 열과 행의 개수 너비, 비율 등을 조절합니다. 단위는 %, px(pixel), fr(fraction, 비율)을 주로 사용하며 repeat과 같은 함수도 사용할 수 있어 자유롭게 설정할 수 있습니다.

```css
.card-wrapper{
  display: grid;
  grid-template-columns: repeat(auto-fill, 170px);
  grid-auto-rows: 50px;
}

.card-wrapper .card-wrapper__card{
  width: 150px;
  /* height: 150px; */
  grid-row-end: span 3;
  background-color: #000;
  color: #fff;
  padding: 10px;
  margin: 10px;
}

.card-wrapper .card-wrapper__card.card-wrapper__card--small{
  /* height: 100px; */
  grid-row-end: span 2;
}

.card-wrapper .card-wrapper__card.card-wrapper__card--large{
  /* height: 200px; */
  grid-row-end: span 4;
}
```

<img src="/assets/img/commitnpush/masonry/10.png" style="width: 600px;" alt="masonry(grid)">

grid-template-rows: repeat(auto-fill, 170px)으로 계속해서 부모의 너비가 허용하는 한 170px(엘리먼트의 너비 150px + 여백 좌우 10px)의 열들이 반복해서 추가되게 하였고 grid-auto-rows: 50px로 행의 기본 높이를 지정했습니다. 그리고 각 엘리먼트에서는 높이를 사용하는 대신 grid-row-end: span n을 통해서 행의 기본 높이 * n만큼의 높이를 차지하게 하였습니다. 

높이를 지정할 때 grid-row-end 속성과 span 속성값을 사용하면 엘리먼트의 실제 높이를 지정하는 것이 아닌 몇 개의 행을 차지할 것인지를 지정하기 때문에 엘리먼트와 엘리먼트의 공간이 발생하지 않습니다. 이해가 잘 안 되시면 테트리스를 상상해 보세요 :)

자 이제 마지막 관문이 남았습니다. 과연 엘리먼트들이 추가되었을 때 순서가 잘 유지되는지를 확인해보면 될 것 같습니다.

```html
<div class="card-wrapper">
  <div class="card-wrapper__card card-wrapper__card--small">1</div>
  <div class="card-wrapper__card">2</div>
  <div class="card-wrapper__card card-wrapper__card--large">3</div>
  <div class="card-wrapper__card">4</div>
  <div class="card-wrapper__card card-wrapper__card--small">5</div>
  <div class="card-wrapper__card">6</div>
  <div class="card-wrapper__card card-wrapper__card--large">7</div>
  <div class="card-wrapper__card">8</div>
  <div class="card-wrapper__card">9</div>
  <div class="card-wrapper__card card-wrapper__card--small">10</div>
  <div class="card-wrapper__card card-wrapper__card--small">11</div>
  <div class="card-wrapper__card">12</div>
  <div class="card-wrapper__card card-wrapper__card--large">13</div>
  <div class="card-wrapper__card">14</div>
</div>
```

<img src="/assets/img/commitnpush/masonry/11.png" style="width: 600px;" alt="masonry(grid)">

기존 10개의 엘리먼트의 배치가 그대로인 상태에서 4개의 엘리먼트가 잘 추가되었습니다. 이러한 레이아웃이 적용된 대표적인 사이트가 <a href="https://www.pinterest.co.kr/">핀터레스트</a>입니다. 다음에 기회가 된다면 더 재미있고 참신한 레이아웃을 다뤄보길 기대하며 이쯤에서 마무리 짓겠습니다.
