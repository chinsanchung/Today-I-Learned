# Redux (타이머 강의의 내용들)
### 6강 리덕스 설치
1. 리덕스를 설치하고 그 다음 리액트 리덕스를 설치해야 합니다. 리덕스는 리액트만을 위한게 아닌 앵귤러 등도 지원하기 때문에 리액트 리덕스도 따로 설치해야 합니다.
`npm install redux react-redux --save`

### 7강 리덕스 이론
리덕스는 간단히 말해 상태 관리 툴입니다. [영상 링크](https://www.youtube.com/watch?time_continue=42&v=CZ3fiRUwLdA)
1. 리덕스가 필요한 이유?
- 이유 1: 컴포넌트는 로컬 상태를, 앱은 전역 상태를 가지고 있어서입니다. 인스타그램을 예로 들면 하트는 빨강, 회색의 두 상태를 가집니다. 즉 하트 컴포넌트는 두 개의 로컬 상태를 가집니다.
전역 상태는 유저가 로그인한 여부를 예로 들 수 있습니다. 전역 상태는 모든 컴포넌트에 영향을 줍니다. (로그인했느냐 안했느냐에 따라 보이는 화면이 다르지 않나요. 그겁니다.)
- 이유 2: 때때로 상태는 공유해야만 합니다. 왜냐면 유저가 로그인한 여부(전역 상태)를 모든 컴포넌트가 알아야 하기 때문입니다. 하트 컴포넌트의 로컬 상태를 댓글 컴포넌트가 알아야 할 필요는 없지만, 모든 컴포넌트는 전역 상태인 로그인 여부를 알아야만 합니다.
- 이렇게 꼭 필요한 전역 상태는 어딘가에 저장하고 싶어한다면, 리덕스가 정답입니다.
**공유할 상태를 저장하는 방법이 리덕스이기 때문입니다. 리덕스는 글로벌 공유 상태를 저장합니다. 즉 리덕스는 상태 컨테이너입니다.**
- 리액트의 노멀 상태를 생각해보면, 상태를 바꿀 때 setState 로 상태를 바꾸고 렌더링을 하는데, 리덕스도 이와 비슷하게 동작합니다. 다만 전역으로 작동합니다.
2. 리덕스가 필요없는 경우?
- 1. 블로그를 만들 때는 필요없습니다. 메인 컴포넌트가 api 로 블로그를 불러오고, 각 포스트 컴포넌트에서 내용을 출력, 포스트마다 댓글 컴포넌트를 연결해 댓글을 출력합니다. 이 과정은 간단해서 리덕스가 필요 없습니다. 블로그 컨테이너로 모든 걸 관리 가능합니다.

- 여기서 리덕스는 어떨 때 필요할까요. 해당 포스트에 댓글을 달 때 필요합니다. 왜냐면 댓글에 답글을 달 수 있어야 하기 때문입니다.
답글을 다는 함수는 두 매개변수가 필요합니다. 작성자, 그리고 답글을 달 댓글입니다. 문제는 현 상황에선 작성자가 누구인지 알 수가 없습니다.
- 만약 처음부터 로그인한 유저를 프롭으로 전달한다면 어떨까요. 포스트, 댓글 모두 그 유저를 프롭으로 받게 말입니다.
하지면 이러면 `flying prop` 문제가 생깁니다. 댓글과 달리 포스트 컴포넌트는 로그인한 유저의 정보가 필요하지 않습니다. 단지 댓글 컴포넌트가 필요하기에 전달하는 것 뿐입니다.
*이 방식은 useless props 들이 생긴 좋지 않은 방식입니다.* 그리고 또한 이 방식은 메인 블로그 컨테이너에 모든 내용을 담고 있어야 한다는 건데 이 또한 단점입니다.
3. 리덕스
리덕스는 컴포넌트의 상태들을 저장한 박스와 같습니다. 각 컴포넌트가 부모 컨테이너로부터 정보를 물려받지 않고 리덕스로부터 상태를 가져오기만 하면 됩니다. 그러면 `flying prop`를 사용하지 않아도 됩니다. 또한 컴포넌트마다 모든 정보를 각각 불러와서 사용하고 그럴 필요도 없어집니다.
- 리덕스가 있다면 컴포넌트들은 서로에게 프롭을 전달할 필요 없이 리덕스 상태 저장소에서 필요한 걸 가져오는 걸로 끝낼 수 있습니다.
4. 리덕스는 전역 상태 컨테이너입니다.
그래서 컴포넌트가 필요한 것들만 뽑아서 사용하면 됩니다.
5. 알아야 할 사항들
- 상태는 단순한 객체 형태로 저장됩니다. 이 자바스크립트 객체를 `store`라고 부릅니다.
- 상태가 복잡해질 수 있기 때문에 리덕스는 상태 객체를 수정하는 데 있어서 매우 엄격합니다. (리덕스에서 객체 데이터를 수정하려면 '액션'을 `reducer`로 보내야(dispatch || send) 합니다. 즉 직접 수정은 안되고 간접적으로 바꿔야 합니다.)
- 만약 '액션'을 `reducer`로 전달한다면 `reducer`는 그 액션을 받은 후 프로그래머를 대신해서 객체 데이터를 바꿔줍니다.

### 9강
reducer 는 액션이 어떠냐에 따라 다른 행동을 합니다. 그래서 if else 문을 쓰면 될 것 같지만 그건 너무 길고 복잡해서, switch 문을 사용합니다.

### 10강 리듀서 만들기
현 강의에서는 redux ducks 방식으로 리듀서를 만듭니다. 순서는 다음과 같습니다.
  - 필요한 것들을 불러오고 그 다음 액션, action creator, 리듀서를 정의하고 리듀서 함수를 생성합니다. 그리고 export action creater 를 한 후 마지막으로 export reducer 를 작업합니다.

#### 순서
1. reducer.js 파일을 만듭니다.
2. 먼저 액션 단계입니다. 나중에 switch 문에서 쓸 변수를 만듭니다.
3. 액션 creator 를 만들고 그 안에 실행할 함수를 적습니다. 리턴값으로 액션 변수를 넣습니다.
4. 리덕스의 장점은 *initial state* 를 만들 수 있다는 것입니다. reducer 에 만들어봅니다. 실행중인지는 거짓(처음엔 실행 안하니까), 시간은 0, 지속시간도 지정합니다.
5.  그리고 리덕스를 만들어봅니다. 첫 매개변수로 상태를 전달하는데 기본적으로는 initialState 를 전달합니다. 그 다음 매개변수는 action 입니다. 액션을 보낼 때마다 리덕스는 자동으로 리듀서를 디폴트로 실행하게 됩니다.
첫째로 만든 액션 크리에이터의 함수를 리듀서로 전달할 것입니다. switch 문으로 작성합니다. switch 문의 default 로 처음 앱을 실행할 때의 상태를 만들어줍니다.
6. Reducer functions 정의
버튼을 눌러 액션을 보내면 리덕스는 리듀서를 통해서 해당 액션을 리덕스 데이터 객체에서 케이스를 가져오고, 데이터를 변환시킵니다.
- 그 전까지의 상태가 무엇이던 상관없이 그대로 놔두고 isPlaying 만 false 에서 true 로 바꿨습니다.
즉 현재 상태에서 isPlaying 을 *변환* 해서 덮어씌운것(overwritting)입니다.
7. export action creater 를 합니다. 여기서 export 하는 것들은 버튼을 눌렀을 때 실행하는 명령들입니다.
```javascript
//reducer.js
// Import

// Actions 정의
const START_TIMER = 'START_TIMER';
const RESTART_TIMER = 'RESTART_TIMER';
const ADD_SECOND = 'ADD_SECOND';
// Action Creator 정의
function startTimer() {
  return {
    type: START_TIMER
  }
}

function restartTimer() {
  return {
    type: RESTART_TIMER
  }
}

function addSecond() {
  return {
    type: ADD_SECOND
  }
}
// Reducer 정의
const TIMER_DURATION = 1500;

const initialState = {
  isPlaying: false,
  elapsedTime: 0,
  timerDuration: TIMER_DURATION
}
//return 해서 실행하는 함수들의 state 는 현 상태를 뜻합니다.
function reducer(state = initialState, action) {
  switch(action.type) {
    case START_TIMER:
      return applyStartTimer(state);
    case RESTART_TIMER:
      return applyRestartTimer(state);
    case ADD_SECOND:
      return applyAddSecond(state);
      default:
        return state;
  }
}
// Reducer functions 정의
function applyStartTimer(state) {
  return {
    ...state,
    isPlaying: true
  }
}

function applyRestartTimer(state) {
  return {
    ...state,
    isPlaying: false,
    elapsedTime: 0
  }
}
/*
현 상태의 시간이 제한시간보다 작다면 elapsedTime + 1 으로 1초 움직인 것을 상태에 저장합니다.
제한시간을 넘기면 isPlaying 을 false 로 해서 멈춥니다.
*/
function applyAddSecond(state) {
  if(state.elapsedTime < TIMER_DURATION) {
    return {
      ...state,
      elapsedTime: state.elapsedTime + 1
    }
  } else {
    return {
      ...state,
      isPlaying: false
    }
  }
}
// Export Action Creator
const actionCreators = {
  startTimer,
  restartTimer,
  addSecond
}
export { actionCreators };
// Export Reducer
export default reducer;
```
- 정리 : 시작 버튼을 누르면 => action creator 의 startTimer 를 실행 => 리듀서의 switch 문에서 case 가 START_TIMER 이므로 해당 리듀서 함수 실행 => applyStartTimer 실행 => 상태 변경

### 11강 리덕스를 프로젝트에 연결하기
타이머 앱과 리덕스를 연결하려면 우선 리듀서를 불러온 다음 App.js 의 렌더 부분을 바꾸면 됩니다.
```javascript
import reducer from './reducer';
import { createStore } from 'redux';
//App.js
let store = createStore(reducer);
//...
render() {
  return (
    <Provider store={store}>
      <Timer />
    </Provider>
  )
}
```
1. 아까 리듀서에서 `export default reducer;`로 했기에 import 를 하면 자동으로 리듀서를 불러옵니다. export default 는 아까 만든 리듀서 reducer() 를 뜻합니다.
2. export 된 후 스토어를 생성합니다. `import { createStore } from 'redux';` 로 스토어를 만들 수 있습니다.
3. 그리고 리듀서와 앱의 상태를 각 컴포넌트로 복사할 수 있어야 합니다. 이럴 때는 provider 를 사용합니다. `import { Provider } from 'react-redux';`
4. 스토어를 만듭니다. `let store = createStore(reducer)` (Provider 로 스토어를 복사하는데 쓰입니다.)
5. 렌더링할 내용을 Timer 가 아닌 Provider 로 바꿉니다. 프롭으로 store 를 넣습니다. 하위로 타이머를 넣으면 됩니다. 이제 타이머는 복사가능한 스토어를 가지게 됩니다.
  - Provider 의 역할은 스토어를 복사해서 하위 자식 컴포넌트에 넣는 것입니다. 참고로 Provider 를 바꿀 때마다 반드시 새로고침을 해야합니다.
6. components/Timer 에서 새 파일 presenter.js 를 만들고 index.js 의 내용을 옮김니다. index.js 에는 스토어에 필요한 것을 집어넣을 것입니다.
presenter.js 에는 자바스크립트를 전달(present)하기만 합니다. 상태나 리덕스 작업을 하지 않습니다. 상태, 리덕스 작업은 index.js 에서 하게 만듭니다. 즉, 컨테이너를 index.js 로 리덕스 관련 내용을 넣고, presenter.js 는 데이터를 보여주기만 합니다.
#### index.js
1. connect 를 import 합니다. connect 는 react-redux 의 기능입니다. 컴포넌트를 스토어에 연결하도록 돕습니다. 그리고 타이머 컴포넌트를 import 합니다.
2. mapStateProps 함수를 만듭니다. App.js 에서 만든 스토어에서 상태를 복사해서 컨테이너의 프롭에 붙여넣습니다. 매개변수 상태에서 내용들을 리턴합니다.
3. connect 로 위의 함수를 타이머 컴포넌트와 연결합니다.
```javascript
import { connect } from 'react-redux';
import Timer from './presenter';

function mapStateProps(state) {
  const { isPlaying, elapsedTime, timerDuration } = state;
  return (
    isPlaying,
    elapsedTime,
    timerDuration
  )
}
export default connect(mapStateProps)(Timer);
```
---
7. presenter.js 에서 스토어 내용들을 프롭 형태로 불러옵니다.
`const { isPlaying, elapsedTime, timerDuration } = this.props;`
8. presenter.js 의 버튼들을 isPlaying 에 맞춰서 바꿔봅니다. 삼항연산자도 좋지만 false 부분을 null 로 할 것이기에 && 을 사용해도 좋습니다.
```javascript
{!isPlaying && (
  <Button iconName="play-circle" />
)}
{isPlaying && (
  <Button iconName="stop-circle" />
)}
```

### 12강 액션을 컴포넌트에 연결
1. index.js 에서 bindActionCreators 를 불러옵니다. 이것은 액션들을 묶어줍니다.
2. mapDispatchToProps 함수를 생성합니다. 매개변수로는 dispatch 를 사용합니다.
  - dispatch : 액션을 리듀서로 보내는 함수입니다. 아래는 예시입니다.
```javascript
//액션
function startTimer() {
  return {
    type: START_TIMER
  }
}
//리듀서
function reducer(state) {
  //...
}
```
여기에 bindActionCreators 함수로 디스패치와 액션을 묶을 것입니다. 연결할 함수는 각각 import 해야합니다.
`import {actionCreators as timerActions } from '../../reducer';` 는 리듀서에서 export 한 액션 크리에이터들을 timerActions 로 묶어서 사용하겠다는 뜻입니다. 참고로 왼쪽의 이름은 자기 마음대로 지정할 수 있습니다.
3. export 에 mapDispatchToProps 함수를 추가해서 connect 합니다.
```javascript
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import {actionCreators as timerActions } from '../../reducer';
import Timer from './presenter';

function mapStateProps(state) {
  const { isPlaying, elapsedTime, timerDuration } = state;
  return {
    isPlaying,
    elapsedTime,
    timerDuration
  }
}

function mapDispatchToProps(dispatch) {
  return {
    startTimer: bindActionCreators(timerActions.startTimer, dispatch),
    restartTimer: bindActionCreators(timerActions.restartTimer, dispatch)
  }
}

export default connect(mapStateProps, mapDispatchToProps)(Timer);
```
4. presenter.js 의 프롭 임포트에 디스패치해서 연결한 함수를 추가합니다.
`const { isPlaying, elapsedTime, timerDuration, startTimer, restartTimer } = this.props;`
5. 이제 남은건 버튼의 onPress 에 프롭으로 연결한 startTimer, restartTimer 함수를 연결하면 됩니다.

### 13강 interval
이제 elapsedTime 에 시간을 추가합니다. 그 전에 `interval` 을 알아봅니다.
---
`interval` : 초가 지나면 실행되는 함수입니다. setInterval(함수, 밀리초) 형태입니다. 변수에 setInterval 함수를 할당한 후, `clearInterval(변수)`로 멈출 수 있습니다.
---
### 14강 초재기
인터벌을 만드려면 카운터가 첫째로 시작하는 순간을 찾아야합니다. 그리고 인터벌을 멈추기 위해 카운터가 멈추는 순간을 찾아야합니다. 일단 컴포넌트의 라이프사이클을 파악해야 합니다.
1. presenter.js 에 componentWillReceiveProps 를 추가합니다. 새로운 프롭을 얻을 때마다 불러오는 함수입니다. 우선 현재 프롭과 새 프롭을 확인할 console.log 를 넣어봅니다.
```javascript
componentWillReceiveProps(nextProps) {
  const currentProps = this.props;
  console.log(`current ${currentProps.isPlaying} / next ${nextProps.isPlaying}`)
}
/*불러오면 현 isPlaying 은 false, 새 isPlaying 도 false 입니다.
재생 버튼을 누르면 현 isPlaying 은 false, 새 isPlaying 은 true 로 바뀝니다.
*/
```
2. 현 상태와 새로운 상태를 감지해서 사용합니다. isPlaying 의 참, 거짓 여부를 활용합니다.
```javascript
componentWillReceiveProps(nextProps) {
  const currentProps = this.props;
  if(!currentProps.isPlaying && nextProps.isPlaying) {
    const timerInterval = setInterval(() => {
      currentProps.addSecond()
    }, 1000);
    //밑의 clearInterval 에 접근하기 위해 상태를 만듬
    this.setState({ timerInterval });
  } else if(currentProps.isPlaying && !nextProps.isPlaying) {
    clearInterval(this.state.timerInterval);
  }
}
```

### 15강
1. timerDuration(최대 시간) 을 가져와 elapsedTime(경과시간) 을 빼야 합니다. 우선은 presenter.js 의 시간 부분을 `{timerDuration - elapsedTime}`으로 맞춥니다. 그러면 밀리초가 화면에 나옵니다.
2. 초를 분으로 바꾸는 함수를 작업합니다. Math 함수를 사용했습니다.
```javascript
function formatTime(time) {
  let minutes = Math.floor(time / 60);
  time -= minutes * 60;
  let seconds = parseInt(time % 60, 10);
  return `${minutes < 10 ? `0${minutes}` : minutes}:${seconds < 10
     ? `0${seconds}` : seconds}`
}
```
