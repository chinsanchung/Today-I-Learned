---
title: 인사이드 자바스크립트 4장. 함수&프로토타입 체이닝 03
date: 2020-03-10 12:50:42
category: Book
draft: false
---

## Inside JavaScript 정리 4장. 함수와 프로토타입 체이닝 03

### 4.5 프로토타입 체이닝

#### 4.5.1 프로토타입의 두 가지 의미

- 자바스크립트는 **프로토타입 기반의 객체지향 프로그래밍**을 지원합니다.
  - 객체 리터럴이나 생성자 함수로 객체를 생성하며, 이렇게 생성된 객체의 부모 객체가 '프로토타입 객체'입니다. (자식 객체는 부모 객체가 가진 프로퍼티 접근이나 메소드를 상속받아 호출할 수 있습니다.)
- 자바스크립트의 모든 객체는 자신의 부모인 프로토타입 객체를 가리키는 참조 링크 형태의 숨겨진 프로퍼티 '암묵적 프로토타입 링크'를 가지며, 모든 객체의 `[[Prototype]]`프로퍼티에 저장됩니다.
  - 함수 객체의 **prototype 프로퍼티**와 객체의 숨은 프로퍼티 `[[Prototype]]`링크를 구분할 수 있어야 합니다. 구분하려면 **자바스크립트 객체 생성 규칙**을 알아야 합니다.
- 생성 규칙: 자바스크립트의 모든 객체는 자신을 생성한 생성자 함수의 prototype 프로퍼티가 가리키는 프로토타입 객체를 자신의 부모 객체로 설정하는 `[[Prototype]]`링크로 연결합니다.
  - 아래 예제: Person 생성자 함수는 prototype 프로퍼티로 자신과 링크된 프로토타입 객체를 가리킵니다. foo 객체는 Person()함수 프로토타입 객체를 `[[Prototype]]`링크로 연결했습니다. 결국 prototype 프로퍼티나 `[[Prototype]]`링크는 같은 프로토타입 객체를 가리킵니다.

```javascript
function Person(name) {
  this.name = name
}
var foo = new Person('foo')
```

- prototype 프로퍼티는 자신과 링크된 프로토타입 객체를 가리키지만, `[[Prototype]]`링크는 객체의 입장에서 자신의 부모 객체인 프로토타입 객체를 내부의 숨겨진 링크로 가리킵니다.
  - 결국, 자바스크립트에서 객체를 생성하는 건 생성자 함수의 역할이지만, 생성된 객체의 실제 부모 역할을 하는 것은 생성자 자신이 아닌, 생성자 prototype 프로퍼티가 가리키는 프로토타입 객체입니다.
- `[[Prototype]]` 프로퍼티와 `__proto__` 프로퍼티는 같습니다.

#### 4.5.2 객체 리터럴로 생성된 객체의 프로토타입 체이닝

- **프로토타입 체이닝**을 통해, 객체는 자기 자신의 프로퍼티뿐만 아니라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티에도 접근할 수 있습니다.
  - 예시: obj 에 hasOwnProperty() 메소드가 없어도 결과가 정상적으로 출력됩니다.
  - 프로토타입 체이닝 순서: 우선 obj 에 hasOwnProperty() 메소드가 없으니, obj 객체의 `[[prototype]]`링크를 따라 부모 역할인 Object.prototype 프로토타입 객체에서 검색합니다. 거기에 hasOwnProperty() 메소드가 있으므로 오류없이 출력합니다. (Object.prototype 객체는 자바스크립트 모든 객체의 조상 역할을 합니다. 모든 객체가 호출할 수 있는 표준 메소드를 제공합니다.

```javascript
var obj = {
  name: 'foo',
  sayName: function() {
    console.log('name: ' + this.name)
  },
}
obj.sayName() // name: foo
console.log(obj.hasOwnProperty('name')) // true
console.log(obj.hasOwnProperty('nickname')) // false
obj.sayNickName() // Uncaught TypeError: Object #<Object> has no method 'sayNickName'
```

#### 4.5.3 생성자 함수로 생성된 객체의 프로토타입 체이닝

- 생성자 함수는 객체 리터럴 방식과 약간 다른 프로토타입 체이닝이 이뤄집니다. 하지만 기본 원칙을 잘 지킵니다.
  - "자바스크립트에서 모든 객체는 자신을 생성한 생성자 함수의 prototype 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체(부모 객체)로 취급한다."
- 예시
  - foo 객체의 프로토타입 객체는 Person.prototype 입니다.
  - 함수에 연결된 프로토타입 객체는 디폴트로 constructor 프로퍼티(`function Person(name, age){}`)만을 가지고 있어 hasOwnProperty() 메소드는 없습니다.
  - 하지만 Person.prototype 역시 Object.prototype 객체로 이어지므로 hasOwnProperty() 메소드가 실행됐습니다.

```javascript
function Person(name, age) {
  this.name = name
  this.age = age
}
var foo = new Person('foo', 30)
// 프로토타입 체이닝
console.log(foo.hasOwnProperty('name')) // true
console.dir(Person.prototype)
```

#### 4.5.4 프로토타입 체이닝의 종점

- Object.prototype 객체는 **프로토타입 체이닝의 종점**입니다.
  - 달리 말하자면, 모든 자바스크립트 객체는 프로토타입 체이닝으로 Object.prototype 객체가 가진 프로퍼티와 메소드에 접근하고, 서로 공유가 가능합니다.

#### 4.5.5 기본 데이터 타입 확장

- Object.prototype 에 정의된 메소드들은 자바스크립트 모든 객체의 표준 메소드입니다.
  - 숫자, 문자열, 배열 등의 표준 메소드는, 이들의 프로토타입인 Number.prototype, String.prototype, Array.prototype 등에 정의되어 있습니다. 이러한 기본 내장 프로토타입 객체도 프로토타입 체이닝으로 연결됩니다.
- 표준 빌트인 프로토타입 객체도 사용자가 직접 정의한 메소드를 추가할 수 있습니다.
  - `console.dir`을 보면 testMethod()가 추가되어 있음을 알 수 있습니다.

```javascript
String.prototype.testMethod = function() {
  console.log('test method')
}
var str = 'Test'
str.testMethod() // test method
console.dir(String.prototype)
```

#### 4.5.6 프로토타입도 자바스크립트 객체다

- 프로토타입 객체 역시 자바스크립트 객체이므로 일반 객체처럼 동적으로 프로퍼티를 추가, 삭제할 수 있습니다. 변경된 프로퍼티는 실시간으로 프로토타입 체이닝에 반영됩니다.
- 예시
  - foo 객체의 프로토타입 객체 Person.prototype 객체에 동적으로 sayHello() 메소드를 추가했습니다.
  - foo 객체에서 sayHello() 호출 -> foo 객체에는 없음 -> 프로토타입 체이닝으로 Person.prototype 객체에서 검색 -> 출력

```javascript
function Person(name) {
  this.name = name
}
var foo = new Person('foo')
Person.prototype.sayHello = function() {
  console.log('hello')
}
foo.sayHello() // hello
```

#### 4.5.7 프로토타입 메소드와 this 바인딩

- 프로토타입 객체의 메소드 내부에서 this 를 사용할 경우
  - 메소드 호출 패턴의 this 는 그 메소드를 호출한 객체에 바인딩됩니다.
  - foo 객체에서 getName() 메소드를 찾는 프로토타입 체이닝이 발생합니다. getName() 을 호출한 객체는 foo 이므로, this 는 foo 객체에 바인딩됩니다.
  - Person.prototype 객체에 접근해 getName() 메소드를 호출하면, this 는 Person.prototype 에 바인딩됩니다.

```javascript
function Person(name) {
  this.name = name
}
Person.prototype.getName = function() {
  return this.name
}

var foo = new Person('foo')
console.log(foo.getName()) // foo
// Person.prototype 객체에 name 프로퍼티 동적 추가
Person.prototype.name = 'person'
console.log(Person.prototype.getName()) // person
```

#### 4.5.8 디폴트 프로토타입은 다른 객체로 변경이 가능합니다.

- 함수를 생성할 때 해당 함수와 연결되는 **디폴트 프로토타입 객체를 다른 일반 객체로 변경할 수 있습니다.** 이를 통해 객체지향의 상속을 구현합니다.
- 생성자 함수의 프로토타입 객체가 바뀌면, 변경된 시점 이후의 생성된 객체들은 변경된 프로토타입 객체로 `[[Prototype]]`링크를 연결하는 것에 주의해야 합니다.
  - 1. Person() 함수를 생성할 때 같이 생성되는 Person.prototype 객체는 자신과 연결된 Person() 생성자 함수를 가리키는 constructor 프로퍼티만을 가집니다.
  - 2. foo 객체는 객체 생성 규칙에 따라 Person.prototype 객체를 자신의 프로토타입으로 연결했습니다. 다만 foo, Person.prototype 둘 다 country 프로퍼티가 없습니다.
  - 3. 프로토타입 객체를 객체 리터럴로 생성한 country 프로퍼티를 가진 객체로 바꿨습니다.
  - 변경한 프로토타입 객체는 디폴트와 달리 **constructor 프로퍼티가 없습니다.** 그래서 Object.prototype 까지 프로토타입 체이닝이 발생합니다. Object.prototype 역시 Object() 생성자 함수와 연결된 빌트인 프로토타입 객체여서, Object() 생성자 함수를 consturctor 프로퍼티에 연결했고, 결과적으로 Person.prototype.constructor 는 **Object() 생성자 함수**가 출력됩니다.
  - 4. bar 객체는 새로 변경된 프로토타입 객체를 `[[Prototype]]`링크로 가리킵니다.
  - 5. foo 와 bar 를 출력하면 다른 결과가 나오게 됩니다.

```javascript
function Person(name) {
  this.name = name
}
console.log(Person.prototype.constructor) // 1. Person(name)

var foo = new Person('foo')
console.log(foo.country) // 2. undefined

Person.prototype = {
  country: 'korea',
}
console.log(Person.prototype.constructor) // 3. Object()

var bar = new Person('bar') // 4
// 5
console.log(foo.country) // undefined
console.log(bar.country) // korea

console.log(foo.constructor) // Person(name)
console.log(bar.constructor) // Object()
```

#### 4.5.9 객체의 프로퍼티를 읽거나 메소드를 실행할 때만 프로토타입 체이닝이 동작합니다.

- 객체의 특정 프로퍼티를 읽으려 할 때, 프로퍼티가 해당 객체에 없는 경우 프로토타입 체이닝이 발생합니다.
  - 반대로 객체의 특정 프로퍼티에 값을 쓰려고 할 때는 프로토타입 체이닝이 일어나지 않습니다.
- 예시
  - foo.country 값에 'USA'를 저장하면, foo 객체에 country 프로퍼티 값이 동적으로 생성됩니다. 프로토타입 체이닝 없이 바로 'USA'가 출력됩니다.
  - 반면 bar 객체는 프로토타입 체이닝을 거쳐 'korea'를 출력합니다.

```javascript
function Person(name) {
  this.name = name
}
Person.prototype.country = 'korea'

var foo = new Person('foo')
var bar = new Person('bar')
console.log(foo.country) // korea
console.log(bar.country) // korea
foo.country = 'USA'

console.log(foo.country) // USA
console.log(bar.country) // korea
```
