# JavaScript의 함수
## 익명 함수와 선언적 함수
- 'let 함수 = function () {};' : 일반적인 함수의 형태입니다. 함수를 실행하는 것을 함수를 호출한다고 합니다.
  + 함수의 이름이 같을 경우 뒤에 선언한 함수가 앞의 함수를 덮어씌웁니다.
- 'function 함수() { }' : 선언적 함수입니다.
  + 웹 브라우저는 script 태그 내부를 읽기 전에 선언적 함수부터 읽습니다.
```javascript
//익명 함수
let a = function () { alert('A'); }
//선언적 함수
function a() { alert('B'); }
//함수 호출
a();
```
  + 선언적 함수가 먼저 생성되고 익명 함수가 나중에 생성됩니다. 나중에 생성된 것이 덮어씌우므로 결과는 A입니다.

## 매개변수
- 매개변수 : 함수를 호출하는 쪽과 함수를 연결하는 매개가 되는 변수입니다.
  + 함수가 허용하는 매개변수보다 많은 수를 선언하면 뒤의 매개변수는 무시하고, 적은 수를 선언하면 지정하지 않은 수는 undefined로 입력됩니다.

- 매개변수 와 인수 (Arguments)
  + 처음에는 뭔가가 매개 변수인지 인수인지를 아는 것이 약간 까다로울 수 있습니다. 주요 차이점은 코드에 나타나는 위치입니다. 매개 변수는 항상 변수 이름이되고 함수 선언에 나타납니다. 반면에 인수는 항상 값(JavaScript 데이터 유형 - 숫자, 문자열 등)이 될 것이며 함수가 호출되거나 호출 될 때 항상 코드에 나타납니다.
  + 매개 변수는 함수가 사용할 함수에 전달되는 데이터를 저장하는 데 사용되는 변수입니다.
  + 인수는 함수가 호출 될 때 함수에 전달되는 실제 데이터입니다.

## 가변 인자 함수
- 매개변수의 개수가 변할 수 있는 함수입니다. 사용했을 때 매개변수를 모두 활용합니다.

## 내부 함수
- 내부 함수는 함수 내부에 선언하는 함수입니다.
  + 규모가 커져 많은 코드가 생길 경우의 충돌을 막습니다.
  + 내부 함수를 쓰면 함수 외부에 같은 함수가 있어도 내부 함수를 우선 실행합니다.
  + 내부 함수는 그 함수에 포함되는 함수 내에서만 사용 가능합니다. 밖에서는 사용이 안됩니다.
```javascript
function 외부함수() {
  function 내부함수01 () {

  }
  function 내부함수02 () {

  }
  function 내부함수03 () {

  }
}
```

## 콜백 함수
### 콜백 함수
- 콜백 함수 : 함수를 매개변수로 전달하는 함수입니다.
  + 함수를 매개변수로 넣을 수가 있습니다.
```javascript
//함수 선언
function callTenTimes(callback) {
  for (let i = 0; i < 10; i++) {
    //매개변수로 전달된 함수를 호출합니다.
    callback();
  }
}
//변수 선언
let callback = function () {
  alert('함수 호출');
};
//함수를 호출
callTenTimes(callback);
```
  + 실행하면 alert('함수 호출')을 10번 반복해서 출력합니다.

```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    alert(i);
  });
}
```
  + 실행 결과는 0, 1, 2 를 순서대로 출력하지 않고 3을 세 번 출력합니다.
  + 이유는 setTimeout() 함수를 호출하는 시점이 반복문이 모드 끝난 이후이기 떄문입니다.
```javascript
//클로저를 사용해 반복문을 실행하는 동안 클로저를 만들어 변수 closed_i에 값을 저장합니다.
for (let i = 0; i < 3; i++) {
  function (closed_i) {
    setTimeout(function() {
        alert(closed_i);
    });
  }
}
```
### 익명 콜백 함수  
- 콜백 함수는 익명으로 사용하는 경우가 많습니다.
```javascript
//함수 선언
function callTenTimes(callback) {
  for (let i = 0; i < 10; i++) {
    //매개변수로 전달된 함수를 호출합니다.
    callback();
  }
}
//함수 호출
callTenTimes(function () {
  alert('함수 호출');
});
```
## 클로저
- 클로저 : 외부에서도 지역 변수를 사용할 수 있도록 지역 변수를 남겨두는 현상입니다.
  + 남겨두더라도 리턴된 클로저 함수를 사용해야만 그 지역 변수를 사용할 수 있습니다.
```javascript
//함수 선언
function test(name) {
  let output = 'hello' + name;
  return function () {
    alert(output);
  };
}
//출력
let test_1 = test('JavaScript');
let test_2 = test('Java');
//함수 호출
test_1();
test_2();
```

## 자바스크립트 내장 함수
### 코드 실행 함수
- 'eval(string)' : string을 자바스크립트 코드로 실행합니다.
```javascript
//문자열 생성
let willEval = '';
willEval += 'let number = 10;';
willEval += 'alert(number);';
//함수 호출
eval(willEval);
```
- eval() 함수 활용 : 호출한 코드의 변수를 활용할 수 있습니다.
```javascript
let willEval = '';
willEval += 'let number = 10;';
willEval += 'alert(number);';
//함수 호출
eval(willEval);
//eval() 함수로 호출한 코드의 변수 활용
alert(number);
```

### 숫자 확인 함수
- 'isFinite()' : number가 무한한 값인지 확인합니다. 무한대의 수를 확인할 때 사용합니다.
```javascript
let number = 1 / 0;
alert(number + ' : ' + isFinite(number));
```
  + 자바스크립트는 0으로 숫자를 나누면 Infinity 값이 들어갑니다. 그래서 alert의 결과는 true입니다.
  + 음수를 0으로 나누면 -Infinity이므로 "Infinity가 아님" 을 출력합니다.
- 'isNaN()' : number가 NaN인지 확인합니다.
  + NaN은 자신을 비교할 수 없습니다. 그래서 NaN을 확인할 때는 반드시 'isNaN()' 함수를 사용해야 합니다.
```javascript
if (isNaN(NaN)) {
  alert('NaN' == NaN);
} else {
  alert('NaN' != NaN);
}
```

### 숫자 변환 함수
- 'parseInt()' 함수와 'parseFloat()' 함수는 'Number()' 함수의 단점을 보완합니다.
  + Number() 함수는 숫자로 바꿀 수 없으면 NaN으로 변환합니다.
  + 'parseInt()' 함수와 'parseFloat()' 함수는 숫자로 변환할 수 있는 부분까지는 모두 숫자로 바꿉니다.
- 'parseInt(string)' : string을 정수로 바꿉니다.
- parseFloat(string) : string을 유리수로 바꿉니다.
```javascript
let won = '1000원';
let dollar = '1.5$';
alert(parseInt(won) + ' : ' + parseInt(dollar));
alert(parseFloat(won) + ' : ' + parseFloat(dollar));
```
  + parseInt() 함수는 1000 : 1, parseFloat() 함수는 1000 : 1.5를 출력합니다.
