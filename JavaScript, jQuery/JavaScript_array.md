# 배열
## 배열의 메소드들
- 'length()' : 배열의 요소 개수를 구합니다.
  + 요소의 개수를 return 합니다.
- 'push()' : 배열의 끝에 요소를 삽입합니다.
  + 요소가 추가된 현재의 결과값이 return 됩니다.
```javascript
let array = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
array.push('h');
//return : 8. 요소는 이제 8개
alery(array); // 결과 a, b, c, d, e, f, g, h
```
- 'pop()' : 배열의 끝에 있는 요소를 제거합니다.
  + 제거한 요소가 return 됩니다.
```javascript
let array = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
array.pop(); // 'g' 제거
array.pop(); // 'f' 제거
alert(array); //결과는 a, b, c, d, e
```
- 'splice()' : 특정 위치의 배열을 추가 또는 제거할 수 있습니다.
  + 'splice('제거할 요소의 위치', '제거할 요소의 수', '삭제한 요소 대신 삽입할 요소')'
```javascript
let array = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
//0번째 인덱스에서 두 개의 요소를 삭제
array.splice(0, 2); //결과는 c, d, e, f, g
//1번째 인덱스에서 두 개의 요소를 삭제, a와 b 를 추가
array.spice(1, 2, 'y', 'z'); //결과는 c, y, z, e, f, g
```

## 배열의 반복
### loop
- loop : 배열에서도 반복문을 이용할 수 있습니다.
```javascript
let alphabet = ["a", "b", "c"];

for (let i = 0; i < alphabet.length; i++) {
    donuts[i] += " zzz";
    donuts[i] = alphabet[i].toUpperCase();
}
console.log(alphabet); //결과: AZZZ, BZZZ, CZZZ
```
### forEach
- 'forEach()' : 배열을 반복하고 배열의 각 요소를 인라인 함수 표현식으로 조작하는 다른 방법을 제공합니다.
  + 명시적으로 정의된 인덱스가 없어도 배열을 반복합니다.
- 파라미터로 element, index, array 가 있습니다.
  + element 매개 변수는 배열 요소의 값을 가져옵니다.
  + index 매개 변수는 요소 인덱스를 가져옵니다 (0부터 시작).
  + array 매개 변수는 전체 배열에 대한 참조를 가져 오며, 요소를 수정하려는 경우에는 편리합니다.
- forEach() 메소드는 원래 배열을 영구적으로 수정하려는 경우 유용하지 않습니다.
  + 왜냐면 undefined 를 반환하기 때문입니다. 그럴 경우에는 map() 메소드를 사용합니다.

```javascript
words = ["cat", "in", "hat"];
words.forEach(function(word, num, all) {
  console.log("Word " + num + " in " + all.toString() + " is " + word);
});
```
### map
- 'map()' 메소드는 기존의 배열로부터 새로운 배열을 생성할 때 유용합니다.
  + 배열을 가져와 배열의 각 요소에 대한 연산을 수행하고 새로운 배열을 반환합니다. 인덱스를 사용할 필요가 없습니다.
```javascript
let donuts = ["jelly donut", "chocolate donut", "glazed donut"];

let improvedDonuts = donuts.map(function(donut) {
  donut += " hole";
  donut = donut.toUpperCase();
  return donut;
});
// donuts 배열을 이용해 improvedDonuts 라는 새로운 배열을 만들었습니다.
```
## 배열 속의 배열
```javascript
let alphabet = [
  ['a', 'b', 'c'],
  ['d', 'e', 'f'],
  ['h', 'i', 'j']
]

for (let i = 0; i < alphabet.length; i++) {
  console.log(alphabet[i]);
  //결과는 ['a', 'b', 'c'] ['d', 'e', 'f'] ['h', 'i', 'j']
}
```
- 반복문을 두 번 사용해 배열의 요소들을 출력할 수 있습니다.
```javascript
for (let row = 0; row < alphabet.length; row++) {
  for (let column = 0; column < alphabet[i].length; column++) {
    console.log(alphabet[row][column]);
    //결과는 'a', 'b', 'c', 'd', 'e', 'f', 'h', 'i', 'j'
  }
}
```
