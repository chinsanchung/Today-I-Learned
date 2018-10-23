# 생활코딩 CSS 수업 01
---
### 선택자
선택자는 CSS 에서 주어에 해당합니다.
#### 선택자와 선언
```CSS
h1 { color: red; }
```
  - 선택자 : h1
  - 서술어 : { ... }
  - 프로퍼티 / 값 : color / red
  - 선언과 선언을 구분 : ;

#### 선택자의 종류
```CSS
/* 태그 선택자 */
li { color: red; }
/* 아이디 선택자 */
#select { font-size: 10px; }
/* 클래스 선택자 */
.item { text-decoration: line-through; }
```

#### 부모 자식 선택자
```CSS
/* 조상 자손 선택자 */
ul li { color: red; }
/* 부모 자식 선택자 */
#lecture>li { border: 1px solid red; }
/* ul,ol 을 동시에 선택 */
ul, ol { text-align: center; }
```
  - 조상 자손 선택자: ul 의 자식인 밑에 있는 li 태그만 지정합니다.
  - 부모 자식 선택자: 아이디 선택자의 바로 밑, 즉 직계 자식인 li 태그만 지정합니다.
  - 동시 선택: 같은 내용을 굳이 반복해서 적지 않도록 해줍니다.

#### 가상 클래스 선택자
가상(pseudo) 클래스 선택자는 아니지만 엘리먼트들의 상태에 따라서 마치 클래스 선택자처럼 여러 엘리먼트를 선택할 수 있다는 점에서 붙은 이름입니다.
```CSS
a.link { color:black; }
a.visited { color:red; }
a.hover { color:yellow; }
a.active { color: green; }
a.focus { color:white; }
```
  - link: 방문한 적이 없는 링크의 스타일을 바꿉니다.
  - visited: 방문한 적이 있는 링크의 스타일을 바꿉니다. visited의 경우는 보안상의 이유로 color, background-color, border-color, outline-color, The color parts of the fill and stroke properties 만 변경이 가능합니다.
  - hover: 마우스를 해당 링크에 올려둘 때의 스타일을 바꿉니다.
  - active: 마우스로 해당 링크를 클릭할 때의 스타일을 바꿉니다.
  - focus: tab 키로 링크로 포커싱을 했을 때의 스타일을 바꿉니다.
가상 클래스 선택자는 CSS 의 특징(동급의 선택자를 사용해야 할 경우 뒤의 것을 우선시한다)으로 인해 적용이 안될 수도 있으므로 link, visited, hover, active, focus 순서대로 지정하는 것이 좋습니다.

#### 다양한 선택자들
- `#blue plate` : blue 아이디 요소의 자식 plate
- `orange.small` : orange 태그 중 클래스가 small
- `bento orange.small` : bento 태그 밑 중 클래스가 small 인 orange 태그
- `plate,bento` : 문서 전체에서 plate, bento 태그 둘 다를 선택
- `*` : universal selector. 모든 태그를 선택
- `plate *` : plate 태그 밑의 모든 태그들을 선택
- `plate+apple` : adjacent sibling selector. plate 옆에 근접한 apple 태그를 선택. plate 를 선택하지 않음.
- `bento~pickle` : general sibling selector. bento 와 인접한 모든 pickle 태그를 선택
- `plate>apple` : 'plate apple' 은 plate 밑의 모든 apple 태그를 가리키지만, 이것은 문서의 모든 apple 중 plate 바로 밑에 있는 apple 태그만 선택
- `plate orange:first-child` : plate 밑의 여러 orange 태그 중에서 첫 번째로 등장하는 태그
여기 밑에서부터는 사용도가 낮아 잘 쓰이지 않는 용어들입니다.
- `*:only-child` : 문서 전체에서 유일하게 하나의 자식만 있는 요소만 선택
- `.table>*:last-child` : 테이블 클래스 요소에서 가장 뒤에 있는 요소만 선택
- `.small:last-child` : small 클래스 요소의 마지막 자식 요소만 선택
- `plate:nth-child(3)` : plate 태그 중 3번째 요소만 선택
- `:nth-last-child(4)` : nth-child 와 비슷하지만 이건 뒤에서부터 시작함.
- `span:first-of-type` : 문서에서 처음으로 등장한 span 태그를 선택
- `plate:nth-of-type(2)` : plate 태그에서 두번째로 등장하는 태그만 선택. `nth-of-type(odd)`은 홀수 태그들만 선택하고, 반대로 `nth-of-type(even)`은 짝수 태그들만 선택
- `plate:nth-of-type(3n)` : 3의 배수인 태그들만 선택함. 2n+2 이런 식으로도 가능함.
- `apple:only-of-type` : 자기 자신과 같은 형제가 존재하지 않는 요소만 선택.
```javascript
<ul>
  //li, p 는 only-of-type
  <li>aaa</li><p></p></ul>
<ul>
  <li></li><li></li>
</ul>
```
- `orange:last-of-type, apple:last-of-type` : orange 와 apple 태그 중 마지막 태그만 따로 선택
- `bento:empty` : bento 태그 중 아무 자식도 가지지 않은 태그만 선택
- `:not(#fancy)` : 아이디 fancy를 가지지 않은 모든 태그를 선택.
  - `div:not(:first-child)` : 모든 div 중 첫번째 자식만 제외
  - `:not(.big, .medium)` : 모든 태그들 중에서 클래스가 big, medium 이 아닌 태그들만 선택
  - `apple:not(.small)` : 클래스가 small 이 아닌 모든 apple 태그를 선택
