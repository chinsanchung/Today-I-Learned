# Accessibility (Udacity)
- Accessibility(접근가능성)은 유저가 웹 사이트에 쉽게 접근하고 사용가능하도록 만들어주는 목적입니다. 줄임말로 `Ally` 라고 합니다.
  + 사이트의 콘텐츠를 누구나 접근할 수 있게 만들어야 합니다.
  + 페이지의 기능들은 누구나 조작할 수 있어야 합니다.
- 인지(시각, 청각) 문제, 신체의 불편 등의 문제가 있어도 웹 콘텐츠에 접근하도록 도와야 합니다.
- 접근가능한 웹 페이지를 위해 다음 장에서 `Focus`, `Semantics`, `Styling` 을 다룰 것입니다.

## WCAG 2.0
- 웹에서 정한 접근가능성 표준 가이드라인이 있습니다.
  + [Web Content Accessiblity Guidelines 2.0](https://www.w3.org/TR/WCAG20/)
  + [Web Aim Checklist for WCAG 2.0](https://webaim.org/standards/wcag/checklist)
- WCAG 는 4가지 원칙으로 구성됩니다.
  + Perceivable : 인지가 가능해야 합니다. (예 : 시각이 약한 사람도 접근할 수 있어야 합니다.)
  + Operable : UI 컴포넌트와 콘텐츠를 탐색할 수 있어야 합니다. (예 : 터치나 마우스를 쓰지 못하는 사람은 hover 기능을 사용하지 못합니다.)
  + Understandable : 모든 사용자들은 기능을 이해하고 혼란을 피해야 합니다.
  + Robust : 다양한 사용자들이 콘텐츠에 충분히 사용할 수 있어야 합니다.

# Focus
- `Focus` 는 페이지 어디에서 키보트 이벤트를 위치시킬지를 결정합니다. (예 : 화살표키)
- `Tab Order` : `Tab` 키로 포커스를 앞으로, `Shift + Tab` 으로 뒤로 이동시킵니다.
- HTML 의 input, button, select 는 implicitly focusable 입니다.
  + 이들에게는 기본적으로 `Tab Order` 와 키보드 이벤트 핸들링이 탑재되어 있습니다.
- 이미지, 글자들은 implicitly focusable 이 아닙니다. 따라서 `Tab Order` 를 쓸 수 없습니다.
- `Tabindex` : 이 attribute 는 `<div tabindex="0">aa</div>` 형식으로 사용합니다.
  + `tabindex="-1"` 은 자연적인 tab order 가 아니고, `focues()` 메소드를 이용해서 임의로 포커스를 맞출 수 있습니다.
```javascript
document.querySelector('#modal').focus();
```
  + `tabindex="0"` 는 자연스런 tab order 입니다. 이것 역시 `focus()` 메소드로 포커스를 맞출 수 있습니다.
```HTML
<!-- 이것 하나만으로는 tab 키로 포커스를 잡지 못합니다. -->
<div id="dropdown">Settings</div>
<!-- 이렇게 tabindex 를 줘야 포커스를 할 수 있습니다. -->
<div id="dropdown" tabindex="0">Settings</div>
```
  + `tabindex="0 이상"` : 자연스러운 tab order 입니다. tab 순서의 앞쪽으로 점프합니다. 이 방식은 anti-pattern 이기에 권장되지 않습니다. 스크린 리더 사용자에게 혼란을 줄 수 있어서입니다. 만약 어떤 것을 tab order 보다 앞에 두고 싶다면 DOM 을 그 위로 위치시키는게 더 좋습니다.
- `skip links` : tab order 를 주지 않는 방법입니다.
```HTML
<style>
/* top 을 -40 으로 해서 안보이도록 만듭니다. */
.skip-link { position: absolute; top: -40px; left: 0; background: #BF1722; color: white; padding: 8px; z-index: 100; }
/* 가상의 클래스입니다. 해당 element 가 포커스를 받으면 발생합니다. */
.skip-link:focus{ top: 0; }
</style>
<a href="#maincontent" class="skip-link">aaa</a>
<nav>~~</nav>
<!-- a 태그와 main 태그는 연결됐습니다. 만약 오래된 브라우저에서 포커스를 주고 싶지 않다면
main 안에 tabindex="-1" 을 줍니다.-->
<main id="maincontent">~~</main>
```
- `keyboard trap` : 선택이 끝났는데도 계속 해당 위치에 남아있는 것을 말합니다. (예 : select 태그에서 다음 태그로 넘어가지 않는 경우, 모달 창이 떴는데도 모달 창 밖에서 tab 키가 먹히는 경우)
```HTML
<!-- 해결책 -->
<button class="modal-toggle">Open Modal</button>
<div class="modal">
  <h1>test</h1>
  <div class="field">
    <labe for="fullname">Full Name</label>
    <input id="fullname" type="text">
  </div>
  <div class="field">
    <label for="email">Email address</label>
    <input id="email" type="text">
  </div>
  <button id="signup">Sign me up already!</button>
</div>
<div class="modal-overlay"></div>
```
```javascript
/*모달을 열기 이전에 포커스된 element 의 reference 를 계속 붙잡는 역할입니다.
모달이 끝난 후 저장해뒀던 포커스를 이걸 통해 다시 꺼냅니다. */
let focusedElementBeforeModal;
//모달과 오버레이를 찾는 변수입니다.
let modal = document.querySelector('.modal');
let modalOverlay = document.querySelector('.modal-overlay');
let modalToggle = document.querySelector('.modal-toggle');
modalToggle.addEventListener('click', openModal);

function openModal() {
  //이 reference 는 현재 포커스를 저장합니다.
  focusedElementBeforeModal = document.activeElement;

  //listen for and trap the keyboard function
  modal.addEventListener('keydown', trapTakKey);

  //listen for indeicators to close the modal
  modalOverlay.addEventListener('click', closeModal);
  //Sign up button
  let signUpBtn = modal.querySelector('#signup');
  signUpBtn.addEventListener('click', closeModal);

  //모달 다이로그 안에 있는 모든 포커스 될 수 있는 (focusable)  자식들을 찾습니다.
  let focusableElementsString = 'a[href], area[href], input:not([disabled]), select:not([disabled]), textarea:not([disabled]), button:not([disabled]), iframe, object, embed, [tableindex="0"], [contenteditable]';
  //포커스될 가능성이 있는 자식들을 모두 선택해서 변수에 담습니다.
  let focusableElements = modal.querySelectorAll(focusableElementsString);
  //NodeList 를 배열로 바꿉니다.
  focusableElements = Array.prototype.slice.call(focusableElements);
  //위의 그룹에서 가장 처음, 가장 마지막 element 입니다.
  let firstTabStop = focusableElements[0];
  //처음부터 tab 해서 마지막에 도달하면, length - 1 로 다시 firstTabStop 으로 이동합니다.
  //반대로 firstTabStop 에서 shift + tab 을 하면 lastTabStop 으로 이동합니다.
  let lastTabStop = focusableElements[focusableElements.length - 1];

  //모달과 오버레이를 보여줍니다.
  modal.style.display = 'block';
  modalOverlay.style.display = 'block';

  //첫 자식을 포커스하니다.
  firstTabStop.focus();

  function trapTakKey(e) {
    //tab 키를 눌렀을 때 실행합니다.
    if (e.keyCode === 9) {
      // shift + tab 키
      if (e.shiftKey) {
        //현재 firstTabStop 에 포커스됐는지 여부
        if (document.activeElement === firstTabStop) {
          e.preventDefault();
          lastTabStop.focus();
        }
      // tab 키  
      } else {
        if (document.activeElement === lastTabStop) {
          e.preventDefault();
          firstTabStop.focus();
        }
      }
    }
    // escape 키
    if (e.keyCode === 27) {
      closeModal();
    }
  }
}
function closeModal() {
  //모달과 오버레이를 숨깁니다.
  modal.style.display = 'none';
  modalOverlay.style.display = 'none';

  //포커스를 모달이 열리기 전에 있던 element 로 이동시킵니다.
  focusedElementBeforeModal.focus()
}
```
# Semantics
- Role, Name, Value
- Accessiblity Tree : screen reader 가 바라보는 Accessiblity 나무는 아래와 같습니다.
  + main -> form -> radio button / edit text / combo box / button
  + 중점은 HTML 에서 screen reader 가 읽을 부분만을 추출해서 만든 게 Accessiblity tree 입니다.
- DOM 은 브라우저나 다양한 플랫폼에서 인식할 수 있는 standard native HTML elements 를 사용합니다. (link, button 등) 이들을 활용해서 접근가능성을 높일 수 있습니다.
- 텍스트가 아닌 내용이라도 대안으로라도 텍스트로 제공해야 합니다.
- label 에는 visible label 과 text alternative 가 있습니다.
  + visible label : 모든 사용자가 element 의 의미에 연결할 수 있습니다. (예 : 라디오 버튼)
  + text alternative : visual label 이 필요없을 때에만 사용됩니다. (예 : 이미지) text alternative 는 페이지 상에는 나타나진 않습니다.
```HTML
<!-- alt 에 설명을 적으면 screen reader 가 읽어줍니다. -->
<img alt="Rabbit is eating a carrot" src="rabbit.jpg">
```
  + visible, non-visible label 을 사용하려면 다음의 사항을 준수하면 됩니다. 'Form 버튼은 descriptive value 가 있습니다.' 'Form 인풋(체크박스 등)은 텍스트 label과 연결해야 합니다.'
```HTML
<input type="checkbox" checked name="jLetter">Receive
<!-- 위의 방식은 좋지 않은 방식입니다. 해결하려면 -->
<label><input type="checkbox" checked name="jLetter">Receive</label>
<!-- 또는 input 에 id 를 주고 연결합니다. 이러면 라벨을 체크해서 체크박스를 움직일 수 있습니다. -->
<input type="checkbox" checked name="jLetter" id="letter">
<label for="letter">Receive</label>
```
# Navigating by Headings
- screen reader 에 있어서 헤더의 존재는 매우 중요합니다. 헤더로 목적지를 정하고 출발해야 하기 때문입니다.
  + 'reading and navigation order(determined by code order) is logical and intuitive(직관적인)'
  + 전체 문서 구조에서 개인적인 콘텐츠 섹션은 적절한 위치에 헤더를 사용해서 디자인해야 합니다.
  + 모든 헤더를 페이지에 보여줄 필요는 없습니다. 몇몇은 off screen 으로 사용합니다.
- link Text
  + 패턴 1 : `<a href="#internal">Now I'm in a link</a>` 올바르게 경로를 설정했습니다.
  + 패턴 2 : 다른 태그로 클릭 이벤트를 설정하기보다는 버튼을 만듭니다.
```HTML
<!-- 권장되지 않습니다. -->
<a href="#" onclick="do()">do<a>
<!-- 버튼으로 설정합시다. -->
<button class="link" onclick="do()">do</button>
```
  + 패턴 3 :
```HTML
<a href="/">
  <img src="logo.jpg">
</a>
<!-- 아래의 방식으로 접근성을 높입니다. -->
<a href="/">
  <img src="logo.jpg" alt="Udacity">
</a>
```
- Landmarks : HTML5 에서는 Landmarks 를 지원합니다. [Udacity Landmarks](https://youtu.be/bww3IaktlRY)
  + header(->nav) - main(->section -> article / aside) - footer
