# Rendering UI with React (Udacity)
## JSX 로 element 만들기
### 살펴보기
- 앞서 말했듯이 리액트는 탬플릿으로 UI 를 표현하지 않고, element 를 사용합니다.
- 리액트의 `.createElement()` 를 알아봅니다. 이것은 리액트가 새로운 element 를 만들 떄 사용하는 탑 레벨의 API 입니다.
  + 첫 인수는 사용할 element 의 태그 이름을 쓰거나, 혹은 사용할 div 만을 적습니다.
  + 두 번째 인수는 HTML 속성(attribute) 의 객체이자 element 에 대한 커스컴 데이터입니다.
  + 세 번째 인수는 첫 인수 안에 작성할 텍스트입니다.
```javascript
React.createElement( /* type */, /* props */, /* content */ );
```
- 간단한 리액트와 리액트 DOM 을 만들어봅니다.
```javascript
import React from 'react'
import ReactDOM from 'react-dom'

const element = React.createElement('div', null, 'hello world')
/*리액트 DOM 로 만든 element 를 DOM node 에 렌더합니다.
(예제는 이미 있는 페이지의 루트 element 를 사용합니다.)*/
ReactDOM.render(
  element,
  document.getElementById('root')
)
```
  + 변수 element 는 다른 프로퍼티를 가진 순수한 자바스크립트 객체입니다.
  + 리액트의 `.createElement()` 메소드는 element 에 대한 설명을 가져오고 일반 자바스크립트 객체로 return 합니다.
- 리액트 DOM 은 리액트 라이브러리를 사용하는 한 방법입니다.
  + 리액트에서 무엇을 렌더링할지 결정하는 과정은 실제 렌더링과 완전히 분리됩니다.
  + 분리(decoupling)를 통해 네이티브 장치나 심지어 VR 환경의 서버에 물건을 렌더링할 수 있게 해줍니다.

### element 를 DOM 에 렌더링하기
- 위에서 사용한 루트 element 는 어디서 왔을까요?
  + 리액트로 빌드된 앱은 일반적으로 단일 루트 DOM 노드 를 가집니다. 예를 들어 HTML 파일에는 아래의 div 를 가질 것입니다.
```HTML
<div id='root'></div>
```
- 이러한 DOM node 를 `getElementById()` 로 전달하면 리액트는 해당 내용 전체를 제어하게 됩니다.
  + 특정 `<div>` 가 리액트 앱의 hook 역할을 한다는 뜻이기도 합니다. 해당 영역이 바로 리액트가 UI 를 대신해 맡은 영역입니다.
- 이번에는 element 에 클래스를 넣습니다. 두번째 인수는 DOM 노드 에 줄 프로퍼티를 위한 것입니다.
```javascript
const element = React.createElement('div', {
  className: 'welcome-message'
} , 'hello world')
```
  + 참고로 `class` 로 하면 에러가 뜹니다. class 는 타당한 DOM 프로퍼티가 아닙니다. 대신 `className` 을 써야 합니다.
  + 왜냐면 element 는 DOM 노드 이고 class 는 HTML 의 속성(attribute) 이기에 그렇습니다. 브라우저가 class 를 파싱(parse)해 실제 DOM 노드 로 바꾸면, DOM 프로퍼티 이름은 className 이 됩니다.
- 따라서 리액트 element 를 만들 때 DOM 노드 를 정의하는 것이지 HTML 문자열을 정의하는게 아니라는 걸 기억해야 합니다.
- 속성(attribute) 에 대한 기본값을 쓸 수 없습니다. class 대신 className 을 써야하는 것처럼 for 대신 htmlFor 를 사용해야 합니다.(for 는 자바스크립트의 언어이기 때문입니다.)
- `VirtualDom` 은 만들었던 실제 DOM element 가 아니라 실제 DOM 노드 를 정의한 객체입니다.
  + React.createElement() 를 호출할 때, 실제로는 DOM 에 아무것도 만들진 않았습니다. 브라우저가 실제로 DOM element 를 만드는 렌더링을 호출하기 전까진 말입니다.
- `nesting` : 대다수의 UI 는 다른 뷰의 안에서 뷰로 표시됩니다. 리액트는 UI 를 만드는 라이브러리이므로, nesting (뷰 안에 뷰를 중첩하기) 를 잘합니다.
```javascript
/*세번째 인수로 createElement 를 전달해 다른 element 안에 element 를 중첩했습니다.
*/
const element = React.createElement('ol', null,
  React.createElement('li', null, 'a'),
  React.createElement('li', null, 'b'),
  React.createElement('li', null, 'c')
)
```
- 리스트를 나열할 때, 배열 항목들이 있다면 리액트는 element 배열을 자식으로 사용하게 해줍니다. 이것은 배열 데이터를 쉽게 표현하게 해줍니다.
  + 단순히 배열의 map 을 사용했고 관련 탬플릿 언어를 사용하지 않았습니다. 또한 person 객체는 이미 scope 안에 있어서 관련 탬플릿 언어가 필요하지 않습니다.
  + 대신 person 객체를 자바스크립트 함수 scope 안에서 사용했습니다.
  + 배열을 자식으로 사용할 때 알아야 할 점은 리액트가 그것에 유니크한 키를 주지 않는다면 에러를 표시할 거라는 것입니다.
```javascript
const people = [{name: 'a'}, {name: 'b'}, {name: 'c'}]
//map 을 사용해서 배열의 각 항목들을 뽑아냅니다.
const element = React.createElement('ol', null,
  people.map(person => (
    //React.createElement('li', null, person.name)
    React.createElement('li', {key: person.name}, person.name)
  ))
)
//참고. 배열의 index 를 키로 주고 싶을 때
const element = React.createElement('ol', null,
  people.map((person, index ) => (
    //React.createElement('li', null, person.name)
    React.createElement('li', {key: index}, person.name)
  ))
)
```
- 키를 줘야 하는 이유는 목록의 항목들 중 하나가 바뀔 수도 있는데 만약 각 목록의 항목에 고유한 키 속성(attribute) 을 가진다면, 리액트는 매번 목록을 다시 작성하지 않고 목록의 어떤 항목이 바뀐건지를 능동적으로 알 수 있습니다.
### `.createElement()` 는 하나의 루트 element 를 return 합니다.
- 이번에는 `.createElement()` 대신 JSX 를 사용해봅니다. JSX 는 HTML 과 비슷합니다.
  + 단순하게 people[0].name 으로 하면 문자열로 출력합니다.
  + 대신 {people[0].name} 으로 묶어서 자바스크립트 표현으로 인식하게 해줍니다. 이것은 함수를 사용할 때도 마찬가지입니다.
  + 여기도 마찬가지로 유니크한 key 를 줘야 합니다. JSX 는 HTML 과 비슷하니 li 태그 안에 넣으면 끝납니다.
```javascript
const element = <ol>
  {people.map(person => (
    <li key={person.name}>{person.name}</li>
  ))}
</ol>
```
- 디버거로 소스 뷰어를 확인해보면 실제로는 `.createElement()` 로 element 를 만들었음을 알 수 있습니다. JSX 는 간편합니다. 그리고 위의 코드들은 실제 자바스크립트로 컴파일됩니다.
### JSX 도 하나의 루트 element 를 return 합니다.
- JSX 를 쓸 때 하나의 element 만을 return 한다는 점에 주의합니다.
  + 이 element 가 하위 element 를 여러 개 가지더라도 전체 JSX 를 wrapping 하는 단일 루트 element 가 있어야 합니다.(일반적으로 div 나 span 입니다)
```javascript
const message = (
  <div>
    <h1>All About JSX:</h1>
    <ul>
      <li>JSX</li>
    </ul>
  </div>
);
/*div 하나에 모든 JSX 가 내장되어있습니다.
이런 식으로 다수의 element 를 작성해야합니다.
아래처럼 하면 에러가 뜹니다.*/
const message = (
  <h1>All About JSX:</h1>
  <ul>
    <li>JSX</li>
  </ul>
);
```
  + 에러 코드에서 h1 과 ul 형제 element 가 같은 루트 단계에 있는데 구문 오류를 띄웁니다.
  + JSX 는 `.createElement()` 의 구문 확장일 뿐입니다. `.createElement()` 는 첫 인수로 하나의 태그 이름만 사용하기에 오류가 나온 겁니다.
### 컴포넌트 개요
- 컴포넌트는 페이지에 렌더링하기 위해서 HTML 을 return 하는 재사용 가능한 코드를 말합니다.
- 리액트의 주요 초점은 앱의 UI 를 간소화하는 것이니 모든 리액트의 컴포넌트 클래스에서 반드시 필요한 유일 메소드는 `render()` 입니다.
  + `render()` 는 컴포넌트가 렌더링할 element 나 JSX 를 return 합니다.
- 리액트는 기본 컴포넌트 클래스를 제공합니다. 그걸로 많은 element 를 묶어서 하나의 element 처럼 사용할 수 있습니다.
  + 리액트 컴포넌트를 마치 필요한 리액트 element 를 만드는 공장으로 생각하면 됩니다. 그래서 커스텀 컴포넌트나 클래스를 만들어 쉽게 커스텀 element 를 만들 수 있습니다.
```javascript
class ContactList extends React.Component {
  render() {
    const people = [{name: 'a'}, {name: 'b'}, {name: 'c'}]

    return <ol>
      {people.map(person => (
        <li key={person.name}>{person.name}</li>
      ))}
    </ol>
  }
}
//element 대신 ContactList element 를 렌더링합니다.
ReactDOM.render(
  <ContactList/>
  document.getElementById('root')
)
```
- 리액트에서는 컴포넌트를 `class ComponentName extends React.Component {}` 로 정의합니다. 다시말해 `React.Component` 를 상속받은 자바스크립트 클래스의 컴포넌트를 정의합니다.
```javascript
class ComponentName extends React.Component {}
```
  + 실제로는 아래의 형태로 사용합니다.
```javascript
class ComponentName extends Component {}
```
  + 두 방식 모두 같지만, 모듈을 import 한 게 일치하는지 확인해야합니다. 즉 위처럼 컴포넌트를 선언했다면 `React` 로부터의 import 는 이렇게 써야합니다.
```javascript
import React, { Component } from 'react';
```
### 정리
- 리액트는 오직 앱의 뷰 레이어만을 중시한다는 걸 기억합시다. (뷰 레이어는 사용자가 보고 상호작용하는 부분)
- `.createElement()` 를 사용해 HTML 을 문서에 렌더링할 수 있습니다.
- 하지만 실제로는 구문 확장을 이용해 UI 의 모양을 설명합니다. 이러한 구문 확장은 JSX 이며 이것은 자바스크립트 파일에 바로 작성된 일반 HTML 과 유사합니다.
- JSX 는 브라우저에서 렌더링될 HTML 을 출력하는 리액트의 `.createElement()` 메소드의 호출로 컴파일됩니다.
- 리액트 앱을 만들 때 중요한 것 중 하나는 컴포넌트를 생각하는 것입니다.
- 컴포넌트는 리액트의 모듈화 및 재사용을 나타냅니다.
  + 컴포넌트 클래스를 컴포넌트의 인스턴스를 만드는 공장이라고도 생각할 수 있습니다.
  + 컴포넌트 클래스는 [단일 책임 원칙](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)을 따르며 오직 하나만 수행합니다.(단일책임원칙: 모든 클래스는 하나의 책임만 가지며, 클래스는 그 책임을 완전히 캡슐화해야 함)
  + 너무 많은 작업을 관리한다면 컴포넌트를 더 작은 하위 컴포넌트로 분해하는게 좋습니다.

## 리액트 앱 만들기
- JSX 는 훌륭하지만 앱을 만드려면 개발 단계를 따라야 합니다.
  + 기본적으로 JSX 코드를 가지고 실제 브라우저에서 움직이는 자바스크립트 코드로 컴파일할 컴파일러가 필요합니다.
  + 가장 유명한 빌드 툴은 Webpack 입니다. 페이스북은 빌드 툴 설치작업을 간편하게 해줄 create-react-app 을 제공합니다. 그것은 리액트와 JSX 를 사용한 앱 개발에 필요한 모든 것들을 설정해줍니다.
### 리액트 앱 스카폴딩
- 브라우저에 닿기 전에 JSX 를 일반 자바스크립트로 컴파일해야 합니다. 보통 Babel 같은 트랜스파일러를 사용합니다. 웹 프로젝트를 위해 모든 에셋(자바스크립트 파일, CSS, 이미지 등)을 묶는 Webpack 같은 빌드 툴로 Babel 을 실행할 수 있습니다.
- 초기 구성을 간소화해주는 create-react-app 패키지를 사용해봅니다. `npm install -g create-react-app`
  + constacts 라는 이름의 앱을 만들어봅니다. `create-react-app contacts` 그러면 리액트, react-DOM, 그리고 react-scripts 를 만들어줍니다.
  + react-scripts 패키지는 Babel(최신 자바스크립트 구문을 쓰게 해줌), Webpack(빌드 생성), Webpack-dev-server(자동-리로드 행동)
#### Yarn 패키지 매니져
- 프로젝트 생성 후 `yarn start` 로 개발 서버를 시작합니다.
  + Yarn 은 NPM 같은 패키지 매니져입니다. 페이스북이 NPM 의 부족한 부분을 보완해 만들었습니다. 굳이 쓰지 않아도 괜찮습니다. `npm start` 로 동일합니다.
### 정리
- 페이스북의 `create-react-app` 은 리액트 앱을 스카폴딩해주는 도구입니다.
- 이것을 사용해 모듈 번들이나 트랜스파일러를 설치할 필요가 없어집니다. `create-react-app` 이 알아서 설정해주기 때문입니다.

## 컴포넌트로 작성하기
### 컴포넌트의 모든 것
- 리액트 API 의 대부분은 모두 컴포넌트에 관한 것입니다. 그래서 컴포넌트가 매우 중요하다는걸 알 수 있습니다.
- 컴포넌트는 리액트가 제공하는 캡슐화의 기본 단위입니다.
- 컴포넌트는 UI 를 작은 조각으로 분해하는데 도움을 줍니다.
  + 이 조각들은 명확한 책임과 제대로 정의된 인터페이스를 가집니다. 그것은 커다란 앱을 구축하는데 매우 중요합니다. 왜냐면 다른 나머지 부분에 영향을 미치지 않으면서 작업하도록 도와주기 때문입니다.
  + 컴포넌트는 상속 대신에 구성(composition) 을 사용해 앱을 구축합니다.
### 리액트에서 작성하기
- 구성(composition) 을 사용해 UI 를 만드는 것을 설명합니다.
```javascript
import React, { Component } from 'react';

class ContactList extends React.Component {
  render() {
    const people = [{name: 'a'}, {name: 'b'}, {name: 'c'}]

    return <ol>
      {people.map(person => (
        <li key={person.name}>{person.name}</li>
      ))}
    </ol>
  }
}

class App extends Component {
  render() {
    return (
      <div className="App">
        <ContactList/>
      </div>
    );
  }
}

export default App;
```
- ContactList 를 가져와 앱 안에 넣었습니다. 컴포넌트에 많은 element 를 캡슐화 하는 것은 두 가지 장점이 있습니다. 이 둘은 리액트 composition 모델을 이해하는데 있어 매우 중요합니다.
  + 첫째로 element 들을 재사용하기 쉬워집니다. 다시 쓰고 싶다면 <ContactList/> 를 입력하면 끝입니다.
  + 둘쨰로는 매우 깔끔한 인터페이스입니다. 다른 props 를 주기만 하면 컴포넌트들을 다르게 구성할 수 있습니다. 원래 people 의 배열을 contacts 라는 props 에 넣고 변수 people 은 `this.props.contacts` 가 됩니다.
  + `this.props.contacts` 의 뜻은 각 컴포넌트가 this.props 객체를 가졌음을 알 수 있습니다. this.props 프로퍼티들은 컴포넌트([...])에 전달했던 props 입니다.
```javascript
<div className="App">
/* contacts prop 에다 people 배열을 전달합니다.
props 를 통해 약간의 구성만을 전달해 컴포넌트를 쉽게 재사용합니다. */
  <ContactList contacts={[
    {name: 'a'}, {name: 'b'}, {name: 'c'}
  ]}/>
  <ContactList contacts={[
    {name: 'd'}, {name: 'e'}, {name: 'f'}
  ]}/>
</div>
```
- props 를 여러 개 쓰려면 HTML 속성(attribute) 처럼 개별로 작성해야 합니다.
```javascript
<Clock time={Date.now()} zone='MST' />
```
### 상속에 대한 구성을 선호
- 리액트는 구성(composition)을 사용해 UI 를 작성합니다.
  + 리액트 컴포넌트를 확장하더라도 두 번 이상 확장하진 않습니다. UI 또는 행동을 더하기 위해서 기본 컴포넌트를 확장하지 않고 중첩(nest) 과 소픔(props) 를 사용해 다양한 방법으로 컴포넌트를 작성합니다. 궁극적으로는 UI 컴포넌트는 독립적, 집중적, 재사용이 가능해야 합니다.
  + 리액트를 이용하면 상속에 대한 구성을 선호한다는 뜻을 이애하게 될 것입니다.
