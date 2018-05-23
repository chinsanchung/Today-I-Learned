# 리액트 도움닫기 3장 : 외부 API 사용하기
## 생명주기 메소드
- ES6 클래스 컴포넌트 메소드인 `constructor()` 와 `render()` 메소드는 생명주기 메소드 입니다. 이것은 ES6 클래스 컴포넌트에서만 사용가능합니다.
- 컴포넌트 인스턴스가 만들어져 DOM 에 삽입될 때, `constructor()` 생성자 메소드가 호출됩니다. 이 과정을 컴포넌트가 마운트(탑재)되는 `마운트 프로세스` 라고 합니다.
- `render()` 메소드는 마운트 프로세스 중에 호출되며, 컴포넌트 state 와 props 가 변경되어 컴포넌트가 업데이트 될 때마다 호출됩니다.
- 마운트 프로세스 의 순서
  + `constructor()` -> `componentWillMount()` -> `render()` -> `componentDidMount()`
- state 나 props 가 바뀌면 '업데이트 프로세스' 가 시작됩니다. 5개의 생명주기 메소드가 호출됩니다.
  + `componentWillReceiveProps()` -> `shouldComponentUpdate()` -> `componentWillUpdate()` -> `render()` -> `componentDidUpdate()`
- 컴포넌트 마운트가 해제되기 전에 `componentWillUnmount()` 메소드가 호출됩니다.
- 생명주기 메소드의 종류, 사용 방법
  + `constructor(props)` : 컴포넌트 초기화시 호출됩니다. 초기 컴포넌트 상태 및 클래스 메소드를 정의합니다.
  + `componentWillMount()` : `render()` 메소드 전에 호출됩니다. 컴포넌트의 두 번째 rendering 을 일으키지 않아 메소드에서 컴포넌트 상태를 설정가능하지만, `constructor()` 메소드에서 초기 상태값을 return 하는게 좋습니다.
  + `render()` : props 와 state 를 읽고 element 를 return 합니다. 컴포넌트 출력(컴포넌트 인스턴스)을 return 하기 때문에 반드시 사용해야 합니다.
  + `componentDidMount()` : 컴포넌트가 마운트될 때 한 번만 호출됩니다. 일반적으로 이 메소드에서 비동기 API 를 사용합니다. 응답받은 외부 데이터는 내부 컴포넌트 상태에 저장되어 컴포넌트가 업데이트되면 `render()` 메소드가 실행됩니다.
  + `componentWillReceiveProps(nextProps)` : 업데이트 생명주기에서 호출됩니다. 다음 props 는 nextProps 와 이전 props 인 `this.props` 의 차이를 비교합니다. nextProps 를 컴포넌트 state 로 사용할 수 있습니다.
  + `shouldComponentUpdate(nextProps, nextState)` props 또는 state 가 바뀌어 컴포넌트가 업데이트될 때 호출됩니다. 주로 고도화된 애플리케이션의 성능 최적화를 고려할 때 사용합니다. 변환되는 불리언 값에 따라 컴포넌트와 모든 자식이 업데이트 주기 동안 rendering 되거나, 반대로 rendering 이 안되도록 처리할 수 있습니다. 또한 문제있는 특정 컴포넌트의 생명주기 rendering 을 막을 수 있습니다.
  + `componentWillUpdate(nextProps, nextState)` : `render()` 메소드 전에 즉시 호출됩니다. `nextProps` 는 다음 props 를, `nextState` 는 다음 state 를 조회합니다. `render()` 메소드가 실행되기 전에 상태를 업데이트할 마지막 기회이기도 합니다. `render()` 메소드가 실행된 이후에는 `setState()` 메소드를 사용할 수 없습니다. `nextProps` 를 state 값으로 사용하려면 `componentWillReceiveProps()` 메소드를 사용합니다.
  + `componentDidUpdate(prevProps, prevState)` : `render()` 메소드 후에 즉시 호출됩니다. DOM 조작이나 추가 비동기 요청을 수행합니다.
  + `componentWillUnmount()` : 컴포넌트 해제 전에 호출됩니다. 컴포넌트를 초기화합니다.

## 검색 결과 데이터 가져오기
- 해커 뉴스 API 로 외부 데이터를 가져오기 위해 `componentDidMount()` 메소드 안에 있는 자바스크립트 네이티브 API `fetch()` 메소드를 사용합니다.
  + 참고로 ES6 에서는 템플릿 리터럴 을 도입했습니다. 여러 줄로 이뤄진 문자열을 쉽게 표현하도록 돕습니다. URL 을 구성해서 API 엔드 포인트에 연결도 가능합니다.
```javascript
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url); //결과 : https://hn.algolia.com/api/v1/search?query=redux
```
- 검색 결과 데이터를 가져오는 코드들입니다.
```javascript
import React, { Component } from 'react';
import './App.css';

const DEFAULT_QUERY = 'redux';
//요청 URL 을 상수(constants)와 기본 매개변수로 분절해서 구분합니다.
const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      result: null,
      //searchTerm 은 첫번째 API 를 요청할 때와 Search 컴포넌트에서 입력 필드 기본값을 표시할 때 사용합니다.
      searchTerm: DEFAULT_QUERY
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  setSearchTopStories(result) {
    this.setState({ result });
  }
//컴포넌트가 마운트된 후 데이터를 가져오기 위해 componentDidMount() 메소드를 사용합니다.
  componentDidMount() {
    const { searchTerm } = this.state;
//fetch API 를 사용합니다. 구성된 URL 은 fetch API 의 인자로 전달됩니다.
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
    //응답 데이터는 반드시 JSON 데이터 구조로 변환시켜야 합니다.
      .then(response => response.json())
    //이 데이터를 컴포넌트 내부 상태인 result 에 저장합니다.
      .then(result => this.setSearchTopStories(result))
    //오류가 생기면 catch() 를 실행합니다.
      .catch(error => error);
  }
//버튼을 클릭하면 result 객체에서 해당 item 이 제거된 리스트가 업데이트되고, 다른 프로퍼티는 그대로 유지됩니다.
  onDismiss(id) {
    const isNotId = item => item.objectID !== id;
/* filter 를 쓰는 이유는 리액트가 불변 데이터 구조 원칙을 따라 객체나 상태를 바로 변경할 수 잆어서입니다. 
동일한 객체롤 새로 만들어 데이터 형태를 유지합니다. */
    const updatedHits = this.state.result.hits.filter(isNotId);
    this.setState({
      //ES6 전개 연산자인 ... 을 활용합니다. (아래의 형태가 객체 스프레드 연산자입니다. 이것은 Object.assign() 메소드를 대체합니다.)
      result: { ...this.state.result, hits: updatedHits }
    });
  }

  render() {
    const { searchTerm, result } = this.state;
    //축약 표기법입니다. (result === null) = (!result), (list.length === 0) = (!list.length), (string !== '') = (string) 입니다.
    if (!result) { return null; }

    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
        </div>
/*조건부 rendering 에 result 에 의존하는 Table 컴포넌트를 포함합니다. 
조건이 참이면 && 논리 연산지의 뒷부분이 출력됩니다. 거짓이면 리액트는 표현식을 무시하고 건너뜁니다. */
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
```
  + 컴포넌트는 생성자에서 초기화된 후 rendering 됩니다. 현재 내부 상태 result 는 null 이므로 if 문에 따라 아무것도 표시하지 않습니다.
  + 그 후 componentDidMount() 메소드가 실행돼 해커뉴스 API 요청에 따라 비동기로 데이터를 가져옵니다. 그리고 나서 업데이트 생명주기가 시작됩니다.
  + render() 메소드가 다시 실행되는데 이번에는 result 값이 있어서 리스트가 표시됩니다. 그리고 App 컴포넌트와 Table 컴포넌트가 rendering 됩니다.
- `객체 스프레드 연산자` : 배열 스프레드 연산자도 있는데 같은 형식으로 쓰면 됩니다.
```javascript
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
//userNames 와 age 를 합친 새로운 객체입니다.
const user = { ...userNames, age }; //결과 : { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
```
- `조건부 rendering` : 하나 또는 여러 컴포넌트와 element 의 rendering 여부를 결정합니다. (예 : if-else 문)
  + App 컴포넌트는 초기에 아무것도 rendering 되지 않다가 result 값이 업데이트 되면 다시 rendering 하게 됩니다.
