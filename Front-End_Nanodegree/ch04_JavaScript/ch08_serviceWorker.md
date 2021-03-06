# Service Worker
- [구글 개발자 블로그: 서비스 워커 소개](https://developers.google.com/web/fundamentals/primers/service-workers/?hl=ko)
## 오프라인에서의 실행
- 모든 데이터를 네트워크로 받는 방식은 단점이 있습니다. 통신 연결이 좋지 않거나, 주변에 사람이 너무 많거나, 서버에 버그가 있다던가 등등 의 이유로 네트워크에서 데이터를 받지 못하고 하염없이 기다려야만 할 수도 있습니다.
- 그래서 오프라인과 네트워크를 병행하면 서버에 부담도 적게 들고 통신환경이 좋지 않더라도 앱에 접근하게 만들어줍니다.
## witter 데모 앱 체험하기
- [witter 깃허브](https://github.com/jakearchibald/wittr) 에서 클론이나 다운을 받고 `npm install` 다음에 `npm run serve` 를 실행합니다.
- 현재 주소는 http://localhost:8888/ 로 실행하고, http://localhost:8889/ 를 들어가면 연결상태를 조절할 수 있습니다.
- witter 의 연결 방식
  + witter 로 이동하면 브라우저는 HTML 을 요청합니다. 모든 웹 요청처럼 witter 의 요청은 브라우저 HTTP 캐시를 통해서 이동합니다.
  + 만약 HTTP 캐시에서 맞는게 없다면 인터넷으로 이동합니다. 만약 거기서 응답이 있다면, 응답을 다시 브라우저로 보냅니다.
  + 브라우저가 받은 HTML 은 다음으로 CSS 를 가져오라고 요청합니다.
  + CSS 를 받고, 처음으로 모든 콘텐츠를 가진 페이지를 처음으로 render 합니다. 여기서 HTML 로 게시물의 초기 세트를 보게 됩니다.
  + 그와 동시에 브라우저는 CSS 를 다운받는데 이것 또한 자바스크립트로 요청합니다.
  + 그리고 그것을 받을 때 웹소켓이 열립니다. 웹소켓은 영구적인 연결으로 서버는 새로운 게시물이 도착할 때 지속적으로 stream 을 보냅니다.
  웹소켓은 라이브 업데이트를 제공해 사용자가 다른 사람이 올린 게시물을 놓치지 않도록 합니다.

## Service Worker 간략한 소개
- [구글 개발자 블로그](https://developers.google.com/web/fundamentals/primers/service-workers/?hl=ko)
- Service worker 는 브라우저와 네트워크 요청 사이에 있는 자바스크립트 파일입니다.
- web worker 처럼 웹 페이지와는 별개로 작동합니다. 사용자에게 보이지 않습니다. 자바스크립트 Worker 이므로 DOM에 액세스 할 수 없지만 페이지를 제어하고 컨트롤(DOM 조작)을 사용합니다.
- 그 뜻은 브라우저가 만든 요청을 가로채서 HTTP 캐시, 인터넷에 전송하고, 전달받은 응답을 다른 캐시에 보내거나 새로운 커스텀 랜덤 응답을 만들어서 브라우저에 전달하거나 둘을 합쳐서 진행할 수도 있습니다.
- 서비스 워커는 `navigator.serviceWorker.register('/sw.js')` 형식으로 등록합니다.  이것은 프로미스를 리턴합니다. 그래서 성공이나 실패에 대한 콜백을 얻을 수 있습니다.
```javascript
navigator.serviceWorker.register('/sw.js').then(function (reg) {
  console.log('success');
}).catch(function (err) {
  console.log('error');
});
```
- 만약 서비스 워커를 이미 등록했는데도 register 를 호출하더라도 문제는 없습니다. 브라우저는 다시 등록하지는 않을 겁니다. 단지 이미 존재하는 등록에 대한 프로미스를 return 할 뿐입니다.
- 또한 scope 를 제공할 수 있습니다. 서비스 워커는 scope 로 시작하는 URL 이 있는 모든 페이지를 제어하고 그렇지 않은 페이지는 무시합니다.
  + 예를 들어 서비스 워커는 `/my-app/` , `/my-app/hello/world/`(깊은 URL) 은 실행합니다.
  + 하지만 `/` (아무것도 없음, 얕은 URL) 은 서비스 워커가 제어하지 못합니다. 또한 `/another-app/` 같은 관련없은 URL 도 제어 못합니다. 그리고 `/my-app` URL 도 제어 못합니다. 왜냐면 / 가 없기 때문입니다. (얕은 URL 로 칩니다.)
- 서로 다른 scope 를 가진 다양한 서비스 워커를 사용할 수 있습니다. 여러 프로젝트가 동일한 출처를 공유 할 때 github 페이지와 같은 기능을 하는 경우일 때 다양한 서비스 워커를 쓰면 매우 편리합니다.
- scope 는 각 프로젝트에 다른 서비스 워커를 가지게 해줍니다.
  + 기본 scope 는 서비스 워커 스크립트의 위치에 따라 결정됩니다. 기본적으로 스크립트가 있는 경로에 따릅니다.(URL : /foo/sw.js -> 기본 scope : /foo/)(URL : /foo/bar/sw.js -> 기본 scope : /foo/bar)
  + URL 이 `/sw.js` 일 떄는 기본 scope 를 `/` 으로 할 수도 있습니다.
  + 예를 들어 scope 가 `/foo/` 일 때 서비스 워커는 /foo/ /foo/bar/index/html /foo/bar 를 제어할 수 있습니다.
- 서비스 워커는 모든 브라우저에 지원하는 건 아닙니다. (지원여부 확인)[https://jakearchibald.github.io/isserviceworkerready/]
- 서비스 워커를 안전하고 점진적인 방법으로 향상시키려면 아래의 기능을 감지하는 코드를 작성합니다.
```javascript
if (navigator.serviceWorker) {
  navigator.serviceWorker.register('/sw.js');
}
```
  + 만약 브라우저가 서비스 워커를 지원하지 않으면, `navigator.serviceWorker` 는 undefined 됩니다. 오래된 브라우저는 undefined 인 것을 실행하지 않을 것입니다.

## witter 에 서비스 워커 설치하기
- public/js/sw/index.js 가 서비스 워커 스크립트입니다. 여기에 코드를 작성합니다.
  + 서비스 워커는 이벤트를 받기 때문에 fetch 리스너를 작성해봅니다.
```javascript
self.addEventListener('fetch', function (event) {
//해당 객체에 어떤 정보가 있는지 알아봅니다.
  console.log(event.request);
});
```
- 사용자가 서비스 워커 scope 를 통해 페이지로 이동할 때 이것을 제어합니다. HTML 의 네트워커 요청은 서비스 워커로 이동하고 fetch 이벤트를 작동(trigger)시킵니다.
  + 그것 뿐만 아니라 해당 페이지에 의해 작동된 모든 요청에 대한 fetch 이벤트를 얻게 됩니다. 또한 다른 출처에서의 CSS 나 자바스크립트, 이미지 요청이더라도 각각에 대해 fetch 이벤트를 얻습니다.
  + 그리고 그것들을 자바스크립트로 검사할 수 있습니다.
- 설치가 완료되면 활성화 단계가 진행되고, 여기서 오래된 캐시를 관리할 수 있게 됩니다.
- 활성화 단계 후에는 해당 범위 안의 모든 페이지를 제어합니다.
  + 다만 처음 등록한 페이지는 다시 로드해야만 제어가 가능합니다.
### 서비스 워커 등록하기
- 우선 프로젝트를 샘플 상태로 만들어야 합니다. `git reset --hard `, `git checkout task-register-sw`
- 그 다음 public/js/main/indexController.js 로 이동합니다.
  + 참고로 자바스크립트는 private 메소드는 없지만, _ 를 메소드 앞에 쓰면 해당 객체의 다른 메소드에 의해서만 호출됩니다. (예 : `this._openSocket();`)
- 서비스 워커는 URL에 관계없이 제어 된 페이지에 의해 만들어진 요청을 거의 차단합니다. 그로 인해 요청을 헷갈리거나 헤더를 바꾸거나 다른 응답을 해버릴 수도 있습니다.
- 그래서 서비스 워커는 오직 HTTPS 에서만 작동합니다.(안전한 폼의 HTTP)
  + 서비스 워커는 페이지보다 오래 지속됩니다. 그래서 해커같은 사람들이 중간에 조작할 수도 있습니다.
- 구글 블로그에서 나온 등록 코드입니다.
```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      console.log('서비스 워커 등록 성공: ', registration.scope);
    }).catch(function(err) {
      console.log('서비스 워커 등록 실패: ', err)
    });
  });
}
```
- 그리고 구글 블로그에서 나온 install 이벤트를 처리하는 서비스 워커 스크립트입니다.
```javascript
self.addEventListener('install', function(ev) {
  ev.waitUntil(
    caches.open(CACHE_NAME)
      .then(function(cache) {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
});
```
- 설치 후에 다른 페이지로 이동하거나 새로고침을 하면 서비스 워커는 fetch 이벤트를 수신합니다.

### 서비스 워커의 라이프사이클
- 페이지를 열 준비가 되고 서비스 워커 등록 코드를 더했습니다. 그 다음 새로고침을 해봅니다.
- 새로고침을 누르면 새로운 윈도우 클라이언트가 생기고 요청은 네트워크로 전달되고 다시 응답이 돌아옵니다. 그 다음 오래된 윈도우 클라이언트는 사라집니다.
- 서비스 워커는 오직 로드됐을 때 페이지 제어만 하기 때문에 요청 로그를 페이지에서 볼 수 없습니다.
  + 현재 이 페이지는 서비스 워커가 존재하기 전에 로드돼서, 이 페이지가 만드는 추가 요청은 서비스 워커를 우회합니다.
  + 하지만 새로고침으로 새 윈도우 클라이언트를 만들면 그 때는 서비스 워커가 있으므로 서비스 워커는 새 윈도우를 제어합니다.
  + 그것이 새로고침을 두 번 해도 저장된 요청이 나오는 이유입니다.
- 만약 서비스 워커를 과거의 요청 코드가 있는데도 바꾼다면 새로운 서비스 워커가 생성됩니다. 하지만 새로운 서비스 워커는 응답을 받아 기다리기만 하고 페이지를 제어하진 않습니다.
  + 새로고침은 새 버전으로 바꿔주지 않습니다. 창을 닫거나 서비스 워커가 제어하지 않은 새 페이지로 이동해야 합니다.

### 크롬 개발자 도구와 서비스 워커
- 크롬에서 Application 을 들어가 서비스 워커를 선택하면 Source, Status 를 알 수 있습니다. Status 는 서비스 워커의 상태를 보여주는데 만약 서비스 워커가 있는 파일(index.js) 을 수정하고 새로고침하면 waiting to active 라는 메시지가 뜹니다. 수정한 서비스 워커가 대기하고 있다는 뜻입니다.
  + 만약 새로고침이 아니라 서비스 워커가 없는 다른 페이지로 이동하거나 브라우저를 닫았다 열면 waiting to active 란 메시지 없이 그저 active 상태만 보이게 될 것입니다.
- 같은 도구의 상단에 Update on reload 를 하면 새로고침이나 탭을 닫지 않아도 바로 결과가 반영됩니다.

## 하이재킹 요청

```javascript
self.addEventListener('fetch', function(event) {
	// TODO: respond to all requests with an html response
	// containing an element with class="a-winner-is-me".
	// Ensure the Content-Type of the response is "text/html"
  event.respondWith(
    new Response('<b class="a-winner-is-me">Hello</b>', {
      headers: {'Content-Type': 'text/html'}
    })
  );
});
```
- `event.respondWith()` 은 브라우저에게 이 요청을 스스로 처리할 거라고 알려줍니다.
  + 이벤트나 respondWith 은 응답 객체나 응답으로 해결되는 프로미스를 가집니다.
- 응답을 만드는 방법 중 하나는 `new Response()` 입니다.
  + 첫 번째 인수로 응답의 body 를 가집니다. blob, buffer, 다른 것들이 될 수 있지만 가장 간단하게 문자열을 인수로 취합니다.
    + 새로고침하면 직접 만든 응답을 얻게 됩니다.(Hello world)
    + url 을 아무렇게나 쳐도 결과는 같습니다. 모든 fetch 를 가로채기 때문입니다.
  + 두 번째 인수는 객체입니다. 위에서 입력한 프로퍼티가 Network 개발자 도구의 Response Headers 에 나타납니다. 예를 들어 위의 코드대로 작성하면 첫 번쨰 인수의 HTML 태그가 제대로 반영되어 나타나게 만들어줍니다.
- 참고로 `New Response()` 의 마지막에 세미콜론을 넣어선 안됩니다. 코드들이 미완성이 되기 때문입니다.
- 하이재킹한 응답은 lie-fi 나 offline 이 되더라도 작동합니다. 네트워크는 해당 콘텐츠를 제공할 때 완전히 바뀌지 않습니다.

### 하이재킹 요청 02 - fetch
- fetch(url) 은 네트워크 요청을 만들고 응답을 읽을 수 있게 만들어줍니다.
- 현재 서비스 워커의 `event.respondWith` 은 응답 혹은 응답을 확인하는 프로미스를 가집니다.
  + 반면 `fetch` 는 응답을 확인하는 프로미스를 return 합니다. 그래서 `event.respondWith` 과 fetch 를 같이 post 할 수 있습니다.
```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch('/imgs/dr-evil.gif')
  );
});
```
  + 출력 결과 다른 콘텐츠를 네트워크를 사용해 전달했습니다.
- fetch API 는 normal browser fetch 를 수행합니다.
  + 그래서 결과는 캐시에서부터 옵니다.
- 퀴즈 : 끝이 jpg 인 url 요청만 응답하기
```javascript
self.addEventListener('fetch', function(event) {
  // TODO: only respond to requests with a url ending in ".jpg"
  if(event.request.url.endsWith('.jpg')) {
    event.respondWith(
      fetch('/imgs/dr-evil.gif')
    );
  }
});
```
  + `endsWith()` 메소드는 유용한 문자열 메소드입니다.

### 하이재킹 요청 03
- 요청에 대한 네트워크 가져 오기로 응답해봅니다. 브라우저가 요청한대로 자체 장치에 남겨두면됩니다.
- fetch 메소드는 URL 같은 요청 객체를 가지고 프로미스를 return 합니다.
  + 프로미스로 .then 콜백에 접근해 성공, 실패 결과를 얻습니다. 이 콜백은 프로미스에 대한 이벤트 값이 됩니다.
  + fetch 는 서버의 연결을 만들 수 없으면 실패하게 됩니다.
```javascript
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request).then(function(response) {
      if (response.status == 404) {
        return new Response("not found");
      }
      return response;
    }).catch(function() {
      return new Response("failed all");
    })
  );
});
```

### 캐싱과 에셋 전달
- 만약 Witter 를 네트워크 없이 사용하려면 HTML, CSS, 자바스크립트, 이미지, 폰트를 저장할 어딘가가 필요합니다.
- `cache API` 를 사용하면 가능합니다. cache API 는 전역의 캐시 객체를 제공합니다.
  + 캐시를 만들거나 열고 싶으면 `cache.open('캐시이름').then(function(cache) {});` 을 실행합니다.
  + 그것은 캐시의 이름에 대한 프로미스를 return 합니다. 만약 이전에 그 이름의 캐시를 열지 않았다면 캐시를 생성하고 return 합니다.
- 캐시 박스는 안전한 출처(origin) 으로부터의 요청과 응답을 가지고 있습니다.
  + 여기서 폰트, 스크립트, 이미지 등을 자신의 출처뿐만 아니라 웹의 다른 곳에서도 저장할 수 있습니다.
  + `cache.put(request, response)` 으로 캐시 아이템을 추가하고 요청, URL 및 응답을 전달할 수 있습니다. 또는 `cache.addAll([])` 을 써도 됩니다.
```javascript
/*요청이나 URL 을 받아서 전달하고, 요청-응답 묶음을 캐시에 넣습니다.
만약 배열 중 어느 하나라도 틀리면 아무것도 추가되지 않습니다.
addAll 은 fetch 를 사용합니다. 그러니 요청은 브라우저 캐시를 통해 전달됩니다.*/
cache.addAll([
  '/foo',
  '/bar'
]);
```
- 나중에 캐시에서 원하는 걸 꺼내려면 `cache.match(request)` 를 호출합니다. 이것은 요청이나 URL 을 전달합니다.
  + 만약 매칭된 응답을 찾으면 프로미스를 return 하고 그렇지 않으면 null 을 return 합니다.
- `caches.match(request)` 도 비슷하지만, 이것은 가장 오래전부터 시작해 맞는 캐시를 찾아내려 합니다.
- 브라우저가 처음 서비스 워커를 실행하면 이벤트가 열리고 설치 이벤트가 나타납니다.
  + 설치 이벤트로 만든 새 서비스 워커는 설치가 완성되기 전까진 브라우저는 페이지를 제어할 수 없고 관련 작업만 제어할 수 있습니다.
  + 이것을 이용해 모든 것을 네트워크로부터 얻어내고 그것들을 저장할 캐시를 만들어봅니다.
  + `event.waitUntil()` : 설치 진행 상황을 알려줍니다. 프로미스가 resolve 면
  브라우저는 설치가 다 됐다는 걸 알게 됩니다.
  아니라면 서비스 워커는 버려집니다.
```javascript
//index.js 캐시에 url 넣기 퀴즈
self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('wittr-static-v1').then(function(cache) {
      return cache.addAll([
        '/',
        'js/main.js',
        'css/main.css',
        'imgs/icon.png',
        'https://fonts.gstatic.com/s/roboto/v15/2UX7WLTfW3W8TclTUvlFyQ.woff',
        'https://fonts.gstatic.com/s/roboto/v15/d-6IYplOFocCacKzxwXSOD8E0i7KZn-EPnyo3HZu7kw.woff'
      ]);
    })
  );
});

self.addEventListener('fetch', function(event) {
  // TODO: respond with an entry from the cache if there is one.
  // If there isn't, fetch from the network.
  event.respondWith(
    caches.match(event.request).then(function(response) {
      if (response) return response;
      return fetch(event.request);
    })
  );
});
```

## 정적 캐시 업데이트
- Application 에서 Update on reload 를 지우고 실제 사용자가 접하는 방식으로 바꿉니다. 그러고 나서 `/public/scss/_theme.scss` 를 바꿔봅니다.
  + 그리고 새로고침을 하면 전혀 바뀌질 않은걸 보게 됩니다. Shift+refresh 로 서비스 워커를 bypass 해야 바뀐걸 볼 수 있습니다.
  + 왜냐면 캐시가 아직도 엣날 CSS 를 가지고 있어서입니다.
- 캐시는 index.js 의 addEventListener('install') 으로 업데이트됩니다.
  + 하지만 현재 작성한 것에는 설치할 새로운 서비스 워커가 없습니다. 그래서 실행되질 않습니다.
  + 위에서 서비스 워커를 바꾸지 않고 CSS 만 바꿨기 때문에 새로 설치해야 할 것이 없습니다.
- CSS 도 업데이트하려면 서비스 워커도 바꿔야 할 필요가 있습니다. 그러면 그 업데이트된 워커를 브라우저는 새 서비스 워커로 인식할 것입니다.
  + 새 서비스 워커는 자바스크립트, HTML, 업데이트된 CSS 를 가져와 새 캐시에 넣는 인스톨 이벤트를 발생시킵니다.
  + 그 캐시는 자동으로 생기지 않고 직접 캐시의 이름을 바꿔서 만들어야합니다. 옛 캐시를 굳이 건드릴 필요는 없습니다. (옛 서비스 워커의 정보가 담겨있습니다.)
  + 옛 서비스 워커가 출시되고 인계받을 준비가 되면 이전 캐시를 삭제하고 다음 페이지 로드가 새 캐시에서 리소스(최신 CSS)를 가져옵니다.
- `self.addEventListener('activate', function(event) {})` activate 이벤트는 서비스 워커가 active 일 때(페이지를 제어하고 옛 서비스 워커가 없어질 때입니다) 발생합니다. 이것으로 옛 캐시를 없앨 수 있습니다.
  + 인스톨 이벤트처럼 `event.waitUntil` 을 사용해 프로세스의 길이를 알립니다.
  + activate 이면, 브라우저는 fetch 같은 다른 서비스 워커 이벤트를 대기시킵니다. 서비스 워커는 첫 fetch 를 받을 때까지는 캐시를 가지고 있습니다.
- `caches.delete(cacheName)` 으로 캐시를 지웁니다. 프로미스를 return 합니다.
- `caches.keys()` 로 모든 캐시의 이름을 가져옵니다. 프로미스를 return 합니다.
- 참고로 good caching 으로 일하면 편합니다. 밑의 코드에서 CSS 를 업데이트하면 CSS URL 이 바뀌고 캐시 이름도 바뀝니다.
```javascript
var staticCacheName = 'witter-static-8fe4b8f';
self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open(staticCacheName).then(function(cache) {
      return cache.addAll([
        '/',
        'js/main-a4929ce.js',
        'css/main-2fee7ea.css',
        'imgs/icon-5012ac1.png' //등등
      ]);
    })
  );
});
```

### UX 를 프로세스에 추가하기
- 서비스 워커의 업데이트를 알리는 업데이트 알림 API 가 서비스 워커에 내장되어있습니다.
```javascript
navigator.serviceWorker.register('/sw.js').then(function(reg) {
  //메소드들
  reg.unregister();
  reg.update();
  //프로퍼티
  reg.installing;
  reg.waiting;
  reg.active;
  //registration 객체가 새 업데이트를 찾으면 updatefound 이벤트를 발생시킵니다.
  reg.addEventListener('updatefound' function() {
    //reg.installing has changed
  });
});
var sw = reg.installing;
console.log(sw.state);
/*결과창의 용어 정리
"installing"
"installed"
"activating" : activate 이벤트 시작, 완료는 아님
"activated" : 서비스 워커는 fetch 이벤트를 받을 준비가 됨
"redundant" : 서비스 워커가 새 워커로 대체되고, 설치가 실패하더라도 중복이 발생함
*/
sw.addEventListener('statechange', function() {
  //sw.state has changed
})
//controller 가 없다 : 서비스 워커를 사용해 페이지를 로드하지 못했다는 뜻입니다.
if(!navigator.serviceWorker.controller) {
  //그래서 네트워크의 콘텐츠를 로드했다는 것입니다.
}
//waiting 워커가 있다면 업데이트 준비가 됐다는 뜻
if (reg.waiting) {
  //there's an update ready
}
if (reg.installing) {
  //there's an update in progress
  //만약 실패라면
  reg.installing.addEventListener('statechange', function() {
    if (this.state == 'installed') {
      //there's an update ready
    }
  })
}
reg.addEventListener('updatefound', function() {
  reg.installing.addEventListener('statechange', function() {
    if (this.state == 'installed') {
      //there's an update ready
    }
  });
});
```

### 업데이트 발생(trigger)하기
- `self.skipWaiting()` 서비스 워커가 기다리거나 설치 중일 때 호출합니다.
- 대기중인 서비스 워커에 신호를 보내는 방법 :
  + 페이지에서 보내려면 `reg.installing.postMessage({foo: 'bar'});`
  + 서비스 워커에서는 `self.addEventListener('message', function(event) {event.data;})` 로 보냅니다.(event.data 내용 =  {foo: 'bar'})
- 사용자가 새로고침을 누르면 대기를 스킵한다는 호출 메시지를 서비스 워커에게 보냅니다.
- navigator.serviceWorker.controller 의 값이 바뀌면 페이지는 이벤트를 얻습니다.(새로운 서비스 워커를 승계했다는 뜻입니다.)
```javascript
navigator.serviceWorker.addEventListener('controllerchange', function() {
  //navigator.serviceWorker.controller has changed
})
//참고: 서비스 워커가 skipWaiting 이라는 메시지 보내기
worker.postMessage({action: 'skipWaiting'});
```
