# ARIA (Accessible Rich Internet Applications)
- 보통은 WAI(Web Accessibility Initiative)-ARIA 라고 합쳐서 부릅니다.
- ARIA 는 Accessiblity tree 로 번역될 elements 의 attributes 를 지정합니다.
  + 예 : checkbox 를 screen reader 가 읽지 못할 때 ARIA attributes 를 넣으면 screen reader 가 해당 elements 를 checkbox 로 인식하게 만들어줍니다. (name만 있음 -> name, state 있음)
```HTML
<!-- navitve semantics 가 없는 element 입니다. -->
<div class="checkbox checked">screen reader can't read checkbox</div>
<!-- aria-checked attribute 를 사용했습니다. -->
<div role="checkbox" aria-checked="true">It's checked by using ARIA</div>
<!-- 이미 존재하는 semantics 도 변경합니다. -->
<button class="toggle" checked>enable</button>
<button role="switch" aria-checked="true" class="toggle">enable</button>
```
- ARIA 는 오직 Accessibility tree 만을 수정할 수 있습니다.
  + element apperance, element behavior 수정, add focusablility, add keyboard event handling 도 불가능합니다.
- role 설정 시 주요 사항들
  + 라디오 버튼을 사용할 때 container 의 role 을 radiogroup 으로 작성합니다.
  + 각각의 라디오 버튼들읠 role 은 radio 로 합니다.
  + 선택된 것은 aria-checked="true" 로 합니다. 선택 안된 것은 aria-checked="false" 로 합니다.
```javascript
RadioGroup.prototype.changeFocus = function () {
  this.focusedButton.tabIndex = -1;
  this.focusedButton.removeAttribute('checked');
  this.focusedButton.setAttribute('aria-checked', 'false');

  //새로운 버튼을 tabindex 를 0으로 잡고 포커스합니다.
  this.focusedButton = this.buttons[this.focusedIdx];
  this.focusedButton.tabIndex = 0;
  this.focusedButton.setAttribute('checked', '');
  this.focusedButton.setAttribute('aria-checked', 'true');
};
```
  + radiogroup 과 라디오 버튼은 aria-labelledby 프로퍼티를 사용하고 보여야(visible) 합니다.
```HTML
<div id="secret" hidden>hotdog</div>
<button aria-label="menu" aria-labelledby="secret" class="hambuger"></button>
<!-- 버튼의 이름은 위의 id secret 의 값인 hotdog 가 됩니다. -->
<!-- 참고로 aria-label 은 특정 라벨가능한 element 에만 사용가능하지만 aria-labelledby 는 div 등 모든 element 에 사용가능합니다. -->
<!-- aria-labelledby 는 모든 타입의 라벨들을 오버라이드합니다. 위의 secret 도 그 예입니다. -->
```
  + aria-describedby 프로퍼티를 사용해 라디오 버튼이나 radiogroup 에 정보를 추가합니다.
- 이미 정해진 role 을 바꿔서 입력하면 안됩니다. 또한 role 을 중복해서 작성할 필요는 없습니다.
```HTML
<!-- 이렇게 하면 안됩니다. -->
<input type="text" role="heading">
<input type="checkbox" role="checkbox">
<main role="aside">
```
- aria-labelledby 은 해당 element 와 relationship attribute 를 구축합니다. 위의 secret id 레퍼런스를 활용한 것이 예입니다.
  + aria-owns 는 해당 element 와 relationship attribute 를 구축해서 해당 element 를 자식으로 만들거나 존재하는 자식 element 으로 다른 명령을 내릴 수 있습니다.
```HTML
<div role="menu">
  <div aria-owns="submenu"></div>
</div>
<!-- 이 submenu 는 위의 aria-owns 부모 메뉴의 자식이 됩니다. -->
<div role="menu" id="submenu">~~~</div>

<!-- 또한 aria-owns 는 해당 id 들에게 팝업창읠 true 로 하라는 명령을 내릴 수 있습니다. -->
<div role="menu" aria-owns="mi1 sm1 mi2 mi3" aria-haspopup="true"></div>
```
  + aria-activedescendant 는 해당 자식 element 를 포커스하게 해줍니다.
  + aria-posinset 은 리스트박스에서 해당하는 것을 우선시해서 보여줍니다. aria-setsize 는 형제 elements 와의 관계를 정의합니다.
```HTML
<div role="listbox">
  <!-- aria-posinset 은 해당 element 의 포지션을 알려주고, aria-setsize 로 리스트 전체의 크기를 알려줍니다. -->
  <div role="option" aria-posinset="857" aria-setsize="1000">Item 857</div>
</div>
```
- 해당 element 를 숨기는 것도 때론 사용합니다.
```HTML
<button style="visibility: hidden;"></button> <div style="display: none;"></div> <span hidden>

<div class="deck"><div class="slide" aria-hidden="true">aaa</div>

<style>
.screenreader {
  position: absolute;
  left: -10000px;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
</style>
```
- aria-live 는 aria-atomic, aria-relevant, aria-busy 와 함께 쓰여 live 지역이 바뀔 때 알려줍니다.

# Style
- focus style : 포커스 가상 클래스는 오직 element 가 포커스 됐을 때만 적용됩니다.
  + 포커스 가상 클래스를 사용해 자신만의 포커스 지시를 제공할 수 있습니다.
```CSS
:focus {
  /* 테두리를 만듭니다. */
  outline: 1px dotted #fff;
  /* 아래의 방식으로 아웃라인을 지우면 anti-pattern 이 됩니다.
  포커스가 어디에 있는지를 모르게 됩니다. */
  outline: 0;
}
/* 버튼 예시. hover 가상 클래스(마우스가 가리킬 때 스타일이 적용됩니다.)
만약 포커스 고리를 지우고 교체하려면 button:focus 를 같이 사용하면 됩니다. */
button:hover,
button:focus {
  background: #E91E63;
  color: #ffffff;
  text-decoration: underline;
}
```
- 키보드와 마우스의 포커스는 다릅니다. 마우스 유저보다는 키보드만 사용하는 사람(시각장애인 등)을 배려하기 위해서인 듯합니다.
- ARIA attribute 와 CSS 를 같이 사용하는 것도 좋습니다.
```CSS
/* 클래스만으로 하기 */
.toggle.pressed {  }
/* ARIA 와 CSS 조합하기 */
.toggle[aria-pressed="true"] {  }
