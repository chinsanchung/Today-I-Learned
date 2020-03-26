---
title: 인사이드 자바스크립트 3장. 데이터타입&연산자
date: 2020-03-09 09:05:00
category: Book
draft: false
---

## Inside JavaScript 정리 3장. 데이터 타입과 연산자

### 목차

- [3.1 기본 타입](#31-기본-타입)
  - [3.1.1 숫자](#311-숫자)
  - [3.1.2 문자열](#312-문자열)
  - [3.1.3 불린값](#313-불린값)
  - [3.1.4 null, undefined](#314-null-undefined)
- [3.2 참조 타입](#32-참조-타입객체-타입)
  - [3.2.1 객체 생성](#321-객체-생성)
  - [3.2.2 객체 프로퍼티 읽기/쓰기/갱신](#322-객체-프로퍼티-읽기쓰기갱신)
  - [3.2.3 for in 과 객체 프로퍼티 출력](#323-for-in-과-객체-프로퍼티-출력)
  - [3.2.4 객체 프로퍼티 삭제](#324-객체-프로퍼티-삭제)
- [3.3 참조 타입의 특성](#33-참조-타입의-특성)
  - [3.3.1 객체 비교](#331-객체-비교)
  - [3.3.2 참조에 의한 함수 호출](#332-참조에-의한-함수-호출)
- [3.4 프로토타입](#34-프로토타입)
- [3.5 배열](#35-배열)
  - [3.5.1 배열 리터럴](#351-배열-리터럴)
  - [3.5.2 배열의 요소 생성](#352-배열의-요소-생성)
  - [3.5.3 배열 length 프로퍼티](#353-배열-length-프로퍼티)
    - [3.5.3.1 배열 표준 메소드와 length 프로퍼티](#3531-배열-표준-메소드와-length-프로퍼티)
  - [3.5.4 배열과 객체](#354-배열과-객체)
  - [3.5.5 배열의 프로퍼티 동적 생성](#355-배열의-프로퍼티-동적-생성)
  - [3.5.6 배열 프로퍼티 열거](#356-배열-프로퍼티-열거)
  - [3.5.7 배열 요소 삭제](#357-배열-요소-삭제)
  - [3.5.8 Array 생성자 함수](#358-Array-생성자-함수)
  - [3.5.9 유사 배열 객체](#359-유사-배열-객체)
- [3.6 기본 타입과 표준 메소드](#36-기본-타입과-표준-메소드)
- [3.7 연산자](#37-연산자)
  - [3.7.1 +](#3.7.1-)
  - [3.7.2 typeof](#372-typeof)
  - [3.7.3 ==, ===](#373--)
  - [3.7.4 !!](#374-)

### 3.1 기본 타입

- 기본 타입은 '숫자', '문자열', '불린값'을 비롯해 'null', 'undefined'가 있습니다.
- 자바스크립트는 느슨한 타입 체크 언어입니다. 따라서 변수를 선언할 때 어떤 형태의 데이터를 저장하느냐에 따라 변수의 타입이 결정됩니다.

#### 3.1.1 숫자

- 자바스크립트는 하나의 숫자형만 존재합니다. 정수형이 따로 없고, 모든 숫자를 실수로 처리하므로 나눗셈 연산을 할 시 주의해야합니다.

#### 3.1.2 문자열

- 한 번 정의된 문자열은 변하지 않습니다.
- 문자열은 문자 배열처럼 인덱스를 이용해 접근할 수 있습니다.

#### 3.1.3 불린값

#### 3.1.4 null, undefined

- 두 타입 모두 '값이 비어있음'을 나타냅니다.
- undefined
  - 값이 할당되지 않는 변수입니다. 타입이자 값을 나타낸다는 의미를 가집니다.
- null
  - 개발자가 명시적으로 값이 비어있음을 나타낼 때 사용합니다.
  - null 타입 변수의 `typeof`는 null 이 아닌 object 임에 주의해야합니다. 일치연산자(===)으로만 null 타입 변수인지 확인할 수 있습니다.

### 3.2 참조 타입(객체 타입)

- 기본 타입을 제외한 모든 값은 객체입니다. 따라서 배열, 함수, 정규표현식 등도 모두 객체입니다.
- 객체는 단순히 '이름(key):값(value)' 형태의 프로퍼티들들 저장하는 컨테이너입니다.
  - 컴퓨터 과학 부야의 해시라는 자료구조와 유사합니다.
  - 객체는 여러 프로퍼티를 가질 수 있습니다. 프로퍼티는 기본 타입의 값을 포함하거나 다른 객체를 가리킬 수 있습니다.
  - 프로퍼티의 성질에 따라 객체의 프로퍼티는 함수로 표햔할 수 있습니다. 이를 메소드라 부릅니다.

#### 3.2.1 객체 생성

- 방법 1. Object() 생성자 함수

```javascript
var foo = new Object()
foo.name = 'foo'
foo.age = 30
console.log(typeof foo) // object
console.log(foo) //{name:'foo', age: 30}
```

- 방법 2. 객체 리터럴 방식
  - 리터럴은 표기법을 뜻합니다. 객체 리터럴이란 객체를 생성하는 간단한 표기법입니다.

```javascript
var foo = { name: 'foo', age: 30 }
```

- 방법 3. 생성자 함수 이용
  - 객체를 생성하는 함수를 생성자 함수라 부릅니다.

#### 3.2.2 객체 프로퍼티 읽기/쓰기/갱신

- 객체의 프로퍼티에 접근하려면 대괄호 표기법, 마침표 표기법 두 개를 사용합니다.

```javascript
var foo = { name: 'foo', major: 'computer science' }
// 읽기
console.log(foo.name)
console.log(foo['name'])
// 갱신
foo.major = 'electronics'
console.log(foo.major)
//동적 생성
foo.age = 30
console.log(foo.age)
//대괄호 표기법
foo['full-name'] = 'foo bar'
console.log(foo['full-name'])
console.log(foo.full - name) // NaN
```

- NaN: 수치 연산을 해서 정상적인 값을 얻지 못할 때 출렵됩니다.
  - 위의 full-name 은 'full 빼기 name' 으로 인식합니다. full 은 undefined, name 도 undefined 이므로 결과는 NaN 입니다.

#### 3.2.3 for in 과 객체 프로퍼티 출력

- for in 문으로 객체의 모든 프로퍼티에 대해 루프를 실행할 수 있습니다.

```javascript
var foo = { name: 'foo', age: 30, major: 'computer science' }
var prop
for (prop in foo) {
  console.log(prop, foo[prop])
}
/*
결과: name foo \n age 30 \n major 'computer science'
*/
```

#### 3.2.4 객체 프로퍼티 삭제

- delete 연산자는 객체의 프로퍼티를 삭제합니다. 다만 객체 자체를 삭제하진 못합니다.

```javascript
var foo = { name: 'foo', age: 30, major: 'computer science' }
delete foo.major
console.log(foo.major) // undefined
```

### 3.3 참조 타입의 특성

- 기본 타입을 제외한 모든 값을 객체입니다. 이러한 객체를 참조 타입이라 부릅니다. 왜냐면 객체의 모든 연산이 실제 값이 아닌 참조값으로 처리되기 때문입니다.

```javascript
var obj1 = { val: 40 }
var obj2 = obj1
console.log(obj1.val) // 40
console.log(obj2.val) // 40
obj2.val = 50
console.log(obj1.val) // 50
console.log(obj2.val) // 50
```

- obj1은 실제로는 객체를 참조하는 겂을 저장할 뿐, 실제 객체를 나타내진 않습니다.
- obj2는 obj1과 같은 객체의 참조값을 저장합니다. obj1, obj2 는 동일한 객체의 참조값이 저장됩니다. 그래서 값이 동일하게 바뀐 것입니다.

#### 3.3.1 객체 비교

- 동등 연산자 === 을 써도 참조값을 비교합니다.

```javascript
var a = 100
var b = 100
var obj1 = { val: 100 }
var obj2 = { val: 100 }
var obj3 = obj2
console.log(a === b) // true
console.log(obj1 === obj2) // false
console.log(obj2 === obj3) // true
```

- 기본 타입은 값 자체로 비교합니다. 하지만 참조타입은 참조값이 같아야 참입니다. `obj1===obj2`가 거짓인 이유입니다.

#### 3.3.2 참조에 의한 함수 호출

- 가본 타입은 값에 의한 호출 방식으로 동작합니다.
  - 함수 호출 시 인자로 기본 타입의 값을 넘길 경우, 호출된 함수의 매개변수로 복사된 값이 전달됩니다.
  - 그래서 내부에서 매개변수로 값을 변경해도 호출된 변수 값이 바뀌진 않습니다.
- 객체같은 참조 타입은 함수 호출 시 참조에 의한 호출로 동작합니다.
  - 호출 시 인자로 객체를 전달할 경우, 인자로 넘긴 객체의 참조값이 그대로 함수 내부로 전달됩니다. 그래서 실제 객체의 값을 변경할 수 있습니다.

```javascript
var a = 100
var obj1 = { val: 100 }
function changeArg(num, obj) {
  num = 200
  obj.value = 200
  console.log(num, obj)
}
changeArg(a, obj1)
console.log(a, obj1)
// 200, {val:200}
// 100, {val:200}
```

### 3.4 프로토타입

- 모든 객체는 자신의 부모 역할을 하는 객체와 연결되어 있습니다.
  - 객체지향의 상속과 같이, 부모 객체의 프로퍼티를 자신의 것처럼 쓸 수 있습니다.
  - 이러한 부모 객체를 프로토타입 객체라 부릅니다. (프로토타입으로 축약)

```javascript
var foo = { name: 'foo', age: 30 }
console.log(foo.toString()) // [object Object]
console.dir(foo) // Object
```

- 두번째를 확장해보면, `__proto__`프로퍼티가 있는데, 이는 foo 객체의 부모인 프로토타입 객체입니다.
  - ECMAScript 명세서의 [[Prototype]] 프로퍼티를 크롬에서는 `__proto__`라 뜻합니다.
  - `__proto__`프로퍼티가 가리키는 객체는 Object.prototype 입니다. 모든 객체에서 호출가능한 기본 내장 메소드를 가집니다.
  - 프로토타입 객체는 임의의 다른 객체로 변경할 수 있습니다.

### 3.5 배열

#### 3.5.1 배열 리터럴

배열을 만들 때 사용하는 표기법입니다. 배열 리터럴에서는 각 요소의 값만을 포함합니다. 배열 내 위치 인덱스값으로 원소에 접근합니다.

```javascript
var numArr = [1, 2, 3, 4, 5]
```

#### 3.5.2 배열의 요소 생성

배열도 동적으로 원소를 추가할 수 있습니다.

```javascript
var emptyArr = []
emptyArr[0] = 100
emptyArr[2] = 8
emptyArr[4] = true
console.log(emptyArr) // [100, undefined, 8, undefined, true]
console.log(emptyArr.length) // 5
```

#### 3.5.3 배열 length 프로퍼티

- `length()`는 배열의 원소 개수를 나타내지만, 실제로 배열에 존재하는 원소 개수와 일치하진 않습니다.
  - 이 프로퍼티는 배열 내 가장 큰 인덱스에 1을 더한 값입니다.
  - 명시적으로 값을 바꿀 수 있습니다.

```javascript
var arr = [0, 1, 2]
arr.length = 4
console.log(arr) // [0, 1, 2, undefined]
arr.length = 2
console.log(arr) // [0, 1]
```

##### 3.5.3.1 배열 표준 메소드와 length 프로퍼티

배열 메소드는 length 프로퍼티를 기반으로 동작합니다. length 프로퍼티는 push, pop, shift, unshift 등 여러 메소드에 영향을 줄만큼 중요한 프로퍼티입니다.

#### 3.5.4 배열과 객체

자바스크립트에서는 배열도 객체입니다. 다만 일반 객체와는 조금 다릅니다.

```javascript
var arr1 = ['a', 'b', 'c']
console.log(arr1[0]) // 'a'

var arr2 = { '0': 'a', '1': 'b', '2': 'c' }
console.log(arr2[0]) // 'a'

console.log(typeof arr1) // object (not array)
console.log(typeof arr2) // object

console.log(arr1.length) // 3
console.log(arr2.length) // undefined

arr1.push('d') // ['a','b','c','d']
arr2.push('d') // uncaught typeerror

// 객체의 프로토타입과 배열의 프로토타입은 다릅니다.
console.dir(arr1.__proto__) // Array.prototype
console.dir(arr2.__proto__) // Object.prototype
```

- 배열도 객체처럼 'key:value' 형태로 배열 원소와 프로퍼티를 가집니다.

#### 3.5.5 배열의 프로퍼티 동적 생성

```javascript
var arr = [0, 1, 2]
arr.color = 'blue'
arr.name = 'number_array'
console.log(arr.length) // 3

arr[3] = 3
console.log(arr.length) // 4
```

#### 3.5.6 배열 프로퍼티 열거

```javascript
for (var prop in arr) {
  console.log(prop, arr[prop])
}
for (var i = 0; i < arr.length; i++) {
  console.log(i, arr[i])
}
```

- for in 문은 배열 요소에 color, name 프로퍼티도 출력합니다. for 문은 정확히 배열 요소만을 출력합니다.

#### 3.5.7 배열 요소 삭제

```javascript
var arr = [0, 1, 2, 3]
delete arr[2]
console.log(arr) // [0, 1, undefined, 3]
console.log(arr.length) // 4

// 배열에서 요소를 완전히 삭제하려면 splice() 를 사용합니다.
arr.splice(2, 1)
console.log(arr) // [0, 1, 3]
```

#### 3.5.8 Array 생성자 함수

- 배열 리터럴도 결국 자바스크립트 기본 제공 Array() 생성자 함수로 배열 생성 과정을 단순화시킨 것입니다.

```javascript
var foo = new Array(3)
console.log(foo) // [undefined, undefined, undefined]
console.log(foo.length) // 3
var bar = new Array(1, 2, 3)
console.log(bar) // [1, 2, 3]
console.log(bar.length) // 3
```

#### 3.5.9 유사 배열 객체

- 일반 객체에 length 프로퍼티를 가진 것을 유사 배열 객체라 부릅니다. 객체임에도 배열 메소드를 사용할 수 있습니다.

```javascript
var arr = ['bar']
var obj = { name: 'foo', length: 1 }

arr.push('baz') // ['bar', 'baz']
obj.push('baz') // error

// apply() 로 배열 메소드 호출
Array.prototype.push.apply(obj, ['baz'])
console.log(obj) // {'1': 'baz', name: 'foo', length: 2}
```

### 3.6 기본 타입과 표준 메소드

- 기본 타입을 위해 정의된 표준 메소드를 사용하면, 기본값을 메소드 처리 순간에 객체로 변환한 다음 각 타입별 표준 메소드를 호출하게 됩니다. 그 후 호출이 끝나면 다시 기본값으로 복귀합니다.

### 3.7 연산자

#### 3.7.1 +

- 두 문자 모두 숫자일 때 더하기 연산이 수행되고, 나머지는 문자열 연결 연산이 이뤄집니다.

#### 3.7.2 typeof

- typeof 연산자는 피연산자의 타입을 문자열로 리턴합니다.
  - 'number', 'string', 'boolean', 'object'(null, 객체, 배열), 'undefined, 'function'(함수)

#### 3.7.3 ==, ===

- 동등 연산자 == 은 타입이 다를 경우 타입 변환을 거치고 비교합니다.
- 일치 연산자 === 는 피연산자의 타입이 다르더라도 변환을 하지 않습니다.

#### 3.7.4 !!

- !! 은 피연산자를 불린 값으로 변환합니다. 참고로 객체는 빈 객체라도 true 로 변환됩니다.
