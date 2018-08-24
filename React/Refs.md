# Refs 와 DOM
## neigborhood-map 에서 사용한 refs
부모 App 컴포넌트에서 자식 Map 컴포넌트의 함수 initMap 을 사용해야 하는데 연결을 하기 위해서는 refs 를 사용해야만 했습니다.
1. 우선 생성자를 작성하고 밑에 `React.createRef()` 를 작성합니다.
2. render() 에서 리턴할 때 Map 컴포넌트에 ref 를 작성합니다.
3. 그러면 Map 컴포넌트의 함수를 App 컴포넌트에서 사용할 수 있게 됩니다.
```javascript
class App extends Component {
  constructor(props) {
    super(props);
    this.child = React.createRef();
  }
  async changeFilter(value) {
    console.log(value)
    await this.setState({ markers: [], filter: value });
    console.log('filter ' + this.state.filter)
    this.initMap();
  }
  render() {
    return (
      <Map
        ref={this.child}
        storeMap={this.storeMap.bind(this)}
        markers={this.state.markers}
        filter={this.state.filter}
        inputMarkers={this.inputMarkers.bind(this)}
        getDetails={this.getDetails}
        createInfoWindow={this.createInfoWindow}
      />
    )
  }
}
```

## 정의
 [리액트 공식 페이지 링크](https://reactjs.org/docs/refs-and-the-dom.html)
 `Refs` 는 render 메소드에서 생성된 DOM 노드 또는 React 요소(element)에 액세스하는 방법을 제공합니다.
 일반적인 React 데이터 흐름에서는 부모 컴포넌트가 자식과 상호 작용할 수있는 유일한 방법은 소품입니다. 하위를 수정하려면 새 하위로 다시 렌더링하십시오.
 그러나 일반적인 데이터 흐름 외부의 자식을 수정해야하는 경우가 있습니다. 수정할 자식 요소(element)는 React 컴포넌트의 인스턴스이거나 DOM 요소 일 수 있습니다. 두 경우 모두 React가 이스케이프 해치를 제공합니다.

### 사용해야 하는 경우
1. 포커스, 텍스트 선택이나 미디어 재생을 관리할 경우
2. 명령적인 애니메이션을 트리거할 경우
3. 제 3자 DOM 라이브러리와 통합할 경우
선언적으로 수행할 수 있는 모든 것에는 `ref`를 사용하지 마세요.
(예를 들어 dialog 컴포넌트에 `open()` 및 `close()` 메소드를 표시하는 대신 `isOpen` prop 을 전달하는 것입니다.)

### 과다하게 사용하지 말 것
일을 발생시킬 때마다 refs 를 사용하기 전에 컴포넌트 계층에서 state 를 어디서 소유할 것인지를 고민하셔야 합니다.
가끔 state 를 기존의 위치보다 더 상위 계층 컴포넌트에 있어야 하는 경우도 있기 때문입니다.

### ref 만들기
`ref`는 `React.createRef()` 을 사용해서 만들고 `ref` 속성(attribute)를 통해서 React element 에 첨부됩니다.
Refs 는 일반적으로 컴포넌트가 컴포넌트를 통해서 참조할 수 있도록 구성될 경우에 인스턴스 등록 정보로 지정합니다.
```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

### Refs 에 접근하기
ref 를 `render`의 요소(element)에 전달하면, ref 에 대한 현재 속성(attribute)에서 노드에 대한 참조가 액세스 가능해집니다.
```javascript
const node = this.myRef.current;
```
ref 의 값은 노드의 유형에 따라 다릅니다.
- HTML 요소(element)에서 `ref` 속성(attribute)를 사용하면 `React.createRef()`을 사용해서 생성자에서 만든 `ref` 는 기본 DOM 요소(element)를 `current` 프로퍼티로 받습니다.
- `ref` 속성을 사용자가 정의한 클래스 컴포넌트에 사용하면 `ref` 객체는 마운트된 컴포넌트의 인스턴스 `current`를 받습니다.
- 기능(Functional) 컴포넌트에서는 인스턴스가 없기 때문에 ref 속성을 사용할 수 없습니다.

1. Ref 를 DOM 요소에 추가하기
```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```
리액트는 컴포넌트가 마운트될 때 DOM 요소로 `current` 프로퍼티를 할당하고, 마운트를 해제할 때 다시 `null` 에 할당합니다.
`ref` 업데이트는 `componentDidMount` 또는 `componentDidUpdate` 라이프 사이클 이벤트 전에 발생합니다.

2. Ref 를 클래스 컴포넌트에 추가하기
위의 `CustomTextInput` 을 래핑하여 마운트 직후에 클릭한 상태로 시뮬레이션하려는 경우 ref 를 사용하여 사용자 정의 입력에 액세스하고 해당 `focusTextInput` 메소드를 수동으로 호출할 수 있습니다.
```javascript
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />
    );
  }
}
```
참고로 이것은 오직 `CustomTextInput`가 클래스로 선언될 때만 사용이 가능합니다.

3. Refs 와 기능(Functional) 컴포넌트
기능 컴포넌트에는 인스턴스가 없기 때문에 ref 속성을 사용할 수 없습니다.
```javascript
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will *not* work!
    return (
      <MyFunctionalComponent ref={this.textInput} />
    );
  }
}
```
라이프 사이클 메소드나 state 가 필요할 때와 마찬가지로, 컴포넌트를 참조해야하는 경우 컴포넌트를 클래스로 변환해야합니다.
그러나 DOM 요소 또는 클래스 컴포넌트를 참조하는 한 기능(Functional) 컴포넌트 내에서 ref 속성을 사용할 수 있습니다.
```javascript
function CustomTextInput(props) {
  // textInput must be declared here so the ref can refer to it
  let textInput = React.createRef();

  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```
### DOM Refs 를 부모 컴포넌트에 표시하기
드물지만 부모 컴포넌트에서 하위 DOM 노드에 접근할 수 있습니다. 이것은 일반적으로 컴포넌트 캡슐화를 깨뜨려서 권장하진 않지만 때때로 포커스를 트리거하거나 자식 DOM 노드의 크기, 위치를 측정하는데 도움을 줄 수 있습니다.
하위 컴포넌트에 ref 를 추가할 수 있지만, DOM 노드가 아닌 컴포넌트 인스턴스만 가져오기 때문에 이상적인 해결책은 아닙니다. 그리고 기능(Functional) 컴포넌트에서는 작동하지 않습니다.
리액트 16.3 이상 버젼에서는 이럴 때 [ref 전달(forwarding)](https://reactjs.org/docs/forwarding-refs.html)을 사용하는게 좋습니다. ref 전달은 컴포넌트가 하위 컴포넌트의 ref 를 자체적으로 노출하도록 선택할 수 있게 만듭니다. ref 전달 문서에서 자식 DOM 노드를 부모 컴포넌트에 노출하는 방법에 대한 예제를 볼 수 있습니다.
만약 리액트 16.2 이하를 사용하거나 ref 전달보다 더 많은 유용성을 원한다면 다른 방식을 써야 합니다. 다른 이름의 prop 으로 ref 를 명시적으로 전달합니다.
가능하다면 DOM 노드를 노출하지 말 것을 권장합니다만 이 벙법을 써야 한다면 하위 컴포넌트에 일부 코드를 추가해야 합니다. 하위 컴포넌트 구현을 완전히 제어할 수 없다면 finalDOMNode() 를 써야 하는데 권장하지 않습니다.

### 콜백 Refs
React는 또한 "콜백 참조(refs)"라는, refs 를 설정하는 또 다른 방법을 지원합니다. 이 방법은 refs 를 설정 및 해제 할 때보다 미세한 제어를 제공합니다.

`createRef()` 에 의해 생성된 `ref` 속성을 전달하는 대신 함수를 전달합니다. 이 함수는 React 컴포넌트 인스턴스 또는 HTML DOM 요소를 인수로 받습니다.(이 요소는 다른 곳에 저장하고 액세스 할 수 있습니다.)

아래 예제는 일반적인 패턴을 구현합니다 : ref 콜백을 사용하여 인스턴스 속성에 DOM 노드에 대한 참조를 저장합니다.
```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // Focus the text input using the raw DOM API
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // autofocus the input on mount
    this.focusTextInput();
  }

  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in an instance field (for example, this.textInput).
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```
리액트는 컴포넌트가 마운트될 때 DOM 요소로 ref 콜백을 호출하고, 마우트가 해제될 때 null 로 호출합니다. Refs 는 `componentDidMount` 또는 `componentDidUpdate` 가 시작되기 전에 최신 상태로 보장됩니다.
`React.createRef()`로 생성한 객체로 콜백 참조(refs)를 할 수 있는 것처럼, 컴포넌트 간에도 콜백 참조를 전달할 수 있습니다.
```javascript
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```
위의 예제에서 `Parent` 는 ref 콜백을 `inputRef` prop 으로 `CustomTextInput`에 전달하고 `CustomTextInput` 은 특수한 `ref` 속성과 동일한 함수를 `<input>`에 전달합니다. 결과적으로 `Parent` 의 `this.inputElement` 는 `CustomTextInput` 의 `<input>` 요소에 해당하는 DOM 노드로 설정됩니다.

### Legacy API: 문자열 참조(Refs)
리액트와 함께 작업한다면, `ref` 속성이 `"textInput"`과 같은 문자열 인 이전 API에 익숙해질 수 있으며 DOM 노드를 `this.refs.textInput` 으로 접근합니다. 문자열 참조에는 몇 가지 문제가 있고 레거시로 간주되며 이후 릴리스 중 하나에서 제거 될 가능성이 있기 때문에 반대 의견을 제시합니다.
(만약 this.refs.textInput 를 사용한다면 콜백 패턴이나 createRef API 로 바꾸길 추전합니다.)

### 콜백 참조에 대한 주의사항

만약 `ref` 콜백을 인라인 함수로 정의한 경우 업데이트 도중 두 번 호출됩니다. 먼저 `null` 이 반환되고 DOM 요소가 다시 호출됩니다. 이것은 함수의 새 인스턴스가 각 렌더링과 함께 만들어지기 때문에 React 가 이전 ref 를 지우고 새 `ref` 를 설정해야하기 때문입니다. ref 콜백을 클래스의 바인딩된 메소드로 정의하면 이 문제를 피할 수 있지만 대부분의 경우에는 크게 중요하지는 않습니다.
