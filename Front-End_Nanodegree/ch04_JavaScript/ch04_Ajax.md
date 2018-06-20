# Ajax (Asynchronous JavaScript And Xml)
## Ajax 와 XHR
- 90년대 후반의 XMLHTTP 에서 나온 XMLHttpRequest 를 이용해 브라우저에서 자바스크립트로 Http 요청을 만들고 웹 페이지를 전부 새로고침할 필요 없이 현 페이지 그대로 업데이트하도록 만들었습니다.
- `Ajax` : 2005년, XMLHttpRequest 을 사용해 데이터를 가져오고(fetch) 그리고 현 페이지를 수정하는 기술이 나타났습니다.
  + 하지만 코드가 복잡하지고, 더 많은 기기와 호환해야하고, 브라우저가 계속 변함에 따라 Ajax 앱은 개발하기가 어려워졌습니다. 이에 따라 자바스크립트 프레임워크의 필요성이 늘어났습니다.
  + 자바스크립트 프레임워크로 더 복잡한 작업을 처리할 수 있게 됩니다.

### API (Application programming Interface)
- 보통 많은 데이터를 가진 애플리케이션은 데이터를 가져오는 데 제 3자 API 를 사용합니다. (예 : 구글 API, Giant database of APIs 등)

### XHR 객체로 비동기 요청 만들기
- 자바스크립트 엔진은 비동기 Http 요청을 만들 방법을 제공합니다. 바로 `XMLHttpRequest` 객체입니다.
  + 이 객체와 `XMLHttpRequest` 생성자 함수를 만들어 사용합니다.
```javascript
const asynRequestObject = new XMLHttpRequest();
```
  + 이 생성자 함수는 XML 을 가지고 있지만, 오직 XML 문서에서만 가능합니다. (AJAX 약어는 비동기 자바스크립트 및 XML 을 나타내기 위해 사용됩니다.)
  + `XMLHttpRequest` (줄여서 XHR) 은 다양한 파일 타입 혹은 API 데이터를 요청하는데 사용됩니다.
[모질라 XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/open)

### XHR .open 메소드
- 위에서 만든 asynRequestObject 객체에 `.open` 메소드를 사용할 수 있습니다.
  + 파라미터로 요청 방식과 요청을 보낼 HTTP 메소드 URL 이 사용됩니다.
```javascript
asynRequestObject.open('GET', 'https://google.com');
```
  + `GET` 은 데이터를 받을 때, `POST` 는 데이터를 보낼 때 사용합니다.
- `.open` 메소드는 실제로 데이터를 전송하지 않습니다. 스테이지를 설정하고 객체에 나중에 요청할 때 필요할 정보를 객체에 전달합니다.
- 세번째 옵션으로 불리언 값이 있습니다. 기본값이 true 이며 false 일 경우 XHR 요청이 동기 요청으로 변환됩니다. 그래서 자바스크립트 엔진을 멈추고 요청이 return 될 때까지 기다립니다.

### XHR .send 메소드
- 실제로 요청을 보내려면 send 메소드를 써야 합니다.
```javascript
asynRequestObject.send();
```
  + 콘솔창에는 아무것도 안 뜨지만, 네트워크창에서 녹화버튼으로 실행해보면 요청을 보냈음을 알 수 있습니다.
- 성공적으로 XHR 응답을 처리하기 위해 객체의 onload 프로퍼티를 처리할 함수로 설정합니다.
```javascript
function handleSuccess () {
  /* 여기서 this 값은 XHR 객체를 의미합니다.
  this.responseText 는 서버로부터의 응답을 가지고 있습니다. */
  console.log(this.responseText); //결과 : the HTML of https://google.com
}
asynRequestObject.onload = handleSuccess;
```
  + `onload` 를 사용해 요청이 return 하게 했지만 아무 일도 생기진 않습니다.
- 에러 다루기 : 요청에 문제가 있어 실행되지 않았다면 `onerror` 프로퍼티를 써야합니다.
```javascript
function handleError () {
  //this 값은 XHR 객체입니다.
  console.log('Error 발생');
}
asynRequestObject.onerror = handleError;
```
  + `onerror` 프로퍼티를 쓰지 않았을 때 에러가 생긴다면 아무런 에러 메시지가 없어 뭐가 문제인지조차 알지 못할 것입니다.

### Full request
- 아래는 XHR 객체를 만들고 성공, 실패 핸들러를 성정한 후 실제로 요청을 보내는 코드입니다.
```javascript
function handleSuccess () {
  console.log(this.responseText); //결과 : the HTML of https://~~
}
function handleError () {
  console.log('에러 발생 \uD83D\uDE1E');
}
const asynRequestObject = new XMLHttpRequest();
asynRequestObject.open('GET', 'https://unsplash.com');
asynRequestObject.onload = handleSuccess;
asynRequestObject.onerror = handleError;
asynRequestObject.send();
```
- JSON 을 사용한다면 원하는 데이터로 쉽게 형식화된 구조를 만들 수 있게 됩니다.
```javascript
function handleSuccess () {
  //데이터를 JSON 형식에서 자바스크립트 객체로 바꿉니다.
  const data = JSON.parse(this.responseText);
  console.log(data);
}
```
  + JSON 을 return 하는 API 가 요청할 때 JSON 응답을 자바스크립트 객체로 바꾸면 됩니다.
  + 바로 `JSON.parse()` 입니다.
  + 마지막으로 JSON 응답을 onload 함수로 조절합니다.

### 과제 준비하기
- [프로젝트 깃허브](https://github.com/udacity/course-ajax)
- [Unsplash 계정 생성](https://unsplash.com/developers)
  + 그 후 애플리케이션 생성하기
- [뉴욕타임즈 개발자 계정 생성](https://developer.nytimes.com/)
- Unsplash 요청
  + XHR 로 Unsplash 를 호출할 코드입니다. 이 방식은 에러가 뜰 겁니다.
```javascript
function addImage () {/* 나중에 채웁니다. */ }
const searchForText = 'hippos';
const unsplashRequest = new XMLHttpRequest();

unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
unsplashRequest.onload = addImage;

unsplashRequest.send();
```
  + 이유는 unsplash 에 대한 요청은 HTTP 헤더를 보내야하기 때문입니다. 요청에 헤더를 추가하려면 `.setRequestHeader()` XHR 메소드가 필요합니다.

### 요청 헤더 설정하기
- `setRequestHeader` 를 사용한 코드입니다.
```javascript
const searchForText = 'hippos';
const unsplashRequest = new XMLHttpRequest();
unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
unsplashRequest.onload = addImage;
unsplashRequest.setRequestHeader('Authorization', 'Client-ID <your-client-id>');
unsplashRequest.send();

function addImage () {  }
```
- 뉴욕타임즈는 특정한 헤더는 요구하지 않습니다.
```javascript
function addArticles () { }
  const articleRequest = new XMLHttpRequest();
  articleRequest.onload = addArticles;
  articleRequest.open('GET', `http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=<your-API-key-goes-here>`);
  articleRequest.send();
```

### XHR 요약 정리
- 비동기 요청 전송하기
  + XHR 객체로 XMLHttpRequest 생성자 함수를 만듭니다.
  + `.open()` 메소드 사용 : HTTP 메소드와 전송될 자원의 URL 을 설정합니다.
  + `.onload` 프로퍼티 설정 : 성공적인 fetch 를 위해 함수 안에 onload 를 넣습니다.
  + `.onerror` 프로퍼티 설정 : 에러가 발생할 때를 대비에 함수 안에 onerror 를 넣습니다.
  + `.send()` 메소드 사용 : 요청을 전송합니다.
- `.responseText` 프로퍼티 사용하기 : 비동기 요청의 응답을 텍스트로 붙잡습니다.
- 2012 년 XHR 버젼 2 작업이 시작됐고, 현재 XHR 사양에 XHR 2 가 통합됐습니다.
---
## Ajax 와 jQuery
### ajax 메소드
- `.ajax()` 메소드는 jQuery 로 비동기 요청을 하는데 매우 중요합니다.
  + 가장 일반적인 사용법은 구성 객체에서만 사용하는 것입니다.
- 구성 객체 : 구성 객체는 뭔가를 구성하는데 사용하는 일반적인 자바스크립트 객체입니다.
```javascript
var settings = {
  frosting: 'buttercream',
  colors: ['orange', 'blue'],
  layers: 2,
  isRound: true
}
//settings 구성 객체는 가상 MakeCake 생성자 함수로 쓸 수 있습니다.
const myDeliciousCake = MakeCake(settings);
//한편 settings 객체를 직접 전달할 수도 있습니다.
const myCake = MakeCake({
  frosting: 'buttercream',
  colors: ['orange', 'blue'],
  layers: 2,
  isRound: true
});
```
- 단순한 Ajax 요청은 `$.ajax({ url: 'https://aaa.com' });` 이런 방식으로 호출합니다.
- 요약
  + 새로운 XHR 객체를 호출될 때마다 생성합니다.
  + 모든 XHR 프로퍼티와 메소드들을 설정합니다.
  + XHR 요청을 전달합니다.

### return 된 데이터 다루기
- `ajax()` 메소드는 `done()` 메소드로 연결할 수 있습니다. `done()` 을 전달하면 Ajax 호출로 실행되는 함수가 완료됩니다.
```javascript
function handleResponse(data) {
  console.log('the ajax request has finished.');
  console.log(data);
}
$.ajax({
  url: 'https://swapi.co/api/people/1'
}).done(handleResponse);
```
- jQuery 코드 사용시 참고사항
  + XHR 객체를 만들 필요가 없습니다.
  + 요청을 `GET` 요청으로 지정하는 대신, 기본적으로 요청을 `GET` 으로 하고 요청하는 리소스의 URL 을 제공합니다.
  + `onload` 대신 `.done()` 메소드를 사용합니다.
```javascript
$.ajax({
  url: https://api.unsplash.com/search/photos?page=1&query=${searchedForText}
  //headers 객체를 프로퍼티로 전달해 요청에 추가됩니다.
  headers: {
      Authorization: 'Client-ID 123abc456def'
  }
}).done(addImage);
```
### 성공 콜백을 정리하기
- jQuery 는 응답을 감지해 JSON 일 경우 자바스크립트로 알아서 변환해줍니다.
  + 따라서 'const data = JSON.parse(this.responseText)' 는 필요없습니다.
```javascript
function addImage(images) {
  const firstImage = images.results[0];
  responseContainer.insertAdjacentHTML('afterbegin', `<figure>
    <img src="${firstImage.urls.small}" alt="${searchedForText}">
    <figcaption>${searchedForText} by ${firstImage.user.name}</figcaption>
    </figure>`);
}
```
  + addImage 함수에 images 파라미터가 생겼습니다.
  + 파라미터는 이미 JSON 에서 자바스크립트 객체로 바뀌었고, `JSON.parse()` 는 필요가 없어졌습니다.
  + `firstImage` 변수는 images.results 의 첫 항목으로 설정됩니다. (원래는 data.results)

### call stack
- 2장 JavaScript_performance 에서 call stack 에 대한 설명이 있습니다.
  + 진행중인 함수의 호출 순서를 나타냅니다.
  + stack 맨 아래의 함수가 처음으로 실행된 후 그 다음 두번째 함수를, 그 다음에는 세 번째 함수를 호출하는 형식입니다. 함수는 stack 위의 stack 이 return 될 때까지 stack 에 남아있습니다.
- ajax() 에서도 call stack 을 사용할 수 있습니다.
  + 익명의 함수에서 ajax() 를 호출합니다.
  + ajax() 가 send() 메소드를 호출합니다.
  + send() 메소드는 options.xhr() 을 호출합니다.
  + options.xhr() 은 새로운 XHR 객체를 만드는 jQuery.ajaxSettings.xhr 을 호출합니다. (즉 새로운 XMLHttpRequest() 객체를 만듭니다.)
- ajax() 가 호출될 때마다 jQuery.js 코드는 새로운 XHR 객체를 생성합니다.
  + 코드를 보면 `return new window.XMLHttpRequest()` 로 되어있습니다. 따라서 이 코드는 `$.ajax()` 가 실행될 때마다 매번 새로운 XHR 객체를 return 합니다.
- jQuery 는 for in 반복문을 사용해서 headers 객체의 데이터를 반복하도록 설정합니다.

### 기타 비동기 메소드들
- .get() .getJSON() .getScript() .post() .load()
  + 이 함수들은 jQuery 의 .ajax() 메소드를 호출합니다. 편리한 인터페이스를 제공하고 ajax() 를 호출하기 전에 요청의 기본 구성을 수행하기에 편리합니다.
---
## aJax 와 fetch
- fetch 를 사용하면 jQuery 보다도 훨씬 편하게 비동기 요청을 할 수 있습니다.

### fetch 요청 작성하기
- fetch 요청은 오직 `fetch()` 함수와 요청될 리소스만 필요합니다. 매우 쉽습니다.
```javascript
fetch('https://api.unsplash.com/search/photos?page=1&query=flowers');
```
  + fetch 요청은 프로미스를 return 합니다.
  + 하지만 XHR 객체를 대체한다고 해도 네트워크 요청을 만드는 규칙을 무시하지는 못합니다.
  + 그래서 cross-origin 프로토콜을 준수해야 합니다. (기본적으로 데이터를 불러올 사이트와 동일 도메인에 있는 asset 과 데이터에 대한 요청만 가능합니다.)

- HTTP 메소드 변경하기
  + 기본적으로 fetch 요청에 대한 HTTP 메소드는 `GET` 메소드입니다. `GET` 말고도 다른 HTTP 메소드를 전달 할 수 있습니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    method: 'POST'
});
```
  + fetch 는 HTTP 메소드를 제한하지 않지만 대문자로 작성하면 좋습니다.

### 응답 다루기
- fetch 는 프로미스 기반이라서 요청을 실행하면 응답을 수신하는데 사용가능한 프로미스가 자동으로 return 됩니다.
  + 프로미스가 return 되면 `.then()` 으로 그 프로미스를 호출합니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID f9eb6dc3f8ca5dceba921b978ae24cfcef5666674fc1df37b0bea07423270b57'
    }
}).then(function(response) {
    debugger; // work with the returned response
});
```
  + 실행한다면 검색 후 디버거로 인해 멈춥니다. return 한 응답은 응답 객체임을 알 수 있습니다.

### 응답 객체
- 응답 객체는 fetch API 에서 새로 추가된 것입니다. 그리고 fetch 요청이 해결됐을 때 return 되는 객체입니다.
  + 하지만 응답 객체에는 아무런 데이터를 가지고 있지 않습니다. 왜냐면 응답 객체는 응답에 대한 정보만을 가지고 있기 떄문입니다.
  + 실제 데이터를 얻기 위해서는 응답의 body 를 얻어야 합니다.
- Unsplash API 는 JSON 을 return 하므로, 응답 변수에 `.json()` 을 사용하면 됩니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
  headers: {
        Authorization: 'Client-ID f9eb6dc3f8ca5dceba921b978ae24cfcef5666674fc1df37b0bea07423270b57'
  }
}).then(function (response) {
  return response.json();
});
```
  + 응답 겍체의 `json()` 메소드는 프로미스를 return 합니다. 그 후 다른 `then()` 을 사용해 실제 데이터를 얻고 그 데이터를 사용할 필요가 있습니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
  headers: {
        Authorization: 'Client-ID f9eb6dc3f8ca5dceba921b978ae24cfcef5666674fc1df37b0bea07423270b57'
  }
}).then(function (response) {
  return response.json();
}).then(addImage); //여기 then 은 데이터를 얻어 사용합니다.
function addImage(data) { debugger; }
```
- `json()` 메소드는 JSON 형태인 응답을 받은 후 바꿔줍니다.
  + 참고로 이미지 body 를 응답에서 추출하는 메소드는 `.blob()` 메소드입니다.
```javascript
fetch('https://davidwalsh.name/flowers.jpg')
	.then(function(response) {
	  return response.blob();
	})
	.then(function(imageBlob) {
	  document.querySelector('img').src = URL.createObjectURL(imageBlob);
	});
```

### 화살표 함수와 fetch
- 첫 번째 `then()` 함수를 화살표 함수로 바꿔서 응답을 얻고, 다음에 `json()` 메소드를 쓴 후, 프로미스를 return 할 수 있습니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
  headers: {
        Authorization: 'Client-ID f9eb6dc3f8ca5dceba921b978ae24cfcef5666674fc1df37b0bea07423270b57'
  }
}).then(response => response.json())
.then(addImage);

function addImage(data) { debugger; }
```

### 콘텐츠와 에러 처리를 표시하기
- 이미지와 캡션을 페이지에 표시하는 코드입니다.
```javascript
function addImage(data) {
  let htmlContent = '';
  const firstImage = data.results[0];

  if (firstImage) {
    htmlContent = `<figure>
      <img src="${firstImage.urls.small}" alt="${searchedForText}">
      <figcaption>${searchedForText} by ${firstImage.user.name}</figcaption>
      </figure>`;
  } else {
    htmlContent = 'Unfortunately, no image was returned for your search.'
  }
  responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
}
```
  + firstimage 는 Unsplash 로부터 return 된 것입니다.
  + <figure> 태그에 작은 이미지를 넣어 만들었습니다.
  + 검색한 이미지 관련 글들과 사진을 찍은 사람을 보여주는 <figcaption> 을 만들었습니다.
  + 아무런 이미지가 없이 return 되면 에러 메시지를 띄웁니다.
#### 에러 다루기
- 네트워크 관련 에러, fetch 요청에 대한 에러, Unsplash 가 검색한 이미지를 가지지 않을 때의 에러 등등이 있을 것입니다.
- `.catch()` 메소드를 fetch 요청에 연결할 수 있습니다.
```javascript
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(response => response.json())
.then(addImage)
.catch(e => requestError(e, 'image'));
function addImage(data) { /* 동일내용 */ }

function requestError(e, part) {
  console.log(e);
  responseContainer.insertAdjacentHTML('beforeend', `<p class="network-warning">
  There was an error making a request for the ${part}.`);
}
```
  + `catch()` 요청을 프로미스 체인의 마지막에 추가했습니다.
  + `.catch()` 함수는 에러 객체를 수신한 후, 에러 객체와 실패한 요청을 전달하는 `requestError` 를 호출합니다.
  + 프로미스가 코드를 수행하다가 거절을 한다면 requestError 함수는 그 에러를 기록하고 경고 메시지를 표시해줍니다.
