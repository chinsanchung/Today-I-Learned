# udacity 'Obejct-Oriented JavaScript' 01- Functions at Runtime
## First Class Functions
- JavaScript 는 First Class Functions 입니다. 따라서 다른 언어와 달리 아래의 내용들이 가능합니다.
  + 변수 안에 저장됩니다.
  + 함수로부터 return 됩니다.
  + 다른 함수에서 인수로 실행됩니다.

- 'higher-order function' : 다른 함수를 return 하는 함수입니다.
```javascript
function alertThenReturn() {
  alert('Message 1!');

  return function () {
    alert('Message 2!');
  };
}

//출력
alertThenReturn(); // Message 1 만 출력할 뿐 내부 함수는 실행되지 않습니다.
const a = alertThenReturn(); //내부 함수를 출력하기 위해 변수를 선언합니다. 내부함수를 return 하기에 가능합니다.
a(); //이럴 경우 Message 2 만 출력합니다.
alertThenReturn()(); // () 을 두개 쓰면 alertThenReturn 함수와 내부의 익명 함수를 실행합니다.
```

## callback functions
- 'higher-order function' 에 인수로서 전달되는 함수를 'callback functions' 이라고 합니다.
```javascript
function callAndAdd(n, callbackFunction) {
  return n + callbackFunction();
}

function returnsThree() {
  return 3;
}

let result = callAndAdd(2, returnsThree);

console.log(result);
```

### array method
- 함수는 일반적으로 배열 메소드로 전달되고, 배열 내의 요소 (즉, 메소드가 호출 된 배열)에서 호출됩니다.

#### forEach()
- 'forEach()' :  배열의 각 요소에 대해 콜백 함수를 호출합니다. (for 반복문처럼 배열을 반복합니다.)
  + 'array.forEach(function callback(currentValue, index, array) { // function code here });'
  + 콜백 함수의 인수로는 현재 배열의 요소, 인덱스, 배열 자체 입니다.
```javascript
//단순한 형태의 function
function logIfOdd(n) {
  if (n % 2 !== 0) {
    console.log(n);
  }
}
//배열일 경우 logIfOdd 함수를 콜백함수로 사용합니다.
[1, 5, 2, 4, 6, 3].forEach(function logIfOdd(n) {
  if (n % 2 !== 0) {
    console.log(n);
  }
});

//간단한 형태의 forEach 입니다.
[1, 5, 2, 4, 6, 3].forEach(logIfOdd);
```

#### map()
- 'map()' :  배열의 각 요소에 대해 콜백 함수를 호출하는 점은 'forEach()' 와 같지만 'map()' 은 콜백 함수에서 return 된 값으로 새로운 배열을 만들어 return 합니다. (forEach() 는 값을 return 하지 않습니다.)
```javascript
const names = ['Max', 'Jin', 'Syoko'];

const nameLengths = names.map(function (name) {
  return name.length
});
//nameLengths 는 [3, 3, 5] 를 가지는 새로운 배열이 됩니다.
```
  + 순서 : names 배열의 값들을 호출해서 첫 번째 인덱스를 name 인수에 저장해 name.length 로 return 합니다. 그 후 나머지 두 요소로 반복해서 실행합니다.
#### filter()
- 'filter()' : 'map()' 메소드와 비슷하면서도 차이점이 있습니다.
  + 같은 점 : 배열로 호출되며 함수를 인수로 사용하고 새로운 배열을 return 합니다.
  + 다른 점 : 'filter()' 에 전달된 함수는 테스트에 사용되고 그 테스트를 통과한 배열의 항목만이 새로운 배열에 포함됩니다.
```javascript
const names = ['David', 'Richard', 'Veronika'];

const shortNames = names.filter(function(name) {
  return name.length < 6;
});
console.log(shortNames); //결과 : ['David']
```
  + names 배열을 호출, 첫 번쨰 값 David 를 name에 저장 후 length 로 구한 후 조건과 비교했고 통과됐습니다.
  + Richard, Veronika 는 name 에 저장되고 length 로 구했지만 조건을 만족하지 않아 탈락됩니다.

## scope
- 자바 스크립트의 변수는 전통적으로 블록 범위가 아닌 함수의 범위에서 정의됩니다. 함수를 입력하면 범위가 변경되므로 해당 함수 내부에서 정의 된 변수는 해당 함수 외부에서 사용할 수 없습니다.
  + 블록 내부에 정의 된 변수가있는 경우 (예 : if 문 내에서) 해당 변수는 해당 블록 외부에서 사용할 수 있습니다
- ES6 구문은 'let' 및 'const' 키워드를 사용하여 변수를 선언하는 동안 추가 범위를 허용합니다. 이 키워드는 자바 스크립트에서 블록 범위 변수를 선언하는 데 사용되며 대체로 'var' 를 대신합니다.
- 'scope chain' : 함수를 호출할 떄 함수 interpreter 는 항상 child 함수에서부터 지역 변수를 찾습니다. 만약에 없다면 부모 함수, 부모 함수도 없다면 전역 환경에서 검색합니다. 있다면 값을 검색하고, 마지막에도 없다면 변수는 undefined 로 출력합니다.
```javascript
const myName = 'Andrew';
// Global variable

function introduceMyself() {

  const you = 'student';

//myName 은 전역 변수라서 사용 가능하고, you 는 부모 함수인 introduceMyself 의 지역변수 you 이므로 사용이 가능합니다.
  //you 를 인수로 삼지 않아도 부모 함수이기에 사용할 수 있습니다.
  function introduce() {
    console.log(`Hello, ${you}, I'm ${myName}!`);
  }

  return introduce();
}
```
- 'variable shadowing' : 만약 범위 내에서 이름만 같고 내용이 다른 변수가 있다면 지역 변수는 전역 변수를 일시적으로 'shadowing' 합니다.
  +  다른 맥락에서 변수 간에 이름이 중복되는 경우 모두 내부에서 외부 범위로 scope chain 을 이동하여 해결됩니다. 따라서 동일한 이름을 가진 모든 지역 변수가 더 넓은 범위의 변수보다 우선 적용됩니다.
```javascript
const symbol = '¥';

function displayPrice(price) {
  const symbol = '$';
  console.log(symbol + price);
}
//결과는 $80 입니다. 내부에서 선언된 지역변수 symbol 이 전역변수 symbol 을 shadowing 했습니다.
displayPrice('80');
```

## closures
- 'closures' : scope 에 대한 연결을 유지하는 함수의 프로세스입니다. 외부의 환경과 선언된 함수와의 연관성을 다룹니다.
  + 모든 함수는 closures 를 하나씩 가지고 있습니다. scope chain 에 따라 모든 함수는 결국 전역 범위에서 닫히기 때문입니다.
  + 그러나 함수 안에 내부 함수가 있을 경우 'closures' 는 큰 힘을 발휘합니다.
- 'closures' 와 'scope' 는 굉장히 밀접한 관계를 가지고 있습니다.
```javascript
function remember(number) {
//내부 함수가 number 를 return 함으로써 number 를 닫습니다. 부모 함수 remember 는 내부 함수와의 관계를 return 합니다.
//닫는다는 캡쳐한다와 같은 뜻입니다.
    return function() {
        return number;
    }
}

const returnedFunction = remember(5);

//예제 2.
const myName = 'Andrew';

function introduceMyself() {
  const you = 'student';
//introduce 함수는 클로저를 생성해 부모 범위의 you 와 전역 범위의 myName 에 접근할 수 있습니다.
  //여기서 introduce 함수는 변수 you 밖에서 닫혔지만, 함수가 닫히더라도 introduce 함수는 you 변수에 접근할 수 있습니다.
  function introduce() {
    console.log(`Hello, ${you}, I'm ${myName}!`);
  }

  return introduce();
}

introduceMyself();
//결과: 'Hello, student, I'm Andrew!' introduceMyself 함수를 선언할 시, introduce 함수는 여전히 you 변수와 관계를 맺고 있습니다.
```
- 클로저 는 비공개(private) 상태로 클로저 외부에서 내부 변수를 조작하지 못하게 만듭니다.
```javascript
function myCounter() {
  let count = 0;
//return 된 함수가 부모의 지역 변수 count 에 접근함으로써 private scope 를 생성했습니다.
  return function () {
    count += 1;
    return count;
  };
}

let counter = myCounter();
//클로저는 counter.count; 나 count; 등으로 클로저 밖에서 부모 변수 count 를 조작할 수 없게 보호합니다.
```

### garbage collection
- 자바스크립트는 데이터를 더 이상 참조할 수 없는 경우 garbage 로 수집되어 나중에 폐기합니다. 하지만 클로저로 내부 함수가 부모의 변수를 사용하는 경우 계속 참조할 수 있는 한 데이터는 유지됩니다.


## immediately-invoked function
### 함수 선언과 함수 표현
- 함수 선언 : 선언할 변수는 필요없습니다. 단순한 선언이며 함수 자신의 값을 return 하지 않습니다.
```javascript
function returnHello() {
  return 'Hello!';
}
```
- 함수 표현 : 함수 표현은 값을 return 합니다. 익명 함수와 선언적 함수가 있습니다.

### immediately-invoked function (IIFE)
- 이 함수는 정의된 후 즉시 호출됩니다. 괄호로 함수를 묶고 다시 뒤에 () 를 붙여 사용합니다.
```javascript
(function sayHi(){
    alert('Hi there!');
  }
)();
//괄호 () 를 안쪽에 넣어서 선언할 수도 있습니다. 둘 중 어떤 것을 사용할지는 자유입니다.
(function sayHi(){
    alert('Hi there!');
  }());
```
- 인수를 뒤의 괄호에 붙여 함수를 실행하는데 사용할 수 있습니다.
```javascript
(function (name){
    alert(`Hi, ${name}`);
  }
)('Andrew');
```
- IIFE 는 비공개 범위(private scope) 를 사용할 수 있습니다. 함수에 내부 함수를 만든 경우 클로저와 마찬가지로 함수 밖에서 접근할 수 없는 범위를 유지하고, garbage scope 에 버리지 않게 만듭니다. 또한 변수 간 이름의 충돌도 막습니다.
