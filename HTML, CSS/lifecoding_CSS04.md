# 생활코딩 CSS 수업 04
### 미디어 쿼리
미디어 쿼리는 화면의 종류와 크기에 따라 다른 디자인을 적용시킬 수 있습니다. [Today I learned 의 링크](https://github.com/chinsanchung/Today-I-Learned/blob/master/Front-End_Nanodegree/ch01_WebFoundations.md#media-query)
### float
글의 본문에 이미지를 삽입할 때 주로 사용하고, 그리고 레이아웃을 잡을 때도 사용합니다.(flex 의 등장으로 앞으로 비중이 줄어들지도 모르지만 현재까지 많이 사용해왔으므로 배워야 할 필요는 있습니다.)
```HTML
<style>
  img {
    float: left;
    margin: 20px;
  }
  p {
    border: 1px solid gray;
  }
</style>
<body>
  <img src="aaa" alt="">
  <p>aaa</p>
  <p style="clear:both;">aaa</p>
</body>
```
- `clear` : both, left, right 값이 있습니다. 이것은 float 한 요소를 감싸지 않도록 줄바꿈해서 보여주도록 만듭니다.
  - float 한 이미지가 right 일때 clear:right; 로 해야 clear 속성이 작동합니다. left 일 경우에도 마찬가지입니다.
  - both 는 좌우 상관없이 무조건 float 이미지를 무시할 경우 사용합니다.
#### float 로 holy grail layout
float 로 할 경우 화면 크기에 상관없이 고정된 레이아웃을 가지는 한계가 있습니다.(자바스크립트로 여러 복잡한 설정을 거치면 flex 처럼 반응형으로 만들 수 있기는 합니다.) 생활코딩 강좌의 [링크](https://opentutorials.org/course/2418/13527)를 누르면 코드를 볼 수 있습니다.

### 다단(multi column)
다단은 신문처럼 화면의 열을 나눠서 읽기 편하게 만들어주는 기법입니다. 화면의 크기를 키우면 알아서 반응형으로 조절해주는 기능도 있습니다.
```CSS
.column {
  text-align: justify;
  column-count: 4;
/* column-width: 200px */
  column-gap:30px;
  column-rule-style: solid;
  column-rule-width: 5px;
  column-rule-color: red;
}
h1 {
  column-span: all;
}
```
  - `column-count` 을 얼마를 주냐에 따라 화면의 열을 나눕니다. 여기서 `text-align: justify;`으로 열을 나눴을 때 생기는 들쑥날쑥한 글들을 정렬합니다.
  - `column-width` 는 열의 폭을 지정하면 그 지정한 값에 맞춰서 브라우저의 너비에 따라 열의 수를 늘리고 줄입니다. 먼저 `column-count`를 지정했다면 그 숫자 이상의 열을 만들지 않게 해줍니다. 폭을 중시하려면 `column-width`만, 숫자만 중요하다면 `column-count`만 씁니다. 둘 다 써도 괜찮습니다.
  - `column-gap`은 열과 열 사이의 간격을 조절합니다.
  - `column-rule-style`로 열과 열 사이에 선을 만듭니다. (solid, dotted, dashed 등등) `column-rule-width`는 해당 픽셀만큼의 선을 만듭니다. 그리고 `column-rule-color`로 선의 색을 지정합니다.
  - 열 안에 제목을 열에 구속받지 않고 자유롭게 위치를 설정하려면 `column-span:all`을 사용합니다.
다단은 신문뿐만 아니라 핀터레스트같은 이미지 리스트를 구현하는데에도 유용하게 사용합니다. [핀터레스트 스타일 레이아웃 만들기](https://opentutorials.org/module/2398/13712)
