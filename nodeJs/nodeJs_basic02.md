# node.js 기본 02
## 라우터_응답과 응답 형식
- 서버에서 HTML이 아닌 다른 형식으로 데이터를 제공하기
  + '/data.html' : 데이터를 HTML 형식으로 제공합니다.
  + '/data.json' : 데이터를 JSON 형식으로 제공합니다.
  + '/data.xml' : 데이터를 XML 형식으로 제공합니다.
- 'send()' 메소드의 매개변수에 따른 응답 형식
  + 문자열 -> HTML
  + 배열 -> JSON
  + 객체 -> JSON
- 기본 서버 구성. 여기서 HTML, JSON, XML 응답으로 구분합니다.
```javascript
//모듈 추출
let express = require('express');

//변수 선언
let items = [{
  name: 'milk',
  price: '2000'
}, {
  name: 'tea',
  price: '5000'
}, {
  name: 'coffee',
  price: '5000'
}];

//웹 서버 생성
let app = express();
app.use(express.static('public'));

//라우트
app.all('/data.html' , function (request, response) {});
app.all('/data.json' , function (request, response) {});
app.all('/data.xml' , function (request, response) {});
//웹 서버 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```
### HTML 응답
- 문자열 output을 생성 후 send()메소드에 매개변수에 output을 넣습니다.
```javascript
app.all('/data.html' , function (request, response) {
  let output = '';
  output += '<!DOCTIPE html>';
  output += '<html>';
  output += '<head>';
  output += ' <title>Data HTML</title>';
  output += '</head>';
  output += '<body>';
  //배열의 각 요소에 작업을 수행하는 forEach() 메소드
  items.forEach(function (item) {
    output += '<div>';
    output += ' <h1>' + item.name + '</h1>';
    output += ' <h2>' + item.price + '</h1>';
    output += '</div>';
  });
  output += '</body>';
  output += '</html>';
  response.send(output);
});
```

### JSON 응답
- JSON 형식으로 응답할 경우에는 배열 자바스크립트 객체를 send() 메소드의 매개변수에 넣어야 합니다.
```javascript
app.all('/data.json' , function (request, response) {
  response.send(items);
});
```

### XML 응답
- 'send()' 메소드는 HTML과 JSON형식으로만 데이터를 제공합니다. 다른 형식으로 제공하려면 'response.type()' 메소드로 데이터 응답 형식을 지정해줘야 합니다.
```javascript
app.get('/data.xml', function (request, response) {
  let output = '';
  output += '<?xml version="1.0" encoding="UTF-8" ?>';
  output += '<products>';
  items.forEach(function (item) {
    output += '<product>';
    output += ' <name>' + item.name + '</name>';
    output += ' <price>' + item.price + '</price>';
    output += '</product>';
  });
  output += '</products>';
  //type() 메소드의 데이터 응답 형식을 xml로 지정합니다.
  response.type('text/xml');
  response.send(output);
});
```
  + Content-Type 속성이 text/xml으로 나옵니다.
- HTML, JSON처럼 일반적으로 send() 메소드를 써도 익스플로러에서는 일단 XML으로 출력됩니다.
  + 개발자 도구로 네트워크 통신 메시지를 조회하면 Content-Type 속성을 보면 데이터를 XML이 아니라 HTML로 제공했다고 나옵니다.
  + 대부분의 라이브러리는 요청을 완료한 후 Content-Type 속성을 확인해 어떤 형식의 데이터를 수신했는지 알아냅니다.
  + 따라서 형식을 맞춰주는 편이 좋습니다.

## 요청과 요청 매개변수
- 요청 매개변수 : 클라이언트가 서버로 전달하는 데이터
- 검색의 요청 매개변수 ( 키 : 값)
  + 'w' : tot
  + 'DA' : YZRR
  + 't_nil_searchbox' : byn
  + 'sug' : 빈 문자열
  + 'q' : html5

### 일반 요청 매개변수
- '키 = 값' 블록으로 데이터를 전달하는 방법입니다.
  + 일반 요청 매개변수를 추출할 때는 request.query 객체를 사용합니다.
```javascript
//app.all() 메소드로 /parameter 경로를 라우트합니다.
app.all('/parameter', function (request, response) {
  //변수 선언. 요청매개변수 name과 region을 추출해 문자열로 만듭니다.
  let name = request.param('name');
  let region = request.param('region');
  //응답
  response.send('<h1>' + name + ':' + region + '</h1>');
});
```

### 동적 라우트 요청 매개변수
- 동적 라우트 : 동적으로 변할 수 있는 부분을 처리하는 라우트입니다.
 + 경로가 변할 수 있는 부분을 지정해서 라우트합니다.
```javascript
//id를 동적 라우트합니다.
app.all('/parameter/:id', function (request, response) {
  //변수 선언
  let id = request.params.id;

  //응답
  response.send('<h1>' + id + '</h1>');
});
```

## 요청 방식
- 요청 방식
 + GET : 자원 조회  (예시 = GET 요청 : 127.0.0.1:52273/products)
 + POST : 자원 추가
 + PUT : 자원 수정
 + DELETE : 자원 삭제
 + HEAD : 자원의 메타 데이터 조회
 + OPTIONS : 사용 가능한 요청 방식 조회
 + TRACE : 테스트 목적의 데이터 조회
 + CONNECT : 연결 요청
- 위의 형식으로 구성되는 웹 서비스를 'RESTful 웹 서비스' 라고 합니다. 요청 방식들은 아래 메소드로 처리합니다.
  + app.get() : 클라이언트의 GET 요청을 처리합니다.
  + app.post() : 클라이언트의 POST 요청을 처리합니다.
  + app.put() : 클라이언트의 PUT 요청을 처리합니다.
  + app.del() : 클라이언트의 DELETE 요청을 처리합니다.
- GET 요청 이외의 요청에서 요청 매개변수를 추출하려면 'body parser' 매들웨어가 필요합니다.
- bodyParser 미들웨어 구성
```javascript
//모듈을 추출
let express = require('express');
let bodyParser = require('body-parser');

//변수 선언
let items = [{
  name: 'milk',
  price: '2000'
}, {
  name: 'tea',
  price: '5000'
}, {
  name: 'coffee',
  price: '5000'
}];

//웹 서버 생성
app.use(express.static('public'));
  //urlencoded() 메소드는 URL인코딩 요청으로 오는 데이터를 자동으로 분해하는 함수를 리턴합니다.
app.use(bodyParser.urlencoded({ extended: false}));

//웹 서버를 실행
app.listen(52273, function () {
  console.log('server running at http://127.0.0.1:52273');
});
```

### 데이터 조회 : GET
```javascript
app.get('/products', function (request, response) {
  response.send(items);
});
```
- 데이터를 하나만 제공하는 GET 요청
```javascript
app.get('/products/:id', function (request, response) {
  //변수 선언
    //request.param() 메소드로 추출하면 문자열이 나오므로 Number() 함수로 문자열을 숫자로 변환합니다.
  let id = Number(request.params.id);
  //응답
  response.send(items[id]);
});
```
  + 위처럼 데이터를 하나만 출력할 경우 아래와 같은 문제가 발생합니다.
    1. 클라이언트가 숫자가 아닌 값을 입력할 경우
    2. 숫자를 입력했는데 데이터가 없을 경우
```javascript
app.get('/products/:id', function (request, response) {
  //변수 선언
  let id = Number(request.params.id);
  //오류일 경우(잘못된 경로)
  if(isNaN(id)) {
    response.send({
      error: '숫자를 입력하시오'
    });
  } else if (items[id]) {
  //정상일 경우
    response.send(items[id]);
  } else {
  //오류일 경우(요소가 없을떄)
    response.send({
      error: '존재하지 않는 데이터'
    });
  }
});
```

### 데이터 추가 : POST
- GET 요청 이외의 요청 방식에서는 데이터가 주소에 나오지 않습니다.
  + 그래서 body parser 미들웨어로 별도로 전달되는 데이터를 분해합니다.
```javascript
app.post('/products', function (request, response) {
  //변수 선언
  let name = request.body.name;
  let price = request.body.price;
  let item = {
    name : name,
    price: price
  };
  //데이터 추가
  items.push(item);
  //응답
  response.send({
    message: '데이터를 추가했습니다',
    data: item
  });
});
```

### 데이터 수정 : PUT
- 요청 매개변수를 추출하고 데이터를 수정합니다.
  + params와 body를 이용해 추출합니다.
```javascript
app.put('/products/:id', function (request, response) {
  //변수 선언
  let id = Number(request.params.id);
  let name = request.body.name;
  let price = request.body.price;

  if (items[id]) {
    //데이터 수정
    if (name) { items[id].name = name; }
    if (price) { items[id].price = price; }

    //응답
    response.send({
      message: '데이터를 수정했습니다.',
      data: items[id]
    });
  } esle {
    //오류: 요소가 없을 때
    response.send({
      error: '존재하지 않는 데이터입니다.'
    });
  }
});
```

### 데이터 수정 : DEL
- id를 추출하고 이를 기반으로 데이터를 제거합니다.
```javascript
app.del('/products/:id', function (request, response) {
  //변수 선언
  let id = Number(request.params.id);

  if (isNaN(id)) {
    //오류: 잘못된 경로
    response.send({
      error: '숫자를 입력하세요.'
    });
  } else if (items[id]) {
    //정상일 경우: 데이터 삭제
    items.splice(id, 1);
    response.send({
      message: '데이터를 삭제했습니다.'
    });
  } else {
    //오류: 요소가 없을 때
    response.send({
      message: '존재하지 않는 데이터입니다.'
    });
  }
});
```

## 서버 정리
- 응답 형식에 따른 웹 서비스
  + 'All /data.html'
  + 'All /data.json'
  + 'All /data.xml'
- 요청 매개변수에 따른 웹 서비스
  + 'All /parameter'
  + 'All /parameter/:id'
- 요청 형식에 따른 웹 서비스
  + 'GET /products'
  + 'POST /products'
  + 'PUT /products/:id'
  + 'DELETE /products/:id'
  + 'GET /products/:id'
