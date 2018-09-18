# State Management (Udacity)
## 소개
- 리액트에는 세 가지 새로운 컨셉이 있습니다.
1. props : 데이터를 컴포넌트 안에 전달하게 해줍니다.
2. Functional Components : 리액트 컴포넌트를 만드는 직관적이고 대체가능한(alternative) 방법입니다.
3. Controlled Components : 앱의 양식(form)을 컴포넌트 `state` (state) 에 연결하게 해줍니다.
- [contacts](https://github.com/udacity/reactnd-contacts-complete/tree/starter-files-added) 와 백엔드용 [서버](https://github.com/udacity/reactnd-contacts-server) 를 설치합니다.
## props 로 데이터 전송
### 컴포넌트
- 기본적인 fetch 유저 함수입니다. 이 함수의 목적은 특정 사용자를 가져오는(fetch) 것입니다.
  + 문제는 어느 유저를 가져올지를 알려야 한다는 것입니다.
  + 함수를 정의할 때 매개변수를 추가하고, 함수를 호출할 때 사용자 이름을 전달하면 됩니다.
```javascript
function fetchUser(username) {}
fetchUser('jin')
```  
  + 리액트의 좋은 점은 함수에 대한 직관을 React 컴포넌트에도 적용할 수 있다는 것입니다.
- 매우 기본적인 리액트 컴포넌트입니다. 목적은 사용자를 UI 에 표시하는 것입니다.
  + 이것도 유저 컴포넌트에 사용자 이름을 전달해야만 합니다.(뷰에 사용자를 보여주기 위해서)
  + 그러기 위해선 커스텀 속성(attribute) 를 컴포넌트에 추가하고 거기에 값을 줘야 합니다.
```javascript
<User username='jin' />
```
- 이제 `this.props.username` 을 사용해 컴포넌트 정의 내부에서 그 값에 접근할 수 있습니다.
```javascript
class User extends React.Component {
  render() {
    return (
      <div>
        <p> Username: {this.props.username} </p>
        <p> Is Friend?: {this.props.friend} </p>
      </div>
    )
  }
}
<User username='jin' friend={true}/>
```
  + 컴포넌트에 추가된 모든 속성(attribute) 들은 해당 컴포넌트 내부에서 props 객체에 접근할 수 있습니다.
- 함수에 인수를 전달하는 것처럼 컴토넌트에 props 를 전달할 수도 있습니다.
  + 컴포넌트가 정규 자바스크립트 함수에 전달했던 인수에 접근할 수 있는 것처럼 `this.props` 를 사용해서 컴포넌트의 props 에 접근할 수 있습니다.
  + 같은 말로 컴포넌트로 전달한 모든 props 는 `this.props` 객체에서 접근할 수 있습니다.
### 정리
- `props` 는 리액트 컴포넌트에 전달하는 입력(input) 입니다. HTML 속성(attribute) 와 마찬가지로 prop 이름과 값이 컴포넌트에 추가됩니다.
```javascript
//컴포넌트에 prop 을 전달합니다.
<LogoutButton text='aaa' />
```
  + `text` 는 `prop` 이고, `aaa` 문자열은 값입니다.
- 모든 props 는 `this.props` 객체에 저장됩니다. 그래서 컴포넌트 내부의 `prop` 에 접근하려면 `this.props.text` 를 써야합니다.
```javascript
//컴포넌트 안의 prop 에 접근합니다.
render() {
  return <div>{this.props.text}</div>
}
```

## Functional Component
- 실은 오래된 함수 형태로도 컴포넌트를 만들 수 있습니다. 다만 더 이상은 이 키워드를 써서 컴포넌트의 props 에 접근하진 않습니다.
  + 대신 함수형 컴포넌트는 함수에 대한 첫 번째 인수로 props 를 전달합니다. (리액트는 특정 stateless 함수형 컴포넌트를 전달할 것입니다.)
  + 정리하자면 모든 컴포넌트가 render 메소드만 사용해서 콘텐츠를 보여준다면, 컴포넌트를 stateless 함수형 컴포넌트로 리팩토링할 수 있습니다. 보통의 함수 형태에 props 를 첫 인수로 가집니다.
```javascript
function ListContacts(props) {
  return (
    <ol className='contact-list'>
    //this.props.contact 가 아닙니다.
      {props.contacts.map((contact) => (
        /*...*/
      ))}
    </ol>
  )
}
```
- `stateless functional components` 는 직관이 매우 뛰어납니다.
  + 이미 익숙한 함수를 사용하고, 이 함수는 props 를 인수로 취해  UI 에 대한 설명을 return 합니다.
  + 함수형 컴포넌트는 `this` 키워드가 없습니다. 함수를 호출할 컨택스트에 대해 걱정하지 않아도 됩니다.
- 비교 : state 없이 props 만 있는 컴포넌트일 경우 함수형 컴포넌트로 작성할 수 있습니다. 함수형 컴포넌트는 state, componentDidMount, componentWillMount 등의 함수를 가지지 않고 단지 HTML 을 리턴하는 역할만 합니다.
  + 함수형 컴포넌트는 매개변수로 props 를 사용합니다. 다만 매개변수에 괄호로 감싸서 사용해야 합니다.
```javascript
//기본 컴포넌트
class Test from Components {
  render() {
    return (
      <div className="test">
        <p>{this.props.testText}</p>
      </div>
    )
  }
}
//함수형 컴포넌트
function Test({testText}) {
  return (
    <div className="test">
      <p>{testText}</p>
    </div>
  )
}

```
### 정리
- 컴포넌트가 내부 `state` 를 추적하지 못하는 경우(즉 실제로는 render() 메소드만 있는 경우), 컴포넌트를 stateless functional component 로 선언할 수 있습니다.
- 리액트 컴포넌트는 실제 렌더링을 위해 HTML 을 return 하는 단순한 자바스크립트 함수일 뿐입니다. 따라서 아래의 예 두 개는 모두 같습니다.
```javascript
class Email extends React.Component {
  render() {
    return (
      <div>
        {this.props.text}
      </div>
    );
  }
};

const Email = (props) => (
  <div>
    {props.text}
  </div>
);
```
  + 두 번째 예시에서 `this.props` 에서 props 에 접근하지 않고 함수 자체의 인수로써 직접 props 를 전달한 것을 볼 수 있습니다.
  + 이 정규 자바스크립트 함수는 Email 컴포넌트의 render() 메소드로 사용할 수 있습니다.

## state 를 컴포넌트에 더하기
### state
- 앞에서 `props` 가 컴포넌트의 속성(attribute)을 참조하고 결국 `props` 는 변하지 않는 읽기 전용 데이터를 나타냅니다.
- 반면 컴포넌트의 `state` 는 페이지에 렌더링되는 것에 최종적으로 영향을 주는 변경 가능한 데이터입니다.
  + `state` 는 리액트 컴포넌트의 핵심 프로퍼티입니다. `state` 의 사용법과 설정에 익숙해지면 앱 UI 를 간소화하는데 도움을 줄 겁니다.
  + `state` 는 컴포넌트 자체에 의해 내부적으로 관리되며 사용자의 입력으로 시간이 지나면서 바뀝니다.(예: 버튼 클릭)
- 여기서는 state 관리의 복잡성을 개별 컴포넌트로 캡슐화하는 방법을 알아봅니다.
### 더하기
- 리액트의 컴포넌트 모델 덕분에 개별 컴포넌트에 `state` 관리의 복잡성을 캡슐화할 수 있습니다. 이를 통해서 여러 작은 앱을 구축해 커다란 앱을 만들게 돕습니다.
- `state` 를 컴포넌트에 더하려면 값이 객체인 `state` 프로퍼티를 클래스에 넣어야 합니다.
  + 이 객체는 컴포넌트의 `state` 를 나타냅니다. 각 키와 객체는 이 컴포넌트에 대한 고유한 `state` 를 나타냅니다.
  + 리액트의 좋은 점 중 하나는 컴포넌트가 어떻게 나타나고 또 애플리케이션의 현재 `state` 를 분리해서 보여준다는 점입니다. 분리로 인해서 UI 나 애플리케이션의 모양은 단순한 앱 `state` 의 함수 형태입니다.
  + 리액트와 함께라면, 걱정해야 할 점은 오직 애플리케이션의 `state` 이며 UI 는 해당 `state` 를 기반으로 어떻게 바뀌었는지 입니다.
```javascript
class User extends React.Component {
  state = {
    username: 'Tyler'
  }
  render() {
    return (
      <p>Username: {this.state.username}</p>
    )
  }
}
```
  + props 를 사용해 `this.state.username` 으로 `state` 에 있는 username 프로퍼티에 접근합니다.
  + `constructor()` 메소드가 아닌 클래스 내부에 직접적으로 `state` 객체를 넣었습니다.
```javascript
//이렇게 하지 않았습니다.
class User extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: 'Tyler'
    }
  }
}
```
  + `constructor()` 외부에 `state` 를 두는 것은 그것이 언어에 대한 새로운 변화를 제안하는 `class field` 임을 의미합니다. 자바스크립트에서는 지원 안하지만 Babel 덕분에 사용할 수 있습니다.
### contacts 에서 state 관리 사용하기
- contacts 데이터가 바뀌면 리액트가 알아채고 UI 를 업데이트하도록 바꿔야 합니다.
  + 그러려면 contacts 변수를 컴포넌트 안으로 옮겨서 앱 컴포넌트가 `state` 를 관리할 수 있게 하고 또 언제나 `state` 가 바뀔 때 리액트는 그것을 알고 `state` 변화에 따른 UI 변경을 해줘야 합니다.
- App 클래스에 state 프로퍼티를 더합니다. 그리고 contacts 변수의 데이터를 객체 형태로 넣습니다.
### 초기 state 안의 props
- 컴포넌트의 초기 state 를 정의할 때는 해당 state 를 props 로 초기화하면 안됩니다. 오류가 발생합니다.
  + 왜냐면 state 는 컴포넌트가 처음 생성될 때에만 props 로 초기화되기 때문입니다.
```javascript
this.state = {
  user: props.user
}
```
- 위의 예에서 `props` 가 업데이트된다면 컴포넌트를 새로고침(refresh)하지 않는 한 현재 state 는 바뀌지 않습니다.
  + 컴포넌트의 초기 state 를 만들기 위해서 props 를 사용하면 데이터 중복이 발생합니다.
### 정리
- 컴포넌트가 자신의 state 를 관리한다면, 리액트가 해당 state 가 바뀔 때마다 이를 인식해 페이지에 필요한 업데이트를 자동으로 수행할 것입니다.
  + 이는 리액트를 사용해 UI 컴포넌트를 구축할 때 얻을 수 있는 이점 중 하나입니다.
  + 페이지를 다시 렌더링할 때는 state 를 업데이트만 하면 되기 떄문입니다.
  + 업데이트가 있을 때마다 페이지의 어떤 부분이 바뀐건지 정확하게 추적할 필요가 없습니다. 또 페이지를 효율적으로 렌더링할 방법을 결정할 필요도 없습니다.
- 리액트는 이전 출력(output)과 새 출력을 비교하고 변경사항을 확인, 다음 결정을 내립니다.
  + 이러한 결정 과정을 `조정(Reconciliation)`이라고 합니다.

## setState 로 state 업데이트
### state 업데이트
```javascript
//이렇게 직접 state 를 바꾸면 안됩니다.
componentDidMount() {
  setTimeout(() => {
    this.state.greeting = 'hello'
  }, 5000)
}
```
- 참고로 state 를 직접 업데이트하면 render 실행 과정(`componentWillMount` => `render` => `componentDidMount`)이 작동하질 않게 됩니다. 그래서 `this.setState`라는 함수를 사용해야 합니다.
- `setState` 는 두 가지 사용법이 있습니다.
  + 우선 `setState` 에 함수를 전달하는 것입니다. 이 함수는 첫 번째 인수로 이전 state 를 전달합니다. 이 함수가 return 한 새 객체는 현재 state 와 합병해서(merge) 컴포넌트의 새로운 state 를 만듭니다.
```javascript
this.setState((prevState) => ({
  count:prevState.count + 1
}))
```
  + 두번째 방법은 `setState` 에 객체를 전달하는 것입니다. 객체는 현재 state 와 합병해서(merge) 컴포넌트의 새로운 state 를 만듭니다.
```javascript
this.setState({
  username: 'Tyler'
})
```
- 함수 setState 는 컴포넌트의 새로운 state 가 이전 state 를 의존할 때 사용합니다.
  + 그 외에는 객체 setState 를 사용합니다. 어떤 방법으로 사용하던 결과는 같습니다.
- `setState` 를 호출할 때마다 리액트는 기본적으로 전체 애플리케이션을 다시 렌더링하고 UI 를 업데이트합니다.
  + 그래서 리액트에서는 UI 란 그저 state 의 함수일 뿐입니다.
  + state 가 바뀌면 UI 는 그에 따라 자동으로 업데이트합니다.
### contact 앱에서의 설정
- remove 버튼으로 state 의 특정 데이터를 바꾸고 싶지만 문제는 state 는 App 컴포넌트에, remove 버튼은 ListContacts 컴포넌트에 있다는 것입니다.
  + 그래서 App 컴포넌트 안에 함수를 만들어 state 를 업데이트하도록 만들 겁니다. 그리고 그 함수를 아래의 ListContacts 컴포넌트의 prop 으로 전달할 것입니다.
  + 다음으로 ListContacts 컴포넌트 안의 버튼을 연결합니다. 그래서 클릭하면 특정 contact 를 전달하고 호출한 다음, 컴포넌트의 현재 state 를 업데이트합니다.
- 로컬 컴포넌트 state 를 업데이트하려면 setState 메소드를 써야 합니다.
  + 두 방법 중에 함수 setState 를 씁니다. 왜냐면 이전 state 를 기준으로 contacts 를 업데이트하기 때문입니다.
  + 이 setState 는 새로운 contacts 리스트를 return 합니다.
```javascript
//App.js
/* (contact) 는 클릭한 것, (state) 는 현재의 state.
state.contacts 는 현재의 contacts
(c) 는 map() 처럼 반복해서 사용할 변수 */
removeContact = (contact) => {
  this.setState((state) => ({
//state 의 contact id !== 클릭한 contact id
    contacts: state.contacts.filter((c) => c.id !== contact.id)
  }))
}
//밑의 호출
<ListContacts onDeleteContact={this.removeContact} contacts={this.state.contacts} />
```
```javascript
//ListContacts.js
<button onClick={() => props.onDeleteContact(contact)} className='contact-remove'>Remove</button>
```
  + 화살표 함수는 props.onDeleteContact 를 호출해 반복하는 특정 contact 로 전달합니다.
  + 여기의 contact 는 클릭한 특정 contact 입니다.
### state 설정하기
- 앞에서 초기화할 때 컴포넌트의 state 를 정의하는 방법을 살펴봤습니다.
  + state 는 렌더링된 결과(output) 에 최종적으로 영향을 주는 바뀌는 정보를 반영하기에, 컴포넌트는 `this.setState()` 를 사용해 라이프사이클 전체에서 state 를 업데이트할 수 있습니다.
  + 로컬 state 가 바뀔 때마다 리액트는 `render()` 메소드를 호출해 컴포넌트의 렌더링을 다시 시작합니다.
- `setState()` 를 사용하는 두 방법이 있습니다.
- 첫번째는 state 업데이트를 병합하는 것입니다. 이 방법으로 `this.state.subject` 를 새로운 값으로 대체하면서도 `this.state.message` 를 그대로 둘 수 있습니다.
```javascript
class Email extends React.Component {
  state = {
    subject: '',
    message: ''
  }
  // ...
});
```
  + 컴포넌트의 초기 state 는 두 가지 프로퍼티(subject, message)가
  포함되어 있지만 독립적으로 업데이트할 수 있습니다.
```javascript
this.setState({
  subject: 'Hello'
})
```
- 두번째로는 객체가 아닌 함수를 전달하는 것입니다.
```javascript
this.setState((prevState) => ({
  count: prevState.count + 1
}))
```
  + 여기서 전달한 함수는 `prevState` 인수를 취합니다.
  + 컴포넌트의 새 state 가 이전의 state 에 의존하는 경우(여기서는 이전 state 의 카운트를 1 증가시킬 때), `setState()` 함수를 사용합니다.
- state 퀴즈 정답
  + `setState()` 를 호출할 때마다 컴포넌트는 새로운 state 로 `render()` 를 호출합니다.
  + 객체를 `setState()` 에 전달해서 state 업데이트를 병합(merge)할 수 있습니다.
  + state 업데이트는 비동기일 수 있습니다.(즉 `setState()` 는 첫번째 인수로 이전 state 의 함수로 정할 수 있습니다.)
### 정리
- 컴포넌트는 초기화될 때 state 를 설정할 수 있지만, 일반적으로는 사용자의 입력으로 시간이 지남에 따라 state 가 바뀔 것으로 기대됩니다.
- 컴포넌트는 `this.setState()` 을 사용해 자신의 내부 state 를 바꿀 수 있습니다.
- state 가 바뀔 때마다 리액트는 바뀐다는걸 알고 `render()` 를 호출해 컴포넌트를 다시 렌더링합니다.
- 이렇게 하면 앱의 UI 를 빠르고 효율적으로 업데이트할 수 있습니다.

## PropTypes
### `PropTypes` 으로 컴포넌트의 prop 타입 체크
- 앱의 추가 기능을 구현할 때마다 컴포넌트를 자주 디버깅하게 될 겁니다.(만약 컴포넌트에 전달하는 `props`가 의도치 않은 데이터 유형이라면 등)
- `PropTypes` 는 바로 보고 싶은 데이터 유형을 정의하고, 컴포넌트로 전달한 props 가 예상과 달리 불일치할 떄 경고 메시지를 표시하는 패키지입니다.
- 다시 말해 `PropTypes` 는 특정 컴포넌트에 전달하는 특정 유형의 props 를 지정할 수 있게 해줍니다. 또한 그것이 필요한지 아닌지를 지정하게 해줍니다.
  + 또 좋은 점은 PropTypes 는 경고 메시지를 콘솔에 띄워주는 제3자 컴포넌트라는 점입니다.(NPM 으로 설치하는 컴포넌트입니다.)
- prop-type 를 설치합니다.
```
npm install --save prop-types
```
- 설치 후 ListContacts.js 에서 import 를 작성해야합니다.
```javascript
import PropTypes from 'prop-types'
```
  + 그리고 stateless 함수형 컴포넌트인 ListContacts 에 프로퍼티를 추가합니다.
```javascript
//배열은 .array  함수는 .func 을 썼습니다.
/* ListContacts.propTypes 의 p 는 소문자, 밑은 대문자 P 입니다.
위의 것은 변수, 즉 함수의 프로퍼티(객체 자체)입니다.
후자는 import 로 가져온 라이브러이입니다. */
ListContacts.propTypes = {
  contacts: PropTypes.array.isRequired,
  onDeleteContact: PropTypes.func.isRequired
}
```
- 컴포넌트에 전달해야 하는 특정 props 를 이해하려면, 문서(documentation)를 읽거나 또는 만약 컴포넌트의 작성자가 PropTypes 를 사용한다면 그것을 사용할 수도 있습니다.
  + 위의 ListContacts.propTypes 는 특정 컴포넌트를 사용하는 방법에 대한 훌륭한 문서(documentation) 역할을 할 것입니다.
### 정리
- `PropTypes` 는 리액트 앱에서 의도한 데이터 유형을 검증할 좋은 방법입니다.
- `PropTypes` 를 사용해 데이터를 확인하는 방법은 개발 중일 때 버그를 식별해 원활한 사용에 도움을 줍니다.

## Controlled Component
### 정의
- 일반적으로 form state 는 DOM 안에 있습니다. 하지만 리액트는 애플리케이션 내부의 state 를 효과적으로 관리합니다.
  + 그렇다면 form 을 리액트로 처리하려면 어떻게 해야 할까요.
  + 이것을 `Controlled Component` 로 해결할 수 있습니다.
- `Controlled Component` 는 form 을 렌더링하지만, form state 의 진실 소스(source of truth)는 DOM 내부가 아닌 컴포넌트 내부에 있습니다.
  + 이런 이름을 가진 이유는 리액트가 form 의 state 를 제어하기 때문입니다.
```javascript
class NameForm extends React.Component {
  state = { email: '' }
  handleChange = (event) => {
    this.setState({email: event.target.value})
  }
  render() {
    return (
      <form>
/* input 필드의 텍스트는 컴포넌트 state 의 email 프로퍼티가 될 겁니다.
따라서 input 필드의 state 를 업데이트할 유일한 방법은
컴포넌트 state 의 email 프로퍼티를 업데이트하는 것입니다.*/
        <input type="text" value={this.state.email}
        onChange={this.handleChange} \>
      </form>
    )
  }
}
```
  + 이것이 진실한 Controlled Component 입니다. 왜냐면 리액트가 state 의 이메일 프로퍼티를 제어하고 있어서입니다.
  + 만일 input 필드를 바꾸고 싶다면 handleChange 메소드를 만들고 setState 를 사용해 이메일 주소를 업데이트하도록 만들어야 합니다.
  + 만약 input 필드가 바뀐다면 위의 메소드를 onChange 속성(attribute) 에 전달해 호출할 수 있습니다.
- 비록 Controlled Component 가 쳐야 할 코드가 많지만 좋은 점들이 있습니다.
  + 첫째로 즉시 입력 유효성 검사(instant input validation)를 지원합니다.
  + 둘째로 조건부로 form 버튼을 사용하게/못하게 제어해줍니다.
  + 셋째로 입력 포맷(input format)을 시행합니다.
- 이런 장점들은 몇몇 사용자 input 에 따라 UI 를 업데이트할 때 도움을 줍니다.
  + 이 점은 Controlled Component 뿐만 아니라 일반적으로 리액트의 핵심이기도 합니다.
- 애플리케이션 상태가 바뀐다면 UI 가 새로운 state 를 기반으로 업데이트됩니다.
### 리액트 개발자 도구들
- 리액트 개발자 도구는 각 props 및 state 와 함께 컴포넌트 계층 구조를 검사할 수 있게 해줍니다.
  + [크롬 확장 프로그램](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en-US) 을 설치하고 콘솔 창에서 리액트 탭을 확인하면 됩니다.
### contact 앱의 input 필드 생성
- contacts 앱에서는 input 부분을 만들 겁니다. 입력해서 리스트를 filter 해서 보여주게 만들 것입니다.
  + state 에 있는 특정 프로퍼티 값이 무엇이든 간에 input 필드를 바인딩할 것입니다. 또 UI 를 form 데이터를 기반으로 업데이트할 것입니다.
- 인풋 필드에 무엇을 치던 onChange 함수가 호출되고, updateQuery 를 호출해 인풋 필드 안의 문자열을 전달할 것입니다.
- `value` 속성(attribute) 은 `<input>` elemnt 에 설정됩니다. 표시되는 값은 항상 컴포넌트 state 에 있는 값이 되어 state 를 'single source of truth' 로 만듭니다.
- 리액트가 궁극적으로 input form element 값을 제어하기 때문에 이 컴포넌트는 Controlled Component 로 간주됩니다.
- 사용자 input 이 `ListContacts` 컴포넌트의 자체 `state` 에 미치는 영향은 아래와 같습니다.
  + 사용자가 텍스트를 인풋 필드에 입력합니다.
  + 이벤트 리스너는 모든 onChange 이벤트에 대해 `updateQuery()` 함수를 호출합니다.
  + 그 다음 `updateQuery()` 가 `setState()` 를 호출해서 새로운 state 에서 병합해 컴포넌트의 내부 state 를 업데이트합니다.
  + state 가 바뀐 후 `ListContacts` 컴포넌트가 다시 렌더링됩니다.
- 이러한 업데이트한 state 를 활용해 contacts 를 필터링하는 방법을 알아봅니다.
  + 아래의 패키지가 필요합니다. (escape-string-regxp , sort-by)`npm install --save escape-string-regexp sort-by`
  + 그리고 ListContacts.js 에 import 를 적습니다.
```javascript
import escapeRegExp from 'escape-string-regexp'
import sortBy from 'sort-by'
```
```javascript
let showingContacts
if (this.state.query) {
  const match = new RegExp(escapeRegExp(this.state.query), 'i')
  showingContacts = this.props.contacts.filter((contact) => match.test(contact.name))
} else {
  showingContacts = this.props.contacts
}
```
  + 정규 표현식 객체를 만들어 match 변수에 정의했습니다. 그리고 contacts 의 name 의 포멧을 테스트했습니다.
  + 만약 쿼리 안에 $ 나 / 등의 문자를 입력할 경우 그것들을 회피합니다(escapeRegExp). 그래서 이 특수 문자를 regexp 문자가 아닌 문자열 리터럴로 사용합니다.
  + i 는 case 를 무시하고 그것에 신경을 쓰지 않는 것입니다.
```javascript
showingContacts.sort(sortBy('name'))
```
  + `sortBy()` 가 하는 역할은 객체의 배열에서 특정 프로퍼티를 기준으로 정렬해주는 일종의 유틸리티 도우미입니다. 지금은 name 을 정렬합니다.
#### 정규 표현(Regular Expressions)
- 정규 표현식은 복잡하지만 프로그래밍의 패턴을 검증하는데 큰 가치가 있습니다.
- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D) 에서 정규 표현식에 대해 알아봅니다.
- 그리고 String`.match` 메소드가 정규 표현식을 사용해 텍스트 패턴을 확인하는 방법도 확인합니다. [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match)
### destructuring
- 현재 ListContacts 컴포넌트의 `render()` 메소드는 state 객체 (`this.state.query`)의 query, 그리고 props 객체(`this.props.contacts`)의 `contacts` 에 자주 접근합니다.
  + `props` 와 `state` 는 단순한 자바스크립트 객체이므로, 매번 이들을 `this.state.query` 와 `this.props.contacts` 으로 리팩토링하는 대신, ES6 으로 별개의 변수로 압축을 풀 수 있습니다.
  + 이런 압축해체(unpacking) 과정을 객체 destructuring 이라 부릅니다.
- 결론적으로 객체를 destructuring 하면 코드의 return 값을 바꾸진 않지만 상황을 더 깔끔하게 보여주게 만듭니다.
```javascript
const { contacts, onDeleteContact } = this.props
const { query } = this.state
//이렇게 하면 아래처럼 깔끔하게 작성할 수 있습니다.
if (this.state.query) //에서
if (query)

showingContacts = this.props.contacts //에서
showingContacts = contacts

this.props.onDeleteContact(contact) //에서
onDeleteContact(contact)
```
### 표시된 contacts 수를 보여주기
- input 에 입력하면 전체에서 몇 명이 필터링됐는지 숫자로 표시하고, 또 Show all 을 누르면 특정 필터를 초기화하는 기능을 넣을 겁니다.
### 퀴즈
- 각 state 업데이트는 연관된 핸들러 함수가 있습니다.
- form element 는 속성(attribute)을 통해 현재 값을 받습니다.
- form 인풋 값은 컴포넌트의 state 에 저장됩니다.
- controlled element 에 대한 이벤트 핸들러는 컴포넌트의 state 를 업데이트합니다.
### 정리
- Controlled Components 를 사용하면 리액트 state 가 form 데이터의 '단일 진실 소스'(single source of truth) 역할을 합니다.
  + 이것은 ListContacts 컴포넌트의 사용자 인풋이 궁극적으로 페이지의 렌더링을 다시 발생시키는(trigger) 방법입니다.
- ListContacts 컴포넌트에서는 컴포넌트가 form 을 렌더링할 뿐만 아니라 사용자 인풋을 기반으로 form 에서 발생하는 일들을 제어합니다.
  + 이럴 경우 이벤트 핸들러는 컴포넌트의 state 를 사용자의 검색 쿼리로 업데이트합니다.
  + 그리고 알다시피 리액트 state 를 바꾸면 페이지에 다시 렌더링되어 실제 검색 결과를 효과적으로 표시합니다.

## 프로그래머스 리액트 웹서비스의 12장
### Loading States
state 안을 비운 채로 시작해서 시간이 지나면 state 를 채우는 작업을 진행합니다.
1. 우선 state 를 비웁니다. 그런데 비우기만 하고 map 함수로 불러오기를 그대로 진행하면 에러가 뜹니다. 왜냐면 `this.state.aaa.map` 을 실행할 state 가 비어있기 때문입니다.
2. 그래서 우선 loading 이라는 문자만 띄우고, state 가 없을 때마다 loading 을 띄우고 아닐 경우 state 의 리스트를 출력하도록 만들어봅니다. states 를 map 으로 출력하는 함수를 render 메소드 안이 아니라 바깥으로 옮겨서 만듭니다.
참고로 함수 앞에 _ 를 쓴 이유는 리액트 자체 기능과 직접 만든 기능을 구분하기 위해서입니다.
```javascript
_renderMovies = () => {
  const movies = this.state.movies.map((movie, index) => {
    return <Movie title={movie.title} poster={movie.poster} key={index}
  })
  return movies;
}
```
3. 이제 state 안에 데이터가 있다면 render 의 리턴 안에 위의 함수를 실행하도록 해주는 삼항연산자를 만듭니다.
```javascript
render() {
  return (
    <div className="app">
      {this.state.movies ? this._renderMovies() : "Loading"}
    </div>
  )
}
```
4. 전에 만들었던 componentDidMount 에 setTimeout 을 넣어 5초 후에 state 를 치우도록 하면 완성입니다. render 를 우선 하고(Loading) 5초 후에 componentDidMount 로 state 를 채우면 `_renderMovies` 함수를 실행합니다.
