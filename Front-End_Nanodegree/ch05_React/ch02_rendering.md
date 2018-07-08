# Rendering UI with React (Udacity)
## JSX 로 element 만들기
### 살펴보기
- 리액트의 `.createElement()` 를 알아봅니다. 이것은 리액트가 새로운 element 를 만들 떄 사용하는 탑 레벨의 API 입니다.
  + 첫 인수는 사용할 element 의 태그 이름을 쓰거나, 혹은 사용할 div 만을 적습니다.
  + 두 번째 인수는 나중에 설명합니다.
  + 세 번째 인수는 첫 인수 안에 작성할 텍스트입니다.
```javascript
React.createElement( /* type */, /* props */, /* content */ );
```
- 간단한 리액트와 리액트 DOM 을 만들어봅니다.
```javascript
import React from 'react'
import ReactDOM from 'react-dom'

const element = React.createElement('div', null, 'hello world')
/*리액트 DOM 로 만든 element 를 DOM node 에 렌더합니다.
(예제는 이미 있는 페이지의 루트 element 를 사용합니다.)*/
ReactDOM.render(
  element,
  document.getElementById('root')
)
```
  + 변수 element 는 다른 프로퍼티를 가진 순수한 자바스크립트 객체입니다.
  + 리액트의 `.createElement()` 메소드는 element 에 대한 설명을 가져오고 일반 자바스크립트 객체로 return 합니다.
- 리액트 DOM 은 리액트 라이브러리를 사용하는 한 방법입니다.
  + 리액트에서 무엇을 렌더링할지 결정하는 과정은 실제 렌더링과 완전히 분리됩니다.
  + 분리(decoupling)를 통해 네이티브 장치나 심지어 VR 환경의 서버에 물건을 렌더링할 수 있게 해줍니다.

### element 를 DOM 에 렌더링하기
- 위에서 사용한 루트 element 는 어디서 왔을까요?
  + 리액트로 빌드된 앱은 일반적으로 단일 루트 DOM node 를 가집니다. 예를 들어 HTML 파일에는 아래의 div 를 가질 것입니다.
```HTML
<div id='root'></div>
```
- 이러한 DOM node 를 `getElementById()` 로 전달하면 리액트는 해당 내용 전체를 제어하게 됩니다.
  + 특정 `<div>` 가 리액트 앱의 hook 역할을 한다는 뜻이기도 합니다. 해당 영역이 바로 리액트가 UI 를 대신해 맡은 영역입니다.
