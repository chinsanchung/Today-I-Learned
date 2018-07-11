# Render UI with External Data (Udacity)
## 소개
### `render()` 메소드는 오직 렌더링 목적입니다.
- **`render` 메소드에서 데이터를 fetch 하면 안됩니다.**
  + 컴포넌트의 `render()` 메소드는 오직 컴포넌트를 렌더링할 때에만 쓰여야 합니다.
  + 이 메소드는 HTTP 요청을 만들거나 콘텐츠를 표시하는 데이터를 가져올 수 없고(fetch), 또 DOM 을 변경해서는 안됩니다. Ajax 요청이나 비동기 작업도 안됩니다.
  + 그리고 `render()` 메소드는 이러한 작업을 수행하는 다른 함수도 호출해서는 안됩니다.
  + 오직 props 를 통해 인풋을 받아 UI(JSX)의 설명(description) 을 return 해야 합니다.(그 외에는 아무것도 return 하지 않습니다.)
- 따라서 `render()` 메소드는 오직 콘텐츠를 표시하는데 쓰이고, Ajax 요청 같은 것을 처리하는 코드는 **lifecycle events** 에서 다뤄야 합니다.
### 라이프사이클 이벤트
- 라이프사이클 이벤트는 컴포넌트에서 특별히 명명된 메소드입니다.
- 이 메소드는 컴포넌트 인스턴스에 자동으로 바인딩되며, 리액트는 컴포넌트의 수명동안 특정 시간에 자연스럽게 메소드를 호출합니다.
  + 다양한 라이프사이클 이벤트가 있지만 가장 일반적으로 사용하는 이벤트는 아래 4개입니다.
1. `componentWillMount()`
  + DOM 에 컴포넌트를 삽입하기 직전에 호출됩니다.
2. `componentDidMount()`
  + DOM 에 컴포넌트를 삽입한 직후에 호출됩니다.
3. `componentWillUnmount()`
  + DOM 이 컴포넌트를 삭제하기 직전에 호출됩니다.
4. `componentWillReceiveProps()`
  + 컴포넌트가 새로운 props 를 받을 때마다 호출됩니다.
- 이 메소드들을 사용하기 위해선 컴포넌트에 이름을 가진 메소드를 만들고 리액트가 그것을 호출할 것입니다.
  + 이 방법으로 리액트 컴포넌트의 라이프사이클의 다른 부분들에 쉽게 접근할 수 있습니다.
- 만약 API 에서 외부 데이터를 가져오고 싶다면, `componentDidMount()` 메소드가 바로 완벽한 해답입니다. 오직 `componentDidMount()` 에서만 Ajax 요청을 할 수 있습니다.

## componentDidMount 라이프사이클 이벤트
- 앞서 말했듯이 `componentDidMount()` 메소드는 컴포넌트가 DOM 에 추가된 직후에 실행되기에, 원격 데이터를 가져오거나 Ajax 요청을 수행할 경우에 사용합니다.
  + 공식 문서 : 이 메소드는 컴포넌트가 마운트된 직후 호출됩니다. DOM 노드가 필요한 초기화는 여기에 있어야 합니다. 원격 종점에서 데이터를 로드해야할 때 네트워크 요청을 인스턴스화하는게 좋습니다. 이 메소드의 state 를 설정하면 리렌더링이 실행됩니다.
```javascript
import React, { Component } from 'react';
import fetchUser from '../utils/UserAPI';

class User extends Component {
  constructor(props) {
    super(props)

    this.state = {
      name: '',
      age: ''
    }
  }
  componentDidMount() {
    fetchUser().then((user) => this.setState({
      name: user.name,
      age: user.age
    }))
  }

  render() {
    return (
      <div>
        <p>Name: {this.state.name}</p>
        <p>Age: {this.state.age}</p>
      </div>
    )
  }
}

export default User;
```
- 이 컴포넌트는 `componentDidMount()` 라이프사이클 이벤트를 가지고 있습니다. 이 메소드의 순서를 따라가봅니다.
  1. `render()` 메소드를 호출해 name 단락, age 단락이 있는 <div> 로 페이지를 업데이트합니다. `this.state.name` 과 `this.state.age`가 (처음엔)빈 문자열이므로 name, age 가 실제로는 표기되지 않는다는게 중요합니다.
  2. 컴포넌트가 탑재되면(mounted) `componentDidMount()` 라이프사이클 이벤트가 발생해서
    + 사용자 데이터베이스에 요청을 보내는 `UserAPI` 의 `fetchUser` 요청이 실행됩니다.
    + 데이터가 return 되면 `setState()` 가 호출되고 `name`, `age` 프로퍼티가 업데이트됩니다.
  3. state 가 바뀌고 `render()` 가 다시 호출됩니다. 페이지를 리렌더링하지만, 이번에는 `this.state.name` 과 `this.state.age` 은 값을 가집니다.
### contacts 앱에서 Ajax 요청하기
- 우선 모든 것들을 ContactsAPI 로 import 하는 문장을 작성합니다.
```
import * as ContactsAPI from './utils/ContactsAPI'
```
- 그리고 라이프사이클 이벤트를 컴포넌트 안에 추가합니다. API 요청이 필요한 곳에 componentDidMount 를 넣습니다.
  + 이 함수는 App 컴포넌트가 뷰에 마운트될 때마다 리액트에 의해 호출됩니다.
  + `ContactsAPI.getAll()` 을 호출합니다. 이것은 프로미스를 return 하므로 `.then` 을 부를 수 있습니다.
  + 이 함수는 contacts 와 함께 호출됩니다.
```javascript
componentDidMount() {
  ContactsAPI.getAll().then((contacts) => {
    this.setState({ contacts })
  })
}
```
  + this.setState 안의 객체는 contacts 키와 API 요청 `getAll().then((contacts))`에서 얻은 contacts 값입니다. ('{contacts : contacts}') 둘은 같기 때문에 값만 작성합니다.
- 그러면 이제 컴포넌트가 마운트되면(componentDidMount) API 요청을 만듭니다.(ContactsAPI)
  + API 요청이 해결되면 이 함수 '(contacts) => { this.setState({ contacts }) }' 는 특정 데이터(매개변수 contacts)와 함께 호출되며, 위의 경우 그 데이터는 contacts 입니다.
  + 그리고 contacts 를 가지면 state 를 업데이트하는 `setState` 를 호출합니다.
  + 그것은 컴포넌트를 리렌더링해서 새로운 contacts 가 아래 `ListContacts` 컴포넌트로 전달되어, 다음에 그것들을 뷰로 렌더링합니다.
  + 즉 `componentDidMount()`는 ContactsAPI 의 `getAll()` 메소드에서 return 된 `this.state.contacts`를 업데이트합니다
- 완성입니다. 이제 로컬 state 의 데이터가 아닌 API 요청으로 contacts 를 가져왔습니다.(fetch)
### Contacts 지우기
- 현재 ContactsAPI 에서 모든 사용자들을 가져와 `this.state.contacts` 에 더했습니다. 하지만 아직 contacts 를 지우는 일이 남았습니다.
  + contact 를 지울 때 `this.state.contacts` 에서 지우지만, 아직 백엔드 데이터베이스에는 남아있습니다.
  + 이제 ContactsAPI 의 `remove()` 메소드로 백엔드를 업데이트합니다.
- 전에 만든 `removeContact` 함수는 로컬 state 만 업데이트할 뿐 API 에 요청을 하진 않았습니다. 이제 remove() 를 써봅니다. 인수로 특정 contact 를 전달합니다.
```javascript
ContactsAPI.remove(contact)
```
- 이제 로컬 state 뿐만 아니라, API 요청으로 데이터베이스에서 지우는 작업을 할 수 있습니다.
  + 만약 모두 삭제했다면 백엔드 서버를 재시작하면 돌아옵니다. (아직 새 항목을 추가하는 기능이 없어서 임시방변으로 합니다.)
### 정리
- `componentDidMount()` 는 리액트가 제공하는 라이프사이클 이벤트 중 하나입니다.
- `componentDidMount()` 는 컴포넌트가 마운트된 후(렌더링 후)에 호출됩니다.
- 데이터를 동적으로 가져오거나(fetch) Ajax 요청을 실행할 때 이 메소드를 실행합니다.

## 4장 정리
- 정리하자면, 라이프사이클 이벤트는 리액트가 제공하는 특별한 메소드로 코드를 실행하기 위해 컴포넌트의 수명에서 다른 지점에 연결하게 해줍니다.
- 여러 라이프사이클 이벤트가 있으며 서로 다른 지점에서 실행되지만 세 가지 카테고리로 분류할 수 있습니다.
1. DOM 에 더하기
`constructor()`, `componentWillMount()`, `render()`, `componentDidMount()`
  - 이 이벤트들을 컴포넌트를 DOM 에 추가할 때 호출됩니다.
2. 리렌더링
`componentWillReceiveProps()`, `shouldComponentUpdate()`, `componentWillUpdate()`, `render()`, `componentDidUpdate()`
  - 이 이벤트들은 컴포넌트를 DOM 으로 리렌더링할 때 호출됩니다.
3. DOM 에서 지우기
`componentWillUnmount()`
  - 이 이벤트는 DOM 에서 컴포넌트를 지울 때 호출됩니다.
- 라이프사이클 이벤트를 설명한 그림으로 더 쉽게 이해할 수 있습니다.
![lifecycle event](/image/04_lifecycle-events.png)
  + 리액트 DOM 이 컴포넌트를 렌더링할 때 왼쪽 위에서부터 모든 게 시작됩니다.
  + 이미지에는 더 많은 이벤트들이 있지만 가장 자주 사용하는건 위에서 설명한 네 이벤트입니다. `componentDidMount()`, `componentWillMount()`, `componentWillUnmount()`, `componentWillReceiveProps()`.
