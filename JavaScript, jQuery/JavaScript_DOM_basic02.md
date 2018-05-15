# 자바스크립트 DOM 개념 02 (Udacity)
## 페이지 콘텐츠 업데이트
### innerHTML 프로퍼티
- 모든 element 는 Element 인터페이스로부터 프로퍼티와 메소드를 상속받습니다.
  + 즉 모든 element 는 'innerHTML' 프로퍼티를 가지고 있습니다.
- '.innerHTML' : 선택한 element 의 HTML 콘텐츠를 return 하거나, HTML 콘텐츠의 element 를 설정합니다. '.innerHTML' 은 'DOMString' 이라 불리는 문자열 형태로 return 합니다.

### textContent 프로퍼티
- '.textContent' : element 와 모든 자손들의 텍스트 내용을 설정합니다. 또는 element 와 모든 자손들의 텍스트 내용을 return 합니다.
  + '.innerHTML' 은 element 를 모두 가져오지만 '.textContent' 는 텍스트만을 출력합니다.
```javascript
const nanodegreeCard = document.querySelector('.card');
//텍스트 내용을 설정하기..nanodegreeCard 의 텍스트가 바뀝니다.
nanodegreeCard.textContent = "I will be the updated text for the nanodegreeCard element!";
```
- '.textContent' 는 '.innerHTML' 과 다르게 HTML 문자를 포함해도 그대로 출력만 합니다. 모든 것들을 문자로만 출력합니다.
```javascript
//문자 그대로 출력합니다.
nanodegreeCard.textContent = 'The <strong>Greatest</strong> Ice Cream Flavors';
//strong 태그가 적용되어 출력됩니다.
nanodegreeCard.innerHTML = 'The <strong>Greatest</strong> Ice Cream Flavors';
```

### innerText 프로퍼티
- '.innerText' 는 텍스트 형태로 출력하지만 CSS 를 적용시켜 페이지에 보이는 상태의 element 텍스트를 출력합니다.
  + '.textContent' 가 CSS 를 무시하고 순수한 텍스트 형태로 출력하는 것과 다릅니다.

## 페이지 콘텐츠 새로 추가하기
### createElement 메소드
- '.createElement()' 메소드는 document 객체의 메소드입니다. 괄호 안에 넣은 글자로 element 를 만듭니다.
```javascript
// creates and returns a <span> element
const span = document.createElement('span');
span.textContent = "a"; //콘솔 : <span>a</span>
```
- '.createElement()' 메소드는 단지 element 를 만들기만 할 뿐 DOM 에 그 element 가 추가되지는 않습니다. 이것을 DOM 에 추가하려면 다른 메소드가 필요합니다.

### appendChild 메소드
- '.appendChild()' 메소드로 element 를 페이지에 추가할 수 있습니다. (append : 문장의 끝에 뭔가를 추가한다) 끝부분에 추가하는 점이 특징입니다.
```javascript
const newSpan = document.createElement('span');
//페이지에서 h1 의 첫 element 를 선택합니다.
const mainHeader = document.querySelector('h1');
//새로 만든 span element 를 선택한 h1 의 끝부분에 삽입합니다.
mainHeader.appendChild(newSpan);
```
  + span 은 h1 의 자식으로써 DOM 에 나타납니다. 다만 h1 이 끝난 후에 나타날 것입니다.
- 참고로 'document.appendChild()' 는 에러가 뜹니다. 반드시 존재하는 element 를 통해서 불러야 합니다.

### createTextNode 메소드
- '.createElement()' 처럼 '.createTextNode()' 메소드로 새로운 텍스트 nodes 를 만들 수 있습니다.
```javascript
const myPara = document.createElement('p');
const textOfPara = document.createTextNode('I am text of para');
//paragraph 에 텍스트 노드를 끝부분에 추가합니다.
myPara.appendChild(textOfPara);
//paragraph 를 태그의 끝부분에 추가합니다.
document.body.appendChild(myPara);

//element 의 텍스트를 textContent 프로퍼티로 업데이트하기
const myPara = document.createElement('p');
myPara.textContent = 'I am the text for the paragraph!';
document.body.appendChild(myPara);
```
  + 방법 1과 방법 2는 같은 결과가 나오지만 '.textContent()' 를 쓴 두번째 방법이 더 간단하고 빠릅니다. 그래서 'textContent()' 가 'createTextNode' 메소드보다 자주 쓰입니다.

### insertAdjacentHTML 메소드
- 'appendChild()' 메소드는 last child 로써 부모 element 의 끝에만 자식 element 를 넣을 뿐 넣을 위치를 조절할 수는 없습니다.
- '.insertAdjacentHTML()' 메소드는 두 개의 인수를 사용합니다. HTML 의 위치와 삽입될 텍스트입니다.
  + HTML 의 위치
    1. 'beforebegin' :  before the element itself. inserts the HTML text as a previous sibling
    2. 'afterbegin' : Just inside the element, before its first child. inserts the HTML text as the first child
    3. 'beforeend' : Just inside the element, after its last child. inserts the HTML text as the last child
    4. 'afterend' : after the element itself. inserts the HTML text as a following sibling
  + 두 번째 인수 텍스트는 HTML 이 아닌 텍스트여야 하며 element 를 전달할 수 없습니다. (HTML 은 출력되지 않습니다.)
```HTML
<!-- beforebegin -->
<p>
    <!-- afterbegin -->
    Existing text/HTML content
    <!-- beforeend -->
</p>
<!-- afterend -->
```
```javascript
const mainHeader = document.querySelector('#main-heading');
const htmlTextToAdd = '<h2>ocean</h2>';

mainHeader.insertAdjacentHTML('afterend', htmlTextToAdd);
```

## 페이지 콘텐츠 삭제하기
### 자식 element 삭제하기
- '.removeChild()' 메소드는 '.appendChild()' 메소드의 반대 개념입니다. 부모 element 와 삭제할 자식 element 를 알고 있어야 합니다.
```javascript
<parent-element>.removeChild(<child-to-remove>);
```
- '.removeChild()' 메소드는 부모를 알아야 한다는 단점이 있지만 해결안이 있습니다.
  + 모든 element 는 부모 element 를 참조하는 'parentElement' 프로퍼티를 가지고 있습니다. 그래서 추가하거나 제거하려는 자식 element 에 접근할 수 있다면 'parentElement' 프로퍼티를 사용해서 element의 부모에게로 포커스를 이동할 수 있습니다. 그 다음 '.removeChild()' 나 '.appendChild()' 를 호출하면 됩니다.
```javascript
//첫 번째 h1 태그를 찾아 mainHeader 변수에 넣습니다.
const mainHeader = document.querySelector('h1');
//parentElement 를 호출해서 부모 element 에 포커스를 둡니다. 그 후 mainHeader 자체는 부모에서 제거하는 element 로 전달됩니다.
mainHeader.parentElement.removeChild(mainHeader);
```
- 'fistChild' 프로퍼티는 기본 HTML 소스 코드를 유지하기 위한 whitespace를 반환하는 것입니다. 반면 'firstElementChild' 프로퍼티는 항상 첫 번째 element 를 return 합니다.
### 자식 element 삭제하기 02
- 'remove()' 메소드를 통해 곧바로 자식 element 로 이동할 수 있습니다.
```javascript
const mainHeader = document.querySelector('h1');
mainHeader.remove();
```

## 스타일 페이지 콘텐츠
- 'CSS specificity' : CSS 특이성은 브라우저가 어떤 CSS 프로퍼티 값이 element 와 가장 관련이 있는지를 결정하는 수단입니다. 여러 종류의 CSS 선택자로 구성된 규칙을 기반으로 합니다.

### style 프로퍼티로 스타일 변경
- element 의 스타일을 지정할 때 '.style' 프로퍼티를 통해서 해당 element 의 스타일 attribute 에 접근합니다. 그리고 element 의 스타일을 변경합니다.
```javascript
const mainHeader = document.querySelector('h1');
//'.style' 프로퍼티를 통해 element 의 스타일 attribute 에 접근했습니다.
mainHeader.style.color = 'blue';
```
- 참고로 프로퍼티를 작성할 때 stylesheet 대로 작성하지 않고 캐멀표기법으로 작성합니다. (font-color -> fontColor)

### style.cssText 프로퍼티로 다수의 스타일 변경
- '.style.property' 구문은 스타일을 한 가지만 변경할 수 있습니다. 다양한 CSS 스타일을 바꾸려면 '.style.cssText' 프로퍼티를 사용하면 됩니다.
```javascript
const mainHeader = document.querySelector('h1');

mainHeader.style.cssText = 'color: blue; background-color: orange; font-size: 3.5em';
```
- '.style.cssText' 프로퍼티는 캐멀표기법을 쓰지 않고 stylesheet 대로 CSS 스타일을 작성합니다. (fontColor -> font-color)
- 만약 스타일 attribute 가 있는 상태에서 '.style.cssText' 프로퍼티를 작성한다면 기존의 스타일을 덮어씌우고 새로 작성한 것으로 바뀝니다.
```javascript
<p id="main" style="color: blue;">main</p>
//기존의 스타일이던 blue 는 삭제됩니다.
document.querySelector('#main').style.cssText = 'width: 30px; font-size: 10em';
```

### setAttribute 메소드로 element 의 attribute 를 설정
- 'setAttribute()' 메소드는 스타일을 위한 것이 아니라 element에 대한 attribute 를 설정하는 용도로 쓰입니다.
```javascript
const mainHeader = document.querySelector('h1');
//id 를 heading-sibling element에 추가합니다.
mainHeader.nextElementSibling.setAttribute('id', 'heading-sibling');
//새로 추가된 id 를 이용해 그 element 에 접근합니다.
document.querySelector('#heading-sibling').style.backgoundColor = 'blue';
```

### className 프로퍼티로 element 의 class 에 접근
- '.className' 프로퍼티는 element 의 클래스들을 문자열로 return 합니다.
```javascript
<h1 id="main-header" class="ank-student jpk-modal">learn</h1>

const mainHeader = document.querySelector('#main-header');
//클래스 목록들을 변수에 저장합니다.
const listOfClasses = mainHeader.className;
//결과: ank-student jpk-modal
console.log(listOfClasses);
```
- '.className' 프로퍼티는 클래스들을 띄어쓰기로 구분하지만 자바스크립트의 '.split()' 메소드를 통해 배열 형태로 만들 수 있습니다.
```javascript
const arrayOfClasses = linstOfClasses.split(' ');
//결과 : ["ank-student", "jpk-modal"]
console.log(arrayOfClasses);
```
- 배열로 출력했으므로 클래스들을 for 문으로 반복하고, 'push()' 로 더하거나 혹은 'pop()' 으로 삭제할 수 있습니다.
- '.className' 프로퍼티는 문자열로 return 하기에 클래스를 추가하거나 제거하기 어렵습니다. 그럴떄는 '.classList' 프로퍼티를 써야 합니다.

### classList 프로퍼티
```javascript
<h1 id="main-header" class="ank-student jpk-modal">learn</h1>

const mainHeader = document.querySelector('#main-header');

const listOfClasses = mainHeader.classList;
//결과: ["ank-student", "jpk-modal"]
console.log(listOfClasses);
```
- '.classList' 프로퍼티는 클래스 attribute 의 리스트를 DOMTokenList 형태로 return 합니다.
- '.classList' 프로퍼티는 몇 가지 프로퍼티를 가지고 있습니다.
  + '.add()' : 리스트에 클래스를 추가합니다.
  + '.remove()' : 리스트에 있는 특정 클래스를 삭제합니다.
  + '.toggle()' : 클래스가 리스트에 없으면 추가하고, 이미 있으면 삭제합니다.
  + 'contains()' : 클래스의 유무를 불리언 형태로 return 합니다.
```javascript
mainHeader.classList.contains('aaa');
mainHeader.classList.add('bbb');
mainHeader.classList.remove('bbb');
mainHeader.classList.toggle('aaa'); // 클래스 aaa 를 삭제합니다.
mainHeader.classList.toggle('aaa'); // 클래스 aaa 를 추가합니다.
```
- '.classList' 프로퍼티는 '.className' 보다 자주 사용됩니다.
