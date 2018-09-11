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
