# 한 걸음 더 CSS: 코딩가나다(Programmers) 파트 3, 파트 4
[프로그래머스 페이지](https://programmers.co.kr/learn/courses/4521)
## 파트 3: 디스플레이
디스플레이는 '표시하다' 라는 뜻이 있습니다. 그래서 모든 HTML 요소들이 어떻게 표시되는지, 어떤 성질으로 표시되는지를 담당하는 매우 중요한 속성입니다. div,h1, p 는 블록, span 은 인라인 등, 각 요소들은 기본적인 디스플레이 성질을 가지고 있습니다. 블록과 인라인은 가장 대표적인 디스플레이 속성입니다.
- 인라인은 텍스트의 너비, 높이만큼 자동으로 설정이 됩니다. 인라인은 텍스트를 기반으로 하기 때문에 너비, 높이를 직접 지정할 수 없습니다. (width, height 를 직접 지정하면 무시할 것입니다.)
margin 도 블록의 개념과는 다릅니다. 위아래 마진은 불가능하고 좌우만 생기게 됩니다. (줄간격을 주려면 인라인의 line-height 을 다뤄야 합니다.)
padding 은 좌우가 먹히고 상하는 시각적으로는 실행되지만 공간적인 측면에서는 그대로입니다.
border 는 일반적인 블록의 개념와는 다릅니다. 잘려서 나오기 때문입니다.
- 그런데 작업을 하다 보면 인라인 요소를 블록으로 바꿔야 하는 경우가 있습니다. a(인라인 태그)는 디자인적으로 풀기 어려워서 디스플레이를 블록으로 해야 할 수도 있습니다.
### 01
1. p 태그 안에 강조하는 부분을 div 요소로 넣어봤습니다. 그러면 div 가 위로 가고 p 태그의 글은 밑으로 내려가게 됩니다. div 는 블록이기 때문에 텍스트의 일부만 가져가 쓰는 방식으로는 사용할 수 없습니다. 해당 div 는 한 줄 전부를 차지하는 하나의 단락으로 취급됩니다. (<p>test<div class="bold">Hamburger</div>test02</p>)
2. 그러니 인라인의 성질을 가진 태그를 사용해야 합니다.`strong` 태그를 사용합니다. strong 은 인라인의 속성을 가져 텍스트와 동급이 됩니다.
3. `display: none`을 하면 공간을 차지하지 않게 숨깁니다.(실제로는 있지만 표현을 하지 않은 것입니다.)
4. 아미지는 기본적으로 인라인으로 디스플레이가 지정되어 있습니다. 그런데 이미지는 인라인이면서도 너비, 높이를 가지고 있습니다. 그래서 인라인이 아니라 인라인-블록으로 취급해야 합니다. 왜 인라인이냐면 이미지는 받아주는 태그가 없는 홀 태그이기 때문입니다.
```HTML
<body>
  <div class="box">
    <img class="box-cover" src="test.jpg">
    <h1>Hamburger</h1>
    <p>test<strong class="bold">Hamburger</strong>test02</p>
  </div>
</body>
```
```CSS
.box {
  background-color: white;
  width: 960px;
  margin: 600px auto 0;
  text-align: center;
  border-radius: 20px;
  border-right: 5px solid #ccc;
  border-bottom: 5px solid #999;
}
.box-cover {
  margin-top: -500px;
}
strong.bold {
  background-color: orangered;
  color: white;
}
```

## 파트 4. line-height(줄간격)
### 01
`line-height`는 텍스트를 제어하는 디자인에 많이 사용합니다.
```HTML
<div class="contents">
  <div class="wrapper">
    <h1>TEST</h1>
    <p>test</p>
  </div>
</div>
<!-- 2.부터 시작 -->
<div class="parent">
  <div class="first">aaa</div>
  <div class="second">aaa</div>
</div>
```
1. 본문의 p 태그를 `line-height` 속성으로 작업해봅니다. 줄간격이 높아져서 가독성이 좋아집니다. 참고로 `line-height` 에 픽셀은 어울리지 않습니다.(왜냐면 폰트 사이즈를 `line-height`보다 큰 값으로 지정할 경우 자연스럽지 않게 글이 배치되기 때문입니다. 줄간격은 그대로인데 글자 크기만 바뀌어서 그런 것입니다.) 그래서 보통 1.5나 1.8 이렇게 작성합니다.
2. parent 에 `line-height`를 1으로 주게되면 first 와 second 의 폰트 사이즈에 각각 맞춰서 `line-height`를 자동으로 설정합니다.(즉 parent 의 line-height 를 상속합니다.) 그런데 만약 `line-height` 를 1em 으로 설정하면 줄간격이 먹히게 됩니다. 왜냐면 parent 에는 폰트 사이즈가 20인데 em (해당 요소의 폰트 사이즈의 배수. 1em 이면 20px 의 100%인 20px 가 됩니다.)를 지정했는데, parent 의 폰트 사이즈에 맞춰진 line-height 는 first, second 에서도 그대로 이어지기 때문에 그렇습니다. ***따라서 line-height 는 단위를 빼고 작성하는게 좋습니다.***
3. h1 의 디자인을 바꿔봅니다. `margin-top`을 마이너스로 해서 위로 올리려면 우선 h1 의 높이를 알아야 합니다. 그러려면 line-height 와 폰트와의 관계를 알고 있어야만 합니다.
- HTML 의 루트의 기본 폰트 사이즈를 16으로 설정했는데 이 값을 상속해서 최종 폰트 사이즈는 32픽셀입니다. 그거에 영향을 받아서 h1 의 높이는 39가 됐습니다. 높이가 사이즈보다 큰 이유는 사이즈의 위,아래 공간에 리딩 영역이 있어서입니다.
  - 리딩 영역 : 여러 줄일 때 가독성을 높이기 위해 기본적으로 넣어둔 공간입니다. 위 반쪽, 아래 반쪽으로 나눠집니다.
![ganada02_p3_01](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/ganada02_p3_01.jpg)
만약 `line-height`를 지정하지 않을 경우 `line-height`는 normal 이 됩니다. normal 은 폰트의 사이즈에 따라서 조금씩 값이 바뀝니다. 그래서 보통 `line-height`값을 1.4 이렇게 지정하는 편입니다. 값을 키우면 폰트는 그대로이면서 줄간격만 커지게 됩니다.
`line-height`를 1로 하면 폰트와 동일한 높이를 가지게 됩니다.
```CSS
h1 {
  font-size: 2em;
}
.contents p {
  line-height: 1.5;
  font-size: 30px;
}
.parent {
  overflow: hidden;
  background-color: white;
  width: 960px;
  margin: 50px auto;
  font-size: 20px;
  font-family: 'arial';
  line-height: 1;
}
.first {
  font-size: 30px;
}
.second {
  font-size: 40px;
}
```
### 02 폰트의 종류에 따라서 달라지는 글자 사이즈
1. 동일한 사이즈(30px)이지만 각 글자는 높이가 다릅니다. 왜냐면 `line-height`를 기본값 normal 로 설정했기 때문입니다. normal 은 폰트의 종류에 따라 다르게 설정이 됩니다.
2. `line-height`를 바꾸면 폰트 사이즈 30 의 1.5배가 균등하게 적용됩니다.(line-height: 1.5)
line-height: 1 로 하면 각 글자에 따라 다른 사이즈를 가지고 있다는 것을 확실하게 볼 수 있습니다.
```CSS
h2 {
  font-weight: normal;
  margin: 5px;
  position: relative;
  background-color: #ddd;
  font-size: 30px;
  line-height: 1.5;
}
h2.one { font-family: 'Roboto'; }
h2.two { font-family: 'Oswald'; }
h2.three { font-family: 'Raleway'; }
```
### 03
```HTML
<div class="contents">
  <div class="wrapper">
    <h1>TEST</h1>
    <p>test</p>
  </div>
</div>
```
1. 원래 예제로 돌아와서 h1 의 크기를 알기 위해 배경색을 지정합니다. `line-heigth`는 기본값 normal 로 되어 있어 높이를 알기는 어렵습니다. 그래서 값을 고정해봅니다.(line-height: 1) 여기서는 1로 할 경우 빈 공간이 생기는데 폰트의 기본 설정이 이런 것이라서 바꾸기는 어렵습니다.
2. 폰트를 바꿀 경우 `line-height`의 공간도 바뀌게 됩니다.('impact')
3. 위로 올려봅니다. `line-height`를 1로 해서 높이를 정확히 알고 있으니(80px) margin-top 을 마이너스로 할 수 있게 됩니다.(margin-top: -80px)
4. 하지만 margin 을 고정값으로 한다면 나중에 폰트의 사이즈를 바꿀 경우 달라질 수 있으니 값을 1em 으로 하는게 좋습니다.(margin-top: -1em | 사이즈가 80이니 -80px 가 됩니다.)
5. `line-height`를 1로 해서 생긴 빈 공간도 싫다면 0.8로 잡아봅니다. `margin-top` 도 따라서 -0.8em 으로 해야 합니다. (line-height: 0.8, margin-top: -0.8em) em 으로 지정했기 때문에 앞으로 폰트 사이즈를 바꾸더라도 문제없이 알아서 바뀌게 됩니다.
```CSS
.contents h1 {
  font-size: 80px;
  color: #efefef;
  line-height: 1;
  font-family: 'impact';
  margin-top: -1em;
}
```
