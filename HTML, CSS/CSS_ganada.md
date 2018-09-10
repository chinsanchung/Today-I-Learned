# 한 걸음 더 CSS: 코딩가나다(Programmers)
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
