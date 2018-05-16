# 리액트 도움닫기 1장 : 기초
## 리액트의 특징
- 리액트는 프레임워크가 아니라 라이브러리입니다. 리액트는 오직 뷰 레이어를 만드는 역할만 합니다. 뷰는 컴포넌트로서 다른 컴포넌트와 함께 계층구조를 이룹니다.
  + 시작부터 모든 도구와 구조를 갖춘 프레임워크와 다르기에 먼저 리액트를 배운 후 다른 기술, 도구를 적용시키는 법을 배워나갈 수 있습니다.
  + 개발 상황에 따라 적합한 솔루션을 도입할 수 있습니다. 리액트는 솔루션들을 상호 교환할 수 있기 때문입니다.
- 컴포넌트 : 'props' 를 input 으로 하면서, UI 가 어떻게 보여야 하는지를 정의하는 리액트 element 를 output 으로 하는 함수입니다.
  + 컴포넌트는 'React.Component' 를 상속받아 정의하지만 컴포넌트 간에는 상속보다는 합성을 사용하는게 권장됩니다.
  + 리액트의 컴포넌트는 인스턴스화 되었기 때문에 HTML element 와 같이 애플리케이션의 어느 곳에서든 재사용될 수 있습니다.

## 필요한 도구
- node ('node --version'), npm('npm --version')
- react : create-react-app 으로 애플리케이션을 부트르스래핑합니다. (애플리케이션을 최초로 생성해서 브라우저에서 실행하는 과정입니다.)
  + 'npm install -g create-react-app'
- 앞으로 새로운 리액트 애플리케이션을 부트스트랩 할 때는 'create-react-app <name>' 명령어를 입력하면 됩니다.
  + 애플리케이션 실행은 'npm start' 실행어를 입력합니다.

## JSX 기초
- App.js 파일을 살펴보면 render 메소드의 return 에는 JSX 를 사용했음을 알 수 있습니다. 아래는 설치한 직후의 App.js 파일입니다.
```javascript
//App.js 에 리액트 ES6 클래스 컴포넌트를 선언했습니다.
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
//render 메소드에서 element 가 return 됩니다. 여러 element 가 모여서 컴포넌트 전체를 구성합니다.
  render() {
    return (
      const hello = 'hello';
      //className 은 attribute 입니다.
      <div className="App">
        <h2>{hello}</h2>
      </div>
    );
  }
} 

export default App;
```
  + JSX 문법은 생성한 변수를 중괄호 {} 안에 넣어서 실행합니다.
  + 여기서의 HTML attribute 는 카멜케이스 표기법을 따릅니다.

## let 과 const
- 'const' 는 기본적으로 변경할 수 없지만, 변수가 배열이나 객체 타입일 경우 변경이 가능합니다.
- 리액트 생태계는 *불변성 원칙* 을 준수하기 때문에 변수를 정의할 때 'let' 보다 'const' 사용을 우선시합니다.

## ReactDOM
- 아래는 index.js 파일입니다. 이 파일이 리액트의 첫 번째 진입점입니다.
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  //HTML 에서 id 가 root 인 element 에 리액트 애플리케이션이 들어갑니다.
  document.getElementById('root')
);
```
- 'ReactDOM.render' 메소드는 HTML 의 DOM 노드를 JSX 로 대체하는 일을 합니다. 때문에 타 애플리케이션에서도 리액트가 쉽게 통합될 수 있습니다.
  + 'ReactDOM.render()' 메소드는 여러 번 사용할 수 있습니다. 하지만 관용적으로 리액트 애플리케이션은 전체 컴포넌트 트리를 부트스트랩 하기 위해 'ReactDOM.render()' 메소드를 한 번만 사용합니다.
  + JSX 구문, 단일 리액트 컴포넌트, 다중 리액트 컴포넌트, 또는 전체 응용 프로그램을 부트스트랩 할 수 있습니다.
  + 'ReactDOM.render()' 는 두 개의 인수가 필요합니다. 렌더링된 JSX와 리액트 애플리케이션이 HTML 에 들어갈 위치입니다.
- 현재 'ReactDOM.render()' 메소드를 실행해서 App 컴포넌트를 사용하고 있습니다. 되도록 애플리케이션 전체를 컴포넌트로 만들어 전달하는게 좋습니다.

## Hot Module Replacement
- HMR 은 브라우저 내 애플리케이션을 재실행하는 도구입니다. index.js 파일에 설정을 추가합니다.
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

if (module.hot) {
  module.hot.accept();
}
```
- HMR 의 장점은 새로고침을 하지 않고도 변경사항이 바로바로 적용된다는 점입니다. 애플리케이션이 대규모일수록 변경사항을 새로고침 하는데 시간이 오래 걸려 HMR 으로 이러한 점을 해결할 수 있습니다.

## JSX 내 자바스크립트 객체 처리
- 자바스크립트의 객체를 JSX 에서 출력하기 위해서 'map()' 메소드를 사용합니다.
```javascript
import React, { Component } from 'react';
import './App.css';

const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];

class App extends Component {
  render() {
    return (
      <div className="App">
        {list.map(function (item) {
          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
      </div>
    );
  }
}
export default App;
```
- 리액트는 'key' attribute으로 배열에서 수정, 제거된 항목을 식별합니다. 따라서 위의 JSX 문에서 objectID 프로퍼티를 'key' 로 지정합니다.
  + 'key' 값은 고유값으로 식별이 가능해야 합니다. 자바스크립트 때처럼 배열의 인덱스 값은 고졍값이 아니어서 사용하면 안됩니다. (항목이 변경된다면 리액트는 읽어낼 수 없게 됩니다.)

## 화살표 함수
- 기존 함수 표현식은 자기 자신을 'this' 객체로 정의하지만, 화살표 함수 표현식은 감싸고 있는 본문 context 를 'this' 로 정의합니다.
- 화살표 함수에서는 단일 매개변수라면 괄호 생략이 가능합니다. 다만 매개변수가 두 개 이상이거나 매개변수가 없는 함수라면 괄호를 넣어야 합니다.
```javascript
item => {...} 또는 (itme) => {...}
(item, key) => {...}
```
- 화살표 함수에서 중괄호 {} 부분인 블록 본문을 생략합니다. 간결한 본문에서는 return 이 포함되어, return 문을 삭제할 수 있습니다.
```javascript
{list.map(item =>
  //return 문을 생략했습니다.
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{itme.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
)}
```

## ES6 클래스
- 리액트는 함수형과 객체 프로그래밍의 장점을 가지고 있습니다.
  + 불변하는 데이터 구조를 다룰 때는 함수형 프로그래밍, 컴포넌트를 만들 때는 객체 프로그래밍(클래스 선언)을 적용합니다.
- 클래스에는 인스턴스화를 할 수 있는 생성자가 있습니다.
  + 생성자는 매개변수를 사용해서 클래스 인스턴스에 할당합니다.
- 클래스는 함수를 정의합니다. 그 함수는 메소드라고도 불립니다.
```javascript
class Devloper {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
const jin = new Developer('jin', 'jeong');
console.log(jin.getName()); //결과: jin jeong
```
- App.js 파일에서는 App 클래스가 있습니다.
  + App 클래스는 Component 클래스의 기능을 확장함으로, Component 의 기능을 상속받습니다. ('render()' 메소드 등)
```javascript
// App 클래스는 컴포넌트로 확장됩니다.
class App extends Component {
  ...
}
```
- Component 클래스는 리액트 컴포넌트의 모든 구현 세부 사항을 캡슐화하기 때문에 리액트에서 클래스를 컴포넌트로 사용할 수 있습니다.
