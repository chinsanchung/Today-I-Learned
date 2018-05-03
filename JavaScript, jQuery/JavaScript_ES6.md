# JavaScript ES6
## const
- 'const' : 상수 constant는 값을 변경할 수 없는 상수입니다. 변수는 같은 이름으로 중복하면 뒤의 것을 덮어서 쓰지만 상수는 바꿀 수 없습니다.

## let
- 'let' 키워드를 사용해 변수 영역을 코드 블록 안으로 한정시킬 수 있습니다.
```javascript
var div, container = document.getElementById('container')
for (let i = 0; i < 5; i++) {
  div = document.createElement('div');
  div.onclick = function () {
    alert('this is : ' + i + 'container');
  };
  container.appendChild(div);
};
```
  + i를 let으로 선언해 i의 영역을 블록으로 제한합니다.

## 템플릿 문자열
- 문자열 연결 대신 템플릿 문자열을 사용할 수 있습니다.
  + 문자열 중간에 변수를 삽입할 수도 있습니다.
```javascript
//일반적인 문자열 연결
console.log(lastNmae + ", " + firstName + " " + middleName);
//템플릿 문자열
console.log('${lastNmae}, ${firstName}, ${middleName}');
```
  + 템플릿 문자열의 ${} 에는 값을 만들어 내는 자바스크립트 식이라면 어느 것이든 들어갑니다.
- 템플릿 문자열은 공백을 유지합니다. 아래처럼 코드가 깨질 염려 없이 여러 줄로 된 문자열을 만들 수 있습니다.
```javascript
${firstName},
Thanks to buy ${qty} ${event} tickets
order detail :
  ${lastNmae} ${firstName} ${middleName}
  ${qty} * $${price} = $${qty*price} play : ${event}
From ${ticketAgent}
```
  + 문자열 안의 공백 문자를 + 없이 처리하기 떄문에 정렬된 HTML을 코드에 넣을 수 있습니다. (자바스크립트는 HTML 문자열을 모든 문자열을 +로 연결해 한 줄로 처리해야 했습니다.)
```
document.body.innerHTML = '
  <section>
    <header>
      <h1>The HTML5 Blog</h1>
    </header>
    <article>
      <h2>${article.title}</h2>
    </article>
    <footer>
      <p>copyright ${new Date().getYear()} | The HTML5 Blog</p>
    </footer>
  </section>'
```

## 디폴트 파라미터
- ES6에서 파이썬처럼 디폴트 파라미터를 사용하게 되었습니다. 함수를 호출하면서 값을 지정하지 않으면 디폴트 값이 적용됩니다.
```javascript
function logActivity(name = "kim", activity = "tennis") {
  console.log( '${name} likes ${activity}');
};
```
- 문자열 뿐만 아니라 어떤 값도 디폴트 값으로 사용가능합니다.
```javascript
var defaultPerson = {
  name : { fisrt : "min" last : "kim"}, favActivity : "tennis"
};
function logActivity (p = defaultPerson) {
  console.log('${p.name.first} likes ${p.favActivity}');
}
```

## 화살표 함수
- 화살표 함수로 function 키워드 없이 함수를 만들고 return을 쓰지 않아도 식을 계산한 값을 자동으로 반환합니다.
```javascript
//일반적인 함수
let lordify = function (firstname) {
  return '${firstname} of korea'
}
console.log( lordify("kimmin"));


//파라미터가 두개 이상일 떄
let lordify = firstname => '${firstname} of korea';
let lordify = function (firstName, land) {
  return '${firstName} of ${land}'
}
//화살표 함수 2(괄호가 필요합니다)
let lordify = (firstName, land) => '${firstname} of ${land}';


//결과 계산에 여러 줄이 필요할 떄
let lordify = function (firstName, land) {
  if (!firstName) {
    throw new Error('lordify에 이름을 넘겨야함');
  }
  if (!land) {
    throw new Error('영주는 영지가 필요함');
  }
  return '${firstName} of ${land}'
}
//화살표 함수 3(함수 본문 전체를 중괄호로 둘러싸야 합니다)
let lordify = (firstName, land) => {
  if (!firstName) {
    throw new Error('lordify에 이름을 넘겨야함');
  }
  if (!land) {
    throw new Error('영주는 영지가 필요함');
  }
  return '${firstName} of ${land}'
}
```
- 화살표 함수는 this를 새로 바인딩(묶음)하지 않습니다.
```javascript
//기본 함수
let gangwon = {
  resorts : ["용평", "평창", "강촌", "강릉", "홍천"],
  print : function (delay = 1000) {
    setTimeout (function () {
      //join으로 리조트 이름을 콤마로 연결합니다
      console.log(this.resorts.join(", "));
    }, delay);
  }
};
gangwon.print();
```
  + 출력 시 오류가 발생합니다. this.resorts의 join메소드를 호출하려 했기 때문입니다.(this가 window 객체라 resorts가 undefined 상태입니다.)
  + 화살표 함수를 사용하면 this의 영역이 제대로 유지됩니다. (resorts 배열 5개가 출력됩니다.)
```javascript
let gangwon = {
  resorts : ["용평", "평창", "강촌", "강릉", "홍천"],
  print : function (delay = 1000) {
      setTimeout(() => {
        console.log(this.resorts.join(", "));
      }, delay);
  }
};
gangwon.print();
```

## ES6 트랜스파일링
- 모든 웹 브라우저에서 ES6을 지원하지는 않습니다. 하지만 브라우저에서 ES6 코드를 실행하기 전에 ES5로 컴파일하면 그 브라우저에서도 ES6 코드가 실행됩니다. 이를 '트랜스파일링' 이라 합니다.
```javascript
//이전 코드
const add = (x = 5, y = 10) => console.log(x + y);

//트랜스파일링
  //use strict 선언으로 코드가 엄격한 모드에서 실행되도록 만듭니다
"use strict";
var add = function add() {
  var x = arguments.length <= 0 || arguments[0] === undefined ?
      5 : arguments[0];
  var y = arguments.length <= 1 || arguments[1] === undefined ?
      10 : arguments[1];
  return console.log(x + y);
}
```
- 인라인 바벨 트랜스파일러로 브라우저에서 자바스크립트를 직접 트랜스파일링 할 수 있습니다.
```javascript
<div id="output"></div>
<!-- 바벨 로딩 -->
<script src="http://unpkg.com/babel-standalone@6/babel.min.js"></script>
<!-- 변환할 코드를 script 태그 안에 넣기 -->
const getMessage = () => "hello";
document.getElementById('output').innerHTML = getMessage();
</script>
<!-- 파일에 있는 소스 코드를 트랜스파일링하기 -->
<script src="script.js" type="text/babel" />
```
