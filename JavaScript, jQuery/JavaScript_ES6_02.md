# JavaScript ES6 ch.02
## 객체와 배열
### 구조 분해를 통한 대입
- 구조 분해: 객체 안에 있는 필드값을 원하는 변수에 대입할 수 있습니다. 구조 분해는 선언적입니다. 따라서 코드 작성자의 의도를 더 잘 설명합니다.
```javascript
var sandwich = {
  bread: "크런치",
  meat: "소고기",
  cheese: "스위스",
  toppings: ["상추", "토마토", "머스타드"]
};
var {bread, meat} = sandwich;
console.log(bread, meat);
```
  + 결과는 크런치 소고기 입니다.
  + 위 코드는 sandwich를 분해해서 bread와 meat 필드를 같은 이름의 변수에 넣습니다.
```javascript
//두 변수를 변경합니다.
bread = "마늘",
meat = "칠면조"

console.log(sandwich.bread, sandwich.meat);
```
  + 두 변수를 바꿔도 원래의 필드 값은 바뀌지 않습니다. (결과 : 크런치 소고기)
```javascript
var lordify = regularPerson => {
//firstname만 가져옴으로써 객체의 필드 중에서 firstname만 쓴다는 것을 선언합니다
  console.log('서울의 ${regularPerson.firstname}');
  //= console.log('서울의 ${firstname}');
}
var regularPerson = {
  firstname: "현석",
  lastname: "오"
}
lordify(regularPerson);
```
  + 객체를 분해해서 함수의 인자로 넘길 수 있습니다. (결과 : 서울의 현석)
```javascript
//배열의 첫 번째 원소를 대입합니다.
var [firstResort] = ["용평", "평창", "강촌"];
//결과 : 용평
console.log(firstResort);
//불필요한 값을 콤마로 생략하는 리스트 매칭입니다.
var [,,thirdResort] = ["용평", "평창", "강촌"];
//결과 : 강촌
console.log(thirdResort);
```
  + 배열을 구조 분해해서 원소의 값을 변수에 대입할 수 있습니다.

### 객체 리터럴 개선
- '객체 리터럴 개선' : 구조 분해의 반대로 구조를 다시 만들어내는 과정 또는 내용을 한데 묶는 과정입니다.
  + 현재 영역에 있는 변수를 객체의 필드로 묶을 수 있습니다.
```javascript
var name = "탈락"
var elevation = 9777

var funHike = {name, elevation};
//결과 : {name: "탈락", elevation: 9777}
console.log(funHike);
```
- 객체 리터럴 개선으로 객체 메소드를 만들수도 있습니다.
```javascript
var name = "tallac";
var elevation = 9777
var print = function () {
  console.log('${this.name} 산의 높이는 ${this.elevation} 입니다.');
}
var funHike = {name, elevation, print};
//결과 : 탈락 산의 높이는 9777 입니다.
funHike.print();
```
- 객체 리터럴 개선으로 객체 메소드를 만들 때는 function 키워드를 쓰지 않아도 됩니다.
  + 덕분에 입력할 코드의 양이 줄어듭니다.
```javascript
//예전 방식
var ski = {
  name: name,
  powderYell: function () {
    var yell = this.sound.toUpperCase();
    console.log('${yell} ${yell} ${yell}');
  }
}

//새로운 방식
var ski = {
  name: name,
  powerYell: this.sound.toUpperCase();
  console.log('${yell} ${yell} ${yell}');
}
```

### 스프레드 연산자
- '스프레드 연산자' : 세 개의 점으로 이뤄졌습니다. 몇 가지 다른 역할이 있습니다.
  +  배열의 내용을 조합할 수 있습니다.
```javascript
var peaks = ["대청봉", "중청봉", "소청봉"];
var canyons = ["천계곡", "가계곡"];
var seoraksan = [...peaks, ...canyons];
//결과 : 대청봉, 중청봉, 소청봉, 천계곡, 가계곡
console.log(seoraksan.join(', '));
// peaks와 canyons 안의 모든 원소가 새로운 배열인 seoraksan에 들어갑니다.
```
  + 원본 배열을 변경하지 않고 복사본을 만들 수 있습니다. 이 경우 원본은 변경없이 그대로입니다.
```javascript
//peaks 배열의 마지막 원소를 변수에 담고 싶을 경우
var peaks = ["대청봉", "중청봉", "소청봉"];
  //reverse() 메소드는 원본 배열을 바꿉니다.
var [last] = [...peaks].reverse();
//결과 : 소청봉
console.log(last);
//결과 : 대청봉, 중청봉, 소청봉
console.log(peaks.join(', '));
```
  + 스프레드 연산자로 배열의 나머지 원소를 얻을 수 있습니다.
```javascript
var lakes = ["가호수", "나호수", "다호수", "라호수"];
var [first, ...rest] = lakes;
//결과 : "나호수", "다호수", "라호수"
console.log(rest.join(", "));
```
  + 스프레드 연산자로 함수의 인자를 배열로 모을 수 있습니다.
```javascript
function directions(...args) {
  var [start, ...remaining] = args;
  var [finish, ...stops] = remaining.reverse();

  console.log('${args.length} 도시를 운행합니다.');
  console.log('${start} 에서 출발합니다.');
  console.log('목적지는 ${finish} 입니다.');
  console.log('중간에 ${stops.length} 군데 들립니다.');
}
directions (
  "A", "B", "C", "D", "E", "F"
)
```
  + 스프레드 연산자를 객체에 사용할 수도 있습니다.
```javascript
var morning = {
  breakfast: "빵",
  lunch: "국수"
}
var dinner = "라면"
var backpackingMeals = {
  ...morning,
  dinner
}
//결과 : {breakfast: "빵", lunch: "국수", dinner: "라면"}
console.log(backpackingMeals);
```

### 프라미스
- '프라미스' : 비동기 요청에서의 성공과 실패 유형을 단순하게 바꿔줍니다. 그리서 비동기 요청을 더 쉽게 처리하도록 만들어줍니다.

## 클래스
- ES6에서 클래스 선언이 추가됐습니다. 함수는 객체며 상속은 프로토타입으로 처리됩니다.
```javascript
class Vacation {
  constructor(destination, length) {
    this.destination = destination;
    this.length = length;
  }
  print() {
    console.log('${this.destination} (은)는 ${this.length} 일 걸립니다.')
  }
}
```
- 모든 클래스의 첫 글자는 대문자로 시작합니다.
- 클래스를 정의한 후 new 키워드로 해당 클래스의 새로운 인스턴스를 만들고 그 메소드를 호출할 수 있습니다.
```javascript
const trip = new Vacation("칠레", 7);
//결과 : 칠레 (은)는 7 일 걸립니다.
console.log(trip.print());
```
  + 클래스를 만든 후 새로운 객체를 만들기 위해 원하는대로 new를 호출할 수 있습니다. 또는 클래스를 확장할 수 있습니다.
- Vacation을 추상 클래스로 사용할 수도 있습니다.
```javascript
class Expedition extends Vacation {
  constructor(destination, length, gear) {
    super(destination, length);
    this.gear = gear
  }

  print() {
    super.print();
    console.log('당신의 ${this.gear.join("와(과) 당신의 ")} 를(을) 가져오세요.');
  }
}

//상속으로 만든 하위 클래스의 인스턴스 생성
const trip2 = new Expedition("한라산", 3, ["선글라스", "깃발", "카메라"]);
trip2.print();
//결과 : 한라산은(는) 3일 걸립니다. \n 당신의 선글라스와(과) 당신의 깃발를(을) 가져오세요
//
```
  + 기존의 클래스를 확장한 새 클래스는 상위 클래스의 모든 프로피티와 메소드를 상속합니다.

## ES6 모듈
- ES6 부터는 자바스크립트 자체에서 모듈을 지원합니다.
- 'export' 를 사용해 다른 모듈에서 활용하도록 이름(함수 ,객체, 변수, 상수 등)을 외부에 export 할 수 있습니다.
- 모듈에서 단 하나의 이름만 외부에 export 할 경우 'export default' 를 사용합니다.
  + 오직 하나의 이름만 노출하는 모듈에서 사용 가능합니다.
```javascript
const freel = new Expedition("Mt. Freel", 2, ["water", "snack"]);

export default freel;
```
- 모듈은 'import' 명령으로 다른 자바스크립트 파일을 불러와 사용할 수 있습니다.
```javascript
import { print, log } from './text-helpers';
import freel from './mt-freel';
```
  + 모듈에서 가져온 대상에 다른 이름을 부여할 수 있습니다.
  + import * 를 사용하면 다른 모듈에서 가져온 모든 이름을 사용자가 정한 로컬 이름 공간 안에 가두게 됩니다.
```javascript
import * as fns from './text-helpers';
```

## 커먼JS
- 커먼JS 는 모든 버전의 노드에서 지원하는 일반적인 모듈 패턴입니다.
```javascript
const print(message) => log(message, new Date());

const log(message, timestamp) =>
  console.log('${timestamp.toString()} : ${message}');
//자바 객체를 module.exports 을 사용해 export 합니다.
module.exports = {print, log};
```
- 커먼JS는 import 문이 아닌 require 함수로 모듈을 import 합니다.
```javascript
const { log, print } = require('./txt-helpers');
```
