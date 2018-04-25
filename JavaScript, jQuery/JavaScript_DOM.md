#자바스크립트 문서 객체 모델
- HTML에서 존재하는 html이나 body 등의 태그를 자바스크립트에서 사용할 수 있는 객체로 만드는 것이 '문서 객체'입니다.
- HTML의 각 요소를 '노드'라고 부릅니다.
  + '요소 노드(Element Node)' : HTML 태그
  + '텍스트 노드(Text Node)' : 요소 노드 안에 들어 있는 글자

## 문서 객체 만들기

### 방법 1
```javascript
<script>
  window.onload = function () {
    //변수 선언
    let header = document.createElement('h1');
    let textNode = document.createElement('hello DOM');

    //노드 연결
    header.appendChild(textNode);
    document.body.appendChild(header);
  };
</script>
```
- 출력하면 텍스트 노드를 갖는 h1 태그를 생성하면서 문서 객체가 만들어집니다.
- 'appendChild(node)' : 객체에 노드를 연결하는 메소드입니다.

### 방법 2
```javascript
<script>
  window.onload = function () {
    //변수 선언
    let img = document.createElement('img');
    img.setAttribute('src', 'bird.jpg');
    img.setAttribute('width', 500);
    img.setAttribute('height', 350);

    //setAttribute() 메소드를 사용하지 않으면 이 방법을 쓸 수 없습니다.
    img.setAttribute('data-property', 350);

    //노드 연결
    document.body.appendChild(img);
  };
</script>
```
- img 태그에 이미지를 넣기 위해 src 속성을 지정했습니다.
- 이 방법은 웹 표준에서 정의하거나 웹 브라우저가 지원하는 태그의 속성에서만 사용 가능합니다.
- 웹 브라우저가 지원하지 않는 속성은 아래 메소드를 사용해야 속성을 넣을 수 있습니다.
- 'setAttribute(name, value)' : 객체의 속성을 지정합니다.
- 'getAttribute(name)' : 객체의 속성을 가져옵니다.

### 방법 3
```javascript
<script>
  window.onload = function () {
    //변수 선언
    let output = '';
    output += '<ul>';
    output += ' <li>JavaScript</li>';
    output += ' <li>jQuery</li>';
    output += ' <li>Ajax</li>';
    output += '</ul>';

    //innerHTML 속성에 문자열을 할당합니다.
    document.body.innerHTML = output;
  };
</script>
```
-  'innerHTML' : 태그의 내부를 의미하는 속성입니다. (<h1>~~~</h1>)
- ```javascript
  document.body.innerHTML += '<h1>DOCUMENT OBJECT MODEL</h1>';
  ```
  이런 방식으로 문자열을 추가할 수도 있습니다.
- 익스플로러를 제외한 웹 브라우저는 모든 문서 객체의 innerHTML 속성을 바꿀 수 있지만 익스플로러는 colgroupm frameset, head, html, style, table, tbody, tfoot, thead, title, tr 태그의 innerHTML 속성을 바꿀 수 없습니다.

## 문서 객체 가져오기

### 방법 1
- 'getElementById(id)' : 태그의 id 속성이 id 매개변수와 일치하는 문서 객체를 가져옵니다. 한 번에 하나의 문서 객체만 가져올 수 있습니다.

### 방법 2
- 아래 메소드를 이용하면 한 번에 여러 개의 문서 객체를 가져올 수 있습니다.
  + 'getElementsByName(name)' : 태그의 name 속성이 name 매게변수와 일치하는 문서 객체를 배열로 가져옵니다.
  + 'getElementsByTagName(tagName)' : tagName 매개변수와 일치하는 문서 객체를 배열로 가져옵니다.
```javascript
<script>
  window.onload = function () {
    //문서 객체를 가져옴
    let headers = document.getElementsByTagName('h1');
    //문서 객체 배열 사용
    headers[0].innerHTML = 'with getElementsByTagName()';
    headers[1].innerHTML = 'with getElementsByTagName()';
  };
</script>

<body>
  <h1>header</h1>
  <h1>header</h1>
</body>
```
  + 변수 'headers' 는 문서 객체를 갖는 배열입니다.
- 위의 headers 변수는 배열이니 반복문을 쓸 수 있습니다. (다만 for in 반복문은 쓸 수 없습니다. for in은 문서 객체 이외의 속성에도 접근하기 때문입니다.)
```javascript
<script>
  window.onload = function () {
    //문서 객체를 가져옴
    let headers = document.getElementsByTagName('h1');
    for (var i = 0; i < headers.length; i++) {
      //문서 객체의 속성을 변경
      headers[i].innerHTML = 'with getElementsByTagName()';
    }
  };
</script>
```

### 방법 3
- CSS 선택자로 문서 객체를 선택하는 메소드입니다.
  + 'querySelectior(선택자)' : 선택자로 가장 처음 선택되는 문서 객체를 가져옵니다.
  + 'querySelectiorAll(선택자)' : 선택자를 통해 선택되는 문서 객체를 배열로 가져옵니다.

## 문서 객체의 스타일 조작
- 문서 객체의 style 속성을 이용하면 해당 문서 객체의 스타일을 변경할 수 있습니다.
```javascript
<script>
  // 문서 객체 가져옴
  let header = document.getElementById('header');
  //문서 객체의 스타일 변경
  header.style.border = '2px solid black';
</script>
```
## 문서 객체 제거
- removeChild(child) : 문서 객체의 자식 노드를 제거합니다.
```javascript
<script>
  window.onload = function () {
    //문서 객체 가져옴
    let willRemove = document.getElementById('will-remove');

    //문서 객체 제거
    document.body.removeChild(willRemove);
  };
</script>

<body>
  <h1 id="will-remove">header</h1>
</body>
```
