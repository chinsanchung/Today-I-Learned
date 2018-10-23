# 생활코딩 CSS 수업 03
### flex
---
기존의 테이블로 레이아웃을 짜는 것은 처음에 만드는 건 쉽지만 새로 추가하거나 보수하는 것은 매우 힘들었습니다. 다음으로는 포지션으로 하는 방법, float 로 레이아웃을 잡는 법 등을 사용했었습니다.(float 는 위치이동이 주 목적이 아니라는 것을 참고합니다.)
---
`flex`는 요소의 크기, 위치를 쉽게 잡아줍니다. 이것을 쓰면 레이아웃을 효과적으로 표현해줍니다.
`flex`를 쓰려면 우선 부모 컨테이너와 자식 태그들로 구성해야 합니다. 컨테이너와 자식 태그들은 각각 다른 flex 속성들을 입력해야 합니다. 이것들을 구분할 줄 알아야 합니다. (부모, 자식의 이름을 container, item 으로 입력을 강제하는건 아닙니다.)
  - container : display, flex-direction, flex-wrap, flex-flow, align-items, align-content
  - item : order, flex-grow, flex-shrink, flex-basis, flex, align-self
1. 기본
`flex`를 사용하려면 컨테이너에 `display:flex` 속성을 넣어야 합니다. 그리고 `flex-direction`으로 정렬 방향을 바꾸면 됩니다. 정렬의 기본값은 row 입니다.
```HTML
<div class="container">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```
```CSS
.container {
  background-color: powderblue;
  display: flex;
  flex-direction: row;
}
.item {
  background-color: tomato;
  color: white;
  border: 1px solid white;
}
```
- 자식 div 들이 각각 한 줄을 차지했는데(block 이라서) 부모를 flex 로 한 순간 일렬로 정렬하게 됩니다.
  - `flex-direction`으로 row, row-reverse, column, column-reverse 로 정렬합니다. (기본값 row)
- flex 의 자식 요소들은 기본적으로 컨테이너 화면 전체를 사용합니다.
2. grow 와 shrink
```CSS
.item {
  flex-grow: 1;
}
/*2번째 항목을 설정합니다.*/
.item:nth-child(2) {
  flex-basis: 200px;
  /* flex-grow: 2; */

}
```
- `flex-basis: 200px` : 2번째 항목의 기본 크기를 설정합니다. 방향에 따라 row 면 너비, column 이면 높이를 지정합니다.
- `flex-grow` : 각 아이템들으로 여백을 채우기 위해 사용합니다. 각 요소의 `flex-grow`는 기본적으로 0입니다만, 1로 주면 각 요소가 여백을 꽉 채우게 됩니다.(컨테이너 공간을 1/n 으로 공평하게 나눠가집니다.)
  - 여기서 두번째 칸에만 `flex-grow`를 2로 주면, 우선순위가 높기 때문에 2번 항목만 2/6, 나머지는 각각 1/6을 가지게 됩니다.
  - item 의 `flex-grow`를 지우고 2항목만 1을 주면 2항목가 컨테이너의 90% 이상을 차지하고 나머지는 조금씩 차지하게 됩니다. 100을 주던 200을 주던 결과는 같습니다.
- `flex-shrink` : `flex-shrink: 0`는 flex-basis 로 키운 2항목이 원래는 브라우저 화면을 줄일수록 자신의 크기도 줄여 컨테이너 크기를 반응형으로 맞춰줬던 것을 바꿉니다. `flex-shrink: 0`는 화면이 작아지더라도 자신의 화면을 줄이지 않습니다.
  - `flex-shrink: 1`을 하면 다시 자신의 크기를 줄이기 시작합니다.
  - 만약 1항목을 `flex-shrink: 1`, 2항목을 `flex-shrink: 2`를 준다면 화면을 줄일 때 1항목은 1/3, 2항목은 2/3만큼 공간 분담을 하게 됩니다.
- `flex-shrink`를 쓰려면 우선 flex-basis 값이 주어진 게 있어야 합니다.
3. [holy grail layout](https://opentutorials.org/course/2418/13526)
가장 많이 쓰이는 레이아웃 (헤더, 좌사이드네비, 우광고, 중메인, 푸터)을 만들어봅니다.
```HTML
<div class="container">
  <header>
    <h1>생활코딩</h1>
  </header>
  <section class="content">
    <nav>
      <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li>JavaScript</li>
      </ul>
    </nav>
    <main>
      생활코딩은 일반인을 위한 코딩 수업입니다.
    </main>
    <aside>
      광고
    </aside>
  </section>
  <footer>
    <a href="https://google.com">홈페이지</a>
  </footer>
</div>
```
- `aside`태그는 덜 중요한 것에 쓰이는 태그입니다. 여기에 광고를 넣습니다.
```CSS
.container {
  display: flex;
  flex-direction: column;
}
header {
  border-bottom: 1px solid gray;
  padding-left: 20px;
}
footer {
  border-top: 1px solid gray;
  padding: 20px;
  text-align: center;
}
/*섹션의 내용들을 일렬로 나열합니다.*/
.content {
  display: flex;
}
.content nav {
  border-right: 1px solid gray;
}
.content aside {
  border-left: 1px solid gray;
}
/*화면을 줄일 때 네비와 광고를 고정시키고 중간만 이동시키게 만들어봅니다.*/
nav, aside {
  flex-basis: 150px;
  /*nav, aside 는 이제 줄어들지 않습니다. 중간 메인만 줄어들 것입니다.*/
  flex-shrink: 0;
}
main {
  padding: 10px;
}
```
#### flex 의 속성들 정리
1. 컨테이너
- `flex-direction` : row, row-reverse, column, colum-reverse
- `flex-wrap` : 기본값은 nowrap 입니다. 만약 컨테이너 크기보다 항목들의 크기 합이 클 경우 wrap 을 하면 항목들이 아래로 내려갑니다.
```
12345
---
wrap
123
45
---
wrap-reverse
45
123
```
- `align-items` : 기본값은 stretch 입니다. 항목들 높이가 컨테이너의 높이값만큼 쭉 펴지게 만듭니다.
  - 항목들이 자신들의 콘텐츠만큼만 크기를 지정해주려면 flex-start(top 시작점), flex-end(end 끝점)를 해야합니다. center 는 중간지점에서 지정합니다.
  baseline 은 콘텐츠의 가상의 줄이 있는데 그것대로 일렬로 맞춰줍니다.
```CSS
body {
  display: flex;
  align-items: center;
}
/*
이러면 메인의 글이 적으면 페이지의 한가운데(상하좌우)에서 콘텐츠가 나오게 됩니다.
만약 메인의 글이 길다면 그 길이만큼 콘텐츠는 위로 올라가서 보여줄 것입니다.
 */
```
- `justify-content` : `align-items`가 수직(교차 축)과 관련한 결정이라면, 이것은 수평(주 축)과 관련된 결정을 다룹니다. 주축을 기반으로 하는데, `flex-direction` 이 row 면 column, column 이면 row 를 다루게 되는 것입니다. 값으로는 start, center, space-between, space-around, space-evenly 가 있습니다.
```CSS
body {
  display: flex;
  align-items: center;
  justify-content: center;
}
.container {
  flex-direction: column;
  width: 800px;
  border : 1px solid gray;
}
```
- `align-content` : `align-items`처럼 flex-start, flex-end 를 했을 때 항목들의 배치가 비슷합니다. flex-start, flex-end, center, space-between, space-around, stretch 값이 있습니다.
  - 차이점 : `align-items`는 컨테이너 밑의 항목들 전체를 정렬합니다. `align-content`는 같은 행을 그룹으로 쳐서 그룹과 그룹 사이를 정렬하는 것입니다. (space-between 값으로 보면 확실히 알 수 있습니다.)
2. 항목들
- `align-self` : 컨테이너의 `align-items`로 설정을 한 상태에서 특정 항목만 다르게 위치시킬 때 사용합니다. auto, flex-start, flex-end, center, baseline, stretch 가 있습니다.
- `flex-grow` : 위의 설명 참고. 기본적으로 0입니다만, 1로 주면 각 요소가 여백을 꽉 채우게 됩니다.(컨테이너 공간을 1/n 으로 공평하게 나눠가집니다.)
- `flex-shrink` : 위의 설명 참고. 0은 화면이 작아지더라도 해당 항목을 줄이지 않고, 1부터 줄여가납니다.
- `flex` : 이것은 flex-grow, flex-shrink, flex-basis 를 축약한 것입니다.
`{flex: flex-grow [flex-shrink] [flex-basis];}`로 사용합니다.
- `order` : 항목의 순서를 바꿀 때 사용합니다. HTML 코드는 그대로 작성하고 CSS 의 order 만 바꾸면 순서가 바뀝니다. 양수 말고 음수값도 넣을 수 있습니다.
```CSS
nav { order: -1; }
main { order: 0; }
```
