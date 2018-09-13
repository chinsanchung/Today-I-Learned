# 한 걸음 더 CSS: 코딩가나다(Programmers) 파트 5, 파트 6
[프로그래머스 페이지](https://programmers.co.kr/learn/courses/4521)
## 파트 5. margin collapsing(마진 병합)
마진 병합은 마진 겹침, 마진 붕괴, 마진 상쇄 등 다양한 이름으로 불립니다. 이 현상이 왜 생기는지 그리고 어떻게 해결하는지를 알아봅니다.
### 01
```HTML
<div class="contents">
  <div class="section">Section #1</div>
  <div class="section">Section #2</div>
</div>
```
1. 우선 section 을 디자인합니다. 여기서 `margin: 20px`를 해서 상하좌우 20px 가 적용된 상태인데 1과 2 사이에는 그러면 40px 만큼 비어있어야 정상입니다. 그런데 살펴보면 1의 margin-bottom: 20px 와 2의 margin-top: 20px 는 병합되어 있습니다. 이것이 마진 병합 현상입니다.
- 마진 병합은 의도된 현상입니다. CSS 는 보기 편하게 하는게 최우선이기에 자동적으로 병합을 했습니다. 만약 병합이 없었다면 간격이 넓어서 가독성이 떨어질 것이고 개발자가 수동적으로 작업을 해야 했을 것입니다. 하지만 CSS 에서의 상하단 마진은 겹치도록 의도적으로 설계를 했기에 편하게 작업을 할 수 있었던 것입니다. 즉 마진 병합은 고쳐야 할 단점이 아닌 개발자에게 장점을 주는 것입니다.
- 마진 병합의 조건
  - 인접한 블록 요소끼리만 마진 병합이 일어납니다.
  - 상하단 마진끼리만 해당됩니다.
```CSS
.section {
  background-color: white;
  border: 1px solid #999;
  margin: 20px;
  padding: 20px;
}
```
### 02 마진 병합의 단점
마진 병합이 항상 이로운 점만 제공하는 것은 아닙니다.
```HTML
<div class="parents">
  <div class="children">A</div>
  <div class="children">B</div>
</div>
```
1. margin: 50px; 을 줘서 병합 현상을 일으켰습니다. 그런데 A 의 상단과 B 의 하단에서 마진이 적용이 안됐습니다. parent 요소와 children 요소 간의 마진 병합이 원인입니다. parents 의 margin 상하단 0이 children 의 마진 50px 과 겹친 것입니다.
2. 마진 병합을 일으키는 조건을 바꿔봅니다. parents 와 children 사이의 공간을 차지하는 무언가를 넣으면 병합 현상이 사라집니다.(padding: 1px; 혹은 border: 1px solid transparent;) 더 이상 블록 요소가 인접하질 않기 때문입니다. 이 방법은 자신이 의도한 디자인과 멀어질 수도 있는 단점이 있습니다.
3. 그렇다면 어떻게 바꿔야 할까요. 병합의 조건이 인접한 블록 요소끼리이므로 children 의 display 를 inline-block 으로 바꾸면 병합 현상이 사라집니다만 A 와 B 사이가 50 + 50 인 100px 이 되버렸습니다.
4. parents 에 overflow: hidden 을 주면 간단히 해결됩니다. overflow 는 파트 8에서 다루기로 합니다.
```CSS
.parents {
  background-color: white;
  width: 300px;
  margin: 0 auto;
  /*
  padding: 1px;
  border: 1px solid transparent;
  */
}
.children {
  width: 200px;
  height: 200px;
  background-color: orangered;
  color: rgba(255,255,255,0.2);
  font-size: 200px;
  margin: 50px;
  /* display: inline-block; */
}
```

## 파트 6. vertical-align(세로 정렬)
### 01 기본
초록색 박스를 세로 가운데 정렬할 때 세로 정렬을 사용합니다.
```HTML
<div class="wrapper">
  <div class="box"></div>
</div>
```
1. `vertical-align: middle`을 해놨지만 세로 정렬이 안됐습니다. 세로 정렬은 블록 요소와 전혀 상관이 없고 인라인 또는 인라인 블록에서만 세로 정렬을 해주는 속성입니다.
2. 그래서 박스의 디스플레이를 인라인 블록으로 바꿔야 합니다. 하지만 이것만으로는 부족합니다. 세로 정렬은 박스의 높이만큼만의 범위에서 세로 정렬을 하기 때문입니다. 현재 인라인 요소의 영역은 딱 박스의 높이(텍스트로 치면 한 줄)만큼입니다.
3. 현재 박스 한 줄의 높이를 부모인 wrapper 의 높이(400px)만큼 맞추면 될 것 같습니다. 그래서 wrapper 에 `line-height: 400px`을 주면 한 줄의 높이가 400이 되므로 박스 한 줄의 높이도 400이 됩니다. 따라서 400의 세로 정렬이 적용됩니다.
```CSS
.wrapper {
  background-color: blanchedalmond;
  width: 100px;
  height: 400px;
  margin: 0 auto;
  line-height: 400px;
}
.box {
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: green;
  vertical-align: middle;
}
```
### 02
세로 정렬이 실제로 어떻게 쓰이는지를 보여줍니다.
```HTML
<div class="wow">
  <span class="text">Test Test</span>
  <span class="badge">New</span>
</div>
```
1. 현재 text 와 badge 의 인라인은 같은 상황입니다. 일단 배지는 작게 만들어줍니다. 고정 크기는 나중에 변경하기 번거롭습니다. 상대 단위인 em 을 많이 사용합니다.(0.3 은 부모 사이즈의 30%)
2. 세로 정렬을 지정하질 않아서 현재는 기본값 baseline 으로 돼 있습니다. 배지의 세로 정렬을 위로 해봅니다.(vertical-align: top)
3. 배지의 글 상하단 간격을 패딩을 이용해 조절해봅니다. 패딩 역시 고정 크기로 하지 않는게 좋습니다.(상단 0.2/좌우 0.8/하단 0)
4. 3번대로 할 경우 위로 빠져나옵니다. 인라인의 특징 때문인데 인라인은 width, height 조절이 안되고 또 패딩의 상하단도 조절이 안됩니다. 특히 패딩은 바뀐 것처럼 보일 뿐 실제로는 공간을 차지하지 않습니다. 그래서 정렬을 할 때 파악과 예측이 어려워집니다. 그래서 패딩을 조절할 때는 디스플레이를 인라인이 아니라 인라인 블록으로 하는게 좋습니다. 인라인 블록으로 하면 패딩의 상하단 공간 사용이 가능해져 직관적으로 레이아웃을 짤 수 있게 됩니다.
5. *세로 정렬의 middle 은 공간의 중간이 아니라 텍스트의 중간입니다.* 텍스트의 마지막 t 의 중간지점으로 배지의 세로 정렬이 지정됩니다. 이게 중요한 이유는 폰트가 달라지면 세로 정렬의 중간지점도 달라지기 때문에 주의해야 합니다. 폰트가 바뀌면 패딩의 상하단도 달라지고 세로 정렬의 middle 도 달라집니다. 그래서 폰트를 처음부터 고정하지 않으면 불안한 디자인이 될 수도 있습니다.
- 참고로 한국은 영어가 아니라 한국어를 사용합니다. 그래서 한글이 끝에 있다고 해도 영어 소문자를 기준으로 세로 정렬을 하게 됩니다. 배지의 정렬을 다시 조절해야 하는 것입니다. ***그러니 가장 유동성이 적은 상단을 기준으로 정렬하는게 좋습니다.***
- 정리하자면 세로 정렬의 baseline 과 middle 은 굉장히 유동적이고 top 은 그나마 덜 유동적이므로 top 을 기준으로 정렬하는게 좋습니다. 그리고 폰트의 종류나 브라우저, 운영체제에 따라서도 타이포그래피가 달라질 수 있다는 걸 명심해야 합니다.
타이포그래피는 영어를 기준으로 하기에 한글은 아직 없습니다. 그러니 최대한 맞춰가는게 중요할 것입니다.
```CSS
.text {
  background-color: orange;
}
.badge {
  background-color: orangered;
  color: white;
  font-size: 0.3em;
  vertical-align: top
  padding: 0.2em 0.8em 0;
  display: inline-block;
}
```
