# 한 걸음 더 CSS: 코딩가나다(Programmers) 파트 7, 파트 8
[프로그래머스 페이지](https://programmers.co.kr/learn/courses/4521)
## 파트 7. 상속(Inheritance)
### 01
```HTML
<div class="box">
  <img class="box-burger" src="test.png">
  <h1>Hamburger</h1>
  <button>Submit</button>
  <p><a href="">For</a> test</p>
</div>
```
1. 우선 박스 안의 모든 글자를 오렌지색으로 바꿔봅니다.(h1, p {color : orangered}) 이 방법은 한 가지 문제가 있습니다. 새로운 태그를 넣을 시 그것이 적용이 안된다는 것입니다. 일일이 추가하는 것보다 더 유용한 기법이 바로 상속을 이용하는 것입니다.
2. 상속을 이용해서 부모 요소에 글자 색을 지정합니다. 상속 덕분에 부모, 자식 모든 요소가 같은 글자 색을 가지게 됩니다.(.box{color: orangered;})
이번에는 `border: 10px solid orangered;` 도 넣어봅니다. 보더는 상속이 안되게끔 되어있어서 h1 을 살펴보면 반투명으로 되어 있을 것입니다.
- 이렇듯 ***어떤 요소는 상속이 되고 어떤 요소는 상속이 불가능합니다.*** 그러니 검색해서 미리 파악해두면 상속을 사용할 때 좋을 것입니다.
3. 상속이 안되는 상황을 가정해봅니다. 박스 안의 요소를 직접적으로 선택해서 바꾸면 상속이 불가능해집니다.`.box h1 { color: blue; }` 부모로부터 상속을 받았는데 이미 지정한 CSS 가 있다고 하면 지정해둔 것으로 반영되는 것입니다.
`<a href="">` 태그도 글자 색의 상속이 불가능합니다. 브라우저가 임의로 지정한 색이 있어서 그걸 반영하기 때문입니다.
4. 브라우저가 지정한 a 태그의 글자 색을 바꿔봅시다. (예제라서 선택자는 조금 억지스러울 수 있습니다.)`a {color: orange}`
- 이 방식도 문제점이 있습니다. 모든 페이지의 a 태그가 오렌지색이 되는 것입니다. 그래서 상속을 사용하면 부모에게 상속받은 글자 색으로 지정할 수 있게 됩니다. `a { color: inherit }` 참고로 a 태그의 밑줄도 없앨 수 있습니다. `text-decoration: none`
5. 버튼을 만들어 처음 확인해보면 font: 400(두께) 11px(사이즈) system-ui(font-family) 라고 고정되어 있습니다.(애플 크롬 기준) 이 부분을 상속을 사용해서 초기 설정을 바꿔봅니다. 그러면 부모인 박스의 디자인을 버튼에 적용시킬 수 있게 됩니다.
- 주의점이 있습니다. ***익스플로러 6, 7에서는 inherit 를 사용할 수 없습니다.***
6. 이번에는 상속이 잘 된 사례를 들어봅니다. `h1:before`를 사용하면 실제 HTML 에는 없지만 CSS 에서 가상 콘텐츠 `content: 'BEFORE!'`를 만들어 넣을 수 있습니다. 이 BEFORE 는 h1 의 자식으로 들어가 있음을 개발자 도구에서 확인할 수 있습니다.
이번에는 동그라미를 만들어봅니다. content 를 비우고 display 를 인라인 블록으로 하면 너비, 높이를 지정할 수 있습니다. 그리고 border-radius 까지 활용하면 h1 태그 앞에 동그라미가 생기게 됩니다.(`margin-right 는 상대값으로 하는게 좋습니다.`)
- 지정한 동그라미 색을 옆의 h1 글자 색으로 바꾸고 싶다면 `background-color: currentColor`로 하면 둘이 같은 색이 됩니다.(currentColor 는 상속은 아니지만 부모의 색을 적용하는 속성입니다.) `currentColor`는 색을 지정할 수 있는 모든 요소에 적용이 가능합니다. (보더 색, 박스 섀도우 색, 백그라운드 색)
- 색상 말고 h1 의 폰트 사이즈를 동그라미에 상속해봅니다. `width: 0.8em; height: 0.8em` 으로 하면 h1 폰트의 80% 크기로 처리가 가능합니다. 이렇게 em 을 이용해서 묶어놓으면 유지 보수가 한결 편리해집니다.
```CSS
a {
  text-decoration: none;
  color: inherit;
}
button {
  font: inherit;
}
.box p {
  padding: 3rem;
}
.box-burger {
  margin-top: -150px;
}
.box {
  color: orangered;
  border: 10px solid orangered;
}
.box h1:before {
  content: ' ';
  display: inline-block;
  width: 20px;
  height: 20px;
  background-color: currentColor;
  border-radius: 50%;
  margin-right: 0.5em;
}
```
### 02. em
```HTML
<div class="parent">
  <div class="first">aaa</div>
  <div class="second">bbb</div>
</div>
```
1. 현재 만약 부모에 폰트 사이즈를 지정한다 해도 first 와 second 각각 폰트 사이즈를 지정해서 상속이 안되는 상황입니다.
2. parent 에서 `line-height: 2`를 설정했습니다. 폰트 사이즈의 2배로 한 것인데 상속으로 first 와 second 에도 들어갔습니다. 이미 지정해둔 사이즈가 있으니 거기에 맞춰서 줄 간격을 2배로 만들어줍니다.
- 그런데 줄간격을 단순한 숫자가 아니라 `1em`으로 했다면 결과는 달라집니다. em 단위는 계산이 된 단위입니다. 여기서는 parent 의 폰트 사이즈 20px 을 기준으로 한 20px 가 line-height 가 되고 `line-height: 20px`를 상속하게 됩니다.
***단위를 어떻게 하느냐에 따라 상속의 결과가 달라진다는 점을 유의하셔야 합니다.***
```CSS
.parent {
  overflow: hidden;
  background-color: white;
  width: 960px;
  margin: 50px auto;
  font-family: 'arial';
  font-size: 20px;
  line-height: 2;
}
.first {
  font-size: 30px;
}
.second {
  font-size: 40px;
}
```
## 파트 8. 오버플로우(overflow)
### 01
```HTML
<div class="box">
  <img class="left" src="test.png">
</div>
```
1. 박스의 배경색을 지정했습니다. 여기서 색이 박스의 높이만큼 지정된 것을 확인할 수 있습니다. 박스의 높이는 직접 설정하지 않았어도 자식 클래스인 left 의 높이만큼 자동으로 설정해줍니다.(height: auto)
2. 만약 박스의 높이를 자식보다 작게 설정한다 해도 자식 이미지는 쪼그라들지 않고 그대로 유지합니다. 이 상황은 자식 요소의 영역이 부모의 영역을 벗어나버린 것입니다. 이것을 제어하기 위해서는 오버플로우를 사용해야 합니다.
3. 오버플로우를 히든으로 설정하면 박스가 지정한 높이만큼만 보여주고 (자식 요소가 부모의 높이보다 클 경우) 나머지를 잘라버립니다.
오버플로우를 스크롤로 한다면 히든과 마찬가지로 잘라서 보여주지만 원한다면 사용자가 스크롤로 밑을 볼 수 있게 됩니다.
- 오버플로우 속성은 float 속성과 같이 사용할 때 난이도가 올라갑니다.
```CSS
.box {
  background-color: white;
}
```
### 02 오버플로우와 플로트
```HTML
<div class="box">
  <img class="left" src="test.png">
  <img class="right" src="test2.png">
  <!-- <p>test</p> -->
</div>
```
1. 현재의 기본 상황은 이미지 밑에 글자가 배치됐는데 이미지의 우측이 비어있어 비효율적입니다. 이것을 해결하려면 `float` 를 써야 합니다. 이미지인 left 에 `float:left`를 주면 나머지 우측 부분을 p 태그로 채워 효율적으로 배치할 수 있게 됩니다.
살펴보자면 이미지에 `float:left`를 주면 밑의 p 태그가 영향을 받아 시작점이 바뀌었습니다. 그런데 사실 p 태그의 시작점은 이미지의 좌상단, 즉 처음 부분이 실제 시작점입니다.(border 로 확인이 가능합니다.)
- float 의 목적은 이미지와 텍스트를 조화롭게 보여주는 것입니다. 그래서 이미지를 위로 띄워 텍스트를 덮어버리는 일을 허용하지 않습니다. 만약 문장이 길다면 이미지가 끝나더라도 왼쪽에서 이어갈 수 있도록 p 태그의 시작점을 좌상단으로 한 것입니다. 문장은 `float:left` 로 이동한 이미지로 인해 오른쪽으로 밀려 있는 것입니다.
2. float 의 문제는 p 태그의 글자가 충분치 않을 때입니다. 그러면 이미지는 box 를 빠져나온 형태가 되어버립니다. ***부모는 float 처리한 자식의 높이를 고려하지 않습니다. 혹은 인지하지 못합니다.*** 아직 이것을 수정해야 할 필요성을 못 느낀다면 이번에는 문장을 지우고 이미지 두개로 하면 알 수 있게 됩니다.
3. 두 이미지 left, right 가 있습니다. right 를 오른쪽 끝으로 이동하려 합니다. 좌는 `float:left`, 우는 `float:right`로 하면 눈으로 볼 때는 성공했습니다. 그런데 문제가 있습니다. 부모 box 의 배경색이 사라졌습니다. `박스의 높이가 0이 되버린 것입니다.`
- 다시 말하자면 부모 요소는 float 처리한 자식 요소의 높이를 고려하지 않습니다. 그래서 부모인 박스의 높이를 지정하질 않았고 자식 요소의 높이도 0이라서 비정상적인 레이아웃이 만들어진 것입니다.
float 의 원래 용도(이미지와 텍스트의 조화로운 배치)를 고려하지 않고 단순하게 왼쪽, 오른쪽 이동시키는 데로만 사용하면 이런 부작용이 생기는 것입니다.
4. 이 부작용을 어떻게 해결해야 할까요. 여러 방법이 있지만 부모 박스에 `overflow:hidden` 으로도 해결이 가능합니다.
- 오버플로우 속성은 float 와 연계하기 위해서만 있는 것은 아닙니다. 오버플로우의 원래 용도는 아니지만 float 문제 해결을 위해 적당히 사용하고 있는 것입니다.
- 아까 부모 box 는 float 한 자식의 높이를 인지하지 못한다고 했습니다. 하지만 ***최상위 부모인 body 는 float 한 자식의 높이를 인지할 수 있습니다.*** float 로 위로 뜨더라도 body 의 바깥으로 빠져나가지는 않습니다. body 는 최상위 요소이기 때문에 모든 자식 요소들(float 된 요소라고 해도)을 표현해야만 하는 책임이 있습니다.
5. 따라서 박스를 작은 body 가 되게끔 만들면 문제를 해결할 수 있습니다. body 에서 float 를 표현하는 부분만 가져와서 적용하는 것입니다.
- 예를 들어 `overflow:scroll`을 주면 박스의 배경색이 돌아오고 박스는 이제 작은 body 가 됩니다. 박스는 높이도 지정할 수 있고 만약 높이가 그림보다 작다면 스크롤 기능으로 모든 이미지를 볼 수 있도록 해줍니다. 박스에 오버플로우를 주면 작은 body 가 된다고 생각하면 됩니다. 작은 body 이기 때문에 float 인 이미지의 높이를 인식해 어디서 끝나는지를 알아 정상적인 박스로 바꾸는데 도움을 줍니다.
`overflow:hidden`을 줘도 작은 body 를 만드는 것은 같습니다.
- 다만 실제로 박스가 body 가 된 것은 아닙니다. body 는 유일합니다. 박스는 오버플로우를 통해 body 의 특징 하나(블록 포맷팅 컨텍스트)를 가져온 것입니다. 그래서 박스는 독립적인 영역을 가지게 됩니다.(별도의 페이지가 생긴 것과 같습니다. 따라서 박스와 나머지 밑부분은 서로 영향을 받지 않고 섞이지 않게 됩니다.)
```CSS
.box {
  background-color: white;
  overflow: hidden;
}
.left {
  float: left;
}
.right {
  float: right;
}
```
#### 이미지와 p 태그
```HTML
<div class="box">
  <img src="test.png" class="left">
  <p>test</p>
</div>
```
1. 현재 문장은 `float:left`한 이미지로 인해 오른쪽으로 밀렸을 뿐 시작점은 왼쪽이라고 앞에서 했었습니다. 문장의 시작점을 밀린 오른쪽으로 바꾸려면 어떻게 해야 할까요. `margin-left:100px`(이미지의 크기를 알아서 100픽셀로 잡을 수 있었습니다.)으로 주면 문장이 밀린 부분에서부터 시작하게 됩니다.
2. 이번에는 문장 안에 span 을 넣고 그것에 클리어 `clear:both`를 집어 넣습니다.(인라인인 span 에 클리어를 적용하려면 `display:block`을 추가해야 합니다.)
- 클리어는 상단에 플로트가 있으면 그 플로트가 끝나는 지점을 바리케이트처럼 막아줍니다.(이미지가 끝나는 지점에서부터 span 을 시작합니다.) 현재 이미지와 p 태그는 body 안에서 서로 영향을 주고받는 상황입니다. 만약 p 태그에 `overflow:hidden`을 준다면 이미지와 p 태그는 서로 별개의 영역이 되어 문장은 원래 위치로 올라가게 됩니다. 그리고 별개의 영역이기 때문에 아까 `margin-left:100px`한 것도 더이상 필요가 없게 됩니다.
그래서 고정 레이아웃과 가변 레이아웃을 설정할 때 오버플로우를 굉장히 많이 사용합니다.

```CSS
.box {
  background-color: white;
  overflow: hidden;
}
.box p {
  border: 10px solid blue;
}
.box p span {
  background-color: cyan;
  clear: both;
  display: block;
}
.left {
  float: left;
}
```
### 03
```HTML
<div class="wrapper">
  <div class="box"></div>
  <div class="box"></div>
</div>
```
1. 현재 상자는 가운데와 최상단, 최하단에 마진 병합이 생긴 상황입니다. 최상단, 최하단은 박스의 마진이 wrapper 의 마진처럼 되버린 것입니다. 이것을 해결하려면 wrapper 에 `overflow:hidden`을 줘서 새로운 body 처럼 만들면 됩니다.
```CSS
body {
  background-color: #ddd;
}
.wrapper {
  background-color: #222;
  width: 140px;
  overflow: hidden;
}
.box {
  width: 100px;
  height: 100px;
  background-color: white;
  margin: 20px;
}
```

## 정리
오버플로우는 새로운 문서를 시작한다는 일이라 생각할 수 있습니다.
기본값인 visible 이 아닌 다른 오버플로우 속성이 부여된 요소는 독립적인 문서가 됩니다. (Block formatting context)
단점은 오버플로우의 본래 목적에 맞게 사용하지 않으면 짤리는 경우가 생길 수 있다는 것입니다.
CSS3 의 flex bot 나 grid 를 사용하면 굳이 플로트, 오버플로우를 섞을 필요는 없습니다만 익스플로러 지원을 위해서 아직까지는 그 둘이 필요합니다.
