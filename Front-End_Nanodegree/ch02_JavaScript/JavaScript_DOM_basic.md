# 자바스크립트 DOM 개념 (Udacity)
## HTML과 브라우저
- DOM이 뜨는 순서 : HTML 파일을 전송 받고, HTML 태그가 토큰으로 변환됩니다. 토큰들은 노드로 변환됩니다. 그리고 노드들은 다시 DOM 으로 변환됩니다.
  + 조금 더 자세히 말하자면, 브라우저는 서버에서 보낸 바이트를 수신하고, 바이트를 태그로 변환하고 태그들로 토큰 목록을 작정합니다. 토큰 목록은 트리 형태로 구성되며 이것이 문서 객체 모델 DOM 입니다. (문자들 > 토큰 > 노드 > DOM)
- `DOM` : HTML의 내용과 프로퍼티 및 노드 간의 모든 관계를 캡처하는 트리 구조입니다. `DOM` 은 HTML 의 전체적인 구문을 분석한 것입니다.
  + `DOM` 은 브라우저가 제공하는 특별한 객체 `document` 에 접근할 수 있습니다.
- `document` 객체 : 브라우저에서 제공하며 HTML 문서를 가리킵니다. (자바스크립트에서 제공하지 않습니다. 자바스크립트는 document 객체를 통해 전역 객체에서 DOM 에 접근합니다.)
  + DOM 트리 형태로 HTML 문서와 elements 간의 관계를 표현합니다.

## 페이지 element 를 ID 로 선택하기
- `document` 객체는 자바스크립트 객체와 같이 키와 속성를 갖추고 있습니다. (일부 속성중 메소드도 있습니다.)
- `document.getElementById();` : () 안에 문자열을 전달해 element 의 ID 를 찾아내서 return 합니다. () 는 ID 지만 '#ddd' 이런 형태로 입력하지 않고 문자열로 입력합니다.
  + 페이지에 없는 ID 를 작성할 경우 null 을 return 합니다.
  + return 한 값을 변수에 저장할 수도 있습니다.
```javascript
const head = document.getElementById('header'); //ID 가 header 인 헤더의 element 들을 선택해서 head 에 저장합니다.
```

## 페이지 elements 를 클래스나 태그로 선택하기
- id 는 유니크하고 같은 id 는 가질 수 없습니다. 따라서 `document.getElementById('');` 로는 오직 하나의 element 만을 선택해서 그것 하나만 return 합니다. 다수의 DOM elements 를 선택하려면 클래스나 태그를 return 하도록 해야합니다.
- 참고로 getElement 가 아니라 getElements 입니다. 왜냐면 동일 클래스인 것들이 많이 return 되기 떄문입니다. 그리고 배열 형태가 아니라 `HTML collection` 형태로 return 됩니다.
- `document.getElementsByClassName('');` : () 안에 클래스 이름을 문자열로 적습니다. 그래서 원하는 클래스를 불러서 return 합니다.
- `document.getElementsByTagName('');` : () 안에 태그 이름을 문자열로 적습니다. 그러면 <> 안의 태그를 선택해서 그 태그의 elements 들을 출력합니다.
```javascript
document.getElementsByClassName('pre');
//콘솔창 결과 예시 : pre 와 점 뒤에는 pre 의 클래스 이름이 옵니다.
//이 elements 들은 배열 형태가 아니라 HTML collection 형태로 출력됩니다.
(2)
0: pre.className
1: pre.className02
```

## 노드, elements, 인터페이스
### 노드
- `Node` : 실제 nodes 들의 프로퍼티와 메소드 정보를 가진 설계도 혹은 인터페이스 입니다. (인터페이스 = 설계도, 프로퍼티 = 데이터, 메소드 = functionality(기능))
- `nodes` : 설계도로 만든 실제 객체들을 말합니다.

### Element 인터페이스
- `Element 인터페이스` : elements 들을 만드는 설계도입니다. 프로퍼티와 메소드들을 가지고 있습니다. 중요한 점은 이것은 Node 인터페이스의 자손이라는 것입니다.
  + `Element 인터페이스` 는 Node 인터페이스의 모든 프로퍼티와 메소드를 상속받았습니다. 즉 Element 인터페이스에서 생성된 모든 element 는 Node 인터페이스의 하위 element 이기도 합니다.
- `Element 인터페이스` 에는 document 객체와 동일한 작업을 수행하는 고유한 `.getElementsByClassName()` 가 있습니다.
  + document 객체로 element 를 선택한 다음 `.getElementsByClassName()` 를 호출해 해당 element 의 하위 element 인 클래스 이름을 수신할 수 있습니다.
```javascript
// id 가 sidebar 인 DOM element 를 선택합니다.
const sidebarElement = document.getElementById('sidebar');

// sidebar element 로 클래스가 sub-heading 인 element를 찾습니다.
const subHeadingList = sidebarElement.getElementsByClassName('sub-heading');
```

## elements에 접근하는 그 외의 방법
- 모든 브라우저에서 모든 DOM 표준을 지원하지는 않습니다. 그래서 메소드의 브라우저 호환성 테이블을 확인해야 합니다.
  + 현재는 낫지만 과거에는 각 브라우저마다 표준이 달랐습니다. 그래서 jQuery 메소드로 맞춰줘야 했습니다.
  + 지금은 새로운 DOM 메소드들로 교체됐습니다.
- CSS 로도 `getElementById`, `getElementsByClassName`, `getElementsByTagName` 와 비슷한 역할을 할 수도 있습니다.

### querySelector 메소드
- `querySelector()` : CSS 로 elements 를 선택합니다.
```javascript
//id 가 "header" 인 element 를 return 합니다.
document.querySelector('#header');

//class 가 "header" 인 element 를 return 합니다.
document.querySelector('.header');

//태그가 header 인 첫 번째 element 를 return 합니다.
document.querySelector('header');
//첫번째 단락(paragraph)이면서도 클래스가 'callout' 인 element 를 return 합니다.
document.querySelector('p.callout');
```
- `querySelector()` 메소드는 클래스나 태그를 검색하더라도 하나의 element 만을 return 합니다.(검색한 첫 번째 항목으로 합니다.)

### querySelectorAll 메소드
- `querySelectorAll()` : 특정 클래스나 태그를 가진 모든 elements 를 return 하려면 `querySelectorAll()` 메소드를 써야 합니다.
```javascript
//class 가 "header" 인 모든 element 를 return 합니다.
document.querySelectorAll('.header');

//태그가 header 인 모든 element 를 return 합니다.
document.querySelectorAll('header');
```
- 결과를 출력할 때 배열로 출력하지 않습니다. `NodeList` 라 불리는 특별한 리스트 형태로 출력합니다.
  + 그래서 반복문으로 출력할 수 있습니다.
```javascript
const allHeaders = document.querySelectorAll('header');

for(let i = 0; i < allHeaders.length; i++){
    console.dir(allHeaders[i]);
}
```
