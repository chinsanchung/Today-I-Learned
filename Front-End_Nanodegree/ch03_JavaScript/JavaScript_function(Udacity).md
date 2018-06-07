# 자바스크립트 함수 (Udacity)
## 화살표 함수
- ES6 에서부터 생긴 함수입니다. function 하고 return 없이 작성합니다.
```javascript
const upperizedNames = ['a', 'b', 'c'].map(function(name) {
  return name.toUpperCase();
});
// 화살표 함수
const upperizedNames = ['a', 'b', 'c'].map( name => name.toUpperCase() );
```
- 기존 함수는 함수 선언식 또는 함수 표현식이 될 수 있지만, 화살표 함수는 언제나 함수 표현식입니다. 함수 표현식은 변수에 저장, 함수의 인수로 전달, 객체의 프로퍼티에 저장이라는 과정이 포함됩니다.
  + 참고로 화살표 함수가 변수로 저장되는 경우 이렇게 됩니다.
```javascript
const greet = name => `Hello ${name}.`;
// 이 화살표 함수는 이렇게 불러냅니다.
greet('Jin');  //결과 : Hello jin.
```
- 파라미터가 두 개 이상일 경우 괄호를 써서 묶어야 합니다. 또한 아무런 파라미터가 없을 때도 괄호를 사용합니다.
```javascript
const sayHi = () => console.log('Hello Udacity Student!');
sayHi(); // 결과 : Hello Udacity Student!

const orderIceCream = (flavor, cone) => console.log(`Here's your ${flavor} ice cream in a ${cone} cone.`);
orderIceCream('chocolate', 'peanut'); // 결과 : Here's your chocolate ice cream in a peanut  cone.
```
- 위에서의 단 하나만 함수로 표현한 방식을 `간결한 본문 구문(consice body syntax)` 라고 부릅니다.
  + 함수의 몸을 감싸는 중괄호가 없고, 표현식을 자동으로 return 합니다.
- 화살표 함수의 본문에 한 줄 이상의 코드가 필요할 경우 `블록 본문 구문(block body syntax)` 를 사용할 수 있습니다.
  + 블록 본문 구문은 중괄호를 사용합니다. 그리고 return 할 때는 return 문을 써야 합니다.
```javascript
const upperizedNames = ['a', 'b', 'c'].map( name => {
  name = name.toUpperCase();
  return `${name} has ${name.length} characters in their name`;
});
```
- 화살표 함수의 주의점 : this 키워드는 기존 함수와는 전혀 다르게 사용합니다. 또한 화살표 함수는 선언으로 쓰지 못합니다.

## this 와 기존의 함수
- 기존의 함수에서의 this 는 어떻게 그 함수(혹은 메소드)가 호출된건지에 따릅니다.
1. 새로운 객체
```javascript
const mySundae = new Sundae('chocolate', ['peanut', 'cherry']);
```
- 여기서의 Sundae 생성자 함수 내부의 this 값은 새로운 객체입니다. new 키워드로 호출됐기 때문입니다.
2. 특정 객체
```javascript
// 함수가 call 이나 apply 로 선언됐다면
const result = obj1.printName.call(obj2);
```
- printName() 내부의 값은 obj2 를 참조합니다. call() 의 첫 파라미터는 this 가 참조하는 값을 명시적으로 설정하기 때문입니다.
3. context 객체
```javascript
// 함수가 객체의 메소드라면
const redTrain = new Train('red');
redTrain.increaseSpeed(25);
```
- increaseSpeed() 안의 this 값은 redTrain 을 참조합니다.
4. 전역 객체나 undefined
```javascript
// 함수가 아무런 context 없이 불렸다면
teleport();
```
- teleport() 안의 this 값은 전역 객체 혹은 정의되지 않은 값입니다.

## this 와 화살표 함수
- 화살표 함수의 this 값은 함수의 주변 context 를 기반으로 합니다. (화살표 내부 함수의 this 값 = 함수 외부의 this 값)
```javascript
// 기존 함수에서의 this 값
function IceCream() {
  this.scoops = 0;
}

IceCream.prototype.addScoop = function () {
  setTimeout(function() {
    this.scoops++; // 함수 내부에 있는 this 를 참조합니다.
    console.log('scoop added.');
  }, 500);
};

const dessert = new IceCream();
dessert.addScoop(); //결과 : scoop added.
/* 여기서의 setTimeout 은 call(), apply(), context 객체, 그리고 new 없이 호출됐습니다.
그래서 새로운 scoops 변수가 만들어졌고 그 기본값은 undefined 입니다.
 */
console.log(dessert.scoops); //결과 : 0
console.log(scoops); //결과 : NaN
```
- 이번에는 클로저를 사용해봅니다.
```javascript
function IceCream() {
  this.scoops = 0;
}

IceCream.prototype.addScoop = function () {
  //this 를 cone 변수로 정합니다.
  const cone = this;
  setTimeout(function () {
    cone.scoops++; //이번에는 cone 변수를 참조합니다.
    console.log('scoop added.');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();
/* 함수가 호출될 때 cone 변수를 참조합니다. setTimeout 함수 밖에서의 this 값을 사용하기에
결과는 올바르게 1이 출력됩니다. */
console.log(dessert.scoops); //결과 : 1

//위 함수를 화살표 함수로 변경합니다.
IceCream.prototype.addScoop = function () {
  setTimeout(() => {
    this.scoops++;
    console.log('scoop added.');
  }, 0.5);
};
/* 함수가 호출될 때 addScoop() 안의 this 값은 dessert 를 참조합니다.
화살표 함수는 setTimeout() 에 전달되기 떄문에 주변의 context 를 활용하여 this 가 내부에서 참조하는 것을 판별합니다.
따라서 this 는 화살표 함수 밖의 dessert 를 나타내므로 화살표 함수 내부의 값은 dessert 입니다.
*/
```
  + 위 화살표 함수는 주변 context 에서 this 값을 상속하기에 코드는 정상적으로 작동합니다.
- 만약 addScoop 메소드도 화살표 함수로 바꾼다면 어떻게 될까요?
```javascript
IceCream.prototype.addScoop = () => {
  setTimeout(() => {
    this.scoop++;
    console.log('scoop added.');
  }, 0.5);
};

const dessert = new IceCream();
dessert.addScoop();
```
  + 화살표 함수는 주변 context 에서 this 값을 상속합니다. addScoop() 메소드 바깥에서 this 값은 전역 객체입니다. 따라서 addScoop() 메소드가 화살표 함수라면 addScoop() 안의 this 값은 전역 객체입니다. setTimeout() 함수에 전달되는 this 값은 전역 객체입니다.

## 기본 함수 파라미터 (Default Function Parameters)
```javascript
//잘못된 기본 함수 파라미터
/* 삼항 연산자 : 변수 = 조건 ? 값1 : 값2
조건이 참이라면 조건 연산자는 값1을 가집니다. 아니라면 값2를 가집니다. */
function greet(name, greeting) {
  name = (typeof name !== 'undefined') ? name : 'Student';
  greeting = (typeof greeting !== 'undefined') ? greeting : 'Welcome';

  return `${greeting} ${name}`;
}

//기본 함수 파라미터를 사용함
function greet(name = 'Student', greeting = 'Welcome') {
  return `${greeting} ${name}!`;
}

greet(); //결과 : Welcome Student!
greet('Jin') //결과 : Welcome Jin!
greet('Jeong', 'Jin') // 결과 : Jeong Jin!
```
- 필요한 인수가 제공되지 않을 경우 함수의 기본값을 제공합니다. ES6 에서는 기본값을 만드는 `기본 함수 파라미터` 를 도입했습니다.
- `기본 함수 파라미터` 를 만드려면 등호(=) 를 추가하고 인수가 제공되지 않은 경우 파라미터의 기본값을 지정합니다.

### 기본 함수 파라미터와 Destructuring
- 새로운 함수를 만들 때 기본 함수 파라미터와 destructuring 을 사용할 수 있습니다.
```javascript
function createGrid([width = 5, height = 5]) {
  return `Generates a ${width} * ${height} grid`;
}


createGrid([]); // 결과 : Generates a 5 x 5 grid
createGrid([2]); // 결과 : Generates a 2 x 5 grid
createGrid([2, 3]); // 결과 : Generates a 2 x 3 grid
createGrid([undefined, 3]); // 결과 : Generates a 5 x 3 grid
createGrid(); // 실행안됨
```
  + 배열이 빈 상태이거나 하나만 있다면, 기본 함수 파라미터가 기본값이 됩니다.
  + 다만 createGrid(); 는 실행되지 않습니다. 함수가 배열을 전달하지 않아 destructuring 이 실행되지 않아서입니다. 그러나 기본 함수 파라미터로 해결이 가능합니다.
```javascript
/* = [] 가 전체 파라미터를 기본값으로 설정해줍니다. 그러면 빈 배열이더라도 코드가 작동합니다.*/
function createGrid([width = 5, height = 5] = []) {
  return `Generates a ${width} * ${height} grid`;
}

createGrid(); // 실행됩니다.
```
- 기본 함수 파라미터와 객체 destructuring 을 사용해 함수는 객체를 가질 수 있습니다.
```javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']}) {
  const scoopText = scoops === 1 ? 'scoop' : 'scoops';
  return `Your sundae has ${scoops} ${scoopText} with ${toppings.join(' and ')} toppings.`;
}

createSundae({}); // Your sundae has 1 scoop with Hot Fudge toppings.
createSundae({scoops: 2}); // Your sundae has 2 scoops with Hot Fudge toppings.
createSundae({scoops: 2, toppings: ['Sprinkles']}); // Your sundae has 2 scoops with Sprinkles toppings.
createSundae({toppings: ['Cookie Dough']}); // Your sundae has 1 scoop with Cookie Dough toppings.
createSundae(); // 실행안됨
```
  + 이번 문제 역시 기본 함수 파라미터로 해결할 수 있습니다.
```javascript
//참고로 객체 기본값은 빈 객체 {} 을 넣습니다.
function createSundae({scoops = 1, toppings = ['Hot Fudge']} = {}) {
  const scoopText = scoops === 1 ? 'scoop' : 'scoops';
  return `Your sundae has ${scoops} ${scoopText} with ${toppings.join(' and ')} toppings.`;
}
/* 아무런 인수가 없는 빈 객체를 기본 파라미터로 삼아 앞으로는 인수 없는 함수도 실행이 가능합니다. */
createSundae(); // Your sundae has 1 scoop with Hot Fudge toppings.
```
- 배열 기본값보다 객체 기본값의 좋은 점은 건너뛴 옵션을 다룰 수 있다는 점입니다.
```javascript
function createSundae({scoops = 1, toppings = ['Hot Fudge']} = {}) { … }
/* 여기서 scoops 는 놔두고 toppings 만 고치고 싶으면 아래처럼 하면 됩니다. */
createSundae({toppings: ['Hot Fudge', 'Sprinkles', 'Caramel']});

//배열 기본값과 비교해봅시다.
function createSundae([scoops = 1, toppings = ['Hot Fudge']] = []) { … }
/* 배열 기본값에서는 scoops 자리에 undefined 을 해야만 합니다. */
createSundae([undefined, ['Hot Fudge', 'Sprinkles', 'Caramel']]);
```
  + 배열은 위치 기반이므로 undefined 를 사용해 첫 인수를 건너뛰게 해야 합니다.
- 그래서 배열 기본값을 사용해야만 하는 상황이 아니라면 객체 기본값을 쓰는 것이 좋습니다.

## ES6 클래스 복습
```javascript
//ES5 클래스
function Plane(numEngines) {
  this.numEngines = numEngines;
  this.enginesActive = false;
}
// 메소드를 모든 인스턴스에 상속합니다.
Plane.prototype.startEngines = function () {
  console.log('starting engines');
  this.enginesActive = true;
};
//ES6 클래스
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }
  startEngines() {
    console.log('starting engines');
    this.enginesActive = true;
  }
}
```
- 참고로 클래스 메소드 정의에는 콤마(,) 를 사용하지 않습니다.
- 클래스 정의에 있는 메소드는 클래스의 프로토타입 객체에 위치해 있습니다.
- 클래스는 단지 함수입니다. (typeof Plane === 'function') 은 사실입니다.
## 클래스와 함께 작업하기
- `static method` : 메소드 이름 앞에 `static` 을 적습니다.
```javascript
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }
  static badWeather(planes) {
    for (plane of planes) {
      plane.enginesActive = false;
    }
  }
  startEngines() {
    console.log('starting engines');
    this.enginesActive = true;
  }
}
```
  + `badWeather()` 메소드는 Plane 클래스에 직접적으로 접근이 가능합니다. 따라서 아래처럼 호출할 수 있습니다.
```javascript
Plane.badWeather([plane1, plane2, plane3]);
```
- 클래스의 장점
  + 함수를 작성하는데 적은 코드를 사용합니다.
  + 생성자 함수를 깔끔하게 정의할 수 있습니다.
  + 클래스에 필요한 모든 코드들을 클래스 선언에 집어 넣을 수 있습니다.
- 주의점
  + 다른 언어에서 쓰는 클래스처럼 모든 기능을 사용할 수 없습니다.
  + 자바스크립트 클래스는 프로토타입 상속을 사용합니다.
  + 자바스크립트 클래스의 새로운 인스턴스를 만들 때 new 를 사용합니다.

### super 와 extends
- ES6 에서의 서브클래스
```javascript
//ES6
class Tree {
  constructor(size = '10', leaveas = {spring: 'green', summer: 'green', fall: 'orange', winter: null}) {
    this.size =size;
    this.leaves = leaves;
    this.leafColor = null;
  }
  changeSeason(season) {
    this.leafColor = this.leaves[season];
    if (season === 'spring') {
      this.size += 1;
    }
  }
}
/* Maple 은 extends 를 사용해 Tree 의 서브클래스가 됩니다. */
class Maple extends Tree {
  constructor(syrupQty = 15, size, leaves) {
    //여기서의 super 는 함수로 사용됐습니다.
    super(size, leaves);
    this.syrupQty = syrupQty;
  }
  changeSeason(season) {
    //여기서의 super 는 객체로 사옹됐습니다.
    super.changeSeason(season);
    if (season === 'spring') {
      this.syrupQty += 1;
    }
  }
  gatherSyrup() {
    this.syrupQty -= 3;
  }
}

//ES5
function Tree(size, leaves) {
  this.size = (typeof size === "undefined")? 10 : size;
  const defaultLeaves = {spring: 'green', summer: 'green', fall: 'orange', winter: null};
  this.leaves = (typeof leaves === "undefined")?  defaultLeaves : leaves;
  this.leafColor;
}

Tree.prototype.changeSeason = function(season) {
  this.leafColor = this.leaves[season];
  if (season === 'spring') {
    this.size += 1;
  }
}

function Maple (syrupQty, size, leaves) {
  Tree.call(this, size, leaves);
  this.syrupQty = (typeof syrupQty === "undefined")? 15 : syrupQty;
}

Maple.prototype = Object.create(Tree.prototype);
Maple.prototype.constructor = Maple;

Maple.prototype.changeSeason = function(season) {
  Tree.prototype.changeSeason.call(this, season);
  if (season === 'spring') {
    this.syrupQty += 1;
  }
}

Maple.prototype.gatherSyrup = function() {
  this.syrupQty -= 3;
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
```

### 서브클래스와 작업하기
- super 는 반드시 this 전에 호출되어야 합니다.
```javascript
class Apple {}
class GrannySmith extends Apple {
  constructor(tartnessLevel, energy) {
    //이렇게 super 전에 this 를 쓰면 에러가 생깁니다.
    this.tartnessLevel = tartnessLevel;
    super(energy);
  }
}
```
