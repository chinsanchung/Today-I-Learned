# JavaScript 객체
## 개요
- 객체는 자바스크립트의 데이터 구조로서 어떠한 형태의 데이터라도 저장할 수 있습니다. 그리고 key 를 이용해 데이터의 정보를 얻을 수 있습니다.
- object 안에는 property 가 있습니다. 'property = key + value'
  + property 외에 함수도 들어갈 수 있습니다. (객체 내부의 함수 프로퍼티를 같은 말로 메소드라고 부릅니다.)
  + 메소드는 값이 함수인 객체의 속성입니다.
- 객체에도 끝맺음에는 세미콜론을 사용하는 것이 좋습니다.
- 객체는 brace {} 로 감쌉니다.
- 메소드를 사용할 경우 property 의 이름을 쓸 떄 변수의 이름을 앞에 작성해줘야 key 임을 인식합니다.
```javascript
let abc = {
  name : 'abcde'
  cal : function calculate() {
    //name 이 abc 의 property 임을 증명합니다.
    let num = abc.name.length();
  }
}
```
## this
- 메소드가 속한 객체 내부의 프로퍼티를 사용하려면 this 키워드를 써야 합니다. (this 를 변수나 함수의 이름으로 사용할 수 없습니다.)
```javascript
const triangle = {
  type: 'scalene',
  identify: function () {
    //this 는 triangle 객체를 가리킵니다. this 는 triangle 객체의 속성에 접근하게 해줍니다.
    console.log(`This is a ${this.type} triangle.`);
  }
};
```
- 어떻게 함수가 호출되느냐에 따라 함수 내부의 this 값이 결정됩니다.
```javascript
const triangle = {
  type: 'scalene',
  identify: function () {
    //this 는 triangle 객체를 가리킵니다. this 는 triangle 객체의 속성에 접근하게 해줍니다.
    console.log(`This is a ${this.type} triangle.`);
  }
};

function iden () {
  this.trickyish = true;
}

triangle.identify();
iden();
```
  + triangle.identify(); 에서 불러온 this 는 객체 triangle 을 가리킵니다. 반면 iden() 에서의 this 는 global window 객체입니다.
## window
- 'window' 객체 : 이 객체는 브라우저 환경에서 제공되며 식별자, 윈도우를 사용하여 JavaScript 코드에 전역적으로 액세스 할 수 있습니다. 이 개체는 자바 스크립트 사양(ECMAScript)의 일부가 아니라 W3C에서 개발했습니다.
  + window 객체는 웹 페이지의 정보들에 접근할 수 있습니다. (URL, 수직 스크롤, 새 페이지 열기 등등)
- window 객체는 가장 높은 레벨(global)이어서 전역 레벨(함수 외부) 에서 만들어진 모든 변수 선언은 자동으로 윈도우 객체의 속성이 됩니다.
- 'var' 로 변수를 선언할 때에만 window 객체에 추가됩니다. ES6 의 'let', 'const' 로 함수 외부에서 선언하면 그 변수는 window 객체에 추가되지 않습니다.
- global 함수는 window 객체에 메소드로서 접근할 수 있습니다.
```javascript
//global 함수 선언
function learnSomethingNew() {
  window.open('https://www.udacity.com/');
}

window.learnSomethingNew === learnSomethingNew // 결과는 true
```
- global 선언의 문제점
  + Tight coupling : 코드들의 세부 사항에 너무 긴밀하게 연결되어 의존하는 관계인 상황입니다. 한 코드를 변경하면 다른 코드에도 영향을 주게 됩니다.
```javascript
var instructor = 'James';
// 함수 안의 instructor 를 담을 로컬 변수가 없어서 밖에서 전역으로 선언한  instructor 를 사용합니다.
  // 만약 전역 변수 instructor 를 teacher 로 바꾼다면 아래의 함수에 문제가 생기게 됩니다.
function hello () {
   console.log(`${instructor} says 'hi!'`);
};
```
  + name collision (이름 충돌) : 두 개(또는 그 이상)의 함수가 같은 이름의 변수에 의존할 때 발생합니다. 이 문제의 주요한 문제점은 두 함수가 변수를 업데이트하거나 변수를 설정하려고 시도해도 이 변경 사항이 서로 무시된다는 것입니다. 따라서 전역 변수를 최대한 적게 쓰고 함수 내부에 변수를 선언해서 사용하는 것이 좋습니다.
```javascript
let counter = 1;

function header () {
  counter = counter + 1;
	console.log(counter);
}

function footer () {
  counter = counter + 1;
  console.log(counter);
}
//목표 : header 와 footer 에 각각 다르게 출력하는 것.  header: 2,header: 3, footer:  2,header:  4
  //하지만 counter 는 전역 변수이기 때문에 각각 따로 적용되지 않았습니다.
header();
header();
footer();
header();
```

## 표기법
### 일반적인 형태의 표기
```javascript
let pets = {
  let name = 'cat',
  let color = 'black',
  let eyeColor = ['blue', 'yellow'],
  let weight = '4kg'
};
```
### 리터럴 표기법
```javascript
let pets = {
  name : 'cat',
  color : 'black',
  eyeColor = ['blue', 'yellow'],
  weight : '4kg',
  sleep : function areYouSleeping () { return 'zzz'; }
};

pets["eyeColor"] //return : blue, yellow
pets.eyeColor //return : blue, yellow
pets.sleep();  //return : zzz
pets.['sleep'](); //return : zzz
```
- eyeColor property 를 불러오는 방식 중 'pets["eyeColor"]' 을 '브리켓 표기법' 이라고 합니다. (property 이름에 쌍따옴표를 붙여야 합니다.)
- 그리고 'pets.eyeColor' 로 불러오는 방식은 '점 표기법' 입니다.
- 객체의 키가 숫자형태일 경우 (1 : 'red') pets[1] 은 return 값을 반환하지만, pets.1 점 표기법으로는 반환하지 않습니다.(undefined)
- 또한 프로퍼티의 키로 변수를 만들어도 pets.petColor 는 undefined, pets[petColor] 는 black 을 리턴합니다.
```javascript
//표기법..pets 의 color 프로퍼티를 가져옵니다.
const petColor = 'color';
/*
따옴표가 생략 된 경우에도 JavaScript 객체의 모든 속성 키는 문자열입니다.
점 표기법을 사용하면 JavaScript 인터프리터(번역기)는 값이 'petColor'인 pets 내에서 키를 찾습니다.
객체에 정의 된 키가 없기 때문에 표현식은 정의되지 않은 값을 반환합니다.
*/
```

## property
- property 의 이름(아래의 규칙들은 변수의 이름을 정할 때도 적용됩니다.)
  + 첫 글자에 숫자를 넣어선 안됩니다. 넣을 경우 브리켓으론 가능하지만, 점 표기법으로는 출력할 수 없습니다.
  + 중간에 띄어쓰기나 하이픈( - ) 을 넣으면 점 표기법으로 출력이 안됩니다.
  + 여러 글자일 경우 캐멀 표기법을 권장합니다. (예 : firstChild)
- 'delete' 연산자로 프로퍼티를 제거합니다.
```javascript
delete printer.power; //콘솔에는 true가 뜹니다. (제거 성공)
//power 를 불러오면 undefined 를 출력합니다.
```

## 객체 속의 객체
```javascript
const bicycle = {
  color: 'blue',
  type: 'mountain bike',
  wheels: {
    diameter: 18,
    width: 8
  }
};

//객체 속의 객체를 출력하기.
bicycle.wheels.diameter;
bicycle['wheels']['width'];
```
- 객체 내부의 프로퍼티를 수정할 수도 있습니다.
```javascript
bicycle.wheels.diameter += 1;
bicycle.color = 'black';
// diameter 는 19, color 는 black 이 됩니다.
```
- 프로퍼티를 밖에서 새로 추가할 수 있습니다.
```javascript
const printer = {};
printer.power = 'on';
printer.print = function () {
  console.log('printing');
};

//결과
const printer = {
  power: 'on',
  print: function() {
    console.log('printing');
  }
};
```
## 객체의 메소드
- 'Object.keys()' : 객체의 키만을 추출해서 배열 형태로 출력합니다. (배열의 요소는 string 입니다.)
- 'Object.values()' : 객체의 속성만을 추출해서 배열 형태로 출력합니다.
```javascript
const book = {
  name : 'aaa',
  writer: 'james jin',
  price: '10000'
}
Object.keys(book); //결과 : ['name', 'writer', 'price']
Object.values(book); //결과: ['aaa', 'james jin', '10000']

//참고로 for in 반복문으로도 가능하지만 위 메소드가 간편합니다.
const output = [];
for (const out in book) {
  output.push(name);
}
output; //결과 : ['name', 'writer', 'price']
```

## 문제 예시 01
```javascript
/*
 * Programming Quiz: Facebook Friends (7-5)
 */

// your code goes here
let facebookProfile = {
    name : 'jin',
    friends : 7,
    messages : ['from han', 'erollment', 'tax bills'],
    postMessage : function postMessage(message) {
        facebookProfile.messages.push(message);
        return facebookProfile.messages;
    },
    deleteMessage : function deleteMessage(index) {
        facebookProfile.messages.splice(index, 1);
        return facebookProfile.messages;
    },
    addFriend : function addFriend() {
        facebookProfile.friends += 1;
        return facebookProfile.friends;
    },
    removeFriend : function removeFriend() {
        facebookProfile.friends -= 1;
        return facebookProfile.friends;
    }
};
```
