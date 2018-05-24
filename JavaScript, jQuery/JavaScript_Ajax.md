# JavaScript Ajax
- `Ajax` : Asynchronous JavaScript and XML 의 약자입니다.
  + 자바스크립트를 사용해서 비동기적으로 서버와 브라우저가 데이터를 주고 받는 방식입니다.
## XMLHttpRequest
- `Ajax` 는 `XMLHttpRequest` API 를 사용해서 데이터를 주고 받습니다.
```javascript
document.querySelector('input').addEventListner('click', function (event) {
//XMLHttpRequest 객체를 생성합니다.
  let xhr = new XMLHttpRequest();
//접속하려는 대상을 지정합니다. GET, POST 중 하나를 택하고 접속하고자 하는 서버 쪽 리소스의 주소를 적습니다.
  xhr.open('GET', './time.php');
//onreadystatechange 이벤트는 서버와의 통신이 끝났을 때 호출되는 이벤트입니다.
  xhr.onreadystatechange = function () {
//readyState 는 통신의 현 상태를 알려줍니다. 4는 완료됐음을 의미합니다.
//status 는 HTTP 통신의 결고로 200 이 성공했음을 의미합니다.
    if (xhr.readyState === 4 && xhr.status === 200) {
//responseText 프로퍼티는 서버에서 전송한 데이터를 담고 있습니다.
      document.querySelector('#time').innerHTML = xhr.responseText;
    }
  }
  xhr.send();
});

//POST 방식
document.querySelector('input').addEventListner('click', function (event) {
  let xhr2 = new XMLHttpRequest();
  xhr2.open('POST', './time2.php');
  xhr2.onreadystatechange = function () {
    document.querySelector('#time').innerHTML = xhr2.responseText;
  }
//서버로 전송할 데이터 타입의 형식을 지정합니다.
  xhr2.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
//서버로 전송할 데이터를 형식에 맞게 작성합니다.  이름=값&이름=값...
  let data = '';
  data += 'timezone=' + document.getElementById('timezone').value;
  data += '&format=' + document.getElementById('format').value;
//send 메소드로 데이터를 전송합니다.
  xhr2.send(data);
});
```
