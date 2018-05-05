# React 기초 (출처: https://velopert.com/3612)
## 개요
- 리액트는 페이스북에서 개발한 유저인터페이스 라이브러리입니다. 재사용 가능한 UI를 만드는데 도움을 줍니다.
- 리액트는 자바스크립트 안에서 HTML 처럼 보이는 코드를 작성하게 해줍니다.
- Virtual DOM이라는 개념을 통해 상황에 따라 선택적으로 유저인터페이스를 렌더링합니다.
  + 최소한의 DOM 처리로 요소를 업데이트할 수 있습니다.
  + HTML이 정적이라 jQuery로 했었지만, 대규모의 웹 애플리케이션에서 쓰기에는 어렵습니다. (DOM이 바뀌면 CSS, 레이아웃 등 다양한 처리과정으로 인해 처리시간이 늘어납니다.)
- 준비사항으로는 Node.js, Yarn, 코드 에디터, git이 있습니다.
  + 설치 후 'npm install -g create-react-app'으로 설치 후 'create-react-app hello-react'을 실행합니다. (hello-react폴더에 저장됩니다.)
  + 그리고 해당 폴더의 cmd 창에서 'yarn start' 를 실행합니다.

## 기본 처리 과정
### App.js
- 요소(컴포넌트)에 해당하는 App.js가 웹 페이지에 출력됩니다.
```JSX
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
```
  + 'import' : 무엇인가를 불러옵니다. 위 코드는 React와 그 내부의 Component를 불러옵니다. 파일에서 JSX를 쓰려면 React를 꼭 불러와야합니다.
```JSX
render() {
  return (
    <div className="App">
      <p className="App-intro">
        To get started, edit <code>src/App.js</code> and save to reload.
      </p>
    </div>
  );
}
export default App;
```
  + 요소(컴포넌트)를 클래스를 통해 만듭니다. 클래스 형태는 render 함수가 반드시 필요합니다. 또 내부에서는 JSX를 return해야 합니다.
  + export default App은 작성한 요소를 다른 곳에서 사용할 수 있도록 내보냅니다.

### JSX
- JSX는 지켜야 할 규칙이 있습니다.
  + 태그를 열었으면 반드시 닫아야만 합니다.
  + 두 개 이상의 element는 무조건 하나의 element로 감쌉니다.
```
div로 감싸거나
return (
  <div>
    <div>
      Hello
    </div>
    <div>
      Bye
    </div>
  </div>
);
혹은 Fragment로 감쌉니다.
return (
  <Fragment>
    <div>
      Hello
    </div>
    <div>
      Bye
    </div>
  </Fragment>
);
```
  + JSX 내부에서 자바스크립트 값을 쓰려면 'const' 로 선언합니다.
  + 조건부 렌더링은 보통 삼항 연산자나 AND 연산자를 씁니다.
  + 클래스 설정할 경우 'div class'가 아닌 'div className'을 씁니다.
  + 주석은 {/*...*/} 으로 작성합니다.

### props, state 와 이벤트 설정
- 'props' : 부모 컴포넌트가 자식 컴포넌트에게 주는 값을 props 라고 합니다. 자식은 받기만 할 뿐, 받은 props 를 수정할 수 없습니다.
  + 'defaultProps' : App.js 에서 name 값을 생략할 경우 defaultProps 값으로 props 의 기본값이 정해집니다.
- 'state' : 동적인 데이터를 위해서 사용합니다. state 는 컴포넌트에 대한 정보를 가집니다.
  + 'setState' 으로 state 의 값을 변경할 수 있습니다. 'setState' 는 객체로 전달되는 값만 업데이트합니다.
- 이벤트 설정 : render 함수에서 이벤트 설정을 할 때 이벤트이름을 onClick 이런 식으로 합니다. (기존의 onclick이 아닙니다.)
  + 이벤트에 전달할 값은 함수여야 합니다. (예 : 'this.함수이름' )

# LifeCycle API
- LifeCycle API 는 브라우저에서 컴포넌트가 나타나고, 사라지고, 그리고 업데이트 될 떄 호출됩니다.
## 컴포넌트 생성
- 'componentWillMount()' : 현재는 deprecated(비추천) 입니다.
- 'componentDidMount()' : 컴포넌트가 화면에 나타났을 떄 호출됩니다. DOM 을 사용해야하는 외부 라이브러리와의 연동, ajax 데이터 요청, DOM 의 속성을 읽거나 직접 변경하는 기능을 수행합니다.
## 컴포넌트 업데이트
- 'UNSAFE_componentWillReceiveProps(nextProps)' : 컴포넌트가 새로운 props 를 받을 때 호출됩니다. state 가 props 에 따라 변하는 로직을 작성합니다. 현재 UNSAFE_componentWillReceiveProps() 라는 이름으로 쓰입니다.
- 'static getDerivedStateFromProps()' : props 로 받은 값을 state 로 동기화할 떄 사용됩니다.컴포넌트를 최적화해 CPU 처리량을 줄여줍니다.
- 'componentWillUpdate()' :  shouldComponentUpdate() 에서 true 를 반환했을때만 호출됩니다. 애니메이션 효과를 초기화하거나, 이벤트 리스너를 없애는 작업을 합니다. 현재 deprecate(비추천)이며 getSnapshotBeforeUpdate() 로 대체가능합니다.
- 'componentDidUpdate(prevProps, prevState, snapsho)' : 컴포넌트에서 render() 를 호출한 후에 발생합니다. 이전의 props 값과 state 를 조회할 수 있습니다. snapshot 값은 getSnapshotBeforeUpdate() 에서 생긴 것입니다.
## 컴포넌트 제거
- 'componentWillUnMount()' : 등록했던 이벤트를 제거합니다. 외부 라이브러리에 dispose 기능이 있고 사용했다면 이 API에서 호출합니다.
## 컴포넌트 에러
- 'componentDidCatch(error, info)' : 컴포넌트 자신의 render 함수의 에러는 못 잡지만 자식 컴포넌트 내부의 에러는 잡아냅니다.
- 에러가 발생하는 이유
  + 존재하지 않는 함수를 호출
  + 배열과 객체가 존재하지 않는데 호출할 경우
