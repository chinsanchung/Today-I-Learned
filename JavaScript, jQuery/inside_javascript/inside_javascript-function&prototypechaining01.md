---
title: 인사이드 자바스크립트 4장. 함수&프로토타입 체이닝 01
date: 2020-03-09 12:30:15
category: Book
draft: false
---

## Inside JavaScript 정리 4장. 함수와 프로토타입 체이닝

### 4.1 함수 정의

#### 4.1.1 함수 리터럴

자바스크립트에서는 함수도 일반 객체처럼 값으로 취급됩니다. 그래서 객체 리터럴처럼 **함수 리터럴**으로 함수를 생성할 수 있습니다.

```javascript
function add(x, y) {
  return x + y
}
```

- function 키워드로 함수 리터럴을 시작하고, 함수 이름을 적었습니다. 참고로 함수 이름은 생략 가능합니다.
- 매개변수의 타입을 적지 않는 것이 자바스크립트의 특징입니다.

#### 4.1.2 함수 선언문

주의점은 함수 선언문으로 정의된 함수는 **반드시 함수명이 정의되어 있어야**합니다.

```javascript
function add(x, y) {
  return x + y
}
console.log(add(3, 4)) // 7
```

#### 4.1.3 함수 표현식

함수 리터럴로 하나의 함수를 만든 후, 변수에 함수를 할당해서 생성하는 방법입니다.

```javascript
var add = function(x, y) {
  return x + y
}
var plus = add
console.log(add(3, 4)) // 7
console.log(add(5, 6)) // 11
```

- add 변수는 함수 리터럴로 생성한 함수를 참조하는 변수이지, 함수 이름이 아닙니다.
- add 는 함수 참조값을 가지므로, 다른 변수 plus 에도 값을 할당할 수 있습니다.
- 함수를 호출하려면 함수 변수를 사용해야 합니다.
- plus 도 add 처럼 같은 함수를 참조하므로, add 처럼 호출이 가능합니다.

- 함수 이름이 포함된 함수 표현식은 **기명 함수 표현식**이라 부릅니다.

```javascript
var add = function sum(x, y) {
  return x + y
}
```

- add() 함수 호출은 성공적으로 값을 리턴하지만, sum()은 에러를 발생시킵니다. 함수 표현식에서 사용된 함수 이름은 외부 코드에서 접근이 안됩니다.
- add() 함수가 되는 이유는 자바스크립트 엔진에 의해 함수 표현식 형태로 변경되기 때문입니다. 함수 이름과 변수의 이름이 add 로 같으므로, 함수 이름으로 함수가 호추로디는 것처럼 보이지만 실제로는 add 함수 변수로 외부 호출이 가능하게 된 것입니다.

```javascript
var add = function add(x, y) {
  return x + y
}
```

- 함수 표현식에서 함수 이름을 사용하면, 함수 코드 내부에서 함수 이름으로 재귀적인 호출 처리가 가능합니다.

```javascript
var factorialVar = function factorial(n) {
  if (n <= 1) return 1
  return n * factorial(n - 1)
}
console.log(factoralVar(3)) // 6
console.log(factorial(3)) // Uncaught ReferenceError: factorial is not defined
```

#### 4.1.4 Function() 생성자 함수로 함수 생성하기

- 자바스크립트의 함수는 **Function()**이라는 기본 내장 생성자 함수로 생성된 객체입니다. 함수 선언문이나 함수 표현식도 결국에는 Function() 생성자 함수로 함수를 생성합니다.
- Function() 생성자 함수를 사용한 함수 생성 방법은 일반적으로 자주 사용하진 않습니다.

#### 4.1.5 함수 호이스팅

- <<자바스크립트 핵심 가이드>>에서는 함수 호이스팅 때문에 함수 표현식만을 권장합니다.
- 함수를 정의하지 않았는데도 add() 함수를 호출할 수 있습니다. **함수 선언문 형태로 정의한 함수의 유효 범위는 코드의 맨 처음부터 시작한다**는 것이 함수 호이스팅입니다.

```javascript
add(2, 3); // 5
function add(x, y) {
  return x + y;
}
// 함수 표현식으로 정의하면 실행결과는 다릅니다.
delete(3, 2); // uncauth type error
var delete = function (x, y) {
  return x - y;
}
```

- 함수 호이스팅이 발생하는 원인은 자바스크립트의 **변수 생성**과 **초기화** 작업이 분리되어 실행되기 때문입니다.

### 4.2 함수 객체

#### 4.2.1 자바스크립트에서는 함수도 객체다

- 함수는 특정 기능의 코드를 수행하는 역할뿐만 아니라, 일반 객체처럼 프로퍼티를 가질 수 있는 특별한 객체입니다.

```javascript
// 함수 선언
function add(x, y) {
  return x + y
}
// add() 함수 객체에 result, status 프로퍼티 추가
add.result = add(3, 2)
add.status = 'ok'

console.log(add.result) // 5
console.log(add.status) // ok
```

- add() 함수 생성 시 함수 코드는 함수 객체의 `[[Code]] 내부 프로퍼티`에 자동으로 저장됩니다.

#### 4.2.2 자바스크립트에서 함수는 값으로 취급된다.

- 함수도 일반 객체처럼 취급될 수 있습니다. 따라서 아래의 동작이 가능합니다. 이러한 특징 때문에 자바스크립트에서는 함수를 **일급 객체**라고 부릅니다.
  - 리터럴에 의해 생성
  - 변수나 배열 요소, 객체 프로퍼티 등에 할당
  - 함수 인자로 전달 가능
  - 함수 리턴값으로 함수를 리턴 가능
  - 동적으로 프로퍼티를 생성 및 할당 가능
- 자바스크립트 함수를 제대로 이해하려면 함수가 일급 객체이며 이는 곧 함수가 일반 객체처럼 값으로 취급된다는 것을 이해해야 합니다.

##### 4.2.2.1 변수나 프로퍼티 값으로 할당

```javascript
var foo = 100 // 변수에 할당
var bar = function() {
  return 100
}
console.log(bar()) // 100

var obj = {} // 프로퍼티에 할당
obj.baz = function() {
  return 200
}
console.log(obj.baz()) // 200
```

##### 4.2.2.2 함수 인자로 전달

```javascript
// 함수 표현식으로 생성
var foo = function(func) {
  func()
}
// 함수 리터럴 방식으로 생성된 익명 함수를 func 인자로 넘겼습니다.
foo(function() {
  console.log(111)
}) // 111
```

##### 4.2.2.3 리턴값으로 활용

리턴값으로 함수를 쓸 수 있느 ㄴ이유는 함수 자체가 값으로 취급되기 때문입니다.

```javascript
var foo = function() {
  return function() {
    console.log('return')
  }
}
var bar = foo()
bar() // return
```

#### 4.2.3 함수 객체의 기본 프로퍼티

- 함수는 일반 객체와는 다르게 **함수 객체만의 표준 프로퍼티**를 가지고 있습니다.
  - ECMA5 명세서에는 모든 함수가 length, prototype 프로퍼티를 가져야 한다고 기술합니다.
  - name 프로퍼티는 함수의 이름을 나타냅니다.
  - caller 프로퍼티는 자신을 호출한 함수를 나타냅니다.
  - `__proto__`는 자바스크립트 객체의 `[[Prototype]]` 내부 프로퍼티와 같습니다. `__proto__`는 자신의 부모 역할을 하는 프로터타입 객체를 가리킵니다.
- 함수 객체의 부모 역할을 하는 프로토타입 객체를 **Function.prototype 객체**라 부릅니다. 이 객체는 모든 함수들의 부모 역할을 합니다. 아래의 프로퍼티를 가져야 합니다.
  - constructor 프로퍼티
  - toString() 메소드
  - apply(thisArg, argArray) 메소드
  - call(thisArg, [,arg1 [, arg2,]]) 메소드
  - bind(thisArg, [,arg1 [,arg2, ]]) 메소드

##### 4.2.3.1 length 프로퍼티

```javascript
function func0() {}
function func1(x) {
  return x
}
function func2(x, y) {
  return x + y
}
function func3(x, y, z) {
  return x + y + z
}
console.log(func0.length) // 0
console.log(func1.length) // 1
console.log(func2.length) // 2
console.log(func3.length) // 3
```

##### 4.2.3.2 prototype 프로퍼티

- 자바스크립트에서는 함수를 생성할 때, 함수 자신과 연결된 프로토타입 객체를 동시에 생성하며, 이 둘은 각각 prototype 과 constructor 라는 프로퍼티로 서로를 참조합니다. 함수 객체와 프로토타입 객체는 서로 밀접하게 연결되어 있습니다.

```javascript
function func() {
  return true
}
console.dir(func.prototype.constructor) // func() 함수를 가리킵니다.
console.dir(func.prototype)
```

- `func.prototype`는 constructor, proto 두 프로퍼티를 가집니다.

#### 4.3 함수의 다양한 형태

##### 4.3.1 콜백 함수

- 콜백 함수는 코드를 통해 명시적으로 호출하지 않고, 어떤 이벤트가 발생하거나 특정 시점에 도달했을 때 시스템에서 호출합니다.
- 대표적인 예가 이벤트 핸들러 처리입니다. DOM 이벤트(페이지 로드, 키보드 입력)이 발생하면 브라우저는 정의된 DOM 이벤트에 해당하는 이벤트 핸들러를 실행하는데, 여기에 콜백 함수를 등록해 실행할 수 있습니다.

```javascript
// 페이지 로드 시 콜백 호출
window.onload = function() {
  alert('callback')
}
```

##### 4.3.2 즉시 실행함수 IIFE

- 첫째로 최초 한 번의 실행만을 필요로 하는 초기화 코드에 사용합니다.

```javascript
;(function(name) {
  console.log(name)
})('foo') // foo
```

- 둘째로 jQuery 등의 프레임워크 소스에서 사용합니다.

```javascript
// jQuery 에서 사용된 즉시 실행 함수
;(function(window, undefined) {
  // ...
})(window)
```

- jQuery 에서 즉시 실행함수를 쓰는 이유는 변수 유효 범위라는 특성 때문입니다. 라이브러리 코드를 즉시 실행함수 내부에 정의해서 외부에서 접근하지 못하도록 만들 수 있습니다.
- 그러면 다른 자바스크립트 라이브러리들이 동시에 로드되더라도 라이브러리 간 변수 이름의 충돌을 방지할 수 있습니다.

##### 4.3.3 내부 함수

함수 내부에 정의된 함수를 내부 함수라 부릅니다. 클로저를 생성하거나 부모 함수 코드에서 외부 접근을 막고 독립적인 헬퍼 함수를 구현하는 등 다양하게 사용합니다.

```javascript
function parent() {
  var a = 100
  var b = 200
  function child() {
    var b = 300
    console.log(a)
    console.log(b)
  }
  child()
}
parent() // 100 300
child() // Uncaught ReferenceError: child is not defined;
```

- 내부 함수에서는 자신을 둘러싼 부모 함수의 변수에 접근할 수 있습니다. 이것이 가능한 이유는 자바스크립트의 **스코프 체이닝** 떄문입니다.
- 내부 함수는 일반적으로 자신이 정의된 부모 함수 내부에서만 호출이 가능합니다.

- 함수 외부에서도 특정 함수 스코프 안에 선언된 내부 함수를 호출할 수 있습니다.

```javascript
function parent() {
  var a = 100
  var child = function() {
    console.log(a)
  }
  return child
}
var inner = parent()
inner() // 100
```

- inner 변수는 리턴한 child() 내부 함수를 참조합니다.
- 이와 같이 실행이 끝난 parent() 와 같은 부모 함수 스코프의 변수를 참조하는 inner() 같은 함수를 **클로저**라 부릅니다.

##### 4.3.4 함수를 리턴하는 함수

- 자신을 재정의하는 함수

```javascript
var self = function() {
  console.log('a')
  return function() {
    console.log('b')
  }
}
self = self() // a
self() // b
```
