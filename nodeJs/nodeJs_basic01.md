# node.js 기본
## Ajax
- 'Ajax' 를 사용하면 페이지를 전환하지 않고 한 페이지에서 데이터를 받아 사용자에게 보여줄 수 있습니다.

## 데이터 전송 형식
- 서버와 클라이언트가 통신할 때 CSV, XML, JSON 형식을 사용합니다.
### CSV
- 각 항목을 쉼표로 구분해 데이터를 표현하는 방법입니다.
- 장, 단점
  + 장점 : 형식이 짧고 용량이 적습니다. 많은 양의 데이터를 전송할 때 사용합니다.
  + 단점 : 데이터의 가독성이 떨어집니다. 각각의 데이터가 무엇을 나타내는지 알기가 어렵습니다.
- 가독성을 높이기 위해 'split()' 메소드로 문자열을 분해합니다.
```javascript
  //변수 선언
  let input = '';
  input += 'a, 1, 70\n';
  input += 'b, 3, 80\n';
  input += 'c, 5, 90\n';

  //한 줄씩 자름
  input = input.split('\n')
  for (let i = 0; i < input.length; i++) {
    //쉼표를 기준으로 자름
    input[i] = input[i].split(',');
    for (let j = 0; j < input[i].length; j++) {
      //각 데이터의 양 옆 공백을 제거
      input[i][j] = input[i][j].trim();
    }
  }
```

### XML
- HTML 형식처럼 태그로 데이터를 표기합니다.
- 장, 단점
  + 장점 : 가독성이 좋아 각각의 데이터의 의미를 알 수 있습니다. 태그에 사용자 속성을 넣어 복잡한 데이터를 전달할 수도 있습니다.
  + 단점 : 태그가 많아 용량이 크고 데이터 양이 커지면 분석 속도가 떨어집니다.
```
<?xml version="1.0" encoding="utf-8" ?>
<students>
  <student>
    <name>a</name>
    <class>1</class>
    <grade>70</grade>
  </student>
  <student>
    <name>b</name>
    <class>3</class>
    <grade>90</grade>
  </student>
  <student>
    <name>c</name>
    <class>5</class>
    <grade>90</grade>
  </student>
</students>
```

### JSON(JavaScript Object Notation)
- CSV와 XML의 단점을 모두 극복했습니다.
- 자바스크립트에서 쓰는 객체 형태로 데이터를 표현하는 방법입니다.
- 객체, 배열, 문자열, 숫자, 불, null만 들어갑니다.
  + 문자열은 큰따옴표를 써야만 합니다.
- 장, 단점
  + 장점 : CSV보다 가독성이 좋고, XML보다 적은 용량으로 데이터를 전달할 수 있습니다.
  + 단점 : 데이터의 양이 많아지면 데이터 추출 속도가 많이 떨어집니다.
```JSON
[{
  "name": "a",
  "class": 1,
  "grade": 80
  }, {
  "name": "b",
  "class": 3,
  "grade": 90
  }, {
  "name": "c",
  "class": 5,
  "grade": 80
}]
```

## node.js
### 개요
- 라이언 달이 CommentJS 표준에 따라 크롬 V8엔진을 기반으로 개발한 플랫폼입니다.
- node.js로 웹 브라우저가 아닌 곳에서 자바스크립트로 프로그램을 개발할 수 있습니다.
- node.js를 설치한 후 명령 프롬프트 창에 'node -v'로 버젼을 확인합니다.
  + node.js를 실습할 폴더에 shitf를 누르고 우클릭 -> 프롬프트창을 띄우면 'cd...'로 번거롭게 이동할 필요가 없습니다.
- 파일 실행 : 'console.log()' 메소드로 출력합니다.
```javascript
let output = '';
for (let i = 0; i < 10; i++) {
  console.log(output += '*');
}
```

### 내부 모듈
- node.js가 기본적으로 가지고 있는 모듈입니다.
- 사용법 : 모듈을 추출한 후 그 속성을 출력해 사용합니다.
```javascript
//모듈을 추출
let os = require('os');
//속성을 출력
console.log(os.hostname());
console.log(os.type());
console.log(os.platform());
console.log(os.arch());
```

### 외부 모듈
- 개인 또는 단체가 만들어 배포하는 모듈입니다.
- 사용법 : 명령 프롬프트 창에 'npm install 모듈이름' 을 입력합니다. (request 모듈 설치하려면 'npm install request')
```javascript
//모듈 추출
let request = require('request');
//특정 웹 페이지를 긁음
request('http://google.com', function (error, response, body) {
  console.log(body);
});
```
  + 결과로 구글 메인 페이지의 소스 코드를 출력합니다.

### 서버 생성 및 실행
- 순서
  + 우선 'server.js' 파일을 생성합니다.
  + 웹 서버를 만들 때는 express라는 외부 모듈을 씁니다. 프롬프트창에 'npm install express@4.14'를 입력합니다.
  + 모듈을 추출하고 서버를 생성, 실행합니다.
```javascript
//모듈 추출
let express = require('express');

//웹 서버 생성
let app = express();
  //웹 서버에 app.use() 메소드로 기능을 부여합니다. 이 메소드는 여러 번 사용가능합니다.
  //app.use() 메소드에 입력하는 콜백함수는 request 이벤트 리스너입니다.
app.use(function (request, response) {
  response.send('<h1>hello</h1>');
});

//웹 서버를 실행..52273포트에서 서버가 실행됨
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```
  + 프롬프트 창에 'node server.js' 를 입력합니다. 그리고 브라우저에서 127.0.0.1:52273에 접속하면 hello가 출력됩니다.
  + 프롬프트 창에서 'ctrl + c' 로 서버를 종료합니다.

### 미들웨어
- 미들웨어 : 사용자의 요청을 처리하면서 지나가는 app.use() 메소드의 콜백 함수입니다.
```javascript
//모듈 추출
let express = require('express');

//웹 서버 생성
let app = express();
//매개변수 next는 다음 콜백 함수를 의미합니다.
app.use(function (request, response, next) {
  console.log('first');
  next();
});
app.use(function (request, response, next) {
  console.log('second');
  next();
});
app.use(function (request, response, next) {
  response.send('<h1>hello</h1>');
});
//웹 서버를 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```
  + 실행 시 페이지에는 hello가, 프롬프트 창에는 first와 second가 나옵니다.
  + 각각의 콜백 함수 next를 통과하여 console.log() 메소드를 실행했습니다.
- 미들웨어를 사용하면 request 객체와 response 객체에 기능을 추가할 수 있습니다.
```javascript
//모듈 추출
let express = require('express');

//웹 서버 생성
let app = express();
app.use(function (request, response, next) {
//request와 response 객체에 test 속성을 추가합니다.
  request.test = 'request.test';
  response.test = 'response.test';
  next();
});
//두 번째 미들웨어에서 추가한 속성들을 출력합니다.
app.use(function (request, response, next) {
  response.send('<h1>' + request.test + ' : ' + response.test + '</h1>');
});
//웹 서버를 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```

### 미들웨어_static(정적 파일 제공)
- static 미들웨어 : 정적 파일을 제공합니다.(정적 파일은 변화하지 않는 일반 파일입니다.)
```javascript
//모듈 추출
let express = require('express');

//웹 서버 생성
let app = express();
//express.static() 메소드를 app.use() 메소드의 매개변수로 넣습니다.
  //public 폴더에는 페이지에 보여줄 index.html이 있습니다.
app.use(express.static('public'))
app.use(function (request, response, next) {
  response.send('<h1>hello</h1>');
});
//웹 서버를 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```
- 미들웨어는 위에서 아래 순서대로 실행됩니다.

### 미들웨어_router
- 'route' : 사용자의 요청에 따라 사용자가 필요한 정보를 제공합니다.
- 'router' : route 기능을 수행하는 미들웨어입니다.
  + 'app.get()' : 클라이언트의 GET 요청을 처리합니다.
  + 'app.post()' : 클라이언트의 POST 요청을 처리합니다.
  + 'app.put()' : 클라이언트의 PUT 요청을 처리합니다.
  + 'app.dell()' : 클라이언트의 DELETE 요청을 처리합니다.
  + 'app.all()' : 클라이언트의 모든 요청을 처리합니다.
```javascript
//모듈 추출
let express = require('express');

//웹 서버 생성
let app = express();
app.use(express.static('public'));

//라우트. 각각의 경로로 들어갈 때 각각의 콜백함수를 실행합니다.
app.all('/a', function (request, response) {
  response.send('<h1>page a</h1>')
});
app.all('/b', function (request, response) {
  response.send('<h1>page b</h1>')
});
app.all('/c', function (request, response) {
  response.send('<h1>page c</h1>')
});
//웹 서버를 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```
  + 'http://127.0.0.1:52273/a' 로 접속하면 page a 가 출력됩니다.
