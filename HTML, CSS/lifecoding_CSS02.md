# 생활코딩 CSS 수업 02
프로퍼티(속성)을 빈도가 높은 중요한 속성에부터 시작해서 순차적으로 다뤄봅니다.
## 타이포그라피
### font-size
- px : 모니터 상의 화소 하나의 크기에 대응되는 단위입니다. 절대적인 고정값이기에 반응형으로 웹 페이지를 만든다면 px 를 쓰지 않는 것이 좋습니다.
- rem : HTML 태그에 적용된 font-size 의 영향을 받습니다. 즉 HTML 태그의 폰트 크기에 따라 상대적으로 크기가 결정됩니다. 사용자가 화면의 환경이나 시각적인 불편함 등에 따라 다른 폰트 크기를 지정해야 할 때 그에 따라 크기를 바꿔주는 `rem`을 사용할 것을 권장합니다.
- em : `rem` 과 달리 부모 태그의 영향을 받아 상대적으로 크기가 결정됩니다. 그래서 정확한 크기를 파악하기 어려워 `rem` 을 우선적으로 사용하는 추세입니다.
### color
color 로 색상을 지정하려면 크게 세 가지 방법으로 할 수 있습니다. 직접 색상의 이름으로 적기, RGB 값('rgb(256, 256, 256)')을 적기, 16진수('#000000')으로 적기가 있습니다.
### text-align
text-align 은 left(기본값), right, center, justify(양쪽 균등하게 정렬, 자동 줄바꿈시 오른쪽 경계선 부분 정리) 의 값을 가지고 있습니다.
### font
- font-family : 서체를 지정합니다. 서체는 크게 고정폭, 가변폭으로 구분합니다.
예시) 'font-family: "Times New Roman", Times, serif;'는 서체를 Times New Roman 으로 지정하는데, 사용자의 컴퓨터에 폰트가 없으면 Times, 그것도 없으면 serif 를 쓴다는 뜻입니다.
  - 그래서 마지막 폰트는 포괄적인 것으로 작성합니다. 예)serif(장식 있는 폰트), sans-serif(=not serif), cursive(흘림), fantasy, monospace(고정폭)
- font-weight : 폰트의 두께를 나타냅니다. 예를 들어 bold 를 쓰면 두꺼워집니다.
- line-height : 글자의 행간격을 지정합니다. 기본값 nomal 은 1.2입니다.(현 요소 폰트 크기의 1.2배 간격)
- font : 폰트와 관련된 여러 속성을 축약형으로 표현하는 속성입니다. 형식은 아래와 같습니다. 순서를 지켜서 기술하셔야 합니다. font-size, font-family 는 필수입니다.
```
font: font-style font-variant font-weight font-size/line-height font-family|caption|icon|menu|message-box|small-caption|status-bar|initial|inherit;
```
### 서체
- 서체는 크게 두가지 방법으로 구분할 수 있습니다. 고정폭(monospaced)과 가변폭입니다.
- serif / sans serif : serif는 글꼴에서 사용하는 장식을 의미합니다. sans는 부정의 표현입니다. 즉 sans serif는 serif가 아니다는 뜻입니다.
### 웹 폰트
웹 폰트는 사용자가 가지고 있지 않은 폰트를 웹페이지에서 사용할 수 있는 방법입니다. 폰트를 서버에서 다운로드하는 방식이라고 할 수 있습니다.
고려해야 할 점은 바로 폰트의 용량입니다. 영문 폰트는 크기가 작지만 한글 폰트는 용량이 커서 네트워크 비용을 고려해야 합니다.(점점 저렴해지고 있기는 합니다.)
## 조화
### 상속
상속은 부모 엘리먼트의 속성을 자식 엘리먼트가 물려받는 것을 의미합니다. 상속은 CSS에서 생산성을 높이기 위한 중요한 기능입니다. 예를 들어 서체의 색을 바꿀 때 요소들 각각에서 컬러를 주지 않고, 상속을 이용해서 html의 서체 색상만 조정하면 하위에 있는 모든 엘리먼트의 색상이 자동으로 바뀌게 됩니다.
- 상속을 하는 속성 / 상속을 하지 않는 속성
하지만 모든 속성이 상속을 지원하는 것은 아닙니다. 상속을 하면 오히려 비효율적인 속성들이 있기 때문입니다. [w3.org 프로퍼티 상속](https://www.w3.org/TR/CSS21/propidx.html)페이지에서 상속 여부를 확인할 수 있습니다.
### 캐스케이딩
웹 브라우저, 사용자, 개발자 각각 원하는 디자인을 반영할 수 있도록 조화롭게 하려면 질서가 필요합니다. 그래서 우선순위를 지정해야 합니다. 몇 가지 중첩된 관계를 통해서 드러납니다.
규칙 1: 웹 브라우저, 사용자, 개발자가 동시에 중첩되는 디자인을 반영할 경우 `브라우저 < 사용자 < 개발자` 순서로 우선순위가 반영됩니다. 참고로 사용자가 직접 css 를 바꾸는 경우는 거의 없습니다.
```HTML
<style>
  li{color:red;}
  #idsel{color:blue;}
  .classsel{color:green;}
</style>
<li>html</li>
<li id="idsel" style="color:powderblue" class="classsel">css</li>
<li>javascript</li>
```
  - 여기서는 1 powderblue -> 2 blue -> 3 green -> 4 red -> 5 black(browser기본 스타일) 를 우선해서 색상이 적용됐습니다.
  - 즉 스타일 속성(attribute)>아이디 선택자>클래스 선택자>태그 선택자 순으로 우선순위가 있습니다.
  - 중요도 기준은 더 구체적이고 명시적인게 중요하고 반대로 포괄적이고 일반적인 것이 덜 중요합니다. 가장 포괄적인 것은 li, 그 다음이 클래스(여러 태그를 선택가능해서 태그보다는 정교함), 그 다음 정교한 것은 아이디(특정 태그에 대해서만 아이디 가능), 마지막으로 스타일 속성이 태그에 더 가까워서 명시적이므로 우선순위가 높습니다.
우선순위를 임의로 조절하려면 `!important`를 넣는 것입니다. 만약 li 에 `!important`를 넣는 다면 가장 우선해서 적용되도록 만들어 줍니다. 하지만 그리 추천할 방법은 아닙니다.

## 레이아웃
### 인라인 / 블록
HTML 요소는 화면 전체를 사용하는 `block`, 화면 일부만 차지하는 `inline` 요소로 구분합니다.
border 로 경계를 확인하면 블록은 한 줄 전체를, 인라인은 문자만큼만 경계를 차지하는 것을 확인할 수 있습니다.
- 블록은 너비, 높이 속성을 지정할 수 있어 레이아웃 배치에 주로 사용합니다.
- 인라인은 반대로 너비와 높이를 지정하지 못합니다. 그래서 줄 바꿈은 안되더라도 너비, 높이를 지정할 수 있는 `inline-block`으로 바꾸는 경우도 있습니다.
- `display: none`은 태그를 안 보이게 합니다만, `visibility: hidden`과 달리 영역을 차지하지 않습니다.
### 박스 모델
박스 모델은 사용 빈도가 굉장히 높습니다. margin, padding, border 등 다 박스 모델입니다.
  - 테두리 `border`는 박스 모델의 기준점을 선정하기 때문에 매우 중요합니다.
  - `border`와 안의 내용 사이의 간격을 조절하는 것은 `padding`입니다.
  - 페이지 본문과 테두리 `border`, 태그와 태그 사이의 간격을 조절하는 것이 `margin`입니다.
참고로 p 는 블록으로 한 줄 전체를 사용하는데, 너비를 지정하면 한 줄 전체를 쓰지 않도록 만들 수 있습니다.
### 박스 사이징
`box-sizing`은 박스의 크기를 화면에 표시하는 방식을 변경하는 속성입니다. 너비와 높이는 요소의 컨텐츠의 크기를 지정합니다. 따라서 테두리 `borer`가 있는 경우에는 테두리의 두께로 인해서 자신이 지정할 크기, 높이를 파악하기가 어렵습니다.
`box-sizing`속성을 `border-box` 로 지정하면  테두리를 포함한 크기를 지정할 수 있기 때문에 예측하기가 더 쉽습니다. 최근엔 모든 요소에 이 값을 지정하는 경우가 늘고 있습니다.
- 요소의 크기를 보통 테둘 바깥쪽이라 생각하기 쉽습니다. 하지만 실제로는 border, padding 이 빠진 콘텐츠 영역의 너비가 바로 width 로 조절할 수 있는 것입니다. 초창기에는 이렇게 콘텐츠만 있어서 헷갈리지 않았지만 border 로 테두리를 만들고 두께도 조절할 수 있다보니 이렇게 헷갈리는 경우가 생기게 됐습니다.
- 이를 해결하는게 `box-sizing` 속성입니다. 기본값은 content-box 입니다.(콘텐츠 크기만큼만 너비, 높이 값을 지정합니다.)
  - 그 외에도 border-box 는 테두리의 크기를 기준으로 너비, 높이 값을 지정합니다. 그래서 `*` 을 사용해서 전체 태그에 border-box 로 지정해서 헷갈릴 경우의 수를 줄이는 방법도 사용합니다.
### 마진 겹침
### 포지션
화면상에서 요소의 위치를 지정하는 방법은 `static`, `relative`, `absolute`, `fixed` 네가지가 있습니다.
1. static 과 relative
```CSS
div {
  border: 5px solid tomato;
  margin: 10px;
}
#me {
  position: relative;
  /*left,right, top, bottom 을 실행하려면 포지션을 relative 로 해야 합니다*/
  left: 100px;
}
```
```HTML
<div id="other">other</div>
<div id="parent">
  parent
  <div id="me">me</div>
  <div id="large">large</div>
</div>
```
- CSS 각각의 요소들의 기본 포지션은 `static`입니다. `static`은 offset 값(left, right, top, bottom)을 무시하고 원래 위치해야 하는 곳에 정적으로 위치합니다.
- 만약 offset 값 이동을 원래 위치(부모 요소 아래)를 기준으로 상대적으로 이동시키고 싶다면 포지션을 `relative`으로 지정해야 합니다.
  - offset 값으로 이동하려면 포지션을 `relative`로 해야 사용이 가능해집니다.
2. absolute
```CSS
#parent, #other {
  border: 5px solid tomato;
}
#me {
  background-color: black;
  color: white;
  position: absolute;
  /* absolute 로 top, left 를 0으로 하면 기준점을 볼 수 있게 됩니다. */
  top: 0;
  left: 0;
}
```
- 만약 기준을 부모 요소에서 상대적으로 이동하는게 아니라 웹 페이지 좌상단 꼭대기에 있는 HTML 요소를 기준으로 위치를 지정하려면 `absolute` 포지션을 써야 합니다.
  - `absolute`의 기본값(위치 이동을 하지 않을 때)는 평소대로 부모 요소의 아래에 있게 됩니다.
  - `absolute`인 요소는 더이상 부모의 소속이 아니게 됩니다. #me 는 이제 부모 콘텐츠의 사이즈, 크기 등을 물려받지 않고, 자신의 콘텐츠 크기만큼 변하게 됩니다.
- 만약 #parent 의 포지션을 `relative`로 지정하면 #me 는 `relative`로 지정한 #parent 를 기준으로 offset 값을 설정하게 됩니다. 반대로 부모 요소는 `absolute`인 자식 요소를 없는 셈 무시합니다.
  - 즉 `absolute`는 `static`인 부모 요소를 무시하다가 `static`이 아닌(포지션을 지정한) 부모가 나타나면 그 곳을 기준으로 offset 을 지정합니다.
3. fixed
```CSS
#me {
  position: fixed;
}
#large { height: 1000px; }
```
- `fixed`는 스크롤에서 독랍한, 자기 위치에 고정된 요소를 가지게 됩니다.
- `fixed`는 `absolute`처럼 더이상 부모 요소의 소속이 아니므로 너비, 높이를 지정하지 않으면 자기 콘텐츠만큼의 크기를 가지게 됩니다.
