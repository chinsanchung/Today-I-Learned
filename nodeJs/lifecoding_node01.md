# 생활코딩 node.js
### 5강 웹서버 만들기
```javascript
response.end(fs.readFileSync(__dirname + url));
```
- 사용자가 버튼을 클릭해서 요청(접근)할 때마다 위의 코드로 읽어들여야할 파일을 만듭니다.(read 읽다 + fs 파일로부터 + dirname url 경로를) 그리고 이 작업을 `response.end` 의 괄호 안에 넣었습니다.
즉 node 는 어떤 코드를 `response.end` 안에 넣는가에 따라 사용자에게 보낼 데이터를 생성하게 됩니다.


### 12
Create Read Update Delete
파일 호출하기
```javascript
//fileread.js
var fs = require('fs');
fs.readFile('sample.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```
### 13 파일로 본문 구현
```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var app = http.createServer(function(request,response){
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var title = queryData.id;
    if(_url == '/'){
      title = 'Welcome'
    }
    if(_url == '/favicon.ico'){
      response.writeHead(404);
      response.end();
      return;
    }
    response.writeHead(200);
    fs.readFile(`data/${queryData.id}`, 'utf8', (err, data) => {
      var template = `
      <!doctype html>
      <html>
      <head>
      <title>WEB1 - ${title}</title>
      <meta charset="utf-8">
      </head>
      <body>
      <h1><a href="/">WEB</a></h1>
      <ul>
      <li><a href="/?id=HTML">HTML</a></li>
      <li><a href="/?id=CSS">CSS</a></li>
      <li><a href="/?id=JavaScript">JavaScript</a></li>
      </ul>
      <h2>${title}</h2>
      <p>${data}</p>
      </body>
      </html>
      `;
      response.end(template);
    })
});
app.listen(3000);
```
### 18 콘솔 입력값
```javascript
var args = process.argv;
if(args[2] === 'aaa') {console.log('2')};
console.log(args);
//node filename.js aaa
//2
//['폴더위치', '파일위치', 'aaa']
```
