# 자바스크립트 이벤트(UdaCity)
## 이벤트 응답
### 이벤트 타겟
- `Node Interface` 는 `EventTarget` 인터페이스를 상속합니다.
- `EventTarget` : 이벤트를 수신할 수 있는 객체에 의해 구현되는 인터페이스입니다. listner 를 가질 수 있습니다.
  + element, document, window 는 가장 기본적인 `EventTarget` 지만, 다른 객체들도 `EventTarget` 이 될 수 있습니다.
- 사슬의 최상위이기 때문에 다른 인터페이스에서 프로퍼티나 메소드를 상속하지 않습니다.
  + 모든 다른 인터페이스는 `EventTarget` 으로부터 상속받아 그것의 프로퍼티와 메소드들을 가지게 됩니다. (문서 객체, paragraph element, 비디오 element 등은 `EventTarget` 입니다.)
  + element 인터페이스는 `EventTarget` 인터페이스를 상속합니다. 그리고 문서 객체는 문서 인터페이스에서 나오며, 문서 인터페이스는 `EventTarget` 인터페이스를 상속합니다.
- `EventTarget` 인터페이스는 프포퍼티 없이 세 가지 메소드만 존재합니다. `.addEventListner()`, `removeEventListner()`, `.dispatchEvent()`

### addEventListner 메소드
- `.addEventListner()` 메소드는 이벤트를 수신(listen) 하고 응답할 수 있습니다. 
  + listen for an event, listen to an event, hook into an event, respond to an event 는 모두 같은 뜻입니다.
- 이벤트 리스너는 세가지가 필요합니다.
  + 이벤트 타겟(줄여서 타겟), 어떤 타입의 이벤트를 수신할 것인지(줄여서 타입), 이벤트가 발생했을 때 수행할 함수(줄여서 리스너)
```HTML
<event-target>.addEventListener(<type of event to listen for>, <function-to-run-when-an-event-happens>);
```
```javascript
const mainHeading = document.querySelector('h1');
//타겟은 h1, 이벤트 타입은 click, 리스너는 콘솔창에 띄우는 함수입니다.
mainHeading.addEventListner('click', function () {
  console.log('The heading was clicked');
});
```
- 다양한 이벤트들이 있으니 문서에서 검색해 사용하는 것이 좋습니다. https://developer.mozilla.org/en-US/docs/Web/Events

## 이벤트 리스너 삭제
