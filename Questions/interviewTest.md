# Coding Questions
![출처](https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/questions/coding-questions.md)
*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```
foo = 1020

*Question: What will be the output of the code below?*
```javascript
console.log(0.1 + 0.2 == 0.3);
```
false(소수는 더할 경우 완벽한 0.3이 나오지 않음. 0.30000000000000004 != 0.3)

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```
```javascript
//answer
function add(x) {
    return function(y) {
        return x + y;
    };
}
```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```
우선 split 에서 ""으로 정했으니 글자 하나하나 자릅니다. 그리고 reverse 로 역순으로 뒤집고, join 을 통해 큰따옴표로 다시 묶습니다.
```javascript
//answer
"goh angasal a m'i"
```

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```
`window.foo === bar` 가 정답입니다.

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```
우선  IIFE 로 인해서 `alert(foo+bar)` 먼저 나옵니다. 그 값은 Hello World 입니다.
그 다음 밑에 있는 경고창이 뜨는데, 내부 스코프에 선언한 bar 때문에 밖에서는 사용이 불가능합니다. 그래서 `ReferenceError: bar is not defined` 가 나오고 경고창은 뜨지 않습니다.

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```
`foo.length` 는 push 로 인해 값이 두 개 들어갔으므로 2가 됩니다.
*Question: What is the value of `foo.x`?*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```
foo.x 는 undefined 로 출력됩니다. 그런데 bar 안에는 `{n : 1, x : {n : 2}}` 가 됐습니다. 그래서 bar.x 는 `{n : 2}`라는 값이 제대로 나옵니다.
*Question: What does the following code print?*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
Promise.resolve().then(function() {
  console.log('three');
})
console.log('four');
```
- 우선 two 부분부터 살펴봅니다. 이것은 자바스크립트 엔진의 task que 에 대한 문제입니다. 두 번째의 setTimeout 이 0 이므로 딜레이 없이 1, 2, 3 이 출력될 것 같지만 결과는 다릅니다. 자바스크립트에서 비동기로 호출되는 것들은 콜스텍이 아니라 테스크 큐에 저장됩니다. 비동기로 실행되는 함수(핸들러)는 항상 먼저 실행되지 않습니다. 이것과 관련해선 다음의 예제로 이해할 수 있습니다. [출처](http://asfirstalways.tistory.com/362)
```javascript
function test1() {
  console.log('test1');
  test2();
}
function test2() {
  setTimeout(function() {
    console.log('test2')
  }, 0)
  test3();
}
function test3() {
  console.log('test3')
}
test1();
```
  - test1 이 콘솔에 우선 찍히고, 그 다음 test2 를 호출하면서 setTimeout 함수를 실행하는데 이것은 콜스텍에 저장되더라도 바로 실행되진 않습니다. 이 함수(핸들러)는 콜 스텍이 아니라 이벤트 큐 영역에 들어가서입니다.
  - 그 다음 test3 함수가 콜스텍으로 들어갑니다. test3 을 콘솔에 띄워 작업을 마치면 test3 함수는 콜스텍에서 pop 됩니다. 이어서 test2 함수와 test1 함수도 콜스텍에서 pop 됩니다.
  - 이때 이벤트 루프의 콜스텍은 비어있게 됩니다. 이 시점에서 큐의 헤드에서 하나의 이벤트를 가져와 콜 스텍으로 넣습니다.
  - 넣은 함수는 setTimeout 의 콜백 익명함수입니다. 그래서 결국 결과는 `test1, test3, test2` 순서로 출력이 됩니다.
*Question: What is the difference between these four promises?*
```javascript
doSomething().then(function () {
  return doSomethingElse();
});

doSomething().then(function () {
  doSomethingElse();
});

doSomething().then(doSomethingElse());

doSomething().then(doSomethingElse);
```

*씨이랩 코딩테스트: sort 없이 숫자 배열을 오름차순으로 정렬하기*
```javascript
//출처: https://stackoverflow.com/questions/16243366/sorting-array-with-numbers-without-sort-method
function bubbleSort(array) {
  let done = false;
  while (!done) {
    done = true;
    for (let i = 0; i < array.length; i++) {
      if (array[i] > array[i + 1]) {
        done = false;
        let x = array[i];
        array[i] = array[i + 1];
        array[i + 1] = x;

      }
    }
  }
  return array;
}
var numbers = [12, 10, 15, 11, 14, 13, 16];
bubbleSort(numbers);
console.log(numbers);
```
- 버블 정렬 알고리즘 : 버블 정렬은 간단한 정렬 알고리즘입니다. 알고리즘은 데이터 세트의 시작 부분에서 시작됩니다. 처음 두 요소를 비교하고 첫 번째 요소가 두 번째 요소보다 큰 경우 두 요소를 서로 바꿉니다. 인접한 요소 쌍마다 데이터 집합의 끝까지이 작업을 계속합니다. 그런 다음 처음 두 요소로 다시 시작하여 마지막 단계에서 스왑이 발생하지 않을 때까지 반복합니다. 이 알고리즘의 평균 시간과 최악의 경우 성능은 O (n2)이므로 큰 순서가 아닌 큰 데이터 세트를 정렬하는 데 거의 사용되지 않습니다. 버블 정렬은 적은 수의 항목을 정렬하는 데 사용할 수 있습니다 (점근 적으로 비효율적 인 것은 높은 패널티가 아닙니다). 버블 정렬은 거의 정렬 된 모든 길이의 목록에서 효율적으로 사용할 수 있습니다 (즉, 요소가 크게 어둡지 않은 경우). 예를 들어, 하나의 위치 (예 : 0123546789 및 1032547698)로 임의의 수의 요소가 부재 한 경우 버블 정렬의 교환은 첫 번째 패스에서 순서대로 정렬하고 두 번째 패스는 모든 요소를 ​​순서대로 찾게되므로 정렬 순서는 단지 2n 시간 걸립니다.

*퀵 정렬-기본: 배열 정렬하기*
```javascript
function quickSort(arr) {
  //기본조건: 재귀를 끝내는 기본 조건
  if(arr.length < 2) return arr;
  //가운데에 놓을 값 하나. 가운데로 하는 이유는
  let pivot = arr[Math.floor(arr.length/2)];
  let middle = arr.filter(data => data === pivot )
  //왼쪽의 값들. 재귀함수로 재실행
  let lows = quickSort(arr.filter(data => data < pivot ))
  //오른쪽의 값들. 재귀함수로 재실행
  let greater = quickSort(arr.filter(data => data > pivot ))

  return lows.concat(middle).concat(greater);
}
console.log(quickSort([0, 50, 6, 3, 5, 2, 1, 7]));
```
퀵 정렬의 pivot 을 배열의 가운데로 지정하는 이유는 만약 맨 앞(arr[0])으로 할 경우 실행시간이 O(n제곱)이 되기 때문입니다.
퀵 정렬은 최악의 경우 O(n제곱)이 되고, 최고는 O(n * log n)입니다. 가운데로 pivot 을 지정하면 실행시간을 최고로 맞출 수 있게 됩니다.
- `Math.floor()` : Math.floor() 함수는 주어진 숫자를 내림(floor) 처리합니다. 만약 1.87 을 내림한다면 1이 될 것입니다. Math의 정적 메소드입니다. 따라서, Math 객체를 생성하여 메소드를 사용하기보다, 항상 Math.floor()로 사용해야합니다(Math는 생성자가 아닙니다).[MDN 블로그 링크](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
- 참고로 `Math.ceil()` 은 주어진 숫자를 올림합니다. 예를 들어 1.78은 올림하면 2가 됩니다.

*앞뒤가 같은 글자를 출력하기*
```javascript
function checkString(value) {
  if(value.reverse() === value) return value;
}
```
