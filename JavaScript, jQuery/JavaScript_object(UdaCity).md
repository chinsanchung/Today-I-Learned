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
