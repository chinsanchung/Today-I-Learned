# JavaScript_Built-in (Udacity)
## Symbol
- `symbol` : 특별한 식별자입니다. 객체 내의 프로퍼티를 고유하게 식별하는데 사용됩니다.
```javascript
/* 특별한 symbol 을 syml 에 저장합니다. 'apple' 이 그 심볼을 정의합니다.
하지만 apple 로는 symbol 에 접근할 수 없습니다.*/
const syml = Symbol('apple');
console.log(syml);

const sym2 = Symbol('banana');
const sym3 = Symbol('banana');
console.log(sym2 === sym3); //결과 : false
```
  + 위의 바나나 심볼들은 같은 바나나를 설명하지만 각각 새로운 심볼이 됩니다.
- 만약 특정 객체 내부에 객체를 넣어 프로퍼티로 만들었을 때 같은 이름을 가진 프로퍼티가 있을 경우 그것들을 구분해야만 사용할 수 있게 됩니다. ES6 부터는 `symbol` 으로 그것을 도울 수 있게 됩니다.
```javascript
const bowl = {
  'apple': {color: 'red', weight: 136 },
  'banana': { color: 'yellow', weight: 183 },
  'orange': { color: 'orange', weight: 170 },
  'banana': { color: 'yellow', weight: 140 }
};
console.log(bowl); // 결과 : Object {apple: Object, banana: Object, orange: Object}
//두번째 바나나와 네번째 바나나를 겹처서 썼습니다.

//해결책은 symbol 을 사용하는 것입니다.
const bowl = {
  [Symbol('apple')]: {color: 'red', weight: 136 },
  [Symbol('banana')]: { color: 'yellow', weight: 183 },
  [Symbol('orange')]: { color: 'orange', weight: 170 },
  [Symbol('banana')]: { color: 'yellow', weight: 140 }
};
console.log(bowl);
// 결과 : Object {Symbol(apple): Object, Symbol(banana): Object, Symbol(orange): Object, Symbol(banana): Object}
```

## 반복(iteration) & 반복 가능한 프로토콜
- `Iterable Protocol` : 반복 가능한 프로토콜은 객체의 반복 동작을 정의하고 사용하는데 쓰입니다.
  + 이는 객체의 값을 반복하는 방법을 지정할 유연성을 ES6 이 가지고 있다는 뜻이 됩니다.
  + 일부 객체는 이미 이러한 반복 동작이 내장(built_in)되어 있습니다. (문자열, 배열 등)
```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
for (const digit of digits) {
  console.log(digit);
}
```
  + 객체를 반복 가능하게 하려면 *iterable 인터페이스* 를 구현해야 합니다. 그러려면 기본 iterator 메소드가 포함되어야 합니다.
- `Iterator Protocol` : 반복자 프로토콜은 객체가 일련의 값을 생성하는 표준 방법을 정의하는 데 사용합니다. 이것은 `.next()` 메소드로 구현합니다. `.next()` 메소드는 두 개의 프로퍼티가 있는 객체를 return 하는 0개 인수 함수입니다.
  + 작동 원리 : 객체는 `.next()` 메소드를 구현할 때 Iterator 가 됩니다.
    + value : 객체 내의 값의 순서에서 다음 값을 나타내는 데이터
    + done : 반복자가 값의 순서를 거쳐가는지를 나타내는 불리언(true : 반복자는 값의 마지막에 이르름. false : 반복자는 값의 순서대로 다른 값을 생성)
```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
const arrayIterator = digits[Symbol.iterator]();

console.log(arrayIterator.next()); // 결과 : Object {value: 0, done: false}
console.log(arrayIterator.next()); // 결과 : Object {value: 1, done: false}
console.log(arrayIterator.next()); // 결과 : Object {value: 2, done: false}
```

## 집합(Sets)
- 집합(sets) 는 별개의 항목들의 모임입니다. 만약 중복이 있다면 그것은 집합이 아니게 됩니다.
  + 자바스크립트 배열로 집합을 구현할 수는 있지만 집합은 중복되더라도 별 상관하질 않습니다.
- ES6 에서는 수학적인 집합과 비슷하고 배열처럼 작동하는 새로운 내장(built-in) 객체가 있습니다. 그 객체를 `Set` 이라 부릅니다.
- `Set` 과 배열의 차이점
  + `Set` 은 인덱스 기반이 아닙니다. 배열처럼 값의 위치를 기준으로 항목을 참조하지 않습니다.
  + `Set` 의 항목(item)들은 개별적으로 접근할 수 없습니다.
- Set 은 특별한 항목들을 저장하게 해주는 객체입니다. 항목을 추가하고, 지우고, 반복할 수도 있습니다. 이러한 항목들은 프리미티브 값이거나 객체일 수 있습니다.
- Set 만들기
```javascript
const games = new Set();
console.log(games); // 결과 : Set{ } 아무런 항목이 없는 빈 Set 입니다.
//배열을 사용하면 항목이 있는 Set 을 만듭니다.
const game = new Set(['a', 'b', 'c', 'a']);
console.log(games); // 결과 : Set {'a', 'b', 'c'} 4번째의 a 를 지운채로 Set 을 만들었습니다.
```

### Sets 수정하기
- `.add()` 와 `.delete()` 메소드를 사용해서 Set 을 변경합니다. 모든 항목들을 지우려면 `.clear()` 메소드를 씁니다.
```javascript
const game = new Set(['a', 'b', 'c', 'a']);

games.add('d');
games.add('e');
games.delete('a');

console.log(games); // 결과 : Set {'b', 'c', 'd', 'e'}

games.clear();
console.log(games); // 결과 : Set {}
```
- 만약 이미 있는 항목을 `.add()` 하더라도 에러는 뜨지 않지만 Set 에 추가되지 않습니다. 마찬가지로 없는 항목을 `.delete()` 하면 에러는 뜨지 않아도 Set 은 그대로 유지됩니다.
- `.add()` 는 성공할 경우 Set 을 return 합니다. `.delete()` 는 불리언(성공-true, 실패-false)을 return 합니다.

### Set 으로 작업하기
- Set 의 길이 체크
```javascript
const months = new Set(['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']);
console.log(months.size); // 결과 : 12
```
- 항목들이 존재하는지 체크
```javascript
console.log(months.has('February')); // 결과 : true
```
- 모든 값들을 검색하기
```javascript
console.log(months.values());
// 결과 : SetIterator {'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'}
```

### Sets & 반복자(Iterator)
- Set 의 기본 반복자로 각 항목들을 차례대로 탐색할 수 있고, for of 반복문으로 반복이 가능합니다.
- `values()` 메소드는 새로운 반복자 객체(`SetIterator` 라고 부릅니다)를 return 합니다. 이것을 변수에 저장할 수 있고 또는 `.next()` 로 반복할 수 있습니다.
```javascript
const iterator = months.values();
iterator.next(); /* 처음에는 Object {value: 'January', done: false}
두번째 실행하면 Object {value: 'February', done: false} 이렇게 됩니다.
12월까지 실행 후 다시 실행하면 {value: undefined, done: true} 가 나옵니다. (Set 의 마지막)*/
```
- for of 문으로 반복하기
```javascript
const colors = new Set(['red', 'orange', 'yellow', 'green', 'blue', 'violet', 'brown', 'black']);

for (const color of colors) {
  console.log(color);
}
```
### WeakSets
- `WeakSet` : Set 과 비슷하지만 몇 가지 차이점이 있습니다.
  1. 오직 객체만 담을 수 있습니다.
  2. 반복이 불가능합니다.
  3. `.clear()` 메소드를 가지고 있지 않습니다.
- Set 과 같은 형식으로 WeakSet 을 만들 수 있습니다.
```javascript
let student1 = { name: 'James', age: 26, gender: 'male' };
let student2 = { name: 'Julia', age: 27, gender: 'female' };
let student3 = { name: 'Richard', age: 31, gender: 'male' };

const roster = new WeakSet([student1, student2, student3]);
console.log(roster);
/* 결과 : WeakSet {Object {name: 'Julia', age: 27, gender: 'female'},
 Object {name: 'Richard', age: 31, gender: 'male'}, Object {name: 'James', age: 26, gender: 'male'}} */
//만약 객체가 아닌 것을 넣는다면 에러가 뜹니다.
roster.add('A'); // 에러
```
- *garbage collection* : 자바스크립트는 새 값이 만들어지면 메모리가 할당됩니다. 값이 필요하지 않으면 자동으로 해제됩니다. 더 이상 필요없는 메모리를 해제하는 작업입니다.
  + 객체를 null 으로 작성하면 그 객체를 삭제하고 garbage collector 가 실행되면 그 객체는 WeakSet 에서도 해제됩니다.
```javascript
student3 = null;
console.log(roster);
/* 결과 : WeakSet {Object {name: 'Julia', age: 27, gender: 'female'}, Object {name: 'James', age: 26, gender: 'male'}} */
```

## Maps
- Set 과 Map 은 비슷합니다. 또한 WeakSet 과 WeakMaps 은 비슷합니다.
  + Map 은 키-값 페어의 collections 라서 고유합니다. Set 또한 고유한 값들의 collections 입니다.
  + WeakSet 과 WeakMap 는 객체가 garbage collected 되는 것을 막지 않습니다.
  + Set 은 배열, Map 은 객체가 대상이 됩니다.

### Map 생성, 수정
```javascript
const employees = new Map(); // Map 생성

//수정하기. set() 메소드로 key-value 묶음을 추가합니다.
//이미 있는 것을 추가하면 덮어쓰기만 하고 에러는 뜨지 않습니다.
employees.set('james.parkes@udacity.com', {
  firstName: 'James',
  lastName: 'Parkes',
  role: 'Content Developer'
});
employees.set('julia@udacity.com', {
    firstName: 'Julia',
    lastName: 'Van Cleve',
    role: 'Content Developer'
});
employees.set('richard@udacity.com', {
    firstName: 'Richard',
    lastName: 'Kalehoff',
    role: 'Content Developer'
});

console.log(employees);
/* 결과 : Map {'james.parkes@udacity.com' => Object {...}, 'julia@udacity.com' => Object {...}, 'richard@udacity.com' => Object {...}} */

//삭제하기. key-value 묶음을 삭제하려면 delete() 를 씁니다.
//없는 것을 삭제해도 에러는 없고 Map 은 그대로입니다. Set 때처럼 불리언 형태로 return 합니다.
employees.delete('julia@udacity.com');
employees.delete('richard@udacity.com');

//모든 key-value 묶음을 지우려면 clear() 를 씁니다.
employees.clear();
```
### Map 사용하기
- `.has()` 메소드를 사용해서 키를 전달해 Map 에서 key-value 묶음이 있는지를 체크합니다.
- `.get()` 메소드로 키를 전달해 값을 검색합니다.
```javascript
const members = new Map();
members.set('Evelyn', 75.68);
members.set('Liam', 20.16);
members.set('Sophia', 0);
members.set('Marcus', 10.25);

console.log(memebers.has('Xavier')); //false
console.log(members.has('Marcus')) //true

console.log(memebers.get('Evelyn')); // 결과 : 75.68
```

### Map 과 반복문
- `MapIterator` 사용하기 : Map 에서 `.keys()` 와 `.values()` 메소드를 사용해 새로운 반복자 객체 MapIterator 를 return 합니다. 새 변수에 이를 저장하고 `.next()` 으로 key-value 묶음을 출력합니다.
```javascript
// 위의 members 를 사용합니다.
let iteratorObjForKeys = members.keys();
iteratorObjForKeys.next(); // Object {value: 'Evelyn', done: false}
```
- for of 반복문 사용하기 : 다만 이 방식은 배열 형태로 return 합니다.  key-value 형태가 아닌 element 하나, element 둘 이렇게 합니다.
```javascript
for (const memeber of members) {
  console.log(member);
}
/*  ['Evelyn', 75.68] ['Liam', 20.16] ['Sophia', 0] ['Marcus', 10.25] */
//destructuring 사용해서 key-value 형태로 return 하기
for (const member of members) {
  const [key, value] = member;
  console.log(member);
}
```
- forEach 반복문 사용하기
```javascript
members.forEach((value, key) => console.log(key, value));
/*  
'Evelyn' 75.68
'Liam' 20.16
'Sophia' 0
'Marcus' 10.25 */
```
### WeakMaps
- WeakSet 이 개별 항목(items)들로 작동하고, WeakMap 은 key-value 묶음으로 작동한다는 점 빼고는 둘은 비슷합니다.
- 특징 : WeakMap 은 오직 객체를 키의 형태로 저장합니다. 반복할수 없습니다. `.clear()` 메소드를 쓸 수 없습니다.
```javascript
const book1 = { title: 'Pride and Prejudice', author: 'Jane Austen' };
const book2 = { title: 'The Catcher in the Rye', author: 'J.D. Salinger' };
const book3 = { title: 'Gulliver’s Travels', author: 'Jonathan Swift' };

const library = new WeakMap();
library.set(book1, true);
library.set(book2, false);
library.set(book3, true);

//WeakMap 은 키 형태로 객체를 저장하기에 아래처럼 추가할 수 없습니다.
libray.set('The Grapes of Wrath', false); //오류 : Uncaught TypeError: Invalid value used as weak map key
```
- WeakMaps 는 객체를 키 형태로 독점적으로 사용해서 garbage collection 을 사용합니다.
  + 객체를 null 로 설정해서 객체를 삭제합니다. 그리고 garbage collector 가 실행되면 이전에 점유했던 객체는 나중에 프로그램에서 쓰기 위해 해제됩니다.
```javascript
book1 = null;
console.log(library); // book1 이 사라진 채로 출력됩니다.
/* WeakMap {Object {title: 'The Catcher in the Rye', author: 'J.D. Salinger'} => false, Object {title: 'Gulliver’s Travels', author: 'Jonathan Swift'} => true} */
```
  + 알아서 객체를 참조하는 키를 삭제해주니까 WeakMaps 는 메타 데이터가 포함된 객체 그룹을 만드는 효율적인 방법이 될 수 있습니다.

## Promises
- Promise 는 자바스크립트의 새로운 Promise 생성자 함수인 `new Promise()` 를 사용해서 만듭니다.
- Promise 는 *비동기적* 으로 수행될 작업을 시작하고 요청자는 정규 작업으로 돌아가도록 해줍니다. (마치 제품을 주문하고 계산대에서 끝날 때까지 기다리는게 아니라 일상으로 돌아와서 일을 하는 것과 비슷합니다.)
  + Promise 는 비동기적으로 편리하게 만들 수 있어 자주 사용됩니다.
  + Promise 를 만들 때 비동기적으로 실행될 코드를 제공해야 합니다. 이를 생성자 함수의 인수로 제공합니다.
```javascript
new Promise (function () {
  window.setTimeout(function createSundae(flavor = 'chocolate') {
    const sundae = {};
    // 아이스크림을 요청
    // 콘을 준비
    // 국자를 준비
    // 국자로 퍼서 콘에다 올리기
  }, Math.random() * 2000);
});
```
  + 이 코드는 요청을 한 후 몇 초 후에 시작하는 promise 입니다.

### 성공적인 요청이나 실패한 요청을 표시하기
- 자바스크립트는 요청이 성공이나 실패했음을 resolve 와 reject 함수로 알려줍니다.
- resolve 는 요청이 성공했을 때 호출되어 표시해주는 함수입니다.
```javascript
new Promise(function (resolve, reject) {
  window.setTimeout(function createSundae(flavor = 'chocolate') {
    const sundae = {};
    // 아이스크림을 요청
    // 콘을 준비
    // 국자를 준비
    // 국자로 퍼서 콘에다 올리기
    resolve(sundae);
  }, Math.random() * 2000);
});
```
  + 성공적으로 sundae 가 만들어지면 resolve 메소드를 호출하고 return 할 데이터를 전달합니다.
- reject 는 요청에 문제가 있어 실패했을 때 사용합니다. 참고로 실패를 해도 데이터는 return 할 수 있습니다.
```javascript
new Promise(function (resolve, reject) {
  window.setTimeout(function createSundae(flavor = 'chocolate') {
    const sundae = {};
    // 아이스크림을 요청
    // 콘을 준비
    // 국자를 준비
    // 국자로 퍼서 콘에다 올리기
    if (iceCreamConeIsEmpty(flavor)) {
      reject(`그 맛이 다 떨어졌다.`)
    }
    resolve(sundae);
  }, Math.random() * 2000);
});
```
- Promise 생성자는 함수를 취해 일정 시간이 지나면 promise 는 성취되고, 성공하거나 실패한 결과를 알려줍니다.

### Promise 의 return
- promise 는 즉각적으로 객체를 return 합니다.
```javascript
const myPromiseObj = new Promise(function (resolve, reject) {
  //...
})
```
- 해당 객체는 `.then()` 메소드를 가지고 있습니다. 이 메소드는 promise 에서 만든 요청의 성공, 실패 여부를 알려주도록 해줍니다. `.then()` 메소드는 두 함수를 사용합니다.
  + 요청이 정상적으로 완료했을 경우에 실행하는 함수
  + 요청을 완료하지 못했을 때 실행할 함수
- `.then()` 에 전달한 첫 함수는 호출되어 promise 의 resolve 함수 사용한 데이터를 전달합니다.
  + 두 번째 함수가 호출되고 promise 의 reject 함수가 호출된 데이터를 전달합니다.
```javascript
mySundae.then(function(sundae) {
  console.log(`Time to eat my delicious ${sundae}`);
}, function(msg) {
  console.log(msg);
  self.goCry(); //실제 메소드는 아닙니다.
});
```
  + 첫 함수에 sundae 객체를 받게 됩니다. 그리고 두번째 함수에 reject 에서 썼던 '그 맛이 다 떨어졌다' 를 받습니다.

## Proxies
### 프록시
- 우선 프록시부터 알아봅니다. 프록시는 서버와 클라이언트의 양쪽 역할을 하는 중계 프로그램으로, 클라이언트로부터의 리퀘스트를 서버에 전송하고, 서버로부트의 리스폰스를 클라이언트에 전달합니다.
- 프록시 서버의 기본적인 동작은 클라이언트로부터 받은 요청을 다른 서버에 전송하는 것입니다. 클라이언트로부터 받은 요청 URI 를 변경하지 않고 그 다음의 리소스를 가지고 있는 서버에 보냅니다. 리소스 본체를 가진 오리진 서버로부터 되돌아온 응답은 프록시 서버를 경유해서 클라이언트에 돌아옵니다.
- HTTP 통신을 할 때, 프록시 서버를 여러 대 경유하는 것도 가능합니다. 체인과 같이 여러 프록시 서버를 경유해서 요청과 응답을 중계합니다. 중계할 때는 Via 헤더 필드에 경유한 호스트 정보를 추가해야 합니다.
- 프록시 서버를 사용하는 이유는 첫째로 캐시를 사용해서 네트워크 대역 등을 효율적으로 사용하는 것, 그리고 조직 내에 특정 웹 사이트에 대한 액세스 제한, 액세스 로그를 획득하는 정책을 철저하게 지키려는 목적으로 사용하는 경우도 있습니다.
- 프록시 사용 방법 : 캐시하는지 안하는지 여부 혹은 메시지를 변경하는지 안하는지로 구분합니다.
  - 캐싱 프록시 : 프록시로 응답을 중계하는 때에는 프록시 서버 상에 리소스 캐시를 보존해 두는 타입의 프록시입니다. 프록시에는 다시 같은 리소스에 대한 요청이 온 경우, 오리진 서버로부터 리소스를 획득하지 않고 캐시를 응답으로서 되돌려 주는 것이 있습니다.
  - 투명 프록시 : 프록시로 요청과 응답을 중계할 때 메시지 변경을 하지 않는 타입의 프록시입니다. 반대로 메시지에 변경을 가하는 타입의 프록시를 비투과(비투명) 프록시라고 합니다.
  
### 자바스크립트에서의 프록시
- 자바스크립트 프록시 는 한 객체가 다른 객체에 위치해 그 객체를 위해 모든 상호 작용을 처리해줍니다. (직접적인 요청, 해당 객체에 데이터 전달 등)
- 프록시는 `new Proxy()` 프록시 생성자로 만듭니다. 프록시 생성자는 두 항목을 사용합니다.
  + 프록시될 객체
  + 프록시될 객체를 처리할 모든 메소드들이 담긴 객체 (handler object)
- 프록시는 핸들러에서 사용하는 13 가지의 다른 trap 을 가지고 있습니다.

### 프록시를 통한 전달
```javascript
let richard = {status: 'looking for work'};
let agent = new Proxy(richard, {}); // 프록시될 객체와 빈 핸들러 객체
//trap 이 없다면 기본 동작이 타겟 객체에 전달됩니다.
agent.status // 결과 : 'looking for work'
```
- 프록시를 만들 때는 프록시 생성자에서 두 번째 객체로 전달될 핸들러 객체가 중요합니다.

### Get Trap
- `get` 메소드(프록시에서 쓸 떄는 trap 이라 불립니다.)는 프로퍼티에 대한 호출을 가로챕니다.
```javascript
const richard = {status : 'looking for work'};
const handler = {
  get(target, propName) {
    console.log(target); // 타겟은 첫 항목인 리차드 객체입니다.
    console.log(propName); // propName 은 프록시가 검사할 프로퍼티의 이름입니다.
  }
};
const agent = new Proxy(richard, handler);
/* 리차드 객체(agent 객체가 아님)와 연결되는 프로퍼티 이름을 알려줍니다. 'status' */
agent.status;
```
  + agent.status 에서 호출을 가로채 status 프로퍼티를 가져오고 get trap 을 실행합니다. 그러면 프록시 대상인 리차드 객체가 로그아웃되고 요청된 프로퍼티의 이름도 로그아웃됩니다. (실제 로그아웃은 아닙니다.)
- trap 을 사용할 때는 특정 trap 에 대한 모든 기능을 제공해야 합니다.

### 프록시 안의 타겟 객체에 접근하기
```javascript
const richard = {status : 'looking for work'};
const handler = {
  get(target, propName) {
    console.log(target);
    console.log(propName);
    //이 코드가 타겟 객체의 프로퍼티에 접근해 그것을 return 할 것입니다.
    return target[propName];
  }
};
const agent = new Proxy(richard, handler);
/*
1. 리차드 객체를 기록 2. 접근할 프로퍼티를 기록 3. richard.status 문자를 return
 */
agent.status;
```

### 프록시에서 정보를 직접 return 하기
```javascript
const richard = {status : 'looking for work'};
const handler = {
  get(target, propName) {
    return `He's following many leads, so you should offer a contract as soon as possible!`;
  }
};
const agent = new Proxy(richard, handler);
/* He's ~~~ 문장을 return 합니다. */
agent.status;
```
- 위 코드를 쓰면 프록시는 타겟 객체를 체크할 필요 없이 호출하는 코드에 직접 응답합니다.
- 그러면 프록시의 모든 프로퍼티에 접근할 때마다 `get` trap 이 대신 수행됩니다.
- `set` trap 은 프로퍼티를 바꿀 코드를 가로채기 위해서 사용됩니다.
```javascript
const richard = {status: 'looking for work'};
const handler = {
    set(target, propName, value) {
        if (propName === 'payRate') { // if the pay is being set, take 15% as commission
            value = value * 0.85;
        }
        target[propName] = value;
    }
}
const agent = new Proxy(richard, handler);
agent.payRate = 1000; // 배우의 임금을 1000 으로 설정합니다. (set trap)
agent.payRate; // 1000 * 0.85 가 배우의 임금입니다.
```
  + set trap 은 payRate 프로퍼티가 설정됐는지를 확인합니다. 됐다면 프록시(agent) 는 15%를 가져갑니다.
  + 배우의 임금을 1000 으로 설정했다면 실제 payRate 프로퍼티는 850 으로 설정됩니다.

### 프록시와 ES5 getter/setter
```javascript
//ES5
var obj = {
    _age: 5,
    _height: 4,
    get age() {
        console.log(`getting the "age" property`);
        console.log(this._age);
    },
    get height() {
        console.log(`getting the "height" property`);
        console.log(this._height);
    }
};
//ES6
const proxyObj = new Proxy({age: 5, height: 4}, {
    get(targetObj, property) {
        console.log(`getting the ${property} property`);
        console.log(targetObj[property]);
    }
});

obj.weight = 120; // 'getting the weight property' 없이 오직 120만 저장됩니다.
// get 에서 설정했기에 'getting the weight property' 도 출력이 가능합니다.
proxyObj.weight = 120;
```
-  프록시를 사용하면 객체가 초기화 될 때 각 속성에 대해 getters / setter를 사용하여 객체를 초기화 할 필요가 없습니다.

## Generator
- 기존의 자바스크립트는 함수가 실행되면 중간에 멈출 수 없이 끝까지 실행됐습니다. ES6 에서는 중간에 멈출 수 있습니다.
- generator 함수를 사용하면 됩니다. function 옆의 별표시를 붙이면 이 함수는 generator 가 됩니다.
```javascript
// * 위치는 어디라도 상관없습니다.
function* getEmployee() { /* */ }
function * getEmployee() { /* */ }
function *getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        console.log( name );
    }

    console.log('the function has ended');
}
```

### generator 와 iterator
- generator 가 수행되면 실제로 코드가 실행되지는 않고 반복자가 return 됩니다. 반복자는 generator 코드 안에서 실행될 수 있습니다.
```javascript
const generatorIterator = getEmployee();
generatorIterator.next(); // generator 안의 모든 코드가 멈추지 않고 실행됩니다.
```
- `yield` 는 generator 함수 안에서만 사용되는 새로운 키워드입니다.
  + `yield` 로 generator 를 멈출 수 있습니다.
```javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
        console.log(name);
        yield;
    }
    console.log('the function has ended');
}
```
  + `yield` 는 for of 반복문 안에 들어갔습니다. 이제 iterator.next() 를 실행하면 하나만 출력하고 멈춥니다.
  + 다시 next() 를 실행하면 두번째 값 하나만 나오고 멈춥니다.
- `yield` 로 바깥에 데이터를 전달하기
```javascript
function* getEmployee() {
    console.log('the function has started');

    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];

    for (const name of names) {
    //name 을 함수에 return 한 다음 실행을 중지합니다.
      yield name;
    }
    console.log('the function has ended');
}

const generatorIterator = getEmployee();
let result = generatorIterator.next();
result.value // is "Amanda"

generatorIterator.next().value // is "Diego"
generatorIterator.next().value // is "Farrin"
```

### 데이터를 generator 안/밖으로 전달하기
- `.next()` 메소드로 generator 안으로 다시 데이터를 전달할 수 있습니다.
```javascript
function* displayResponse() {
  const response = yield;
  console.log(`Your response is "${response}".`);
}
const iterator = displayResponse();

iterator.next(); //generator 함수 시작
iterator.next('hello udacity student') //데이터를 generator 안에 전달, yield 키워드를 교체합니다.
```
- 즉, `yield` 는 generator 를 멈추거나 데이터를 generator 밖으로 전달하는데 사용하고 `.next()` 메소드는 generator 안으로 데이터를 전달하는데 사용합니다.
```javascript
// 모든 기능을 활용한 코드입니다.
function* getEmployee() {
    const names = ['Amanda', 'Diego', 'Farrin', 'James', 'Kagure', 'Kavita', 'Orit', 'Richard'];
    const facts = [];

    for (const name of names) {
        // yield *out* each name AND store the returned data into the facts array
        facts.push(yield name);
    }

    return facts;
}

const generatorIterator = getEmployee();

// get the first name out of the generator
/* next() 에 대한 첫 호출은 일부 데이터를 전달하지만 어디에도 저장되지 않습니다. */
let name = generatorIterator.next().value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is cool!`).value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is awesome!`).value;

// pass data in *and* get the next name
name = generatorIterator.next(`${name} is stupendous!`).value;

// you get the idea
name = generatorIterator.next(`${name} is rad!`).value;
name = generatorIterator.next(`${name} is impressive!`).value;
name = generatorIterator.next(`${name} is stunning!`).value;
name = generatorIterator.next(`${name} is awe-inspiring!`).value;

// pass the last data in, generator ends and returns the array
const positions = generatorIterator.next(`${name} is magnificent!`).value;

// displays each name with description on its own line
positions.join('\n');
```
