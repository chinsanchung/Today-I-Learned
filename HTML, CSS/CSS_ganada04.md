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