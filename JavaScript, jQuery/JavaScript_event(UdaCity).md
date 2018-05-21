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
- 최신 버젼에서는 세번째 매개변수로 동작 방식을 구성하는 객체를 지원합니다. 하지만 아직 널리 지원되지는 않습니다.
- 다양한 이벤트들이 있으니 문서에서 검색해 사용하는 것이 좋습니다. https://developer.mozilla.org/en-US/docs/Web/Events

## 이벤트 리스너 삭제
- `.removeEventListener()` 메소드로 이벤트 리스너 를 삭제할 수 있습니다.

### 동등한 객체
```javascript
{ name: 'Richard' } === { name: 'Richard' } //false

var a = {
    myFunction: function quiz() { console.log('hi'); }
};
var b = {
    myFunction: function quiz() { console.log('hi'); }
};
a.myFunction === b.myFunction //false
```
- `.removeEventListener()` 메소드는 `.addEventListner()` 메소드에서 전달한 리스너와 동일한 리스너(함수)를 전달해야 하기에 그 객체가 동등한지를 따져야 합니다.

### removeEventListner
- 이벤트 타겟, 이벤트의 타입, 삭제할 함수인 리스너 를 필요로 합니다.
```HTML
<event-target>.removeEventListener(<type of event to listen for>, <function-to-remove>);
```
 + 여기서 리스너 함수는 `.addEventListner()` 메소드에서 사용한 것과 같은 함수여야만 합니다.
```javascript
document.addEventListner('click', function myEventListeningFunction() {
  console.log('howdy');
 });
 
 document.removeEventListner('click', function myEventListneingFunction() {
  console.log('howdy');
 });
//삭제는 실패합니다. 왜냐면 이벤트를 생성할 때 내부에서 생성한 함수이기 때문입니다. click 이벤트는 그대로 유지됩니다.
//그래서 삭제할 때의 내부함수와 생성할 때의 내부함수는 다릅니다.
```

## 이벤트 페이즈(단계)
- 이벤트의 주기는 세 가지 단계가 있습니다. `capturing` 단계, `at target` 단계, `bubbling` 단계 입니다.
```HTML
<html>  <!-- 그 다음 버튼에서부터 부모로 올라오는 3단계 bubbling -->
 <body>
   <div class="container">
    <p>  
<!-- 버튼 클릭시 1단계 Capturing. html부터 아래로 내려가서 버튼에 도달하면 2단계 at target-->
      <button>dd</button> 
    </p>
   </div>
 </body>
</html>
```
 + 대다수의 이벤트 핸들러들은 `at target` 단계에서 실행됩니다. (버튼에 있는 클릭 이벤트 핸들러에 접근하기 -> 이벤트가 타겟인 버튼에 도착)
 + 하지만 많은 아이템이 있는 목록일 경우에는 이벤트 핸들러 하나가 모두를 커버한다면 문제가 발생합니다. 버블링으로 아래서부터 하나씩 실행되어 부모에 이르기까지 버블링을 계속 유지하게 됩니다.
 + 하지만 캡쳐링을 이용하면 부모는 자식에게 도달하기 전에 이벤트를 가로챌 수 있습니다.

### addEventListner() 로 알아보기
- `.addEventListner()` 메소드는 이벤트 타입과 리스너 두 인수만 있는것같지만 실은 하나 더 있습니다.
 + 바로 불리언인 useCapture 인수입니다. 기본적으로 두 인수만 사용한다면 `.addEventListner()` 는 호출 즉시 `capturing` 단계로 가지 않고 버블링 단계를 사용합니다.
- 세번째 인수는 `capturing` 단계에 대한 불리언입니다. true 면 이벤트 리스너가 `capturing` 단계에서 실행됩니다. false 면 실행되지 않고 기본값인 버블 단계로 실행됩니다.

### 이벤트 객체
- 이벤트가 발생하면 브라우저에는 이벤트 객체가 포함됩니다.
 + `.addEventListner()` 메소드의 리스너 함수는 지정한 타입의 이벤트가 발생할 때 이벤트 인터페이스를 구현하는 객체를 알리는 내용을 수신합니다.
```javascript
//이벤트 객체를 저장할 파라미터를 만들었습니다. 리스너 함수가 호출되면 이벤트 데이터를 저장할 수 있게 됩니다.
document.addEventListener('click', function (event) {  // ← the `event` parameter is new!
   console.log('The document was clicked');
});
```
 + event 라는 이름은 레귤러 변수이기에 써선 안됩니다. 그래서 다른 이름으로 파라미터를 써야 합니다.
- 이벤트 객체를 쓰는 이유는 default 액션(기본 동작)을 막기 위해서입니다. 그러나 이벤트 객체가 없어서 기본 동작이 유지되더라도 `preventDefault()` 메소드를 쓰면 기본 동작을 막을 수 있습니다.
```javascript
const links = document.querySelector('a');
const thirdLink = links[2];

thirdLink.addEvnetListner('click', function (eventtt) {
 even.preventDefault();
 console.log("Look, ma! We didn't navigate to a new page!");
});
```

## 반복문에 이벤트를 효과적으로 적용시키기
- 기본적인 형태로 for 반복문 안에 이벤트 핸들러를 위치시킨다면 반복하는 갯수만큼의 이벤트 리스너가 만들어집니다.
 + 이벤트 리스너 함수를 생성자 함수로 만든다면 반복문으로 함수가 만들어지더라도 같은 함수가 됩니다. 그리고 함수를 외부에서 선언하는 방법도 좋습니다.
```javasciprt
const myCustomDiv = document.createElement('div');

function respondToTheClick() {
 console.log('A paragraph was clicked');
}

for (let i = 1; i < 200; i++) {
 const newElement = document.createElement('p');
 newElement.textContent = 'This is paragraph num' + i;
 
 newElement.addEventlistner('click', respondToTheClick);
 //div 의 밑인 p 에 넣습니다.
 myCustomDiv.appendChild(newElement);
}
//마지막으로 body 밑에 반복문으로 만든 p 를 넣습니다.
document.body.appendChild(myCustomDiv);
```
 + 이 방법은 함수는 하나지만 이벤트 리스너는 여러 개가 됩니다. 만약 <p> 가 아닌 <div> 에 이벤트 리스너를 배치한다면 하나의 이벤트 리스너에 하나의 리스너 함수로 줄일 수 있습니다.
```javasciprt
const myCustomDiv = document.createElement('div');

function respondToTheClick() {
    console.log('A paragraph was clicked.');
}

for (let i = 1; i <= 200; i++) {
 const newElement = document.createElement('p');
 newElement.textContent = 'This is paragraph number ' + i;
 
 myCustomDiv.appendChild(newElement);
}
//이벤트리스너를 div 에 입력합니다.
myCustomDiv.addEventListner('click', respondToTheClick);

document.body.appendChild(myCustomDiv);
```
- 이 방법은 효과적이지만 개별적인 paragraph 에 접근하기가 불가능합니다. 개별 paragraph 에 접근하려면 `event delegation` 이 필요합니다.

### 이벤트 위임(delegation)
- 이벤트 객체는 `.target` 프로퍼티를 가지고 있습니다. 이 프로퍼티는 이벤트의 타겟을 참조합니다.
 + `event.target` 은 클릭한 paragraph element 에 바로 접근하게 해줍니다. 바로 element 에 접근할 수 있기에 textContent 에 접근할 수 있고, 그것의 스타일을 수정하거나 클래스를 업데이트하거나 등등을 할 수 있습니다.
```javascript
const myCustomDiv = document.createElement('div');
//이벤트 리스너 함수에 타겟 element 의 텍스트를 출력합니다.
function respondToTheClick(evt) {
    console.log('A paragraph was clicked: ' + evt.target.textContent);
}

for (let i = 1; i <= 200; i++) {
    const newElement = document.createElement('p');
    newElement.textContent = 'This is paragraph number ' + i;

    myCustomDiv.appendChild(newElement);
}

document.body.appendChild(myCustomDiv);

myCustomDiv.addEventListener('click', respondToTheClick);
```

### 이벤트 위임에 Node 타입 체크
```HTML
<body>
  <article id="content">
  <p>Brownie lollipop <span>carrot cake</span> 
 gummies lemon drops sweet roll dessert tiramisu. Pudding muffin
 <span>cotton candy</span> croissant fruitcake tootsie roll. Jelly jujubes brownie.
 Marshmallow jujubes topping sugar plum jelly jujubes chocolate.</p>
</article>
<script>
 //기존의 방식으로는 전부 다 적용됩니다.
 document.querySelector('#content').addEventListener('click', function (evt) {
    console.log('A span was clicked with text ' + evt.target.textContent);
});
 //특정 Node 에만 이벤트를 적용시킵니다. span 을 클릭한다면 실행, 아니면 실행하지 않습니다.
 document.querySelector('#content').addEventListner('click', function (evt) {
  if (evt.target.nodeName === 'SPAN') {
   console.log('a span was clicked with text ' + evt.target.textContent);
  }
 });
</script>
</body>
```
- 모든 element 는 Node 인터페이스로브터 프로퍼티를 상속하기에 Node 인터페이스 중 하나인 `.nodeName` 도 상속합니다.
 + 이 프로퍼티를 사용해서 타겟 element 가 필요한 그 element 인지를 검증합니다.
 + `.nodeName` 프로퍼티는 대문자 문자열로 return 합니다. 소문자로 작성해서는 검증할 수 없습니다.

## DOM 에서의 자바스크립트 위치 설정
```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script>
    document.querySelector('footer').style.backgroundColor = 'green';
  </script>
</head>
<body>
  <footer>aaa</footer>
  <!-- script 를 여기로 옮긴다면 스타일이 적용됩니다. 
  <script>
    document.querySelector('footer').style.backgroundColor = 'green';
  </script>
  -->
</body>
</html>
```
- head 위에서 선언하면 footer 의 스타일은 바뀌지 않습니다. head 쪽에서 만든 DOM 에는 아직 footer element 가 없기 때문에 querySelector() 에서 오류가 발생합니다. 그래서 DOM element 가 아닌 null 을 return 합니다.
 + 하단부에서 자바스크립트 코드를 삽입하면 모든 DOM elements 가 존재하게 됩니다.
### DOMContentLoaded 이벤트
- DOM 을 완전히 불러온다면 `DOMContentLoaded` 이벤트를 시작합니다.
```javascript
document.addEventListner('DOMContentLoaded', function () {});
```
- `DOMContentLoaded` 이벤트를 사용한다면 head 쪽에서 자바스크립트 코드를 짜더라도 아무런 문제가 없게 됩니다.
```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script>
    document.addEventListner('DOMContentLoaded', function () {
     document.querySelector('footer').style.backgroundColor = 'green';
    });
  </script>
</head>
<body>
  <footer>aaa</footer>
</body>
</html>
```
 + 이벤트 리스너를 `DOMContentLoaded` 이벤트로 설정하면 스타일링 코드가 바로 실행되지 않고, DOM 이 구성이 된 다음에야 코드가 실행되도록 만들어줍니다.
- 과거 개발자들은 `load` 이벤트를 사용해왔습니다.(예 : document.onload()) 일반적으로는 최근 등장한 `DOMContentLoaded` 이벤트를 쓰는게 나은 선택입니다.
 + 그리고 닫는 body 태그 바로 위에서 코드를 작성하는 것이 더 좋습니다.
- head 에서 작성한 자바스크립트 코드는 body 의 코드보다 빨리 실행됩니다. 따라서 가능한 빨리 실행해야 하는 자바스크립트 코드가 있다면 head 에서 `DOMContentLoaded` 이벤트를 사용하면 됩니다.
