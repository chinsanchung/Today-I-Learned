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

## BrowserRouter  컴포넌트
### 설치
- [react-router-dom](https://www.npmjs.com/package/react-router-dom) 을 설치합니다. `npm install --save react-router-dom`
### BrowserRouter
- 리액트 라우터에 대한 첫 컴포넌트는 BrowserRouter  입니다.
  + BrowserRouter 는 URL 의 변경 내용을 청취(listen)합니다.
  + 그리고 실제로 바뀌면, 올바른 화면(screen)이 나왔는지 확인합니다.
- 리액트 라우터의 좋은 점은 전부 다 컴포넌트라는 것입니다. 멋지게 사용하고 또 쉽게 코드에 접근하게 해줍니다.
- BrowserRouter 의 역할을 보여주겠습니다. 리액트 라우터 저장소(repository) 코드입니다.
```javascript
class BrowserRouter extends React.Component {
  static propTypes = {
    basename: PropTypes.string,
    forceRefresh: PropTypes.bool,
    getUserConfirmation: PropTypes.func,
    keyLength: PropTypes.number,
    children: PropTypes.node
  }

  history = createHistory(this.props)

  render() {
    return <Router history={this.history} children={this.props.children}  />
  }
}
```
- `BrowserRouter` 가 실제로 하는 일은 `Router` 컴포넌트를 렌더링하고 `history` prop 을 전달하는 것입니다.
  + `history` 란 리액트 Training 에서 만든 history 라이브러리에서 나온 것입니다. 이 라이브러리는 다양한 환경의 차이점을 추상화하고 history 스택을 관리, 탐색, 탐색 확인, 그리고 세션 간의 state 를 유지할 수 있는 최소한의 API 를 제공합니다.
  + 간단히 말해서 `BrowserRouter` 를 사용할 때 URL 의 변경 사항을 듣고 앱이 변경 사항을 인식하게 해주는 `history` 객체를 만드는 것입니다.
### contacts 앱에 설치하기
- index.js 에 들어가 BrowserRouter  경로를 import 합니다.
```javascript
import { BrowserRouter } from 'react-router-dom'
```
  + 그리고 BrowserRouter 에서 전체 앱을 포장합니다.
```javascript
ReactDOM.render(
  <BrowserRouter><App /></BrowserRouter>,
  document.getElementById('root')
);
```
  + 이 설정은 라우터가 다음에 가져올 다른 모든 컴포넌트와 함께 작업할 수 있도록 만들어줍니다. 그리고 URL 을 청취(listen)하고 URL 이 바뀌면 다른 컴포넌트에 알립니다.
### 정리
- 요약하자면 리액트 라우터가 제대로 작동하려면 `BrowserRouter` 컴포넌트에서 전체 앱을 포장해야 합니다.
- 또한 `BrowserRouter` 는 앱에서 URL 의 변경 사항을 인삭하게 해주는 history 라이브러리를 래핑(wrap)합니다.
- [history github](https://github.com/reacttraining/history)

## Link Component
- 리액트 라우터의 Link Component 는 중요합니다. 그것은 사용자가 앱을 탐색할 방법입니다.
  + 사용자가 링크를 클릭하면 <Link /> 는 BrowserRouter 와 대화해서 URL 을 업데이트하도록 알립니다.
  + 이 컴포넌트는 사용자가 웹에서의 링크가 기대하는 바를 수행합니다.
- `Link` 는 앱 주위에서 선언적이고 접근가능한 탐색을 제공하는 간단한 방법입니다.
  + `to` prop 을 `Link` 컴포넌트에 전달하면 앱에 전달할 경로를 알려줍니다.
```javascript
<Link to="/about">About</Link>
```
- 복잡한 방식(예: 쿼리 매개변수를 전달하거나 페이지의 특정 부분과 연결하기)으로 링크를 작성하는 것도 가능합니다. 만약 새로운 경로에 state 를 전달하려면 아래처럼 객체를 전달하는 코드를 작성합니다.
```javascript
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}>
  Courses
</Link>
```
  + 항상 위의 형식으로 작성할 필요는 없습니다만 존재한다는 것만 알아두세요. 링크에 대한 [문서](https://reacttraining.com/react-router/web/api/Link)입니다.
### contacts 앱에 적용
- ListContacts 화면에 링크를 업데이트해봅니다.
  + 현재 새 연락처를 만드는 새 페이지 링크가 있는 상황입니다.
- 우선 Link 를 import 합니다.
```javascript
import { Link } from 'react-router-dom'
```
- 그리고 <a> 태그로 링크를 걸었던 부분을 <Link> 컴포넌트로 바꿉니다.
  + 이제 링크 컴포넌트는 실제로 앵커(a) 태그를 렌더링합니다.
  + 그리고 href 를 지우고 'to' prop 을 작성합니다. 그리고 실제 URL 을 적습니다.
  + onClick 을 지웁니다. 리액트 라우터가 클릭 작업을 알아서 해주기 때문입니다.
- 완성입니다. 이제 BrowserRouter, 링크, 클릭 시 브라우저의 URL 업데이트 작업을 가집니다.
### 정리
- 리액트 라우터는 앱 주위에 선언적이고 접근 가능한 탐색을 추가해주는 `Link` 컴포넌트를 제공합니다.
- 앵커 태그 대신 <Link> 태그를 사용합니다.
- 링크 컴포넌트는 앱으로 사용자가 접근할 좋은 방법입니다. `to` prop 을 링크에 전달해 사용자를 절대 경로로 안내합니다.

## Route Component
- <Route> 는 URL 과 일치 혹은 불일치한 경로를 사용합니다.
  + 만약 경로가 URL 과 일치하다면, Route 는 UI 를 렌더링합니다. 하지만 아무것도 렌더링하지 않는다면 그건 불일치한 것입니다.
- Route 는 예전에 screen 이 list 인지 create 인지 확인했던 것처럼 비슷한 일을 합니다.
  + 하지만 컴포넌트 state 를 체크하는 대신 Route 는 URL 을 체크합니다. 이걸 설치하면 뒤로가기를 쓸 수 있게 될 것입니다.
### contacts 앱에 설정하기
- 이제 리액트 라우트로 URL 과 UI 를 관리해봅니다.
- 우선 App.js 에서 Route 를 import 합니다.
```javascript
import { Route } from 'react-router-dom'
```
- 이제 screen state 를 체크했던 걸 Route 렌더링으로 바꿔봅니다.
  + Route path 가 URL 과 맡다면 해당 route 를 렌더링합니다.
  + <CreateContact> 도 같은 방식으로 바꿉니다.
```javascript
<div className="app">
  <Route exact path="/" render={() => (
    <ListContacts
      contacts={this.state.contacts}
      onDeleteContact={this.removeContact}
    />
  )}/>
  <Route path="/create" component={CreateContact}/>
</div>
```
  + 위를 component 가 아닌 render prop 을 사용한 이유는 <ListContacts> 에게 props(contacts, onDeleteContact, onNavigate) 를 전달하기 위해서입니다.
  + 반면 아래는 경로가 맞으면 이 컴포넌트를 렌더링해주세요 라는 뜻입니다. 그래서 URL 이 맞다면 CreateContact 의 screen 을 렌더링 할 것입니다.
- 위의 것의 경로를 `path="/"` 로만 하면 `/create` 일 때 두 화면 모두 띄울 것입니다.
  + 그래서 정확한 경로를 지정해줍니다. `exact path="/"`
  + 이것이 exact path 와 기본 path 의 차이점입니다. 정확히 맞거나 아니면 부분적으로 일치하거나 입니다.
- 이제 뒤로가기도 적용되고 URL 로 이동할 수 있게 됐습니다. 그러니 state 의 screen 을 지워도 됩니다. 또 ListContacts 의 onNavigate prop 도 필요 없습니다.
- 알아야 할 점은 리액트 라우터는 URL 로 UI 를 동기화할 라우트를 가져오는데 앱을 크게 바꾸지는 않는다는 것입니다.
### 정리
- 라우터 컴포넌트에 래핑된 컴포넌트는 URL 의 일부 앞부분이 일치할 때 렌더링됩니다. 만약 exact 플래그로 설정되면 완전히 일치할 때만 경로가 맞게 됩니다.
- Route 컴포넌트는 Route 의 render prop 을 사용해서 라우터가 렌더링할 특정 컴포넌트에 props 을 전달해줍니다.
  + render 는 컴포넌트를 렌더링하는 역할을 하며, 렌더링된 컴포넌트에 props 를 전달할 수 있습니다.
- 요약하자면 Route 컴포넌트는 현재 URL 경로를 기반으로 렌더링되는 컴포넌트를 결정하는 컴포넌트입니다. 따라서 리액트 라우터를 사용해 애플리케이션을 작성하는데 중요한 요소입니다.

## contacts 앱 form 마무리
### contact form 만들기
- 이번에는 ImageInput.js 파일이 필요합니다. 없다면 [github](https://github.com/udacity/reactnd-contacts-complete/blob/master/src/ImageInput.js) 으로 복사합니다.
- ImageInput 컴포넌트는 데이터 URL 을 서버에 제출하기 전에 이미지 파일을 동적으로 읽고 크기를 조정하는 커스텀 `<input>` 입니다. 그리고 이미지의 미리보기를 보여줍니다.
  + 구글은 이 컴포넌트가 웹에서 파일 및 이미지 관련 기능을 가지고 있어서 사용자에게 제공하기로 결정했습니다.
- CreateContact 컴포넌트에 콘텐츠를 추가합니다.
  + 우선 이미지 URL 을 불러올 링크 컴포넌트와 ImageInput 컴포넌트를 import 하는 문장을 작성합니다.
```javascript
import { Link } from 'react-router-dom'
import ImageInput from './ImageInput'
```
  + 그리고 이미지 업로드하는 <Link>, 이름과 이메일을 적을 <input>, 버튼을 작성합니다.
```javascript
class CreateContact extends Component {
  render() {
    return (
      <div>
        <Link
          className="close-create-contact"
          to="/"
        >close
        </Link>
        <form className="create-contact-form">
          <ImageInput
            className="create-contact-avatar-input"
            name="avatarURL"
            maxHeight={64}
          />
          <div className="create-contact-details">
            <input type="text" name="name" placeholder="Name"/>
            <input type="text" name="email" placeholder="Email"/>
            <button>Add Contact</button>
          </div>
        </form>
      </div>
    )
  }
}
```
### form 데이터를 직렬화하기
- 이 시점에서 form 은 사용자 입력(contacts 앱에서는 이름, 이메일)의 값을 직렬화해서 URL 에 쿼리 문자열로 추가합니다. (예:localhost:3000/create?avartarURL=&name=Jin+abc&email=aa!%40google.com)
  + 앱이 이러한 form 필드를 독자적으로 직렬화하게 만들면 몇 가지 기능을 추가할 수 있습니다.
- form-serialize 패키지를 사용해 정보들을 자바스크립트 객체로 출력해 앱이 사용하게 해줍니다. `npm install --save form-serialize`
#### contacts 앱에 적용하기
- form 이 브라우저에서 작동하는 방식입니다.
 + `name="name"` 필드의 name 을 가져와서 그 내부의 값을 URL 로 직렬화합니다.
 + 이번에는 자바스크립트를 써서 이 방식을 적용해봅니다.
- 그래서 form 을 직렬화하고 앱에게 'contact 를 만들고 싶다' 고 전달합니다.
  + 그러면 앱은 contact 를 저장하고 이것을 state 에 추가하는 작업을 처리할 것입니다. 이런 form 의 작업은 단순히 앱에 말을 전달하는 것입니다.
- 따라서 form 을 제출할 때 브라우저가 이 form 을 사용하는 대신 직접 form 을 제어해봅니다.
  + `handleSubmit` 라는 핸들러를 만듭니다. 이것은 이벤트를 매개변수로 합니다.
  + e.preventDefault() 를 작성하면 브라우저는 더 이상 form 을 제출하지 않습니다. (URL 에 name, avartarURL, email 이 뜨지 않게 됩니다.)
  + 이제 스스로 직렬화를 할 수 있습니다. `form-serialize` 라이브러리를 사용합니다.(NPM 같은 것입니다.)
```javascript
import serializeForm from 'form-serialize'
```
- `form-serialize` 라이브러리를 사용해 브라우저가 URL 에서 했던 것과 같은 것을 할 것입니다.
  + 다만 문자열로 직렬화하고 페이지를 새로고침하지 않고, 객체로 직렬화할 것입니다.
  + e.target 은 실제로 form 그 자체입니다.
  + {hash: true} 는 객체를 전달해줍니다.
  + 이제 객체 값을 가졌고 contact form 이 할 일은 더이상 없습니다.
- 다음엔 `this.props.onCreateContact(values)` 으로 values 를 전달합니다.
### 새로운 contact 로 서버 업데이트
- contact form 도 만들었고, 데이터를 직렬화해서 부모 컴포넌트에 전달하는 기능도 있습니다. 이제 마지막으로 contact 를 서버에 저장해봅니다.
  + App.js 의 마지막 <Route path="/create"> 에서 component 를 render 로 바꿔 프로퍼티를 넣을 수 있게 합니다.
```javascript
<Route path="/create" render={() => (
  <CreateContact
    onCreateContact={(contact) => {
      this.CreateContact(contact)
    }}
)}/>
```
- 그리고 render() 위에서 CreateContact 메소드를 만듭니다. 그것은 프로미스로 contact 를 서버로부터 전송받습니다. 그리고 그것을 list state 에 넣습니다.
  + 객체의 contacts 키에 현재 contacts 를 연결(concat) 합니다.
    + 연결은 새로운 배열을 return 하고 이제 새로운 contact 가 리스트에 있습니다.
```javascript
createContact(contact) {
  ContactsAPI.create(contact).then(contact => {
    this.setState(state => ({
      contacts: state.contacts.concat([ contact ])
    }))
  })
}
```
- 마지막으로 실제 URL 로 돌아가 contacts 를 나열하고 새로 추가된 것을 보게 만듭니다.
  + 리액트 라우터에서 `history` 라 불리는 prop 을 가져옵니다. 그리고 history.push('/') 를 작성합니다. 완성입니다.
```javascript
<Route path="/create" render={({ history }) => (
  <CreateContact
    onCreateContact={(contact) => {
      this.CreateContact(contact)
      history.push('/')
    }}
)}/>
```
- `concat()` 메소드 : 둘 이상의 배열들을 합병(merge)하는데 쓰입니다.
  + 이 메소드는 존재하는 배열을 바꾸진 않지만 새로운 배열 instance 를 return 합니다.
  + 따라서 concat 안의 contact 는 {} 이 아니라 [] 로 감싸야 합니다. ( {} 로 하면 새로 만든 contact 가 key 가 없다는 에러가 뜹니다. )
```javascript
const new_array = old_array.concat([value1, value2, ..., valueN])
```

## 마지막
- 리액트 라우터를 더 알아보려면 아래의 링크를 클릭합니다.
[Build your own React Router v4](https://tylermcginnis.com/build-your-own-react-router-v4/)
[공식 문서](https://reacttraining.com/react-router/web/guides/philosophy)
