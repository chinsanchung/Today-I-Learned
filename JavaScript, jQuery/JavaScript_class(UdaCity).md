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
### this 설정 4가지
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

  + call() 은 전달된 첫 번째 인수의 범위에서 함수를 호출하려는 경우 효과적입니다.
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
const myGrow = dog.growOneYear.bind(dog);
//myGrow 를 invokeTwice 에 pass 합니다.
invokeTwice(myGrow);

dog.age; //결과: 7
```
