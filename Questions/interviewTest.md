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
  var done = false;
  while (!done) {
    done = true;
    for (var i = 1; i < array.length; i += 1) {
      if (array[i - 1] > array[i]) {
        done = false;
        var tmp = array[i - 1];
        array[i - 1] = array[i];
        array[i] = tmp;
      }
    }
  }

  return array;
}

var numbers = [12, 10, 15, 11, 14, 13, 16];
bubbleSort(numbers);
console.log(numbers);
```
```javascript
//출처: https://stackoverflow.com/questions/7502489/bubble-sort-algorithm-javascript
var a = [33, 103, 3, 726, 200, 984, 198, 764, 9];

function bubbleSort(a)
{
    var swapped;
    do {
        swapped = false;
        for (var i=0; i < a.length-1; i++) {
            if (a[i] > a[i+1]) {
                var temp = a[i];
                a[i] = a[i+1];
                a[i+1] = temp;
                swapped = true;
            }
        }
    } while (swapped);
}

bubbleSort(a);
console.log(a);
```
- 버블 정렬 알고리즘 : 버블 정렬은 간단한 정렬 알고리즘입니다. 알고리즘은 데이터 세트의 시작 부분에서 시작됩니다. 처음 두 요소를 비교하고 첫 번째 요소가 두 번째 요소보다 큰 경우 두 요소를 서로 바꿉니다. 인접한 요소 쌍마다 데이터 집합의 끝까지이 작업을 계속합니다. 그런 다음 처음 두 요소로 다시 시작하여 마지막 단계에서 스왑이 발생하지 않을 때까지 반복합니다. 이 알고리즘의 평균 시간과 최악의 경우 성능은 O (n2)이므로 큰 순서가 아닌 큰 데이터 세트를 정렬하는 데 거의 사용되지 않습니다. 버블 정렬은 적은 수의 항목을 정렬하는 데 사용할 수 있습니다 (점근 적으로 비효율적 인 것은 높은 패널티가 아닙니다). 버블 정렬은 거의 정렬 된 모든 길이의 목록에서 효율적으로 사용할 수 있습니다 (즉, 요소가 크게 어둡지 않은 경우). 예를 들어, 하나의 위치 (예 : 0123546789 및 1032547698)로 임의의 수의 요소가 부재 한 경우 거품 정렬의 교환은 첫 번째 패스에서 순서대로 정렬하고 두 번째 패스는 모든 요소를 ​​순서대로 찾게되므로 정렬 순서는 단지 2n 시간 걸립니다.
