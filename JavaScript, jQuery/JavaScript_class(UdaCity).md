# 자바스크립트 클래스
## 생성자 함수
- 객체를 생성하는 방법은 literal notation 이외에도 생성자 함수를 쓰는 방법도 있습니다.
### 생성자 함수의 규칙
- 1. 'new Constructor();' : 새 객체를 생성하기 위해 new 연산자를 이용하여 함수를 호출합니다. (new 를 생략하면 기존의 함수가 되며 return 하지 않으면 변수는 undefined 가 출력됩니다.)
- 2. 생성자 함수의 첫 글자는 대문자로 해야 합니다. (대문자를 쓰지 않으면 단순한 함수가 됩니다.) 그리고 캐멀 표기법을 써야 합니다.
- 3. 지역 변수를 선언하는 대신 this 키워드로 데이터를 유지합니다.
- 4. 생성자 함수는 return 값을 반환하지 않습니다.
- 함수 생성하기
```javascript
function SoftwareDeveloper() {
  this.favoriteLanguage = 'JavaScript';
}
//생성자 함수 불러냄
let developer = new SoftwareDeveloper();
//developer 는 SoftwareDeveloper { favoriteLanguage: 'JavaScript' } 형태의 객체를 출력합니다.
//참고로 literal notation 으로 출력한 developer 는 생성자가 Object 로 되어있습니다.
```
### 생성자 함수는 파라미터를 가질 수 있습니다.
```javascript
function SoftwareDeveloper(name) {
  this.favoriteLanguage = 'JavaScript';
  this.name = name;
}
//문자열 jin 을 생성자 함수 SoftwareDeveloper() 안으로 통과시켜서 새로운 객체를 인스턴스화시켰습니다.(인스턴스화: 클래스를 실현. 클래스로 객체를 만듦)
let instructor = new SoftwareDeveloper('jin');
//결과: SoftwareDeveloper { favoriteLanguage: 'JavaScript', name: 'jin' }
console.log(instructor);
```
- 생성자 함수의 장점은 같은 생성자 함수를 호출하고 많은 인스턴스나 객체를 만들 수 있다는 것입니다.

## this 키워드
- this 키워드는 new 연산자로 생성자 함수를 호출할 때 새로 생성된 객체로 설정됩니다. (기존의 this 키워드와는 다릅니다.)
```javascript
function Cat(name) {
 this.name = name;
 this.sayName = function () {
   console.log(`Meow! My name is ${this.name}`);
 };
}

const bailey = new Cat('Bailey');
/*bailey 의 구성은 아래와 같습니다.
 {
  name: 'Bailey',
  sayName: function () {
    console.log(`Meow! My name is ${this.name}`);
  }
}
여기서의 this 는 새로 생성된 bailey 객체의 this 가 됩니다.
*/
```
  +  bailey 는 생성자 함수 밖에서 선언됐지만 sayName 의 this 는 Cat 의 name 프로퍼티에 접근할 수 있습니다.
- 객체에 속한 메소드를 호출할 경우, this 값은 그 객체 자체로 설정합니다. (위에서 새로운 객체로 설정한 것과 다릅니다.)
```javascript
const dog = {
  bark: function () {
    console.log('Woof!');
  },
  barkTwice: function () {
    this.bark();
    this.bark();
  }
};
//barkTwice 의 this 는 dog 객체에 접근할 수 있습니다. 접근 후 bark 메소드를 사용합니다.
```
### this 설정 방식 4가지
- 1. new 키워드로 생성자 함수를 호출하고 새로 생성된 객체로 this 를 설정합니다.
- 2. 객체에 속한 메소드를 호출하면 메소드를 호출했던 객체 자신으로 설정합니다.
- 3. 단순한 일반 함수를 호출할 때, 호스트 환경이 브라우저면 this 는 전역 객체인 window 로 설정됩니다.
```javascript
function funFunction() {
  return this;
}
```
- 4. 직접 this 키워드를 설정합니다.

## 스스로 this 설정하기
### call, apply 메소드
- 두 메소드는 함수 자체를 직접적으로 호출될 수 있습니다. 따라서 전달하는 함수는 지정된 this 값과 인수를 불러내게 됩니다.
- 'call()' : 함수에 직접 호출되는 메소드입니다. 메소드를 호출하기 위해 'call()' 을 쓰면 객체에서 메소드를 빌려 다른 객체에 사용할 수 있습니다.
  + 인수들을 쉼표로 구분해 개별적으로 전달합니다.
```javascript
function multiply(n1, n2) {
  return n1 * n2;
}
multiply(3, 4); //일반적인 형태
multiply.call(window, 3, 4); //결과: 12
```
  + 'call()' 를 multiply 함수에 직접 호출하고, call 의 첫 번쨰 인수로 this로 설정될 값을 전달합니다. 그 다음 multiply 함수의 인수를 넣어 마무리합니다.
```javascript
//
const mockingbird = {
  title: 'To Kill a Mockingbird',
  describe: function () {
    console.log(`${this.title} is a classic novel`);
  }
};
//mockingbird 의 메소드를 호출할 수 있습니다. mockingbird.describe();
//반면 call() 을 사용하면 아래의 객체는 mockingbird 의 describe() 를 사용할 수 있습니다.
const pride = {
  title: 'Pride and Prejudice'
};

mockingbird.describe.call(pride);
```
  + 'call()' 가 mockingbird.describe 에 호출됩니다. 그 다음 pride 값을 'call()' 에 전달합니다. describe 메소드는 title 속성에 접근해야 하는데 'call()' 가 pride 로 값을 결정했으므로 describe 의 this.title 에 접근할 수 있습니다.
  + 'call()' 은 전달된 첫 번째 인수의 범위에서 함수를 호출하려는 경우 효과적입니다.
- 'apply()' : 'call()' 과 같이, 'apply()' 는 함수를 호출하고 또 특정 this 값과 연관된 함수에서 호출됩니다.
  + 하지만 인수를 하나씩 전달하지 않고 'apply()' 는 배열으로 함수의 인수를 사용합니다.
```javascript
function multiply(n1, n2) {
  return n1 * n2;
}

multiply.apply(window, [3, 4]);
```

```javascript
//위의 mockingbird 객체에서의 apply()
mockingbird.describe.apply(pride); //call() 과 같은 결과가 나옵니다.
//pride 가 인수를 쓰지 않아서 call() 과 apply() 는 비슷한 형태를 가지게 됐습니다.
```
- 'call()' 과 'apply()' 를 어디에 써야 할까요.
  + 함수가 필요로 하는 인수의 갯수를 모를 때 'call()' 보다는 'apply()' 가 유리합니다.

### callback 과 this
```javascript
function invokeTwice(cb) {
//매개변수 cb 를 함수로 두번 호출하는 콜백함수입니다.
   cb();
   cb();
}

const dog = {
  age: 5,
  growOneYear: function () {
    this.age += 1;
  }
};
//growOneYear() 메소드 실행 (5 + 1)
dog.growOneYear();

dog.age; //결과: 6

invokeTwice(dog.growOneYear);

dog.age;//결과: 6
```
- invokeTwice(dog.growOneYear); 가 그대로인 이유
  + dog.growOneYear 라는 익명함수를 불러내서 dog 객체의 this 는 dog 객체가 아닌 전역 객체의 this로 설정됩니다. (this 의 선언 중 세번째에 해당합니다.) 만약 메소드로 불러냈다면 dog 객체 자신으로 설정됐을 것입니다.
```javascript
invokeTwice(function () {
  dog.growOneYear();
});

dog.age;//결과: 7
```
- 위에서는 익명 클로저를 사용해 dog 객체를 닫았습니다. invokeTwice() 는 여전히 this 가 window 로 설정됐지만, 익명 함수인 클로저에는 해당되지 않습니다. 클로저 안의 growOneYear() 는 여전히 dog 객체를 this 로 설정하고 있습니다.
  + 위와 같은 형태의 메소드가 있는데 바로 'bind()' 입니다.

### bind()
- 'bind()' 는 호출될 때 직접 정한 this 값으로 설정된 새로운 함수를 return 합니다.
  + 새로운 함수는 일반적인 함수처럼 호출할 수 있습니다. 다만 함수 내부에는 메소드 스타일로 호출해야합니다.
```javascript
function invokeTwice(cb) {
   cb();
   cb();
}

const dog = {
  age: 5,
  growOneYear: function () {
    this.age += 1;
  }
};
//함수 내부의 메소드를 메소드 스타일로 호출했습니다. dog.growOneYear
  //growOneYear 메소드의 this를 dog 객체로 지정합니다.
  //여기서의 myGrow 는 bind로 만든 새로운 객체 myGrow 입니다.
const myGrow = dog.growOneYear.bind(dog);
//myGrow 를 invokeTwice 에 pass 합니다.
invokeTwice(myGrow);

dog.age; //결과: 7
```

## 프로토타입
- 각 함수들은 프로토타입 프로퍼티를 가지고 있으며 실제로는 단순한 객체입니다. 함수들이 new 연산자를 통해 생성자로서 호출되면, 새로운 객체를 생성하고 return 합니다. 이 객체는 생성자의 프로토타입과 연결되어있고 링크를 통해 프로토타입 프로퍼티와 메소드에 접근하게 해줍니다.
  + 프로토타입 프로퍼티가 일반 객체를 가리키기 때문에 그 객체 자체도 프로토타입에 대한 링크를 가지고 있습니다. 프로토타입 객체는 프로토타입 자신을 참조합니다. (프로토타입 체인이 형성되는 방법)
```javascript
function Dalmatian (name) {
  this.name = name;
}
//프로토타입으로 메소드를 만들어서 Dalmatian 객체는 메모리를 절약하게 됐습니다.
Dalmatian.prototype.bark = function() {
  console.log(`${this.name} barks!`);
};
```

### 프로토타입 체인에서 프로퍼티와 메소드 찾기
- 순서
  1. 자바스크립트 엔진이 객체의 프로퍼티를 검색합니다. 객체 자신의 프로퍼티와 메소드의 이름이 같다면 다른 프로퍼티와 메소드보다 우선시됩니다. (scope chain 의 변수 shadowing 과 유사합니다.)
  2. 프로퍼티를 검색하지 못한다면, 객체의 생성자의 프로토타입을 검색합니다. 생성자에서도 없더라도 자바스크립트 엔진은 계속해서 chain 을 찾습니다.
  3. chain 의 끝에는 Object() 객체나 최상위 부모 객체가 있습니다. 거기서도 프로퍼티를 찾지 못한다면 그 프로퍼티는 정의되지 않습니다.

### 프로토타입 객체를 바꾼다면
```javascript
function Hamster() {
  this.hasFur = true;
}

let waffle = new Hamster();
let pancake = new Hamster();

//프로토타입
Hamster.prototype.eat = function () {
  console.log('chomp');
}
waffle.eat(); // 이것과 pencake.eat() 는 같은 결과입니다. chomp
//프로토타입 바꾸기
Hamster.prototype = {
  isHungry: false,
  color: 'brown'
};
//바꿔도 바꾸기 전에 선언한 객체들은 과거의 프로토타입 프로퍼티와 연결되어있습니다.
console.log(waffle.color); //undefined
console.log(pencake.isHungry); //undefined
//하지만 새로운 Hamster 객체를 만든다면, 그 객체는 새로운 프로토타입과 연결됩니다.
const muffin = new Hamster();

muffin.eat(); //TypeError. muffin.eat 는 함수가 아닙니다.
console.log(muffin.isHungry); //결과: false
console.log(muffin.color); //결과: 'brown'
```

### 객체의 프로퍼티 확인
- 'hasOwnProperty()' :  특정 프로퍼티가 어디서 왔는지를 찾아줍니다. 문자열을 통해 불린 형태로 리턴해 프로퍼티가 그 객체로부터 왔는지를 알려줍니다.
```javascript
function Phone() {
  this.operatingSystem = 'Android';
}

Phone.prototype.screenSize = 6;
//인스턴스화(객체 생성)
const myPhone = new Phone();
//operatingSystem 가 myPhone 의 프로퍼티인지 확인합니다.
const own = myPhone.hasOwnProperty('operatingSystem');

console.log(own); //결과: true
```
- 'isPrototypeOf()' : 객체가 다른 객체의 프로토타입 체인에 객체가 있는지 여부를 확인합니다. 특정 객체가 다른 객체의 프로토타입 역할을 하는지 확인할 수 있습니다.
```javascript
//객체 rodent
const rodent = {
  favoriteFood: 'cheese',
  hasTail: true
};
//생성자 함수
function Mouse() {
  this.favoriteFood = 'cheese';
}
//프로토타입을 Mouse 에 할당합니다. 이제 Mouse 의 프로토타입은 rodent 의 프로퍼티를 가집니다. Mouse.prototype = { favoriteFood: 'cheese',  hasTail: true };
Mouse.prototype = rodent;
//Mouse 객체 생성. 이것의 프로토타입은 Mouse 객체여야 합니다.
const ralph = new Mouse();
//rodent 객체는 ralph 객체의 프로토타입 역할을 하는가?
const result = rodent.isPrototypeOf(ralph);

console.log(result);//결과: true
```
- 'Object.getPrototypeOf()' : 특정 객체의 프로토타입이 무엇인지 확실하지 않을 때 사용합니다. 주어진 객체의 프로토타입을 가져오는데 유용합니다. 대상 객체가 pass 한 프로토타입을 return 합니다.
```javascript
const myPrototype = Object.getPrototypeOf(ralph);

console.log(myPrototype);//결과: { favoriteFood: 'cheese', hasTail: true }
```

### 생성자 프로퍼티
- 모든 객체는 생성자 프로퍼티가 있습니다.
  + literal notation 으로 했어도 그것의 생성자는 Object() 생성자 함수가 됩니다.
```javascript
function Longboard() {
  this.material = 'bamboo';
}

const board = new Longboard();
//board 의 생성자 프로퍼티에 접근하면, 원래 생성자 함수를 볼 수 있습니다.
console.log(board.constructor); //결과 function Longboard() {this.meterial = 'bammboo';}
//만약 객체가 literal notation 으로 만들어졌다면 생성자는 내장된 Object() 생성자 함수로 나옵니다. 따라서 생성자의 프로토 타입에 대한 reference 가 유지됩니다.
const rodent = {
  favoriteFood: 'cheese',
  hasTail: true
};

console.log(rodent.constructor); //결과: function Object() { [native code] }

Object.getPrototypeOf(rodent) === Object.prototype //결과: true
```

## 프로토타입 상속-서브클래스
### 서브클래스
- 'subclass' : 자식 객체가 자신만의 프로퍼티와 메소드를 유지하면서 부모 객체의 프로퍼티와 메소드를 가져오는 것입니다.
  + 자식 객체는 자신만의 프로퍼티와 메소드를 만들고 나머지는 부모 객체로부터 받으면 됩니다.
### 프로토타입 체인
- 객체에서 프로퍼티를 부를 때 처음에는 그 객체에서 찾습니다. 없으면 객체의 프로토타입에서 찾습니다. 거기서 없더라도 자바스크립트는 prototype chain 으로 계속해서 찾아나갑니다.
```javascript
const bear = {
  claws: true,
  diet: 'carnivore'
};

function PolarBear() {
  // ...
}

PolarBear.prototype = bear;

const snowball = new PolarBear();

snowball.color = 'white';
snowball.favoriteDrink = 'cola';
//snowball 객체는 이런 형태가 됩니다. { color: 'white', favoriteDrink: 'cola' }

//아까 PolarBear 의 프로토타입으로 bear 를 지정했으므로 bear 의 프로퍼티와 연결됐습니다.
console.log(snowball.claws); // true
console.log(snowball.diet); // 'carnivore'
//snowball 객체가 PolarBear 와 연결해 바로 프로토타입을 쓸 수 있었던 이유는 바로 __proto__ 프로퍼티 덕분입니다.
console.log(snowball.__proto__); //결과: { claws: true, diet: 'carnivore' }
// snowball 의 __proto__ 는 Polabear 의 프로토타입과 같습니다.
console.log(snowball.__proto__ === bear); //결과: true
```
- '__proto__' : 생성자 함수에 의해 만들어진 모든 객체(인스턴스)의 프로퍼티이며, 해당 생성자의 프로토타입을 가리킵니다. 객체(인스턴스) 는 '__proto__' 로 생성자 함수의 프로토타입 객체에 몰래 연결됩니다.
  + 하지만 '__proto__' 는 권장되지 않습니다. 브라우저 간의 호환성과 성능 문제가 있습니다. 그래서 '__proto__' 프로퍼티가 객체의 프로토타입에 접근할 수 있더라도 그것을 직접적으로 작성해선 안됩니다.
  + 객체의 프로토 타입을 검토해야하는 경우에는 'Object.getPrototypeOf()' 를 계속 사용하면 됩니다.

### 프로토타입 상속
- '자식.prototype = 부모.prototype' 으로 설정하면 안되는 이유
  + 객체는 reference 로 전달됩니다. 자식.prototype 객체와 부모.prototype 객체가 동일 객체를 따르기 떄문에 자식의 프로토타입을 바꾸면 부모쪽에서도 바뀌게 됩니다. 프로토타입만 상속해서는 안됩니다. 프로토타입 체인을 설정하지 않으면  자식 객체에서 바꾼 것들이 부모 객체에도 반영됩니다.
  + 따라서 프로토타입을 바꾸지 않고도 상속을 관리할 방법이 필요합니다.
- 'Object.create(arguments)' : 단일 객체를 인수로 '__proto__' 프로퍼티가 전달된 인수로 설정한 새로운 객체를 return 합니다. 그 객체를 자식 객체의 생성자 함수의 프로토 타입으로 설정하면 됩니다.
  + 이 방법으로 프로토타입 체인을 쉽게 확장할 수 있고, 원하는 모든 객체로부터 상속을 받아 프로토타입의 상속을 설정할 수 있습니다.
```javascript
const mammal = {
  vertebrate: true,
  earBones: 3
};
//mammal 객체를 써서 새로운 rabbit 객체를 만들었습니다.
const rabbit = Object.create(mammal);
//rabbit 의 __proto__ 프로퍼티는 아까 create() 의 인수였던 mammal 을 가리킵니다. 즉, rabbit 은 mammal 을 상속받았다는 뜻입니다.
console.log(rabbit.__proto__ === mammal); //결과: true
//rabbit 은 mammal 의 프로퍼티에 접근할 수 있습니다.
console.log(rabbit.vertebrate); //결과: true
console.log(rabbit.earBones); //결과: 3
```
