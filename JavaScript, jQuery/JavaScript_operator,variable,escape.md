# JavaScript 변수, 연산자, escape
## 변수 선언
- JavaScript 는 Java 와 다르게 변수를 지정하는데 있어서 느슨한 면이 있습니다.
  + Java 는 정수냐, 문자열이냐, 불리언이냐 등 각각 다른 형태로 변수를 선언해야 합니다.
  + 반면 JavaScript 는 자료형의 형태를 밖에서 선언하는 것이 아니라 내부적으로 알아서 타입을 변환해줍니다.(`Type Coercion`)
```javascript
1 == '1' //true
1 == true //true
0 == false //false
0 == '0' //true
```
  + 값을 연산할 떄도 처음 선언한 형태에 따라 내부적으로 변환한 타입으로 계산해줍니다.
```javascript
const a = '1';
const b = a + 1;
console.log(b); //문자열 11 ('1' + '1')

const c = 2;
const d = '2';
console.log(d); //문자열 22 ('2' + '2')
```

## 연산자
### 논리 연산자
- 종류
  + '&&' : 논리 AND 연산자입니다. A와 B 모두 참이거나 거짓일 때 true 입니다. 둘 중 하나라도 다른 값이면 false 입니다.
  + '||' : 논리 OR 연산자입니다. A나 B 둘 중 하나가 참이거나 모두 참이면 true 입니다. 둘 다 false 면 false 입니다.
  + '!' : 논리 부정 연산자입니다.

### 동등 연산자(Equal operator)
- `Equal operator` 는 일반 연산자 '==' 와 Strict Equal operator 인 '===' 두 종류로 나뉩니다.
- JavaScript 는 느슨한 선언을 하기에 '==' 으로 선언할 경우 내부적으로 타입을 변환합니다.
```javascript
1 == '1' //true
1 == true //true
0 == false //false
0 == '0' //true
```
- 하지만 '===' 은 엄격한 연산자로 내부 변환 없이 무조건 같은 타입이어야만 합니다.
```javascript
1 === '1' //false
1 === true //false
0 === false //false
0 === '0' //false
1 === 1 //true
```

## escape
- 종류
 + `\\`	: 역 슬래시 (backslash)
 + `\"` : 큰따옴표 (double quote)
 + `\'` : 작은따옴표 (single quote)
 + `\n`	: 줄바꿈 (newline)
 + `\t` :	탭 (tab)
