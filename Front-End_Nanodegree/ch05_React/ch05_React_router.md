# Managing App Location with React Router (Udacity)
## 소개
- single-page application 을 알아보기 전에 우선 웹의 작동 원리를 다시 봐봅니다.
  + 처음 사용자가 웹사이트에 방분하면 브라우저는 페이지를 웹사이트 서버에 요청합니다.(/abc/defgh) 그리고 서버는 HTML 을 만들고 그것을 아래로 보냅니다.
  + 사용자가 사이트를 탐색하면 브라우저는 서버에 새 페이지를 요청, 서버는 새 페이지를 HTML 형식으로 브라우저에 보내고, 사용자는 새 페이지를 눈으로 보게 됩니다.
  + 사용자가 탐색할 때마다 위의 사이클이 반복됩니다.
- 사람들이 말하는 single-page application 이란 화면 하나만 있다는 뜻이 아니라 브라우저는 새 페이지를 위해 다시 서버로 돌아갈 필요가 없다는 뜻입니다.
  + 대신 자바스크립트로 페이지 간 전환을 처리할 수 있습니다.
  + 따라서 오직 하나의 첫 페이지만을 서버에서 전송합니다. 이것이 single-page app 이라 부르는 이유입니다.
- 리액트 라우터는 리액트를 사용해 single-page app 을 만들게 도와주는 도구입니다. 현재 유명한 리액트 라이브러리 중 하나입니다.
### single-page app
- single-page app 은 다른 방식으로 작동이 가능합니다.
  + 먼저 전체 사이트 콘텐츠를 한번에 다운로드하는 방법입니다. 이렇게 하면 사이트를 탐색할 때 브라우저는 모든 작업을 할 수 있고 페이지를 새로고침할 필요도 없습니다.
  + 두 번째 방법은 사용자가 요청한 페이지를 렌더링할 때 필요한 모든 것들을 다운로드하는 것입니다. 그 다음 사용자가 새 페이지로 이동(navigate)하면 요청했던 콘텐츠만 비동기 자바스크립트 요청을 합니다.
- 좋은 single-page app 의 또 다른 핵심은 URL 이 페이지 콘텐츠를 제어한다는 점입니다.
  + single-page app 은 상호작용이 뛰어나서, 사용자는 URL 을 사용해서 특정 state 로 돌아갈 수 있기를 원합니다. (예를 들어 사이트를 북마크에 추가하면 해당 북마크는 URL 일 뿐 해당 페이지의 state 을 기록하지 않습니다.)
- 앱에서 수행하는 모든 작업은 페이지의 URL 을 업데이트하지 않습니다. 이제 북마크가 가능한 페이지를 제공하는 리액트 애플리케이션을 만들어 보겠습니다.
### 리액트 라우터
- 리액트 라우터는 리액트 프로젝트를 single-page app 으로 바꿔줍니다.
  + 이 작업은 링크 생성, 앱의 URL 을 관리하고 서로 다른 URL 을 탐색할 때 전환을 제공하는 등 여러 특수 컴포넌트들을 제공합니다.
- [웹 사이트](https://reacttraining.com/react-router/)에 따르면
  + 리액트 라우터는 애플리케이션과 함께 선언적으로 작성된 탐색 컴포넌트의 모음입니다.
- 이제 프로젝트의 this.state 객체의 값을 기반으로 콘텐츠를 페이지에서 동적으로 렌더링하는 작업을 합니다.
  + 이 작업은 리액트 라우터가 state 를 통해 보이는 부분을 제어해서 작동하는 방식을 사용합니다.
  + 리액트 라우터를 설치해 프로젝트에 추가하고 모든 것들을 연결해 링크와 URL 을 관리하도록 만들 것입니다.

## 동적 페이지 렌더링
- contacts 앱에서 새로운 연락처를 만들어 서버에 저장할 form 을 만들어봅니다.
  + 새로운 컴포넌트를 만들 것입니다. 왜냐면 리액트는 컴포넌트들을 함께 구성(compose)하는 걸 좋아하기 때문입니다. 그래서 새로운 UI 를 독립형 컴포넌트로 만들고 composition 을 사용해서 다른 컴포넌트에 포함할 것입니다.
- form 이 항상 표시되길 원하지 않으므로 설정이 가능한 경우에만 form 이 표시되도록 만들 것입니다.
  + 이 설정은 `this.state` 에 저장합니다. 이렇게 하면 리액트 라우터가 어떻게 동작하는지 알 수 있습니다.
### contacts 앱에서 적용하기
- CreateContact.js 를 만들고 App.js 에서 import 하는 코드를 넣습니다. 이 컴포넌트는 새로운 contacts 를 만들 form 을 담당합니다.
- 우선 App.js 의 state 에 screen 프로퍼티를 넣습니다. (screen: 'list')
- 그리고 render() 를 새로 작성합니다.
```javascript
render() {
  return (
    <div className="app">
      {this.state.screen === 'list' && (
        <ListContacts
        onDeleteContact={this.removeContact}
        contacts={this.state.contacts}
        />      
      )}
      {this.state.screen === 'create' && (
        <CreateContact />
      )}
    </div>
  )
}
```
  + 아까 state 에 추가한 screen 프로퍼티를 사용해 화면에 표시할 내용을제어합니다. `this.state.screen` 이 `list` 면 기존의 모든 연락처 목록이 표시되고, `create` 면 CreateContact 컴포넌트가 표시됩니다.
  + 참고: `&&` 은 논리 AND 연산자인데 이것으로 short-circuit evaluation 을 사용했습니다.
### Short-circuit Evaluation Syntax
```
expression && expression
```
- 이런 자바스크립트 테크닉을 *short-circuit evaluation* 이라고 부릅니다.
- 첫 표현이 참이라면, 두번째 표현을 실행합니다. 만약 첫 표현이 거짓이라면 두번째 표현은 스킵합니다.
- 위에서는 올바른 컴포넌트를 표시하기 위해 `this.state.screen` 의 값을 확인하려고 사용했습니다.
- 자세한 내용은 [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/%EB%85%BC%EB%A6%AC_%EC%97%B0%EC%82%B0%EC%9E%90(Logical_Operators)) 사이트에서 확인할 수 있습니다.
### 버튼 추가하기
- 버튼을 만들어 state 를 바꾸고 화면도 전환하는 클릭 핸들러를 만들어봅니다.
  + ListContacts.js 에서 작성합니다.
- 하지만 screen state 는 App.js 에 있습니다. state 를 바꾸려면 App.js 의 lListContacts 컴포넌트에 함수를 작성해야 합니다.
  + onDeleteContact prop 처럼 작성합니다.
```javascript
onNavigate={() => {
  this.setState({ screen: 'create' })
}}
```
- 다시 ListContacts.js 로 돌아가서 onNavigate 를 클릭 핸들러와 연결합니다.
```javascript
<a
  href="#create"
  onClick={this.props.onNavigate}
  className="add-contact"
>Add contact</a>
```
- 이제 버튼을 누르면 새로운 URL 과 함께 create state 로 넘어간 걸 보게 됩니다.
  + 하지만 뒤로가기를 눌러도 화면이 바뀌질 않습니다.
  + 뒤로가기는 화면을 이전으로 되돌리는 것입니다. 하지만 컴포넌트 state 만으로 그것을 구현하기는 어렵습니다. (새로고침으로 어떻게 할 수는 있겠지만 그리 좋은 방법은 아닙니다.)
  + 그래서 리액트 라우터를 구축해 UI 와 URL 을 동기화해서 유지해야 합니다. 리액트 라우터는 화면과 링크, URL 에 대한 사용자의 기대치를 그대로 유지해줍니다.
### 정리
state 를 사용해 사용자에게 표시할 내용을 제어하려 했습니다. 하지만 뒤로가기를 눌렀을 때 작동이 되질 않았습니다.
이제 리액트 라우터로 앱의 화면을 관리해봅니다.

## BrouwserRouter 컴포넌트
### 설치
- [react-router-dom](https://www.npmjs.com/package/react-router-dom) 을 설치합니다. `npm install --save react-router-dom`
### BrouwserRouter
