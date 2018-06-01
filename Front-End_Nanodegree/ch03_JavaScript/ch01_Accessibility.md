# Accessibility (Udacity)
- Accessibility(접근가능성)은 유저가 웹 사이트에 쉽게 접근하고 사용가능하도록 만들어주는 목적입니다.
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
