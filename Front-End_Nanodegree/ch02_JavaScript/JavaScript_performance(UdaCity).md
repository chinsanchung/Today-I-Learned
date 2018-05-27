# 자바스크립트 퍼포먼스 (UdaCity)
## 효율적으로 페이지 콘텐츠 추가하기
- 아래의 코드를 바꿔봅니다.
```javascript
for (let i = 1; i <= 200; i++) {
 const newElement = document.createElement('p');
 newElement.textContent = 'paragraph num ' + i;
 
 document.body.appendChild(newElement);
}

//효율적으로 바꿔봅니다.
const myCustomDiv = document.querySelector('div');

for (let i = 1; i <= 200; i++) {
 const newElement = document.createElement('p');
 
 myCustomDiv.appendChild(newElement);
}

document.body.appendChild(myCustomDiv);
```
 + 이것만으로는 부족합니다. `performance.now()` 메소드를 사용해봅니다.

### performance.now 로 코드 퍼포먼스 테스트
- 코드를 실행하는데 얼마나 오래 걸리는지를 측정합니다. `performance.now()` 는 밀리초 단위의 timestamp 를 return 합니다.
- 이 메소드가 코드를 측정하는 몇 가지 단계가 있습니다.
 + `performance.now()` 를 코드의 시삭 시간을 알기 위해 사용합니다.
 + 테스트할 코드를 실행합니다.
 + `performance.now()` 를 실행해서 다른 시간을 측정합니다.
 + 처음 시간과 마지막 시간을 뺍니다.
```javascript
const startingTime = performance.now();

for (let i = 1; i <= 10; i++) { 
  for (let j = 1; j <= 10; j++) {
    console.log('i and j are ', i, j);
  }
}

const endingTime = performance.now();
console.log('This code took ' + (endingTime - startingTime) + ' milliseconds.');
```

### document Fragment 사용하기
- 브라우저는 계속해서 스크린과 DOM 을 매치시키려고 일합니다. 새로운 element 를 추가하면, 브라우저는 `reflow` 계산(새로운 스크린 레이아웃을 결정)을 수행하고 그 후에 스크린을 `repaint` 합니다. 이것은 시간이 걸리는 일입니다.
 + 만약 위에처럼 새로운 p 태그를 일일이 만든다면 새로운 paragraph 마다 reflow 와 repaint 를 반복하게 되어 비효율적입니다.
 + 필요한 것은 브라우저가 reflow, repaint 작업을 한 번만 수행하면서 DOM 에 필요없는 element 를 추가하지 않는 것입니다.
- `DocumentFragment` : 부모가 없는 작은 문서 객체를 나타냅니다. 표준 문서처럼 node 로 구성된 문서 구조에 대한 segment 를 저장하는 경량 버젼의 문서로 사용됩니다.
 + 이것은 DOM 트리의 일부가 아닙니다. 따라서 `DocumentFragment` 의 변경 사항은 문서에 영향을 주지 않으면서 reflow 를 발생시키거나 퍼포먼스에 영향을 줍니다.
 + `DocumentFragment` 을 만드는 변화는 off 스크린에서 생깁니다. 즉 만드는 동안 reflow 와 repaint 가 없습니다.
- `.createDocumentFragment()` 메소드를 사용해 빈 DocumentFragment 객체를 만들어 사용합니다.
```javascript
//div 태그 대신 DocumentFragment 를 사용했습니다.
const fragment = document.createDocumentFragment();

for (let i = 0; i < 10; i++) {
  const newElement = document.createElement('p');
  newElement.innerText = 'This is paragraph num ' + i;
  
  fragment.appendChild(newElement);
}
//reflow 와 repaint 는 여기서만 단 한번 발생합니다. 
documen.body.appendChild(fragment);
```

## reflow 와 repaint
- `reflow` : 브라우저가 페이지를 레이아웃(배치) 하는 프로세스입니다. 처음에 DOM 을 표시할 때나 레이아웃이 바뀔 때마다 다시 수행됩니다. 상당히 느린 프로세스입니다.
 + CSS 클래스를 추가할 때 reflow 가 발생합니다. 클래스를 추가하면 element 의 위치가 바뀌거나 float 가 발생됩니다.
- `repaint` : 브라우저가 새로운 레이아웃을 화면에 표시해서 `reflow` 가 수행된 후에 발생합니다. 매우 빠르지만 자주 발생하지 않는 편이 좋습니다.
- 변경할 사항이 있는 경우 숨기고, 모두 바꾸고, 다시 보여주는 것은 좋은 패턴입니다. (reflow 와 repaint 를 절약)

### 가상 DOM
-reflow 와 repaint 를 덜 쓰는 방법인 가상 DOM 라이브러리는 인기가 있습니다. 가상 DOM 을 바꾸면 라이브러니는 스크린과 일치하도록 업데이트하는 가장 좋은 방법을 계산합니다.
 + 어떤 라이브러리든 채택하기 위해 코드를 다시 작성해야 하며 때로는 직접 화면을 업데이트하는 작업을 더 잘 수행할 수 있다는 점입니다.

## Call Stack
### 싱글 스레딩
- `싱글 스레드` : 한 번에 하나의 명령을 처리합니다.
```javascript
function addParagraph() {
  const para = document.createElement('p');
  para.textContent = 'add paragraph';
  document.body.appendChild(para);
}

function appendNewMessage() {
  const para = document.createElement('p');
  para.textContent = 'append message';
  document.body.appendChild(para);
}

addParagraph();
appendNewMessage();
```
 + 순서 : 
  1. addParagraph() 함수를 선언합니다 
  2. appendNewMessage() 함수를 선언합니다.
  3. addParagraph() 가 호출됩니다. (excution 이 함수 내부로 이동하고 세 문장을 순서대로 실행 -> 함수가 끝나면 execution 은 호출된 자리로 retunr 됩니다.)
  4. appendNewMessage() 가 호출됩니다. (excution 이 함수 내부로 이동하고 세 문장을 순서대로 실행 -> 함수가 끝나면 execution 은 호출된 자리로 retunr 됩니다.)
  5. 모든 코드를 excute (실행)했기 때문에 종료됩니다.
 + 자바스크립트는 순서대로 함수 내부의 코드를 모두 하나씩 처리합니다. 다양한 라인과 함수를 동시에 실행할 수 없습니다. 싱글 스레딩 언어입니다.
 + 함수가 호출되고 나서 excution 이 호출했던 자리로 다시 return 할 수 있던 이유는 `Call Stack` 덕분입니다.

### Call Stack
- 함수를 호출하면 처음에 그 함수가 Call Stack 에 저장되고 그 함수의 모든 코드를 실행하고 끝나면 Call Stack 에서 삭제됩니다.
```javascript
function addParagraph() {
  const para = document.createElement('p');
  para.textContent = 'add paragraph';
  appendNewMessage();
  document.body.appendChild(para);
}

function appendNewMessage() {
  const para = document.createElement('p');
  para.textContent = 'append message';
  document.body.appendChild(para);
}
//1. <main> 이 Call Stack 에 저장됩니다.
addParagraph();
```
- 순서 :
 1. 파일을 불러오면서 <main> 이 Call Stack 에 저장됩니다.
 2. excution 이 내려가면서 함수 호출을 발견하고 그 함수가 Call Stack 에 저장됩니다. (addParagraph)
 3. excution 이 그 함수를 선언한 곳으로 이동하고 하위 코드들을 실행합니다.
 4. 그러다 appendNewMessage() 로 호출하는 것을 발견합니다. appendNewMessage 가 Call Stack 에 저장됩니다. (appendNewMessage)
 5. excution 이 appendNewMessage 함수로 이동, 코드들을 수행합니다.
 6. 코드들을 실행하고 그 함수가 끝나면 appendNewMessage 함수는 Call Stack 에서 사라집니다. (- appendNewMessage)
 7. 호출했던 자리로 다시 이동하고, addParagraph 함수를 계속해서 실행합니다.
 8. 모든 코드들을 실행해서 함수가 끝나면 그 함수는 Call Stack 에서 사라집니다. (- addParagraph)
 9. 파일 내 모든 코드를 수행하고 파일이 끝나면 <main> 이 Call Stack 에서 삭제됩니다. (- <main>)

## 이벤트 루프
- 코드들을 동시에 실행하면서 호출하면 곧바로 함수들은 Call Stack 에 저장되고, 그리고 완료되면 삭제됩니다 (synchronous). 하지만 그런 과정을 거치지 않는 코드가 있습니다. 
 + 특정 시점에 실행되는 코드입니다. 그것은 바로 이벤트 리스너입니다. 
- setTimeout 또는 .addEventListner() 같은 코드들을 `비동기 코드` 라고 부릅니다. 이들은 이벤트 루프 를 사용합니다.
- 이벤트 리스너의 함수는 즉시 호출되지 않고, 나중에 특정 시점에서 호출됩니다.
```javascript
console.log('howdy');
document.addEventListner('click', function numbers() {
  console.log('123');
});
console.log('green tea ice cream is tasty');
```
 + 순서:
  1. howdy 가 콘솔창에 출력됩니다.
  2. green tea ice cream is tasty 가 콘솔창에 출력됩니다.
 + 문자열 123 은 오직 클릭을 했을시에만 실행됩니다. 클릭하지 않으면 123 은 실행되지 않습니다.
- 이벤트 리스너의 함수는 어디서 기다리는지, 그리고 함수가 필요로 할 때 어떻게 실행되는지는 `이벤트 루프` 가 설명해줍니다.

### 자바스크립트 이벤트 루프
- `이벤트 루프` 는 이벤트를 선택하고, 핸들러로 실행한 후에 반복합니다. 이벤트 루프는 세 가지 파트가 있습니다. `the Call Stack`, `Web APIs/the browser`, `an Event Query` 입니다.
 + 모든 코드가 자바스크립트 코드인 것이 아니고, Web APIs 의 코드인 경우도 있습니다. (예 : .addEventListner(), setTimeout() )
```javascript
console.log('howdy'); // 첫번째
document.addEventListner('click', //두번째
  function numbers() {
    console.log('123');
});
console.log('green tea ice cream is tasty'); //세번째
```
 + 두번째 단계에서는 이벤트 핸들러 numbers 를 나중에 사용할 수 있도록 브라우저에 전달합니다. 브라우저는 클릭 이벤트가 있기 전까지 이 기능을 유지합니다.
 + 만약 코드 블록이 완료되기 전에 클릭할 경우 싱글 스레딩인 자바스크립트는 numbers 함수를 Queue 에 배치됩니다. Call Stack 의 기능이 완료되면 Queue 를 체크해서 거기에 있는 항목을 실행합니다. (Call Stack 에 함수를 저장하면서 실행합니다.)
 + 중요한 점은 현재의 코드들이 완료되어야하고, 브라우저가 사용중이 아닐 때 그 이벤트가 실행됩니다. 
 + 비동기식 코드(Asynchronous code) 는 이벤트 루프 외부에서 실행되고, 끝나면 이벤트를 전달합니다. (예: 이미지 로드)
- 순서를 다시 설명합니다.
 1. 먼저 Call Stack 에 document.addEventListner 가 저장됩니다. 
 2. 그리고 이벤트 리스너는 Call Stack 에서 브라우저로 이동합니다.
 3. 모든 코드가 끝난 후 클릭을 할 시 이벤트 핸들러인 nubers 함수는 Queue 로 이동해서 Call Stack 이 빌 때까지 기다립니다.
 4. 빈다면 numbers 함수는 Call Stack 으로 이동한 후 호출됩니다. 모두 실행하면 numbers 함수는 Call Stack 에서 삭제됩니다. (브라우저에는 이벤트 리스너가 그대로 남아있습니다.)

## setTimeout
### 나중에 코드 실행하기
- `.addEvnetListner()` 와 비슷하게 `setTimeout()` 함수도 필요한 시점에 코드를 실행합니다.
- `setTimeout()` 함수는 나중에 실행할 함수와 몇 밀리초나 기다릴지를 설정해야 합니다.
- `setTimeout()` 함수는 브라우저에서 제공하는 API이므로 호출시 나중에 실행할 함수는 브라우저에 전달됩니다.
```javascript
//hello 는 1000 밀리초(1초) 후에 콘솔에 출력됩니다.
setTimeout(function sayHello() {
  console.log('hello');
}, 1000);
```
 + `setTimeout()` 를 호출하면 sayHello 함수가 브라우저에 전달되며 타이머가 실행됩니다. -> 타이머가 끝나면 sayHello 함수는 Queue 로 이동합니다. -> call stack 이 비었으면 sayHello 함수는 call stack 으로 이동 후 실행됩니다.
- 만약 밀리초를 0 으로 설정한다면 sayHello 함수가 곧바로 실행될 것 같지만 아닙니다. sayHello 함수는 이벤트 루프를 통과합니다.
 + 함수는 브라우저로 전달되고 타이머가 바로 종료되기 때문에 call stack 이 현재 실행중인 작업을 끝내면 함수는 Queue 로 이동한 다음 call stack 으로 이동합니다.
 + 이 방법은 잠재적으로 오래 실행되는 코드(long-running) 를 분할된 코드로 바꾸는데 도움을 줍니다.

### long-running 코드를 분해하기
- 2, 3천 개가 넘는 element 를 추가하는 코드라면 적은 reflow 와 repaint 사용도 중요하지만 사용자 상호작용에 반응하는지 여부도 중요합니다.
- 자바스크립트가 실행되는 동안 페이지는 사용중이며 (클릭, 양식 작성 등의) 조작이 불가능합니다(상호작용이 안됩니다).
 + 이 작업은 끝날 때까지 실행되며 사용자가 수행한 작업에 응답하기 전에 수행됩니다. 이 기능은 이러한 element 를 모두 생성하고 완료될 때까지 call stack 에 페이지를 추가합니다.
 + 사용자가 페이지와 상호 작용하려면 그 내용을 조각조각내는 것입니다.
```javascript
let count = 1;
function generateParagraphs() {
  const fragment = document.createDocumentFragment();
  
  for (let i = 1; i <= 500; i++) {
    const newElement = document.createElement('p');
    newElement.textContent = 'This paragraph num ' + i;
    count = count + 1;
    
    fragment.appendChild(newElement);
  }
  
  document.body.appendChild(fragment);
  
  if (count < 20000) {
    setTimeout(generateParagraphs, 0);
  }
}

generateParagraphs();
```
 1. count 는 1 에서부터 시작합니다.
 2. generateParagraphs() 함수는 호출되면 500 개의 paragraph 를 페이지에 추가합니다.
 3. setTimeout() 함수는 generateParagraphs() 함수 마지막에서 호출됩니다. count 가 20000 보다 적으면 generateParagraphs() 함수를 호출합니다.
 + setTimeout() 함수를 호출했기 때문에 코드가 실행되는 동안 페이지와 계속 상호작용할 수 있습니다. 그리고 이 함수를 호출한다고 해서 페이지가 잠기거나 멈추지도 않습니다.
