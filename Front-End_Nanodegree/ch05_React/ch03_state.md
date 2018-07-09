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
