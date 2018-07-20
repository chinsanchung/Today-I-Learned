# Grid 로 범위 설정하기
[MDN 그리드 레이아웃](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Grid_Layout)
- CSS 에서는 grid 를 지원해 각 엘레먼트에 범위를 지정해 줄 수 있습니다.
- 테이블과 마찬가지로 세로 열과 가로 행으로 정렬이 가능합니다.
## 기본
### 개념
[MDN 그리드 기본개념](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Grid_Layout/%EA%B7%B8%EB%A6%AC%EB%93%9C_%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EC%9D%98_%EA%B8%B0%EB%B3%B8_%EA%B0%9C%EB%85%90)
- 그리드 : 그리드는 수평선과 수직선이 교차해서 이루어진 집합체입니다 - 하나의 집합체는 세로 열을 그리고 다른 하나는 가로 행을 정의합니다. 각 요소는 이러한 열과 행으로 된 라인을 따라 생성된 그리드에 배치할 수 있습니다.
- 그리드의 기능
  + 픽셀 단위로 트랙 크기를 고정시킨 그리드를 만들 수 있습니다.
  + 항목을 지정해 그리드의 정확한 위치에 배치할 수 있습니다.
  + 콘텐츠를 담기 위해 추가 트랙을 만들 수 있습니다.
  + 정렬을 제어합니다.
  + 겹치는 콘텐츠의 우선순위를 z-index 프로퍼티로 제어할 수 있습니다.
- 그리드 컨테이너 : `display: grid;` 또는 `display: inline-grid;` 로 선언해서 만듭니다. 이 밑의 자식 엘레먼트는 모두 그리드 항목이 됩니다.
- 그리드 트랙 : 그리드의 행, 열을 `grid-template-columns`, `grid-template-rows` 프로퍼티로 정의합니다. (template 은 명시적으로 크기를 지정해서 만들어야 합니다.)
- `fr` 단위 : 트랙은 모든 종류의 길이 단위를 써서 정의할 수 있습니다. 또한, 그리드에는 유연한 크기의 그리드 트랙을 생성하는 데 사용할 수 있는 단위를 추가로 소개하고 있습니다. 새로 소개된 fr 단위는 그리드 컨테이너에 남아 있는 사용 가능한 공간의 일정 비율을 나타냅니다. 다음에 정의된 그리드에서는 남아 있는 공간에 따라 확장 및 축소되는 같은 너비의 트랙 3개를 생성합니다.

### 프로퍼티 설명
- `grid-auto-columns` : 암시적으로 생성된 그리드 열 트랙의 크기를 지정합니다.
  + `minmax(min, max)` : min 보다 크거나 같고 max 보다 작거나 같은 크기 범위를 정의하는 함수 표기입니다. max 가 min 보다 작으면 max 가 무시되고 함수는 min 으로 처리됩니다. 최소한 제로(혹은 최소 콘텐츠. 그리드 컨테이너가 최소한의 콘텐츠 제약 조건하에 크기가 조정되는 경우입니다.)로 취급됩니다.
  + `max-content` : 그리드 트랙을 점유하는 그리드 항목의 최대 콘텐츠 기여도를 나타내는 키워드입니다.
  + `min-content` : 그리드 트랙을 점유하는 그리드 항목의 최소 콘텐츠 기여도를 나타내는 키워드입니다.
- `grid-auto-rows` : 암시적으로 생성된 그리드 행 트랙의 크기를 지정합니다.
  + 동일하게 `minmax(min, max)`, `max-content`, `min-content` 가 있습니다.
  + `auto` : 최대일 때 최대 콘텐츠와 동일한 키워드입니다.
- `grid-column` : `grid-column-start` 및 `grid-column-end` 에 대한 속기(shortand) 프로퍼티입니다. 그리드 항목의 크기와 위치를 지정합니다. / 로 구분해서 앞부분이 `grid-column-start` 이고 뒷부분이 `grid-column-end` 입니다.
```CSS
.test {
  grid-column: 1;
  grid-column: 1 / 3;
  grid-column: 2 / -1;
  grid-column: 1 / span 1;
}
```
- `grid-row` : ` grid-row-start` 와 `grid-row-end` 에 대한 속기 프로퍼티입니다. / 로 구분해서 앞부분이 `grid-row-start` 이고 뒷부분이 `grid-row-end` 입니다.
```CSS
.test {
  grid-row: 1;
  grid-row: 1 / 3;
  grid-row: 2 / -1;
  grid-row: 1 / span 2;
}
```
## 예제
```CSS
#grid {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 3;
  grid-auto-rows: auto;
}
```
- 가로는 자동으로 설정했습니다.(`grid-auto-rows: auto;`)
- 3개의 세로 열 트랙을 설정했습니다.(`grid-template-columns: 3;`)
- 각 그리드 사이의 간격은 10px 로 조정했습니다.(`grid-gap: 10px;`)
```CSS
.one {
  grid-column: 1 / 4;
  grid-row: 1;
  background-color: black;
  color: white;
}
.two {
  grid-column: 3 / 4;
  grid-row: 1;
  color: grey;
  background-color: black;
}
.three {
grid-column: 1 / 4;
grid-row: 2;
background-color: purple;
}
.four {
grid-column: 1 / 4;
grid-row: 3;
background-color: green;
}
```
