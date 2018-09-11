# 한 걸음 더 CSS: 코딩가나다(Programmers) 파트 1, 파트 2
[프로그래머스 페이지](https://programmers.co.kr/learn/courses/4521)
## 파트 1. 가운데 정렬
웹 사이트를 만들 때 가장 우선시해야할 것은 레이아웃입니다. 그 레이아웃 중에서 가장 보편적인 것이 가운데 정렬입니다.
```HTML
<body>
  <div class="box">
    <img class="box-cover" src="test.jpg">
    <h1>Hamburger</h1>
    <p>test</p>
  </div>
</body>
```
### 기본
1. 우선 천천히 접근하는 것이 중요합니다. body 와 box 가 이등분되어 있으니 우선 구분지어 봅니다.(background-color) 블록 레벨 요소이니 body 가 꽉 채워져 있을 것입니다.
2. box 의 width 를 줄여 봅니다.(width:960px) 오른쪽의 빈 공간은 box 라는 div 블록레벨 요소의 것입니다. 그래서 새로운 div 를 배치한다 해도 옆이 아니라 밑에서 배치될 것입니다.
3. 오른쪽의 빈 공간에 margin 을 줘서 공간을 나눠서 가지도록 만들어봅니다.  `margin-left: auto;` 값을 주게 되면 오른쪽의 빈 공간을 margin 값으로 자동 완성시켜줍니다. 그러면 이 box 는 오른쪽 끝으로 이동할 것입니다.
4. 그래서 같은 방식으로 `margin-right: auto;` 를 하면 나머지 공간을 왼쪽 절반, 오른쪽 절반으로 구분해서 가운데 정렬을 만들게 됩니다. 이 방법은 가장 보편적으로 가운데 정렬을 해주는 방법입니다.
```CSS
body {
  background-color: #ddd;
}
.box {
  background-color: white;
  width: 960px;
  margin-left: auto;
  margin-right: auto;
}
```
### 내용물 가운데 정렬
1. 내용물은 inline 요소 입니다. box 에게 `text-align: center;` 를 줍니다. 그러면 box 는 블록 요소이기 때문에 center 값을 주면 박스 자체에는 영향이 없지만 그 안의 모든 인라인 요소는 center 값의 영향을 받게 됩니다.
- `text-align: center;` 는 상속이 됩니다. 부모 요소에 적용된 CSS 가 자식 요소에게도 적용이 됩니다.
2. 이번에는 margin 속성을 바꿔 봅니다. 상하단 50px, 좌우는 auto 를 적용합니다.
3. 그림자를 적용해 봅니다. border-radius, border-right, border-bottom 을 작성합니다.
4. box 안의 인라인 요소를 바꿔봅니다.
5. 참고로 h1 자체는 `text-align: center;`이 된게 아니라 h1 안의 인라인이 가운데 정렬된 것입니다. h1 에 background-color 로 살펴보면 h1 자체는 가운데 정렬이 안된 채 box 를 가득 채우고 있음을 알 수 있습니다.
`text-align: center;` 는 인라인 요소만 제어할 수 있고, ***h1 같은 블록 요소는 제어하지 못합니다.*** 다만 h1 이라는 요소의 `text-align: center;` 속성이 부모의 속성을 상속받아서 이 h1 안의 텍스트만 반영된 것입니다.
6. 인라인의 텍스트만 배경을 지정하려면 h1 의 width 를 줄여야 합니다. 그러면 블록 요소 h1 의 사이즈가 줄지만 왼쪽 끝으로 이동합니다. 블록 요소의 줄어든 영역을 기준으로 `text-align: center;` 가 적용이 됩니다.
7. 여기서 가운데 정렬을 하려면 margin 을 잡아야 합니다.(margin: 40px auto;)
8. 이렇게 width 를 잡는 방식은 단점이 있습니다. h1 의 폰트 사이트를 늘리면 글씨가 영역 밖으로 빠져나오게 됩니다. 단순히 크기를 늘려서 해결할 것이 아니라 보다 근본적으로 간편하게 해결하려면 보다 고난이도의 CSS 작업이 필요합니다.
```CSS
.box {
  background-color: white;
  width: 960px;
  margin: 50px auto;
  text-align: center;
  border-radius: 20px;
  border-right: 5px solid #ccc;
  border-bottom: 5px solid #999;
}
.box h1 {
  color: orangered;
  width: 250px;
  margin: 40px auto;
}
.box p {
  padding: 3rem;
}
```

## 파트 2. 네거티브 마진
네거티브 마진은 마이너스 값으로 마진을 설정합니다. 만약 이미지 위치가 기본적으로 제공하는 그리드로는 답답하다, 살짝 튀게 하고 싶을 때 사용합니다.
네거티브 마진은 오래전부터 웹 표준에 있던 정식 개념이라 호환성 문제는 없습니다. 그러니 안정적으로 사용 수 있습니다.
### 01
1. margin-top 을 -500px 으로 하면 위로 튀어나옵니다. 이미지가 잘리기 때문에 상위 컨테이너 box 의 마진을 바꿔줍니다. `margin: 600px auto 0;`(상단 | 좌우 | 하단)
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
```
### 02
```HTML
<div class="header">
  <div class="box-wrapper">
    <div class="box">A</div>
    <div class="box select">B</div>
    <div class="box">C</div>
  </div>
</div>
<div class="contents">
  <h1>NewYork City</h1>
  <p>test</p>
  <div class="map">Map</div>
  <p>test2</p>
</div>
```
1. 두 번째 box 만 색상을 바꿔봅니다. 그리고 네거티브 마진을 줘봅니다.`margin-left: -50px;` B 를 네거티브 마진을 적용해 원래 위치보다 조금 더 빠르게 시작하도록 브라우저를 속인 것입니다.(만약 플러스라면 시작점이 느려집니다.)
***margin 은 언제나 시작점을 속이거나 종점을 속이던지 하는 속성입니다.***
2. 다음에는 `margin-right: -50px` 를 줘봅니다. B 는 움직이지 않았는데 원래 B 가 끝나는 지점보다 더 빠르게 끝나도록 브라우저를 속입니다. 그래서 C 가 더 빠르게 시작해서 세 박스가 겹쳐있게 됩니다.
3. header 를 더 빨리 끝나게 만들어봅니다. `margin-bottom: -100px`(종료 시점이 더 빠르게 끝납니다.) header 말고 contents 의 `margin-top: -100px;` 으로 해도 같은 결과가 나옵니다.(시작점이 더 빨라집니다.)
4. p 태그의 사이에 있는 map 을 디자인합니다. 만들 경우 좌우에 여백이 생기는데 그 여백을 네거티브 마진으로 없앨 수 있습니다.`margin-left: -50px;` 시작점을 속여서 좌측 여백을 없앴지만 우측은 그대로입니다. (전체가 늘어나지 않고 좌측으로만 따라갔습니다. 왜냐면 div 는 블록 요소라서 width 값이 기본으로 auto 로 돼있습니다. 부모의 값을 따라가기 때문에 시작점이 빨라지더라도 종점은 부모측이 끝나는 지점과 같아지는 것입니다.) 이번에는 우측에도 네거티브 마진을 줍니다.`margin: 40px -50px;`(상하|좌우)
5. 네거티브 마진을 응용해봅니다. 마우스를 가져다 대면 호버로 네거티브 마진을 적용해 띄울 수 있습니다.
```CSS
.header {
  margin-bottom: -100px;
}
.box {
  width: 100px;
  height: 100px;
  background-color: yellow;
  margin: 5px;
  float: left;
  text-align: center;
  line-height: 100px;
  font-family: 'arial';
  font-size: 50px;
}
.box.select {
  background-color: orangered;
  margin-left: -50px;
  margin-right: -50px;
}
.map {
  background-color: orange;
  padding: 50px;
  text-align: center;
  border: 10px solid orangered;
  margin: 40px -50px;
}
.map:hover {
  margin: 40px -100px;
  box-shadow: 0 0 30px rgba(0,0,0,0,2);
}
/*5 에서 새로 적용한 .map*/
.map {
  background-color: white;
  padding: 50px;
  text-align: center;
  margin: 40px 0;
  transition: all 1s;
}
```
