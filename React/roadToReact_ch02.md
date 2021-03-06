# 리액트 도움닫기 2장
## 컴포넌트 내부 상태 관리
- '컴포넌트 내부 상태' : 컴포넌트에 저장된 상태를 뜻합니다. 로컬 상태라고도 합니다. 
  + 컴포넌트 상태는 프로퍼티로 그 값을 수정, 삭제, 그리고 저장합니다.
  + 클래스 생성자로 컴포넌트를 만들고 내부 컴포넌트 상태를 초기화하며, 생성자는 컴포넌트가 초기화될 때 한 번만 호출됩니다.
```javascript
//App 컴포넌트는 Component 의 하위클래스이기에 extend Component 로 컴포넌트를 확장시킵니다.
class App extends Component {
  constructor(props) {
    super(props);
  }
}
```
- 클래스 컴포넌트에 생성자가 있으면 반드시 'super();' 를 호출해야 합니다.
- 생성자에서는 'super(props);' 를 호출할 수 있습니다.
  + 'this.props' 를 사용하기 위해선 생성자에서 'this.props' 를 설정해야 합니다.
```javascript
const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0
  }
];

class App extends Component {
  constructor(props) {
    //클래스 컴포넌트에 생성자가 있어서 super() 를 호출했습니다.
    super(props);
    //위에서 list 배열을 만들고 이를 컴포넌트 초기 state 로 설정합니다. 
    //state 는 this 객체를 사용해서 클래스에 바인딩됩니다.
    //바인딩했기 때문에 전체 컴포넌트 내의 로컬 상태에 접근이 가능해집니다. 
    //그래서 render() 메소드에서 state를 사용할 수 있습니다.
    this.state = {
     list: list,
    };
  }
  
  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
```
- 컴포넌트 내부 상태에서 배열 내의 아이템을 추가, 변경, 제거를 할 수 있습니다. (상태를 바꿀 때마다 'render()' 메소드가 재실행됩니다.)
  + 내부 상태를 직접 변경해서는 안됩니다. 'setState()' 메소드를 사용해서 상태를 변경해야 합니다.

## ES6 객체 초기자
- ES6 에서의 객체 정의는 다릅니다.
```javascript
//과거의 객체 정의
var name = 'jin';
var user = {
  name: name,
  email: email
}
//ES6 에서의 객체 정의. 객체 프로퍼티 이름과 변수의 이름이 같다면 하나만 적습니다.
const name = 'jin';
const user = {
  name,
  email
}
//리액트에서도 동일합니다.
this.state = {
  list,
}
```
- 객체 메소드도 축약이 가능합니다.
```javascript
//과거의 메소드
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  }
}
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  }
}
```
- ES6 에서는 계산된 프로퍼티 이름을 사용할 수 있습니다. 이것은 객체에서 동적인 방식으로 값을 할당할 때 사용됩니다.
//과거
var user = {
  name: 'jin'
};

// ES6
const key = 'name'; //프로퍼티 이름을 key 로 지정합니다.
const user = {
  [key]: 'jin'
};

## 단방향 데이터 흐름
- App 컴포넌트의 내부 상태는 정적인 상태지만 state 를 조절해서 동적인 컴포넌트로 만들 수 있습니다.
```javascript
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      list
    };
    //onDismiss() 함수입니다. 클래스에 바인딩되어 클래스 메소드입니다.
      //그래서 this.onDismiss() 로 접근합니다. 
    this.onDismiss = this.onDismiss.bind(this);
  }
  //클래스 메소드를 정의합니다.
  onDismiss(id) {
  /*list에서 ID값과 매칭되는 아이템을 제거, 업데이트된 list 를 state 에 저장합니다.
    그 다음 render() 메소드가 재실행되고 업데이트된 list 가 브라우저에 표시됩니다.
  */
    //filter() 메소드로 배열의 element 를 삭제합니다.
    const updatedList = this.state.list.filter(function isNotId(item) {
      //서로의 값이 일치하면 값은 그대로 유지, 일치하지 않으면 배열에서 필터링됩니다.
      //그러나 return 되는 배열은 불변 데이터 구조입니다.
      return item.objectID !== id;
    });
    //setState() 클래스 메소드로 this.state.list 를 업데이트합니다.
    this.setState({ list: updatedList });
  }
  /* 화살표 함수로 더 간단히 작성할 수 있습니다.
  onDismiss(id) {
    const isNotId = item => item.objectID !== id;
    const updatedList = this.state.list.filter(isNotId);
    this.setState({ list: updatedList });
  }
  */
  
  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
            //item 의 objectID 프로퍼티로 삭제할 item 을 정합니다.
              //참고로 아래처럼 한 element 를 들여쓰는 것은 내용을 쉽게 알기 위해서입니다.
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
```
- 'onClick()' 을 사용해 뷰에서 액션을 트리거하면, 함수 또는 클래스 메소드가 컴포넌트 상태를 수정합니다. 그 후 'render()' 메소드가 재실행되어 최종적인 뷰가 업데이트됩니다.

## 클래스 메소드 바인딩
- 클래스 메소드에서 this 에 접근할 수 있으려면 클래스 메소드를 this 에 바인딩해야 합니다.
  + 'render()' 안에서도 바인딩 할 수 있지만, 'render()' 메소드가 실행될 때마다 바인딩하기 때문에 컴포넌트가 업데이트 될 때마다 실행되어 성능에 영향을 줍니다.
  + 그래서 생성자에서 클래스 메소드를 바인딩해서 컴포넌트가 인스턴스화 될 때에 처음 한 번만 바인딩하는 편이 좋습니다.
```javascript
class bindingComponent extends Component {
  cunstructor() {
    super();
    //클래스 바인딩을 사용합니다.
    this.onClickMe = this.onClickMe.bind(this);
  }
  
  onClickMe() {
    console.log(this);
  }
  
  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
       >
        Click Me
      </button>
    );
  }
}
```
- 생성자에서 클래스 메소드의 할 일을 정의할 수 있습니다. 다만 이 방법은 생성자에게 혼란을 줄 수 있어서 권장하지 않습니다.
 + 클래스 메소드의 로직은 생성자 외부에서 정의하는 것이 좋습니다.
 + 화살표 함수를 사용하면 클래스 메소드를 생성자 내부에 바인딩하지 않고 자동으로 바인딩해줍니다.
```javascript
//생성자 외부에서 클래스 메소드 정의
class bindingsComponent extends Component {
  constructor() {
    super();
    
    this.doSomthing = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }
  doSomething() {  }
  doSomethingElse() {  }
}

//화살표 함수
class bindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }
  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
```
- 화살표 함수를 추천하기는 하지만 공식 문서는 생성자 내부에 클래스 메소드를 바인딩할 것을 제안합니다.

## 제어되는 컴포넌트
- '<input>, <textarea>, <select>' 같은 폼 element 는 HTML 로 자신의 상태를 유지합니다. 이들은 외부에서 그 값이 변경되면 내부 값도 수정됩니다.
 + 이러한 컴포넌트를 리액트에서는 *제어되지 않는 컴포넌트* 라고 부릅니다.
 + 리액트는 제어되지 않는 컴포넌트를 *제어되는 컴포넌트* 로 만들어야 합니다.


