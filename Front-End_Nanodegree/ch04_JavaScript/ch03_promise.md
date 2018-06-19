# JavaScript promise (Udacity)
## 프로미스
[구글 개발자 페이지](https://developers.google.com/web/fundamentals/primers/promises)
[모질라 개발자 페이지](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- 자바스크립트는 단일 스레드로 처리합니다. 하지만 여러 일을 처리해야 할 때 하나씩 처리한다면 속도가 느려지게 됩니다.
  + 콜백 함수나 이벤트를 사용해서 처리할 수 있지만 한계가 있습니다. (이벤트는 동일 객체에 여러 번 발생할 수 있는 작업에 좋습니다.)
- 프로미스 기본 :
  + 한 번만 성공 또는 실패할 수 있습니다. 두 번 성공 또는 실패할 수 없고, 성공을 실패로 또는 실패를 성공으로 바꿀 수도 없습니다.
  + 프로미스가 성공이나 실패한 후에 성공/실패 콜백을 추가하면 이벤트가 일찍 발생한 경우에도 올바른 콜백이 호출됩니다.
  + 비동기적인 성공/실패에 유용하게 사용할 수 있습니다.

### 용어
- 프로미스는 `fulfilled`, `rejected`, `pending`, `settled` 라는 용어가 있습니다.
- `fulfilled` : 프로미스 작업이 성공했을 때 사용됩니다.
- `rejected` : 프로미스 작업이 실패했을 때 사용됩니다.
- `pending` : 프로미스 작업 전이라서 성공, 실패 여부는 아직 모르는 상태입니다.
- `settled` : 프로미스 작업이 성공하거나 실패 둘 중 하나나 나온다는 용어입니다. 프로미스는 오직 한 번만 `settle` 합니다.

### 프로미스 타임라인
- 만약 이벤트 리스너를 이벤트가 실행된 후에 설정하면 아무 일도 생기지 않습니다. 그 이벤트가 다시 실행되지 않는다면 이벤트 리스너도 다시는 호출되지 않습니다.
- 이번에는 프로미스로 프로미스가 성공한 후에 그 성공한 값에 대한 액션을 설정한다면 어떤 일이 생길까요. 그 액션은 실행될 것입니다.
```javascript
new Promise(function (resolve, reject) {
  //프로미스는 오직 한번만 settle 하기에 여기서는 hi 만 실행합니다.
  resolve('hi');
  resolve('bye');
})
```
- 프로미스는 메인 스레드에서 실행됩니다. 그 뜻은 프로미스들은 여전히 잠재적으로 블로킹하고 있다는 것입니다.
  + 그래서 프로미스 생성 후 프로미스가 `settle` 하기까지의 시간은 블로킹되어 있고, 그 사이에 작업을 한다면 오랜 시간이 걸리게 될 것입니다.
  + 따라서 프로미스는 긴 시간이 걸리는 연산에 완벽하게 적합하진 않습니다. 프로미스는 단지 비동기적인 작업이 안정화 될 때 어떤 일이 발생할지를 결정합니다.
- 참고 : 프로미스가 유용한 경우
```javascript
/* ajax 요청을 처리하는 것이므로 프로미스에 적합합니다. */
var data = get('data.json');
data.onload = function () {
  analyze(this.responseText);
};
/* worker 는 다른 스레드에서 실행하고 데이터를 메인스레드에서 post 하는 방식입니다.
이 방식은 확실하게 비동기적이므로 프로미스에 적합합니다. */
var worker = new Worker('worker.js');
worker.postMessage(data);
worker.onmessage = doSomething;
```

### Syntax
- 프로미스는 생성자입니다. 변수에 저장할 수도 있고 단순히 new Promise() 로 선언할 수도 있습니다.
- 프로미스의 인수는 resolve, reject 두 개가 있습니다. 둘은 콜백으로 resolve() 나 reject() 를 실행하게 됩니다.
```javascript
new Promise(function resolve[, reject] {
  var value = doSomething();
  if (thingWorked) {
    resolve(value);
  } else if (somethingWentWrong) {
//인수 없이 실행합니다. 그러면 catch 의 rejectFunction 은  undefined 가 됩니다.
    reject();
  }
}).then(function (value) {
  //성공할 경우
  return nextThing(value);
}).catch(rejectFunction);
```
  + `resolve(value)`, `then(function (value) {})` 에서의 `value` 는 같은 값입니다.
  + `resolve` 는 다음에 `then` 을 이끌고, `reject` 는 다음에 `catch` 를 이끕니다.
  + 프로미스가 통과되면 프로미스를 처음으로 실행합니다.
  + `resolve()` 와 `reject()` 의 값은 `.then` 이나 `.catch` 에 전달되는게 아닙니다. `.then` 이나 `.catch` 가 호출한 함수에 전달됩니다.
```javascript
new Promise(function (resolve, reject) {
  //img 태그를 프로미스 안에다 wrapping 했습니다. 이미지를 불러온 후 작업을 합니다.
  var img = document.createElement('img');
  img.src = 'image.jpg';
  /*onload 핸들러로 프로미스의 성공을 입증합니다.
   onload 와 onerror 는 무엇이 fullfilment 고 rejection 인지 명시적으로 보여줍니다. */
  img.onload = resolve;
  img.onerror = reject;
  document.body.appendChild(img);
})
/*resolve -> appendChild -> .then() 을 실행합니다.
(onload 하자마자 바로 then 을 하지 않습니다. 자바스크립트는 실행을 갑자기 중단하지 않습니다.)*/
.then(finishLoading)
.catch(showAlternateImage);
```
  + resove 나 reject 가 호출된 후 프로미스는 settle 되고 체인에 따라 then 이나 catch 를 실행합니다.
- `resolve` 가 항상 성공을 의미하진 않습니다.

### 각종 도구들
- Service Woker :
[구글 개발자 블로그](https://developers.google.com/web/fundamentals/primers/service-workers/)
  + 오프라인 환경에서도 웹 애플리케이션을 개발하도록 도와줍니다. 현재 firefox 와 opera 브라우저에서 지원합니다.
- fetch API :
[모질라 개발자 블로그](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
  + 네트워크 통신을 포함한 리소스 취득을 위한 인터페이스가 정의되어 있습니다. XMLHttpRequest 보다 유연한 동작이 가능합니다.

## Exoplanet Explorer 으로 작업하기
- 설치하기 : [github 링크](https://github.com/udacity/exoplanet-explorer)
  + 그리고 node.js 와 npm, gulp, node.js 의 패키지인 bower 가 필요합니다.
  + gulp 과 bower 설치 : `npm install -g gulp bower`
  + gulp 와 bower 를 실행한 후 로컬 npm 과 bower 설치 : `cd exoplanet-explorer && npm install && bower install`
- `gulp serve` 를 실행해 테스트에 사용하거나 기기를 네트워크에 연결할 ip 주소를 알려줍니다.

## chaining 프로미스
### 에러 핸들링
```javascript
/*두 에러 처리는 같습니다. .catch 는 .then(undefined, ~) 의 줄임말입니다.
주로 ,catch 를 권장합니다. 더 간편하기 때문입니다.
세번쨰 방식과 달리 이 방식은 resolve 와 reject 함수를 같이 호출할 수 있습니다.*/
get('example.json')
.then(resolveFunction)
.catch(rejectFunction);

get('example.json')
.then(resolveFunction)
.then(undefined, rejectFunction);
/* then 함수의 실제 모습은 이렇습니다. 둘을 동시에 호출할 수는 없습니다.
.then 에서는 오직 하나만 호출됩니다. (혹은 아무것도 호출하지 않음)*/
get('example.json').then(resolveFunction, rejectFunction);
```

### series 와 parallel 요청
- 비동기 작업을 할 때는 다수의 비동기 액션을 수행해야 합니다. 그래서 chaining 단계일 때는 두 프로미스를 함께 묶습니다.
- series 는 동기 작업으로 일이 순차적으로 발생하고, parallel 은 작업들이 동시에 발생합니다.
  + 비동기적 코드는 series 에는 없을 것 같지만 series 와 parallel 둘 다 사용가능합니다.

### 배열 메소드와 프로미스
- 배열 메소드를 사용해서 프로미스들을 연결(chain) 할 수 있습니다. 다만 굳이 series 형태로 하지 않아도 괜찮습니다.
#### forEach
- `forEach` 로 프로미스 생성
```javascript
getJSON('../data/earth-like-results.json')
.then(function(response) {
  var sequence = Promise.resolve();
  response.results.forEach(function(url) {
//forEach 로 sequence.then 과 .then(createPlanetThumb) 가 반복됩니다.           
    sequence = sequence.then(function () {
      return getJSON(url)
    })
    .then(createPlanetThumb);
  });
})
.catch(function (e) {
  console.log(e);
});
```
  + 개발자 도구의 네트워크를 보면 이 코드는 series 로 발생합니다. (한 요청이 끝나면 다음 요청을 시작하는 형식)
```javascript
getJSON('../data/earth-like-results.json')
.then(function(response) {
  var sequence = Promise.resolve();
  response.results.forEach(function (url) {
    sequence.then(function () {
      return getJSON(url)
    })
    .then(createPlanetThumb);
  });
})
.catch(function (e) {
  console.log(e)
});
```
  + sequence 를 더하지 않고 두 .then 을 다음 반복 때 덮어씌웠습니다. 이 코드는 parallel 로 발생합니다.
  + 단점은 순서를 알 수 없다는 것입니다.
- 따라서 parallel 은 정말로 parallel 로 발생하는 요청일 경우에만 사용하는 것이 좋습니다.

#### map
- map 메소드로 프로미스 생성
