# ES6 문법들 (Udacity)
- `let` 과 `const` 는 이미 다뤄서 생략합니다.

## 문자열 표기법
- 과거에는 ('ddd' + ddd.dd + 'ddd') 이런 식이었지만 ES6에서는 더 편리한 방법을 지원합니다.
  + `ddd ${ddd.dd} ddd` 이 방법이 문자열 표기법입니다. 엔터키로 다른 줄도 추가해서 작성이 가능합니다.

## Destructuring
- 배열이나 객체에서 데이터 값들을 추출해서 변수로 만들 수 있습니다.
```javascript
//배열
const point = [10, 25, -34];

const x = point[0];
const y = point[1];
const z = point[2];
console.log(x, y, z);  //결과: 10 25 -34
//객체
const gemstone = {
  type: 'quartz',
  color: 'rose',
  carat: 21.29
};

const type = gemstone.type;
const color = gemstone.color;
const carat = gemstone.carat;

console.log(type, color, carat);  //결과: quartz rose 21.29
```

### Destructuring
- Perl 이나 Python 에서 영감을 얻었습니다. 왼쪽의 배열이나 객체에서 추출할 element 를 지정할 수 있습니다.
  + 위의 방식보다 더 적은 코드로 값들을 변수로 지정하게 됩니다.
```javascript
//배열
const point = [10, 25, -34];

//x, y, z 는 배열의 값들을 저장할 변수입니다.
const [x, y, z] = point;
//const [x, , z] = point; 로 하면 25를 빼고  10, -34 만 써서 변수를 만들게 됩니다.

console.log(x, y, z); //결과: 10 25 -34

//객체
const gemstone = {
  type: 'quartz',
  color: 'rose',
  carat: 21.29
};
//type, color, carat 는 객체의 프로퍼티를 저장할 변수입니다.
const {type, color, carat} = gemstone;
//const {color} = gemstone; 이라 할시 gemstone 객체에서 color 프로퍼티만 선택합니다.

console.log(type, color, carat);
```
- 배열에서 여러 값들이 있을 때 지정해서 추출하는 것도 가능합니다.
```javascript
const things = ['red', 'basketball', 'paperclip', 'green', 'computer', 'earth', 'udacity', 'blue', 'dogs'];

const [one, , , two, , , , three] = things;


const colors = `List of Colors
1. ${one}
2. ${two}
3. ${three}`;

console.log(colors); //결과 : red green blue
```
- 객체의 메소드도 위의 방식으로 추출할 수 있습니다. 하지만 메소드 내부의 `this` 는 더 이상 원래의 객체에 접근할 수 없게 됩니다.

## 객체 문자열 속기 (Object literal shortand)
- 보통 객체를 선언할 경우 같은 변수 이름을 중복해서 쓰고는 합니다. 이제는 줄여서 작성할 수 있습니다.
```javascript
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = {
  type: type,
  color: color,
  carat: carat
};

//속기해서 씀
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = {
  type,
  color,
  carat,
};
```
- 메소드도 줄여서 작성이 가능합니다.
```javascript
const gemstone = {
  //원래 방식
  calculateWorth: function() {
    ...
  }
  //속기
  calculateWorth() { ... }
}
```

## 되풀이 (iteration)
- 원래 iteration 은 for 반복문에서 쓰이고 있었습니다. (let i = 0) 할 떄의 변수 i 가 iteration 입니다.
- ES6 부터는 iteration 에 대한 새로운 기능들이 추가됐습니다.

### 기존의 for 반복문들
- for 반복문과 for...in 반복문을 결합해 반복 가능한 모든 유형의 데이터를 반복합니다.
  + 기본적으로 String, Array, Map 및 Set 데이터 유형이 포함됩니다. 여기에는 객체 데이터 유형은 없습니다.(객체는 반복이 안됩니다)
- 일반적인 for 반복문의 단점은 반복문을 추적할 카운터와 반복문의 종료 조건을 정해야 한다는 점입니다.
  + 배열에서는 적합하지만 일부 데이터는 배열처럼 구조화되지 않아서 for 반복문에는 부적합합니다.
- for...in 반복문은 계산 논리와 종료 조건을 제거해 for 반복문의 약점을 개선합니다.
```javascript
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in digits) {
  console.log(digits[index]);
}
```
  + 하지만 배열의 값에 접근하기 위해서 index 를 사용해야 하는 점은 여전히 단점이 됩니다.
  + 또한 배열에 메소드를 추가할 경우 그 property 자체가 반복문에서 나타납니다.
```javascript
Array.prototype.decimalfy = function () {
  for (let i = 0; i < this.length; i++) {
    //소수의 자릿수의 길이를 제한하는 toFixed 입니다.
    this[i] = this[i].toFixed(2);
  }
};
const digits = [0, 1, 2, 3 ,4];

for (const index in digits) {
  console.log(digits[index]);
}

//결과 : 0, 1, 2, 3, 4, function () {for (let i = 0; i < this.length; i++) { this[i] = this[i].toFixed(2); }}
```

### for of 반복문
- 반복 가능한 모든 유형의 데이터를 반복하는데 사용합니다.
  + for...in 반복문과 비슷하게 작성하지만 index 를 없애고 사용할 수 있습니다.
```javascript
const digits = [0, 1, 2];
for (const digit of digits) {
  console.log(digit);
}
```
  + 참고로 값의 모음인 객체, 배열은 복수형으로 작성하면 for of 반복문을 쓸 때 단수형으로 이름을 작성할 수 있게 됩니다. 복수형으로 작성합시다.
- for of 반복문은 다른 반복문과 다르게 언제든지 멈출 수 있습니다.
```javascript
const digits =[0, 1, 2, 3, 4];
for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;
  }
  console.log(digit);
}
//결과 : 1, 3
```
- 새로운 프로퍼티로 메소드를 넣더라도 for of 반복문에서는 괜찮습니다. for of 반복문은 오직 객체 내부의 값들만 반복합니다.
```javascript
Array.prototype.decimalfy = function () {
  for (let i = 0; i < this.length; i++) {
    //소수의 자릿수의 길이를 제한하는 toFixed 입니다.
    this[i] = this[i].toFixed(2);
  }
};
const digits = [0, 1, 2, 3 ,4];

for (const digit of digits) {
  console.log(digit);
}
//결과 : 0, 1, 2, 3, 4
```
- 연습문제
```javascript
/*
 * Programming Quiz: Writing a For...of Loop (1-4)

    loops through each day in the days array
    capitalizes the first letter of the day
    and prints the day out to the console
*/
const days = ['sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday'];
//정답
for (const day of days) {
    console.log(day.charAt(0).toUpperCase() + day.slice(1));
}
```

## Spread operator
- `Spread operator` 는 점 세개로 반복가능한 객체들을 여러 element 로 확장하거나 확산하는 기능을 제공합니다.
```javascript
//배열
const alphabets = ["a", "b", "c", "d"];
console.log(...books); //결과: a b c d
//객체
const numbers = new Set([1, 2, 3, 4]);
console.log(...numbers); //결과: 1, 2, 3, 4
```
  + 배열과 객체가 개별 element 확장했음을 알 수 있습니다.
- `Spread operator` 는 배열을 결합할 때 유용하게 쓰입니다.
  + 만약 `Spread operator` 를 쓰기 전에 배열을 합해야 한다면 배열의 `concat()` 메소드를 쓰면 됩니다.
```javascript
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];
//방법 1. 보통의 방식
const produce = fruits.concat(vegetables);

//방법 2.  Spread 연산자
//const produce = [...fruits, ...vegetables];
console.log(produce); //결과: ["apples", "bananas", "pears", "corn", "potatoes", "carrots"]
```

## 나머지 매개변수 (Rest parameter)
- 점 세개로 작성된 `Rest parameter` 를 사용하면 배열의 element 의 수가 불명확하게 나타나게 됩니다. 이는 몇 가지 측면에서 도움을 줍니다.
1. 우선 배열의 값을 변수에 할당할 때 입니다.
```javascript
const order = [20.17, 18.67, 1.50, "cheese", "egg", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(total, subtotal, tax, items); //결과: 20.17 18.67 1.5 ["cheese", "egg", "milk", "bread"]
```
  + 배열 order 의 값들을 우선 각각 변수에 할당한 후, `Rest parameter` 를 사용해 items 에 나머지 값들을 할당했습니다.

2. 가변인자 함수 (Variadic functions)를 사용할 때입니다. 가변인자 함수는 무한한 개수의 인수를 사용하는 함수입니다.
```javascript
sum(1, 2);
sum(1, 2, 3, 4);
```
  + `sum()` 함수는 가변인자 함수로 인수의 양이 다르더라도 합을 계산해냅니다.
3. 다음은 arguments 객체를 사용할 때입니다.
```javascript
/* 이 함수는 실행은 되지만 sum() 은 인수가 무한하게 할 수 있는데 여기엔 매개변수가 없습니다.
또한 arguments 객체를 처음 쓰는 사람이라면 이 객체가 어디서 온건지 이해하지 못할 것입니다. */
function sum() {
  let total = 0;
  for(const argument of arguments) {
    total += argument;
  }
  return total;
}
```
- `rest parameter` 사용하기
  + `rest parameter` 로 sum() 함수를 더 쉽게 이해하게 만들어줍니다.
```javascript
funtionc sum(...nums) {
  let total = 0;
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
