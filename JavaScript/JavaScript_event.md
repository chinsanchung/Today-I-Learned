# 자바스크립트 이벤트
이벤트는 키보드나 마우스 버튼 클릭 등 다른 것에 영향을 미치는 것을 의미합니다.
## 이벤트의 종류
- 마우스 이벤트
- 키보드 이벤트
- HTML 프레임 이벤트
- HTML 입력 양식 이벤트
- 유저 인터페이스 이벤트
- 구조 변화 이벤트
- 터치 이벤트

## 관련 용어 정리
- 이벤트를 연결한다

'''javascript
window.onload = function () {};
'''

  + 'window' 객체의 'onload' 속성에 함수 자료형을 할당하는 것을 "이벤트를 연결한다"고 합니다.
  + 'load'는 '이벤트 이름' 또는 '이벤트 타입'이라고 하며, 'onload'를 '이벤트 속성'이라고 합니다.
  + 또한 '이벤트 속성'에 할당한 함수를 '이벤트 리스너' 또는 '이벤트 핸들러'라고 합니다.
- 문서 객체에 이벤트를 연결하는 방법을 '이벤트 모델'이라고 합니다. 종류는 'DOM Level 0', 'DOM Level 2'로 구분됩니다.
  + DOM Level 0 : 인라인 이벤트 모델, 고전 이벤트 모델이 있습니다. 한 번에 하나의 이벤트 리스너만을 가질 수 있습니다.
  + DOM Level 2 : Level 0 모델들의 단점을 보완하기 위해 만들었습니다. 인터넷 익스플로러 모델과 표준 이벤트 모델이 있습니다.

### 고전 이벤트 모델
- 자바스크립트에서 문서 객체의 '이벤트 속성'으로 이벤트를 연결하는 방법입니다.

'''javascript
window.onload = function () {
  //변수를 선언
  let header = document.getElementById('header');

  //이벤트를 연결
  header.onclick = function () {
    alert('클릭');

    //이벤트를 제거(이벤트 리스너를 제거하려면 문서 객체의 이벤트 속성에 null을 넣습니다.)
    header.onclick = null;
  };
};
'''

### 이벤트 발생 객체
- 이벤트 리스너 안에 this 키워드를 사용하면 이벤트가 발생한 객체를 찾을 수 있습니다.

'''javascript
window.onload = function () {
  document.getElementById('header').onclick = function () {
    alert(this);
  };
};
'''

  + 코드를 실행하고 onclick 이벤트를 실행하면 경고창에 [object HTMLHeadingElement]가 출력됩니다. 이벤트가 발생한 객체입니다.

### 이벤트 강제 실행
- 메소드를 호출하는 것처럼 이벤트 속성을 호출하면 이벤트가 강제로 실행됩니다.

'''javascript
<script>
  //문서 객체를 가져옴
  let buttonA = document.getElementById('button-a');
  let buttonB = document.getElementById('button-b');
  let counterA = document.getElementById('counter-a');
  let counterB = document.getElementById('counter-b');

  //이벤트를 연결
  buttonA.onclick = function () {
    counterA.innerHTML = Number(counterA.innerHTML) + 1;
  };
  buttonB.onclick = function () {
    counterB.innerHTML = Number(counterB.innerHTML) + 1;
    //버튼B 클릭시 A의 클릭 횟수 증가
    buttonA.onclick();
  };
</script>
//body
<body>
  <button id="button-a">ButtonA</button>
  <button id="button-b">ButtonB</button>
  <h1>Button A - <span id="counter-a">0</span></h1>
  <h1>Button B - <span id="counter-b">0</span></h1>
</body>
'''

### 디폴트 이벤트 제거
- '디폴트 이벤트'는 이미 이벤트 리스너가 있는 일부 HTML 태그를 뜻합니다.
- '디폴트 이벤트'는 입력 양식에 많이 사용됩니다. 예를 들어 가입 양식을 다 작성하고 제출하는 것이 디폴트 이벤트인데, 만약 양식을 다 작성하지 않았다면 그 양식을 제출해서는 안되기 때문에 이벤트 제거를 사용합니다.

'''javascript
window.onload = function () {
  //이벤트를 연결
  document.getElementById('my-form').onsubmit = function () {
    //변수 선언
    let pass = document.getElementById('pass').value;
    let passCheck = document.getElementById('pass-check').value;

    //비밀번호가 같은지 확인
    if (pass == passCheck) {
      alert('success');
    } else {
      alert('다시 작성하세요')
      return false;
    }
  };
};
'''

  + 이벤트 리스너에서 false를 리턴하면 디폴트 이벤트를 제거됩니다.

### 이벤트 전달
- 어떠한 이벤트가 먼저 발생해 어떤 순서로 발생하는지를 이벤트 전달이라고 합니다.
- 전달의 종류로는 '이벤트 버블링'과 '이벤트 캡쳐링'이 있습니다.
  + 이벤트 버블링 : 자식 노드에서 부모 노드 순으로 이벤트를 실행합니다.
  + 이벤트 캡쳐링 : 부모 노드에서 자식 노드 순으로 이벤트를 실행합니다.
- 인터넷 익스플로러, jQuery가 캡쳐링을 지원하지 않아 버블링을 주로 사용합니다.

### 인터넷 익스플로러 이벤트 모델
- 두 가지 메소드로 이벤트를 연결, 제거할 수 있습니다.

'''javascript
attachEvent(eventProperty, eventListner);
detachEvent(eventProperty, eventListner);
'''

- attachEvent는 익스플로러에서만 실행되므로 다른 브라우저에선 에러가 발생합니다.
- DOM Level 0 모델과 다르게 여러 번 이벤트를 추가할 수 있습니다.
- 이벤트를 제거하는 detachEvent를 쓰려면 이벤트 리스너를 명확하게 지정해줘야 합니다. 따라서 익명 함수를 이벤트 리스너로 사용한 이벤트는 제거할 수 없습니다.

### 표준 이벤트 모델
- DOM Level 2 이벤트 모델로 한 번에 여러 가지의 이벤트 리스너를 추가할 수 있습니다.
- 연걸할 때 다음과 같은 메소드를 사용합니다.

'''javascript
addEventListner(eventName, handler, useCapture)
removeEventListner(eventName, handler)
'''

  + 매개변수 useCapture는 입력하지 않으면 자동으로 false를 입력합니다.
  + 클릭 이벤트 연결하기

'''javascript
window.onload = function () {
  let header = document.getElementById('my-header');
  header.addEventListner('click', function () {
    this.innerHTML += '+';
    });
};
'''

- 표준 이벤트 모델은 이벤트 리스너 안에서 'this키워드'가 이벤트 발생 객체를 의미합니다.
