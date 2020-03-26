---
title: 인사이드 자바스크립트 6장. 객체지향 프로그래밍
date: 2020-03-17 14:03:74
category: book
draft: false
---

## 인사이드 자바스크립트 정리 6장. 객체지향 프로그래밍

### 목차

- [6.1 클래스, 생성자, 메소드](#61-클래스-생성자-메소드)
- [6.2 상속](#62-상속)
    - [6.2.1 프로토타입을 이용한 상속](#621-프로토타입을-이용한-상속)
    - [6.2.2 클래스 기반의 상속](#622-클래스-기반의-상속)
- [6.3 캡슐화](#63-캡슐화)
- [6.4 객체지향 프로그래밍 예제](#64-객체지향-프로그래밍-예제)
    - [6.4.1 클래스 기능을 가진 subClass 함수](#641-클래스-기능을-가진-subClass-함수)
    - [6.4.2 subClass 함수와 모듈 패턴을 이용한 객체지향 프로그래밍](#642-subClass-함수와-모듈-패턴을-이용한-객체지향-프로그래밍)

### 6.1 클래스, 생성자, 메소드

- 자바스크립트로 객체 지향을 구현해봤습니다.
    - 이 방식은 문제점이 있습니다. 각 객체는 setName(), getName() 함수를 따로 생성해서, 불필요하게 중복되는 영역을 메모리에 올려 사용해 자원 낭비를 가져옵니다.
```javascript
function Person(arg) {
    this.name = arg;
    ths.getName = function() {
        return this.name;
    }
    this.setName = function() {
        this.name = value;
    }
}
var me = new Person('zoom');
console.log(me.getName()); // zoom
me.setName('joo');
console.log(me.getName()); // joo
```
- Person 함수 객체의 프로토타입 프로퍼티에 getName(), setName() 함수를 정의하면, 각자 따로 함수 객체를 생성할 필요 없이 이 함수들에 프로토타입 체인으로 접근할 수 있습니다.
    - 이처럼 자바스크립트에서 클래스 메소드를 정의할 때는 프로토타입 객체에 정의한 후, new 로 생성한 객체에서 접근할 수 있도록 하는 것이 좋습니다.
```javascript
// 더글라스 크락포드의 메소드 정의법
Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}
function Person(arg) {
    this.name = arg;
}
Person.method('setName', function() {
    return this.name;
});
Person.method('getName', function() {
    return this.name;
});
var me = new Person('me');
var you = new Person('you');
console.log(me.getName); // me
console.log(you.getName); // you
```

### 6.2 상속

#### 6.2.1 프로토타입을 이용한 상속

- 상속을 이해할 수 있는 예제 코드
    - create_object() 함수는 인자로 들어온 객체를 부모로 하는 자식 객체를 생성해 반환합니다.
    - 새로운 빈 함수 객체 F 를 만들고, F.prototype 프로퍼티에 인자로 들어온 객체를 참조한 후, 함수 객체 F 를 생성자로 하는 새로운 객체를 만들어 반환합니다.
    - 이렇게 반환된 객체는 부모 객체의 프로퍼티에 접근할 수 있고, 자신만의 프로퍼티를 만들 수도 있습니다.
```javascript
function create_object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
```
- 프로토타입 기반 상속의 특징
    - 클래스에 해당하는 생성자 함수를 만들지 않고, 그 클래스의 인스턴스를 따로 생성하지 않았습니다.
    - 단지 부모 객체에 해당하는 person 객체와 이 객체를 프로토타입 체인으로 참조할 수 있는 자식 객체 student 를 만들었습니다.
- jQuery 의 extend()
    - 기능 확장과 상속에서 자식 클래스를 확장할 때 유용하게 사용됩니다.
    - 약점: `obj[i]=prop[i]`는 얕은 복사입니다. 문자, 숫자가 아닌 객체인 경우 복사하지 않고 참조해서, 값이 바뀔 수도 있습니다. 그래서 대상이 객체일 때는 깊은 복사를 합니다.
```javascript
jQuery.extend = jQuery.fn.extend = function(obj, prop) {
    if (!prop) { prop = obj; obj = this; }
    for (var i in prop) { obj[i] = prop[i] }
    return obj;
}
```

#### 6.2.2 클래스 기반의 상속

```javascript
function Person(arg) {
    this.name = arg;
};
// 두 클래스 프로토타입의 중개자
Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}
Person.method('setName', function(value) {
    this.name = value;
});
Person.mehod('getName', function() {
    return this.name;
});
// 빈 함수 객체: Person 인스턴스와 Student 인스턴스를 서로 독립적으로 만듦
function F() {};
F.prototype = Person.prototype;
Student.prototype = new F();
Student.prototype.constructor = Student;
Student.super = Person.prototype;

var me = new Studetn();
me.setName('zoom');
console.log(me.getName()); // zoom
```

- 스토얀 스테파노프의 상속 관계
    - 클로저(반환되는 함수)는 F() 함수를 지속적으로 참조합니다.
    - 이를 이용해 함수 F() 는 단 한번 생성되고, inherit 함수를 계속해서 호출해도 F() 함수를 또 만들 필요가 없어집니다.
```javascript
var inherit = function(Parent, Child) {
    var F = function() {};
    return function(Parent, Child) {
        F.prototype = Parent.prototype;
        Child.prototype = new F();
        Child.prototype.consturctor = Child;
        Child.super = Parent.prototype;
    };
}();
```

### 6.3 캡슐화

- 캡슐화는 관련 여러 정보를 하나의 틀 안으로 담는 것입니다. 예를 들어, 클래스 안에 멤버 변수와 메소드를 담는 것이 캡슐화입니다.
    - 중요한 것은 정보의 공개 여부입니다. 자바스크립트는 public, private 개념이 없지만, 은닉이 불가능하진 않습니다.
```javascript
// 이 방법을 모듈 패턴이라 부릅니다.
var Person = function(arg) {
    // private 멤버
    var name = arg ? arg : 'zoom';
    // public 멤버
    return {
        getName: function() {
            return name;
        },
        setName: function(arg) {
            name = arg;
        }
    };
};

var me = new Person();
console.log(me.getName()); // zoom
me.setName('joo');
console.log(me.getName()); // joo
console.log(me.name); // undefined
```
- 위의 방식은 접근하는 private 멤버가 객체나 배열이면 얕은 복사로 참조만을 반환해 쉽게 바뀐다는 단점이 있습니다.
    - 보통의 경우, 객체를 반환하지 않고 객체의 주요 정보를 새 객체에 담아 반환하는 방법을 사용합니다.
- 또한, 위의 예제는 반환받은 객체가 Person 함수 객체의 프로토타입에 접근할 수 없습니다. 그래서 함수를 반환하면 해결됩니다.

```javascript
var Person = function(arg) {
    var name = arg ? arg : 'zoom';

    var Func = function() {}
    Func.prototype = {
        getName: function() { return name; },
        setName: function(arg) { name = arg; }
    };
    return Func;
}();

var me = new Person();
console.log(me.getName());
```

### 6.4 객체지향 프로그래밍 예제

#### 6.4.1 클래스의 기능을 가진 subClass 함수

##### 6.4.1.1 subClass 함수 구조

- subClass 는 상속받을 클래스에 넣을 변수 및 메소드가 담긴 객체를 인자로 받아 부모 함수를 상속받는 자식 클래스를 만듭니다.
```javascript
function subClass(obj) {
    // 1. 자식 클래스 (함수 객체) 생성
    // 2. 생성자 호출
    // 3. 프로토타입 체인을 활용한 상속 구현
    // 4. obj 를 통해 들어온 변수 및 메소드를 자식 클래스에 추가
    // 5. 자식 함수 객체 반환
}
```

##### 6.4.1.2 자식 클래스 생성 및 상속

```javascript
function subClass(obj) {
    // ...
    var parent = this;
    var F = function() {};
    var child = function() {};
    // 프로토타입 체이닝
    F.prototype = parent.prototype;
    child.prototype = new F();
    child.prototype.consturctor = child;
    child.parent = parent.prototype;
    child.parent_constructor = parent;
    // ...
    return child;
}
```

##### 6.4.1.3 자식 클래스 확장

```javascript
// 사용자가 인자로 넣은 객체를 자식 클래스에 넣어 확장합니다.
for  (var i in obj) {
    if (obj.hasOwnProperty(i)) {
        child.prototype[i] = obj[i];
    }
}
```

##### 6.4.1.4 생성자 호출
- 클래스의 인스턴스가 생성될 때, 클래스 내에 정의된 생성자가 호출돼야 합니다. 물론 부모 클래스의 생성자 역시 호출되어야 합니다. 이를 자식 클래스 안에 구현했습니다.
```javascript
var child = function() {
    if (parent.hasOwnProperty('_init')) {
        parent._init.apply(this, arguments);
    }
    if (child.prototype.hasOwnProperty('_init')) {
        child.prototype._init.apply(this, arguments);
    }  
};
```
- 앞 코드는 부모와 자식이 한 쌍을 이뤘을 때만 구현됩니다. 밑의 예를 보면, 상위의 상위 클래스인 SuperClass 생성자가 호출이 되질 않습니다.
```javascript
var SuperClass = subClass();
var SubClass = SuperClass.subClass();
var Sub_SubClass = SubClass.subClass();

var instance = new Sub_SubClass();
```
- 따라서 부모 클래스의 생성자를 호출하는 코드는 재귀적으로 구현해야 합니다.
```javascript
var child = function() {
    var _parent = child.parent_constructor;
    if (_parent && _parent !== Function) {
/*
    현재 클래스의 부모 생성자가 있으면 그 함수를 호출합니다.
    다만 부모가 Function 이면 최상위 클래스에 도달했으므로 실행하지 않습니다.
*/        
        _parent.apply(this, arguments); // 부모 함수의 재귀적 호출
    }
    if (child.prototype.hasOwnProperty('_init')) {
        child.prototype._init.apply(this, arguments);
    }
}
```

##### 6.4.1.5 subClass 보완

- parent 를 단순히 this.prototype 로 지정해선 안됩니다. 처음 최상위 클래스를 Function 을 상속받는 것으로 정했는데 이를 처리할 코드가 없으니 `parent = this;`를 수정합니다.
```javascript
// Node.js 의 경우 global 을 사용합니다.
var parent = this === window ? Function : this
```
- 그리고 subClass 안에서 생성하는 자식 클래스의 역할을 하는 함수는 subClass 함수가 있어야 합니다. `child.subClass = argumetns.callee;`
- 완성입니다.
```javascript
function subClass(obj) {
    var parent = this === window ? Function : this;
    var F = function() {};
    
    var child = function() {
        var _parent = child.parent;

        if (_parent && _parent !== Function) {
            _parent.apply(this, arguments);
        }
        if (child.prototype._init) {
            child.prototype._init.apply(this, arguments);
        }
    };

    F.prototype = parent.prototype;
    child.prototype = new F();
    child.prototype.constructor = child;
    child.parent = parent;
    child.subClass = arguments.callee;

    for (var i in obj) {
        if (obj.hasOwnProperty(i)) {
            child.prototype[i] = obj[i];
        }
    }
    return child;
}
```

### 6.4.2 subClass 함수와 모듈 패턴을 이용한 객체지향 프로그래밍

- 모듈 패턴으로 캡슐화를 구현, subClass() 함수로 상속을 구현해봅니다.

```javascript
var person = function(arg) {
    var name = undefined;
    return {
        _init: function(arg) {
            name = arg ? arg : 'zoom';
        },
        getName: function() {
            return name;
        },
        setName: function(arg) {
            name = arg;
        }
    };
}
Person = subClass(person());
var joo = new Person('joo');
console.log(joo.getName());

Student = Person.subClass();
var student = new Student('student');
console.log(student.getName());
```