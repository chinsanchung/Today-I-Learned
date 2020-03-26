---
title: 인사이드 자바스크립트 4장. 함수&프로토타입 체이닝 02
date: 2020-03-10 12:30:15
category: Book
draft: false
---

## Inside JavaScript 정리 4장. 함수와 프로토타입 체이닝 02

### 4.4 함수 호출과 this

#### 4.4.1 arguments 객체

- 자바스크립트는 함수 호출 시 형식에 맞춰 인자를 넘기지 않더라도 undefined 값을 할당할 뿐 에러로 처리하지 않습니다.
- 그래서 함수 작성 시 런타임 시에 호출된 인자의 개수를 확인해 다른 동작을 설정해야 할 경우가 있는데 arguments 객체를 씁니다.
- arguments 객체는 함수 호출 시 넘긴 인자들이 배열 형태로 저장된 객체입니다. 실제 배열이 아닌 유사 배열 객체입니다.
- arguments 객체는 세 부분으로 구성됩니다.
  - 함수 호출 시 넘긴 인자
  - length 프로퍼티: 인자의 개수
  - callee 프로퍼티: 현재 실행 중인 함수의 참조값
- arguments 객체는 매개변수 개수가 정해지지 않은 함수, 또는 전달된 인자의 개수에 따라 다른 처리가 필요한 함수에 사용됩니다.

```javascript
function sum() {
  var result = 0
  for (var i = 0; i < arguments.length; i++) {
    result += arguments(i)
  }
  return result
}
console.log(sum(1, 2, 3)) // 6
```

#### 4.4.2 호출 패턴과 this 바인딩

##### 4.4.2.1 객체 메소드를 호출할 때 this 바인딩

이 때 this 는 **해당 메소드를 호출한 객체로 바인딩**됩니다.

```javascript
var obj = {
  name: 'foo',
  sayName: function() {
    console.log(this.name)
  },
}
var objTwo = {
  name: 'bar',
}
objTwo.sayName = obj.sayName
obj.sayName() // foo
objTwo.sayName() // bar
```

##### 4.4.2.2 함수를 호출할 때 this 바인딩

- 함수를 호출할 때, 해당 함수 내부 코드에서 사용된 this 는 **전역 객체에 바인딩**됩니다. 예를 들어 브라우저의 경우 전역 객체인 window 객체에 바인딩됩니다. 그리고 Node.js 에서는 global 객체가 전역 객체입니다.
- 예시
  - 자바스크립트 전역 변수는 window 전역 객체의 프로퍼티로 접근 가능합니다.
  - sayFoo() 에서의 this 는 전역 객체인 window 에 바인딩됩니다.

```javascript
var foo = 'foo'
console.log(foo) // === console.log(window.foo);

var test = 'test'
console.log(window.test) // test
var sayFoo = function() {
  console.log(this.test)
}
sayFoo() // test
```

- 내부 함수를 호출할 때도 그대로 적용됩니다.
  - **내부 함수 호출 패턴을 정의하지 않아서** 원했던 2,3,4 가 나오질 않은 것입니다.

```javascript
var value = 100
var obj = {
  value: 1,
  func1: function() {
    this.value += 1
    console.log('1: ' + this.value)
    func2 = function() {
      this.value += 1
      console.log('2: ' + this.value)
      func3 = function() {
        this.value += 1
        console.log('3: ' + this.value)
      }
      func3()
    }
    func2()
  },
}
obj.func1()
/*
1: 2
2: 101
3: 102
*/
```

- 부모 함수의 this 를 내부 함수가 접근 가능한 다른 변수에 저장하는 방법으로 고칠 수 있습니다.
  - 관례상 쓰는 that 변수로 부모 함수의 this 가 가리키는 객체로 접근할 수 있습니다.

```javascript
var value = 100
var obj = {
  value: 1,
  func1: function() {
    var that = this
    this.value += 1
    console.log('1: ' + this.value)
    func2 = function() {
      that.value += 1
      console.log('2: ' + that.value)
      func3 = function() {
        that.value += 1
        console.log('3: ' + that.value)
      }
      func3()
    }
    func2()
  },
}
obj.func1()
```

##### 4.4.2.3 생성자 함수를 호출할 때 this 바인딩

- 생성자 함수는 객체를 생성하는 역할을 합니다. 다만 자바스크립트에서는 **기존 함수에 new 연산자를 붙여 호출하면 그 함수를 생성자 함수로 사용할 수 있습니다.**
- 생성자 함수 내부의 this 는 기존 메소드와 함수 호출의 this 와든 다릅니다.
- 생성자 함수가 동작하는 방식

```javascript
var Person = function(name) {
  this.name = name // this 빈 객체에 name 이라는 동적 프로퍼티 생성
  return
}
var foo = new Person('foo')
console.log(foo.name) // foo
```

- 객체 리터럴 방식과 생성자 함수 간 객체 생성 방식의 차이
  - 리터럴 방식은 같은 형태의 객체를 재생성할 수 없습니다. 반면 생성자 함수는 호출할 때 다른 인자를 넘겨서 같은 형태의 서로 다른 객체를 만들 수 있습니다.
  - 객체 리터럴 방식의 프로토타입 객체는 Object(실제는 Object.prototype)인데, 생성자 함수 방식은 Person(실제는 Person.prototype)로 서로 다릅니다.
  - 이러한 차이가 발생한 이유는 자바스크립트 생성 규칙 때문입니다. 자바스크립트 객체는 자신을 생성한 **생성자 함수의 프로토타입 프로퍼티**가 가리키는 객체를 자신의 프로토타입 객체로 설정합니다.

```javascript
// 객체 리터럴
var foo = { name: 'foo', age: 35 }
// 생성자 함수
function Person(name, age) {
  this.name = name
  this.age = age
}
var bar = new Person('bar', 33)
var baz = new Person('baz', 25)
```

- 생성자 함수에 new 를 붙이지 않고 호출할 경우, 에러가 발생합니다. 일반 호출 함수는 this 를 window 전역 객체에 바인딩하지만, 생성자 함수 호출의 this 는 새로 생성되는 빈 객체에 바인딩되기 때문입니다.

##### 4.4.2.4 call, apply 메소드를 이용한 명시적인 this 바인딩

- this 를 특정 객체에 **명시적으로 바인딩**시키는 방법입니다.
- apply() 메소드를 호출하는 주체는 함수인만큼, 본질적인 기능은 함수 호출입니다.
  - call() 메소드는 apply() 와 기능은 같지만, 두 번째 인자에서 배열 형태로 넘기는 점이 다릅니다.
  - `Person('foo',30)`함수를 호출하면서, this 를 foo 객체에 명시적으로 바인딩하는 예시입니다.

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
var foo = {}
Person.apply(foo, {'foo', 30});
console.dir(foo)
```

- apply(), call()의 대표적인 용도는 arguments 객체같은 유사 배열 객체에서 배열 메소드를 사용하는 경우입니다. 실제 배열이 아니라서 pop(), shift() 등의 표준 배열 메소드를 이용할 수 없기에 apply()를 사용합니다.
  - apply()로 arguments 객체가 배열 메소드를 가지고 있는 것처럼 처리했습니다.
  - `args`의 의미: `Array.prototype.slice()`메소드를 호출하고, this 는 arguments 객체로 바인딩하라.
  - arguments 는 Object.prototype, args 는 Array.prototype 인 것을 알 수 있습니다.

```javascript
function func() {
  console.dir(arguments)
  // arguments 객체를 배열로 전환
  var args = Array.prototype.slice.apply(arguments)
  console.dir(args)
}
func(1, 2, 3)
```

#### 4.4.3 함수 리턴

- **자바스크립트 함수는 항상 리턴값을 반환합니다.**

##### 4.4.3.1 일반 함수나 메소드는 리턴값을 지정하지 않으면 undefined 가 리턴됩니다.

```javascript
var noReturnFunc = function() {
  console.log('no return statement')
}
var result = noReturnFunc()
console.log(result)
// no return statement
// undefined
```

##### 4.4.3.2 생성자 함수에서 리턴값을 지정하지 않을 경우, 생성된 객체가 리턴됩니다.

- 생성자 함수는 별도의 리턴값이 없으면 this 로 바인딩된 새로운 객체를 리턴합니다.
- 임의의 객체를 리턴할 경우, 명시적이 값이 있기에 foo 에는 해당 객체를 저장합니다.

```javascript
function Person(name, age) {
  this.name = name
  this.age = age
  return { name: 'bar', age: 20 }
}
var foo = new Person('foo', 30)
console.dir(foo) // age: 20, name: 'bar'
```

- 생성자 함수에서 명시적으로 기본 타입을 리턴할 경우, 그 리턴값을 무시하고 this 로 바인딩된 객체를 리턴합니다.

```javascript
function Person(name, age) {
  this.name = name
  this.age = age
  return 100
}
var foo = new Person('foo', 30)
console.log(foo) // Person {name: 'foo', age: 30}
```
