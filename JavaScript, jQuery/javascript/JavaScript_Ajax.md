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

## Async, Await (프로그래머스: 리액트로 웹 서비스 만들기 16장)
Async, Await 는 fetch API 를 더 쉽게 사용하도록 만들어줍니다.
- 기존의 fetch 로 프로미스를 작성하는 것도 콜백 지옥을 만들 가능성이 있습니다. 수행 완료하면 다음 기능을, 그걸 완료하면 또 다른 기능을...반복하는 것입니다. then, then, then 이 계속 반복되는 상황은 헷갈리게 만들 수 있습니다.
  + 참고 : 동기는 한 작업이 끝나야 다음 작업을 시작하는 형태입니다. 문제점은 전 작업이 끝나지 않으면 무한정 기다려야만 한다는 것입니다. 비동기는 전 작업이 끝나던 안 끝나던 상관없이 다음 작업을 수행합니다. 순서와 상관없이 진행합니다.
```javascript
//기존의 함수
componentDidMount() {
  fetch('https://ddd')
    .then(data => data.json())
    .then(json => json.data.movies)
    .catch(err => console.log(err))
}
```
```javascript
//Async, Await 를 위해 만든 새로운 함수들. componentDidMount 에다 함수를 실행하기만 하면 됩니다.
componentDidMount () {
  this._getMovies();
}
async _getMovies = () => {
  const movies = await this._callApi();
  this.setState({
  //최신 자바스크립트의 속기를 사용했습니다. movies: movies 로 할 필요가 없어졌습니다.
    movies
  })
}
_callApi = () => {
  return fetch('https://ddd')
    .then(data => data.json())
    .then(json => json.data.movies)
    .catch(err => console.log(err))
}
```
- await : 여기서의 await 는 그 뒤에 선언한 함수가 끝나는 것을 기다리고(성공적으로 수행이 아니라 끝나기를), 그 값이 무엇이든 간에 그것을 변수에 저장한 것입니다.
`this.setState`는 `await this._callApi` 작업이 끝나기 전까지 실행되지 않습니다.
