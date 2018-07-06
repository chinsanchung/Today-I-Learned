# Why React? (Udacity)
## composition
- composition : 간단한 함수를 결합해 더 복잡한 함수를 만드는 걸 composition 이라고 합니다.
```javascript
//기존의 형식
function getProfilePic (username) {
  return 'https://github.com/' + username + '.png?isze=200';
}
function getProfileLink (username) {
  return 'https://github.com/' + username;
}
function getProfileData (username) {
  return {
    pic: getProfilePic(username),
    link: getProfileLink(username)
  };
}
getProfileData('jin');
```
```javascript
//composition
function ProfilePic (username) {
  return ( <img alt={username}
    src={'https://github.com' + username + '.png?size=200'} />)
}
function ProfileLink (username) {
  return <a href={'https://github.com/' + username}>{username}</a>
}
function Profile (username) {
  return (
    <div className='profile'>
      <ProfilePic username='username' />
      <ProfileLink username='username' />
    </div>
  )
}
```
### composition 의 장점
- composition 은 단순한 함수들으로 만든다는 점이 중요합니다.
- `getProfileLink()` 함수는 매우 단순합니다. 오직 한 줄만 있습니다. `getProfilePic()` 도 마찬가지입니다.
- 그리고 두 함수를 합쳐 새로운 함수 `getProfileData` 에 넣었습니다.
  + `getProfileData` 를 composition 없이 바로 데이터를 전달할 수 있긴 합니다.
```javascript
function getProfileData (username) {
  return {
    pic: 'https://github.com/' + username + '.png?size=200',
    link: 'https://github.com/' + username
  }
}
```
  + 위의 방식은 기술적으로 잘못되진 않았지만 좋은 형태는 아닙니다. 좋은 자바스크립트 함수는 Do One Thing 원칙을 따라야 합니다.
- DOT(Do One Thing) 에 맞춘다면 각 함수는 하나만 해야 합니다.
  + `getProfileLink` : 문자열(사용자의 깃허브 프로필 링크)을 구성
  + `getProfilePic` : 문자열(사용자의 깃허프 프로필 사진)을 구성
  + `getProfileData` : 새로운 객체를 return

### React 와 composition
- React 는 composition 의 힘을 많이 사용합니다.
- React 는 컴포넌트를 사용해서 UI 를 구성합니다. 예를 들어 pseudo 코드로 세 컴포넌트를 보겠습니다.
```javascript
<Page />
<Article />
<Sidebar />
```
  + 이것을 합치고 새로운 복잡한 컴포넌트를 만들어봅니다.
```javascript
<Page>
  <Article />
  <Sidebar />
</Page>
```
  + `Page` 컴포넌트는 이제 'Article', 'Sidebar' 컴포넌트를 포함합니다. 앞서 봤던 `getProfileData` 가 `getProfileLink`, `getProfilePic` 을 가진 것과 비슷합니다.
- 단순한 함수는 one thing (DOT)을 수행하는 단일 빌딩 블록입니다. 이 간단한 함수를 결합하여 더 복잡한 함수를 만드는 게 composition 입니다.

## Declarative Code
- 먼저 상태와 마크업을 선언(declare) 하고, 그 다음 리액트가 명령 작업(imperative work)으로 앱과 동기화된 DOM 을 유지합니다. 리액트는 선언적(declarative) 입니다.
### imperative code
- 많은 자바스크립트는 *imperative code* 입니다. imperative 란 명령을 표현한다는 뜻입니다.
- 자바스크립트 코드를 imperatively 하게(명령형으로) 작성한다면, 자바스크립트에서 무엇을 수행하고 어떻게 해야 할지를 정확하게 알려줍니다.
  + 자바스크립트 명령을 정확히 수행해야하는 단계라고 생각하면 됩니다.
```javascript
const people = ['Amanda', 'Geoff', 'Michael', 'Richard', 'Ryan', 'Tyler']
const excitedPeople = []

for (let i = 0; i < people.length; i++) {
  excitedPeople[i] = people[i] + '!'
}
```
  + `people` 배열의 각 항목을 반복하면서 이름에 ! 를 넣어 `excitedPeople` 배열에 새로운 문자열을 저장합니다.
- 위의 코드가 바로 imperative code 입니다. 자바스크립트를 각 단계마다 명령을 해야 합니다.
  + 반복자(iterator) 의 초기 값을 설정합니다. (let i = 0)
  + for 반복문을 멈출 때를 정합니다. (i < people.length)
  + 현재 위치(position)의 person 을 가져와 느낌표를 더합니다. (people[i] + '!')
  + 다른 배열의 i 번째 위치(position) 에 데이터를 저장합니다. (excitedPeople[i])
  + i 변수를 1씩 증가시킵니다. (i++)
- 예를 들어 추우면 에어컨 온도를 올리고, 더우면 낮추는 것처럼 자바스크립트의 imperative 상황도 조절할 수 있습니다.
  + 다만 이것은 수동으로 여러 단계를 수행하는 방식이니 이상적이진 않습니다. 이것을 개선해봅니다.
### Declarative code
- imperative code (명령형 코드) 와 정반대로 *declarative code* (선언적 코드) 가 있습니다.
  + 선언적 코드에서는 최종 결과를 얻는 모든 단계를 코드로 작성하지 않습니다. 대신 원하는 것- 선언하고 자바스크립트로 처리합니다.
- 위의 for 문을 리팩토링해봅니다.
  + 명령형 코드로 모든 단계를 수행해 결과를 얻었는데 실제로 원하는 최종 단계는 무엇일까요. 출발점은 단순한 이름 배열이었습니다.
```javascript
const people = ['Amanda', 'Geoff', 'Michael', 'Richard', 'Ryan', 'Tyler']
```
  + 원하는 결과는 같은 이름이지만 ! 가 붙은 배열이었습니다.
```javascript
["Amanda!", "Geoff!", "Michael!", "Richard!", "Ryan!", "Tyler!"]
```
  + 이것을 얻기 위해 자바스크립트의 `.map()` 함수를 사용해 원하는 것을 선언합니다.
```javascript
const excitedPeople = people.map(name => name + '!')
```
- 위의 코드에서 수행하지 않은 작업들입니다. 이것들은 `.map()` 배열 메소드로 처리합니다.
  + 반복자 객체를 만들기
  + 멈춰야 할 때를 말해주기
  + 반복자로 `people` 배열의 특정 항목에 접근하기
  + `excitedPeople` 배열에 새로운 문자열로 각각 저장하기
### React is Declarative
```
<button onClick={activateTeleporter}>Activate Teleporter</button>
```
- 위 코드는 타당한 리액트 코드입니다. 그리고 쉽게 이해할 수 있습니다.
  + `onClick` 속성(attribute)이 버튼에 있다는 점에 주목합니다.
  + `.addEventListener()` 를 사용해 모든 절차를 밟아가며 이벤트 핸들링을 하지 않았습니다. 대신 버튼을 클릭했을 떄 실행할 `activateTeleporter` 함수가 필요하다고 선언만 했을 뿐입니다.
- 정리
  + 명령형 코드는 자바스크립트가 각 단계를 수행할 방법을 명령합니다. 선언적 코드를 사용해 자바스크립트에서 수행할 작업을 지정하고, 해당 단계를 자바스크립트로 수행합니다.
  + 리액트는 원하는 코드를 작성해 주기에 선언적입니다. 리액트는 선언했던 코드를 가지고 원하는 결과를 얻도록 모든 자바스크립트 / DOM 단계를 수행합니다.

## 단방향 데이터 흐름(Unidirectional Data Flow)
### 다른 프레임워크의 데이터 바인딩
- 앵귤러나 앰버같은 프레임워크는 양방향(two-way) 데이터 바인딩을 사용합니다.
  + 데이터는 업데이트되는 위치와 상관없이 앱 전체에서 동기화 상태로 유지됩니다.
  + 모델이 데이터를 바꾸면 데이터는 뷰에서 업데이트됩니다. 또는 사용자가 뷰에서 데이터를 바꾼다면 데이터는 모델에서 업데이트됩니다.
- 양방향 데이터 바인딩은 강력해보이지만, 애플리케이션을 추론하기 어려워지고 또 데이터가 실제로 어디서 업데이트되는지를 알기도 어렵습니다.
### 리액트의 데이터 흐름
- 리액트에서는 데이터는 부모 컴퍼넌트에서 자식 컴포넌트 방향으로 흘러갑니다. 데이터 업데이트는 부모가 실제로 변경을 수행하는 부모 컴포넌트로 전송됩니다.
- 데이터는 부모 컴포넌트에 살고 있고 자식 컴포넌트로 전송됩니다.
  + 데이터가 부모 컴포넌트에 있다 해도 부모와 자식 컴포넌트 모두 데이터를 쓸 수 있습니다.
  + 하지만 데이터를 업데이트해야한다면, 오직 부모 컴포넌트만 업데이트를 수행할 수 있습니다.
  + 만약 자식 컴포넌트가 데이터를 바꿔야한다면, 업데이트된 데이터는 데이터를 실제로 바꿔줄 부모 컴포넌트로 보내야 합니다.
  + 한번 부모 컴포넌트에서 바뀌면, 자식 컴포넌트는 (방금 업데이트된)데이터를 받을 것입니다.
- 단방향으로 데이터 흐름을 유지하고, 데이터가 수정되는 곳을 한 장소로 둔다면 응용 프로그램의 작동 방식을 훨씬 쉽게 이해할 수 있을 것입니다.
```javascript
<FlightPlanner>
  <DatePicker />
  <DestinationPicker />  
</FlightPlanner>
/* FlightPlanner 는 부모 컴포넌트고 데이터를 가집니다.
어떠한 데이터에 대한 변경사항이라도 여기서 바뀝니다. */
```
  + 이번엔 `FlightPlanner` 컴포넌트가 두 자식 컴포넌트를 가집니다. `LocationPicker` 와 `DatePicker` 입니다.
```javascript
<FlightPlanner>
  <LocationPicker>
    <OriginPicker />
    <DestinationPicker />
  </LocationPicker>
</FlightPlanner>
```
  + 여기서는 `<FlightPlanner>` 와 `<LocationPicker>` 가 부모 컴포넌트입니다. 데이터를 저장하면서도 데이터를 업데이트하는 컴포넌트입니다.
- 정리
  + 리액트에서는 데이터 흐름은 단방향으로 부모에서 자식으로 흘러갑니다.
  + 데이터가 하위의 형제 컴포넌트 사이로 공유된다면 데이터는 부모 컴포넌트에 저장되야 하며 데이터를 필요로 하는 자식 컴포넌트 모두에 전달해야 합니다.

## React is "just javascript"
### 단지 자바스크립트일뿐
- 리액트의 위대한 점 중 하나는 단지 보통의 자바스크립트를 사용한다는 것입니다. 지난 몇 년간 함수형 프로그래밍은 자바스크립트 생태계와 커뮤니티에 큰 영향을 끼쳤습니다.
  + 함수형 프로그래밍은 고급 주제로 수많은 책들이 나왔었습니다. 하지만 너무 복잡합니다.
  + 리액트는 함수형 프로그래밍의 많은 기술을 기반으로 삼습니다. 여기서 함수형 프로그래밍에 중요한 자바스크립트 함수 `.map()` 과 `.filter()` 메소드를 살펴봅니다.
### 배열의 `.map()` 메소드
- `.map()` 메소드는 존재하는 배열을 호출해 인수로 전달된 함수에서 return 한 것을 기반으로 새로운 배열을 return 하는 메소드입니다.
- `.map()` 메소드는 배열에서 작동하므로 배열을 먼저 사용합니다. 그리고 `names` 배열에 `.map()` 을 호출해 함수의 인수로 전달합니다.
```javascript
const names = ['Michael', 'Ryan', 'Tyler'];

const nameLengths = names.map( name => name.length );
```
  + 화살표 함수는 `names` 배열의 각 항목을 호출합니다. 항목을 각각 받아 `name` 변수에 저장하고 그것의 길이를 return 합니다.
  + `map()` 은 화살표 함수로부터 return 한 값을로 이뤄진 새 배열을 return 합니다.
  + `nameLengths` 는 이제 `[7, 4, 5]` 인 새 배열입니다. *`.map()` 메소드는 새로운 배열을 return 하는데 원래 있던 배열을 수정하지 않습니다.*
### 배열의 `.filter()` 메소드
- `.filter()` 는 `.map()` 과 비슷합니다. 배열에서 호출되고, 인수로 함수를 취하고 또 새로운 배열을 return 합니다.
- 차이점은 `.filter()` 에 전달된 함수는 테스트에 사용하고 그 테스트에서 합격한 배열의 항목만 새로운 배열에 포함됩니다.
```javascript
const names = ['Michael', 'Ryan', 'Tyler'];

const shortNames = names.filter( name => name.length < 5 );
```
  + 이번에도 `.map()` 처럼 화살표 함수는 `names` 배열의 각 항목을 호출해 가집니다. 각 항목들을 `name` 변수에 저장합니다. 그리고 이름의 길이를 체크하는 필터링(이것이 테스트) 를 시작합니다.
  + 테스트가 끝나면 메소드는 기존 배열을 수정하지 않고 새로운 배열을 return 합니다. `shortNames` 는 새로운 배열 `['Ryan']` 가 됩니다.
### `map()` 메소드와 `.filter()` 메소드 합치기
- 두 메소드를 강력하게 만드는 이유는 둘이 결합할 수 있다는 것입니다.
  + 두 메소드 모두 배열을 return 하니까 메소드 호출을 함께 연결해 return 된 데이터가 다음 배열을 위한 새로운 배열이 되도록 만들 수 있습니다.
```javascript
const names = ['Michael', 'Ryan', 'Tyler'];
const shortNamesLengths = names.filter( name => name.legnth < 5 ).map( name => name.length );
```
  + 우선 `name` 배열을 filter 해 새로운 배열을 return 하면 다음에 `.map()` 으로 전에 만든 배열을 호출해서 다시 새로운 배열을 return 합니다. `.map()` 에서 return 한 배열이 이제 shortNamesLengths 에 저장됩니다.
- *`.filter()` 가 처음입니다.* `.map()` 은 배열의 각 항목에 대해서 함수를 한 번만 실행하기 때문에 배열이 이미 filtering 된 경우에는 더 빠르기 때문입니다.
- 정리
  + 리액트는 이미 알고 있듯이 자바스크립트로 구축합니다. 특별한 탬플릿 라이브러리나 다른 것들을 배울 필요가 없습니다.
  + 두 메인 메소드를 자주 사용합니다. `.map()` 하고 `.filter()` 입니다.
  + 두 메소드를 연습해봅니다. for 반복문을 .map() 으로 바꾼다던가, .filter() 를 써서 if 문을 제거할 수 있는지를 연습합시다.
### 기타 참고사항
- [리액트의 가상 DOM](https://reactjs.org/docs/optimizing-performance.html#avoid-reconciliation) : 가상 DOM 은 각 노드가 HTML element 인 트리를 반영합니다.
  + 리액트는 이 가상 DOM 에 대한 이동 및 연산 작업을 수행할 수 있으므로, 실제 DOM 에서라면 비용이 많이 드는 활동을 방지할 수 있습니다.
- [리액트의 Diffing 알고리즘](https://reactjs.org/docs/reconciliation.html#the-diffing-algorithm) : Diffing 은 DOM 을 효과적으로 바꾸는 방법을 결정합니다.
  + diffing 을 사용하면 필요할 때에만 이전 DOM 노드를 꺼내서 교체할 수 있습니다.
  + 이 방법으로 애플리케이션이 언제 콘텐츠를 렌더링할지를 결정하기 위한 불필요한 작업을 수행하지 않게 만들어줍니다.
