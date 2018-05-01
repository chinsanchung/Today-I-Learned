# XMLHttpRequest
## 기본 개요
- 'XMLHttpRequest' : 자바스크립트가 Ajax로 HTML 페이지에서 해당 웹 서비스에 접근해 데이터를 가져올 때 사용합니다.
- 객체 생성 : 'let request = new XMLHttpRequest();'
- 객체 생성 후 open() 메소드로 전송 위치와 방식을 지정합니다.
  + 'request.open(<전송 방식>, <경로>, <비동기 사용여부(ture/false)>)'
- send() 메소드로 전송 후 responseText 속성으로 응답을 출력합니다.
```javascript
let request = new XMLHttpRequest();
request.open('GET', '/data.html', false);

request.send();

alert(request.responseText);
//data.html이므로 페이지에 출력시킬 수 있음
document.body.innerHTML += request.responseText;
```

## 동기, 비동기 방식
### 동기 방식
- 동기 방식 : XMLHttpRequest 객체를 전송한 후 그 답이 올 떄까지 기다리는 방식입니다.
```javascript
let request = new XMLHttpRequest();
request.open('GET', '/data.html', false);

//send() 메소드에 소비되는 시간 측정
let prevDate = new Date();
request.send();
let nowDate = net Date();

alert(nowDate - prevDate);
```
### 비동기 방식
- 비동기 방식 : XMLHttpRequest 객체를 전송한 후 그 답이 올 때까지 다른 일을 처리합니다.
  + open() 메소드의 비동기 사용여부를 true로 지정합니다.
  + 'onreadystatechange' 이벤트 : XMLHttpRequest의 상태가 변경될 때마다 이벤트를 호출합니다. 데이터가 전송된 것을 알아낼 수 있습니다.
```javascript
let request = new XMLHttpRequest();
request.onreadystatechange = function (event) {
  alert(request.readyState);
};
request.open('GET', '/data.html', true);
request.send();
```
  + 실행 시 경고창에 readyState 속성의 숫자를 출력합니다. 0부터 4까지 있으며 4가 모든 데이터를 받은 상태입니다.
```javascript
let request = new XMLHttpRequest();
request.onreadystatechange = function (event) {
  if (request.readyState == 4) {
    if (request.status == 200) {
      document.ready.innerHTML += request.responseText;  
    };
  };
};
request.open('GET', '/data.html', true);
request.send();
```
  + XMLHttpRequest 객체의 status 속성은 100, 200, 300, 400번대가 있으며 200번이 성공입니다.

## 데이터 요청과 조작
### JSON
- XMLHttpRequest 객체로 Ajax 요청을 수행하고 JSON을 자바스크립트 객체로 변환합니다.
  + 'eval()' 함수로 JSON을 자바스크립트 객체로 변환할 수 있습니다.
```javascript
request.onreadystatechange = function (event) {
  if (request.readyState == 4) {
    if (request.status == 200) {
//responseText 속성을 eval() 함수의 매개변수에 직접 넣지 않은 이유는 가끔 발생하는 문제를 막기 위해서입니다.
      let json = eval('(' + request.responseText + ')');
      let output = '';
      for (let i = 0; i < json.length; i++) {
        for (let key in json[i]) {
          output += '<h1>' + i + ':' + json[i][key] + '</h1>';
        }
      }
      //출력
      document.body.innerHTML += output;
    };
  };
};
```
  + 개인적으로 출력 시 127.0.0.1에 304 오류가 뜨는데 이유를 찾지 못했습니다. (Not modified) Postman으로 살펴볼 시 Content-type가 html이었습니다.

### XML
```javascript
let request = new XMLHttpRequest();
request.onreadystatechange = function (event) {
  if (request.readyState == 4) {
    if (request.status == 200) {
      alert(request.responseXML);
    };
  };
};
//객체의 URL을 /data.xml으로 설정합니다.
request.open('GET', '/data.xml', true);
request.send();
```
- XML 문서는 문서 객체 모델로 나타낼 수 있어 그 관련 속성을 사용할 수 있습니다.
  + 'nodeValue' : 문서 객체의 내부 글자
  + 'attributes' : 문서 객체의 속성
- 또한 문서 객체 모델의 메소드도 사용 가능합니다.
  + 'getElementById(id)' : id 속성이 일치하는 문서 객체를 선택합니다.
  + 'getElementByTagName(name)' : 태그 이름이 일치하는 문서 객체를 선택합니다. XML문서는 id 속성을 쓰지 않아 이 메소드를 많이 씁니다.
```javascript
if (request.status == 200) {
  let xml = request.responseXML;
  //데이터를 가공합니다.
  let names = xml.getElementByTagName('name');
  let prices = xml.getElementsByTagName('price');
  //반복문으로 데이터를 추출합니다.
  for (let i = 0; i < naems.length; i++) {
    let name = names[i].childNodes[0].nodeValue;
    let price = prices[i].childNodes[0].nodeValue;
    document.body.innerHTML += '<h1>' + name + '</h1>';
    document.body.innerHTML += '<h1>' + price + '</h1>';
  }
}
```

## 데이터 요청 방식
```javascript
window.onload = function () {
  document.getElementById('get').onclick = function () {
    //Ajax 수행
    let request = new XMLHttpRequest();
    request.open('GET', '/products', true);
    request.send();
    request.onreadystatechange = function (event) {
      if (request.readyState == 4) {
        if (request.status == 200) {
          document.getElementById('output').value = request.responseText;
        };
      };
    };
  };

  document.getElementById('post').onclick = function () {
    //변수 선언
    let name = document.getElementById('name').value;
    let price = document.getElementById('price').value;

    let request = new XMLHttpRequest();
    request.open('POST', '/products', true);
    //setRequestHeader() 메소드로 Content-type 속성을 설정하고 send()메소드에 넣어 전달합니다.
    request.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    request.send('name=' + name + '&price=' + price);
    request.onreadystatechange = function (event) {
      if (request.readyState == 4) {
        if (request.status == 200) {
          document.getElementById('output').value = request.responseText;
        };
      };
    };
  };

  document.getElementById('put').onclick = function () {
    //변수 선언
    let name = document.getElementById('name').value;
    let price = document.getElementById('price').value;
    let request = new XMLHttpRequest();
    request.open('PUT', '/products/0', true);
    request.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    request.send('name=' + name + '&price=' + price);
    request.onreadystatechange = function (event) {
      if (request.readyState == 4) {
        if (request.status == 200) {
          document.getElementById('output').value = request.responseText;
        };
      };
    };
  };

  document.getElementById('delete').onclick = function () {
    let request = new XMLHttpRequest();
    request.open('DELETE', '/products/0', true);
    request.send();
    request.onreadystatechange = function(event) {
      if (request.readyState == 4) {
        if (request.status == 200) {
          document.getElementById('output').value = request.responseText;
        };
      };
    };
  };
}
```

## 구버전의 웹 브라우저에서 XMLHttpRequest가 제대로 동작하지 않을 경우
- 'createRequest()' 함수로 XMLHttpRequest 객체를 생성합니다.
```javascript
function createRequest() {
  try {
    return new XMLHttpRequest();
  } catch(exception) {
    let versions = [
      'Msxml2.XMLHTTP.6.0',
      'Msxml2.XMLHTTP.5.0',
      'Msxml2.XMLHTTP.4.0',
      'Msxml2.XMLHTTP.3.0',
      'Msxml2.XMLHTTP',
      'Microsoft.XMLHttp',
    ];
    for (let i = 0; i < versions.length; i++;) {
      try {
        return new ActivXObject(versions[i]);
      } catch (e) {}
    }
  }
}
```
