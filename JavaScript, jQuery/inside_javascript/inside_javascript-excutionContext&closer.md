---
title: 인사이드 자바스크립트 5장. 실행 컨텍스트&클로저
date: 2020-03-11 23:03:81
category: Book
draft: false
---

## 인사이드 자바스크립트 정리 5장. 실행 컨텍스트와 클로저

### 목차

- [5.1 실행 컨텍스트](#51-실행-컨텍스트)
- [5.2 실행 컨텍스트 생성 과정](#52-실행-컨텍스트-생성-과정)
  - [5.2.1 활성 객체 생성](#521-활성-객체-생성)
  - [5.2.2 arguments 객체 생성](#522-arguments-객체-생성)
  - [5.2.3 스코프 정보 생성](#523-스코프-정보-생성)
  - [5.2.4 변수 생성](#524-변수-생성)
  - [5.2.5 this 바인딩](#525-this-바인딩)
  - [5.2.6 코드 실행](#526-코드-실행)
- [5.3 스코프 체인](#53-스코프-체인)
  - [5.3.1 전역 실행 컨텍스트의 스코프 체인](#531-전역-실행-컨텍스트의-스코프-체인)
  - [5.3.2 함수를 호출한 경우 생성되는 실행 컨텍스트의 스코프 체인](#532-함수를-호출한-경우-생성되는-실행-컨텍스트의-스코프-체인)
- [5.4 클로저](#54-클로저)
  - [5.4.1 클로저의 개념](#541-클로저의-개념)
  - [5.4.2 클로저 활용](#542-클로저-활용)
  - [5.4.3 클로저의 주의사항](#543-클로저의-주의사항)

### 5.1 실행 컨텍스트

- 실행 컨텍스트는 **"실행 가능한 자바스크립트 코드 블록이 실행되는 환경"**이라고 부르고, 이 컨텍스트 안에 실행에 필요한 여러 정보를 담고 있습니다.
  - 실행 가능한 코드 블록은 전역 코드, eval() 함수 실행 코드, 그리고 함수 안의 코드를 실행할 경우입니다. 대부분의 코드 블록은 함수입니다.
  - **현재 실행되는 컨텍스트에서 이 컨텍스트와 관련 없는 실행 코드가 실행되면, 새로운 컨텍스트가 생성되어 스텍에 들어가고 제어권이 그 컨텍스트로 이동합니다.**
- 예제
  - 전역 컨텍스트 실행 -> 새로우 함수 호출로 새 컨텍스트 생성, 실행. 종료 후 반환 -> ... -> 전역 컨텍스트가 완료되면 모든 실행이 끝납니다.

```javascript
console.log('global context')
function ExContext1() {
  console.log('Excontext1')
}
function ExContext2() {
  ExContext1()
  console.log('ExContext2')
}
ExContext2() // global -> ExContext1 -> ExContext2
```

### 5.2 실행 컨텍스트 생성 과정

자바스크립트에서 함수를 실행해 실행 컨텍스트가 생성되면, 자바스크립트 엔진은 아래와 같은 일을 순서대로 진행합니다. (활성 객체, arguments 객체, 스코프 정보)

#### 5.2.1 활성 객체 생성

활성 객체: 실행 컨텍스트에서 실행에 필요한 여러 정보를 담을 객체를 자바스크립트 엔진이 생성합니다. 여기에 매개변수, 사용저 정의 변수 및 객체를 저장하고, 새로운 컨텍스트로 접근 가능하게 되어 있습니다.

#### 5.2.2 arguments 객체 생성

활성 객체는 arguments 프로퍼티로 이 arguments 객체를 참조합니다.

#### 5.2.3 스코프 정보 생성

- 현재 컨텍스트의 유효 범위를 나타내는 스코프 정보를 생성합니다. 스코프 정보는 실행 컨텍스트 안에서 연결 리스트와 유사한 형식인 스코프 체인을 생성합니다.
- 현재 생성된 활성 객체가 스코프 체인의 제일 앞에 추가되며, execute() 함수의 인자나 지역 변수 등에 접근할 수 있습니다.

#### 5.2.4 변수 생성

- 현재 실행 컨텍스트 내부에서 사용되는 지역 변수의 생성이 이뤄집니다.
- 변수 객체 안에서 호출된 함수 인자는 각각의 프로퍼티가 만들어지고 그 값이 할당됩니다. 값이 넘겨지지 않으면 undefined 가 할당됩니다.

#### 5.2.5 this 바인딩

마지막으로 this 키워드를 사용하는 값이 할당됩니다. this 가 참조하는 객체가 없으면 전역 객체를 참조합니다.

#### 5.2.6 코드 실행

- 참고로 전역 컨텍스트에서는 변수 객체가 곧 전역 객체입니다. 따라서 전역 선언된 함수와 변수가 전역 객체의 프로퍼티가 됩니다.
- 전역 실행 컨텍스트는 this 를 전역 객체의 참조로 사용합니다.

### 5.3 스코프 체인

- for, if 가 아닌, 오직 함수만이 유효 범위의 한 단위가 되고, 이 범위를 나타내는 스코프가 `[[scope]]`프로퍼티로 각 함수 객체 내에서 연결 리스트 형식으로 관리됩니다. 이를 스코프 체인이라 부릅니다.
- 각 함수는 **`[[scope]]`프로퍼티로 자신이 생성된 실행 컨텍스트의 스코프 체인을 참조합니다.**
- 함수가 실행되는 순간 실행 컨텍스트가 만들어지고, **이 실행 컨텍스트는 실행된 함수의 `[[scope]]`프로퍼티를 기반으로 새로운 스코프 체인을 만듭니다.**

#### 5.3.1 전역 실행 컨텍스트의 스코프 체인

- 변수 객체의 `[[scope]]`는 변수 객체 자신을 가리킵니다.

#### 5.3.2 함수를 호출한 경우 생성되는 실행 컨텍스트의 스코프 체인

- 스코프 체인 정리
  - 각 함수 객체는 `[[scope]]`프로퍼티로 현재 컨텍스트의 스코프 체인을 참조합니다.
  - 한 함수가 실행되면 새로운 실행 컨텍스트가 만들어지는데, 이 새로운 실행 컨텍스트는 자신이 사용할 스코프 체인을 다음과 같은 방법으로 만듭니다. 현재 실행되는 함수 객체의 `[[scope]]`프로퍼티를 복사하고, 새롭게 생성된 변수 객체를 해당 체인의 제일 앞에 추가합니다.
  - 요약하면, **스코프 체인 = 현재 실행 컨텍스트의 변수 객체 + 상위 컨텍스트의 스코프 체인**이라고 할 수 있습니다.
- 예제 1

```javascript
var value = 'value'
// 예제 01
function printFunc() {
  var value = 'value2'
  function printValue() {
    return value
  }
  console.log(printValue())
}
printFunc() // value2
```

- 예제 2
  - 함수 객체가 처음 생성될 때 `[[scoep]]`는 전역 객체의 `[[scope]]`를 참조합니다. 따라서 결과는 value 입니다.

```javascript
var value = 'value'
function printValue() {
  return value
}
function printFunc2(fun) {
  var value = 'value2'
  console.log(func())
}
printFunc2(printValue) // value
```

- 스코프 체인으로 식별자 인식이 이뤄집니다.
  - 식별자 인식: 스코프 체인의 첫 변수 객체에서부터, 식별자와 대응되는 이름을 가진 프로퍼티가 있는지 확인합니다. 발견하지 못하면 다음 객체로 이동합니다.
  - 식별자 인식에서의 this 는 식별자가 아닌 키워드로 분류되므로, 스코프 체인의 참조 없이 접근할 수 있습니다.
- 호이스팅

```javascript
foo()
bar()
var foo = function() {
  console.log('foo and x = ' + x)
}
function bar() {
  console.log('bar and x = ' + x)
}
var x = 1
// 호이스팅
var foo
function bar() {
  console.log('bar and x = ' + x)
}
var x
foo()
bar()
foo = function() {
  console.log('foo and x = ' + x)
}
x = 1
```

### 5.4 클로저

#### 5.4.1 클로저의 개념

- 예제 1
  - outerFunc 실행 컨텍스트는 사라졌지만, outerFunc 변수 객체는 그대로 남아있고, innerFunc 의 스코프 체인으로 참조되고 있습니다. 이것이 클로저입니다.
  - 다시말해, **이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수를 클로저라고 합니다.**
    - 여기서는 outerFunc 에서 선언된 x 를 참조하는 innerFunc 이 클로저입니다.
    - outerFunc 의 x 같은 변수를 자유 변수라 부릅니다.

```javascript
function outerFunc() {
  var x = 10
  var innerFunc = function() {
    console.log(x)
  }
  return innerFunc
}
var inner = outerFunc()
inner() // 10
```

- 예제 2

```javascript
function outerFunc(arg1, arg2) {
  var local = 8
  function innerFunc(innerArg) {
    console.log((arg1 + arg2) / (innerArg + local))
  }
  return innerFunc
}
var exam1 = outerFunc(2, 4)
exam1(2) // (2+4) / (2+8)
```

#### 5.4.2 클로저의 활용

##### 5.4.2.1 특정 함수에 사용자가 정의한 객체의 메소드 연결하기

- 예제 1
  - Hellofunc 에서 func 프로퍼티로 참조되는 함수를 call() 함수로 호출합니다.

```javascript
function HelloFunc(func) {
  this.greeting = 'hello'
}
HelloFunc.prototype.call = function(func) {
  func ? func(this.greeting) : this.func(this.greeting)
}
var userFunc = function(greeting) {
  console.log(greeting)
}
var objHello = new HelloFunc()
objHello.func = userFunc
objHello.call() // hello
```

- 예제 2
  - 이러한 방식은, 정해진 형식의 함수를 콜백해주는 라이브러리가 있을 경우, 그 정해진 형식과는 다른 사용자 정의 함수를 호출할 때 유용합니다.

```javascript
function saySomething(obj, methidName, name) {
  return function(greeting) {
    return obj[methodName](greeting, name)
  }
}
function newObj(obj, name) {
  obj.func = saySomething(this, 'who', name)
  return obj
}
newObj.prototype.who = function(greeting, name) {
  console.log(`${greeting} ${name || 'everyone'}`)
}
var obj1 = new newObj(objHello, 'zzoon')
obj1.call() // hello zzoon
```

##### 5.4.2.2 함수의 캡슐화

- 예제 1
  - 첫 방식은 전역 변수인 buffAr 이 외부에 노출되어 있어서 쉽게 값을 바꾸거나, 같은 이름의 변수를 선언해 오류가 나올 수도 있습니다.
  - 그래서 클로저를 활용해 buffAr 을 추가 스코프에 넣고 사용하면 이 문제를 해결할 수 있습니다. 주의할 점은, 변수 getCompletedStr 에 익명함수르 바로 실행시켜 반환하는 함수를 할당하는 것입니다.

```javascript
var buffAr = ['I am', '', 'I live in ', '', 'I am', '', 'years old.']
function getCompletedStr(name, city, age) {
  buffAr[1] = name
  buffAr[3] = city
  buffAr[5] = age
  return buffAr.join('')
}
var str = getCompletedStr('zzoon', 'seoul', 16)
console.log(str)
```

```javascript
var getCompletedStr = (function() {
  var buffAr = ['I am ', '', 'I live in ', '', 'I am ', '', ' years old.']
  return (function(name, city, age) {
    buffAr[1] = name
    buffAr[3] = city
    buffAr[5] = age
    return buffAr.join('')
  })
})()
var str = getCompletedStr('zzoon', 'seoul', 16)
console.log(str)
```

##### 5.4.2.3 setTImeout()에 지정되는 함수의 사용자 정의

- 예제 1
	- 클로저로 자신이 정의한 함수에 인자를 넣습니다.
	- 사용자 정의 함수 callLater 를 setTimeout 함수로 호출하기 위해, 변수 func 에 함수를 반환받아 setTimeout 함수 첫 인자로 넣었습니다. 반환받는 함수는 클로저입니다.
```javascript
function callLater(obj, a, b) {
  return (function() {
    obj['sum'] = a * b;
	console.log(obj['sum']);
  })
}
var sumObj = {
	sum: 0
}
var func = callLater(sumObj, 1, 2);
setTimeout(func, 500);
```

#### 5.4.3 클로저를 활용할 때 주의사항

##### 5.4.3.1 클로저의 프로퍼티값이 쓰기 가능하므로, 그 값이 여러 번 호출로 항상 변할 수 있음에 유의하기

- exam 값을 호출할 때마다, 자유 변수 num 값은 계속해서 바뀌니 주의해야 합니다.
```javascript
function outerFunc(argNum) {
	var num = argNum;
	return function(x) {
		num += x;
		console.log(`num ${num}`)
	}
}
var exam = outerFunc(40);
exam(5);
exam(-10);
```

##### 5.4.3.2 하나의 클로저가 여러 함수 객체의 스코프 체인에 들어가 있는 경우
- 리턴하는 객체에 있는 두 함수 모두 자유 변수 x 를 참조합니다. 그리고 함수를 호출할 때마다 x 값이 바뀌니 주의해야 합니다.
```javascript
function func() {
	var x = 1;
	return {
		func1: function() {console.log(++x)},
		func2: function() {console.log(-x)}
	}
}
var exam = func();
exam.func1();
exam.func2();
```

##### 5.4.3.3 루프 안에서 클로저를 활용하는 경우
- 클로저 설명에서 자주 등장하는 예제입니다.
	- setTimeout 함수 인자로 들어가는 함수는 자유 변수 i 를 참조합니다.
	- 하지만 이 함수가 실행되는 시점은 countSecods() 함수의 실행이 종료된 이후이고, i 값은 이미 4가 된 상태입니다. 그래서 setTimeout() 결과는 4, 4, 4입니다.
```javascript
function countSeconds(howMany) {
	for (var i = 1; i <= howMany; i++) {
		setTimeout(function() {
			console.log(i)
		}, i * 1000);
	}
};
countSeconds(3); // 4, 4, 4
```
- 즉시 실행 함수를 실행시켜 루프 i 값을 currentI 에 복사, setTimeout() 에 들어갈 함수에서 사용해 원하는 결과를 얻었습니다.
```javascript
function countSeconds(howMany) {
	for (var i = 1; i <= howMany; i++) {
		(function (currentI) {
			setTimeout(function() {
				console.log(currentI);
			}, currentI * 1000);
		})(i);
	}
};
countSeconds(3); // 1, 2, 3
```