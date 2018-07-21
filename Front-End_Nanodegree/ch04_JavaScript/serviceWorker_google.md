# 구글이 알려주는 서비스 워커
[사이트 주소](https://codelabs.developers.google.com/codelabs/offline/#0)
## 시작
- 파일을 내려받습니다.
```
$ git clone https://github.com/GoogleChrome/airhorn.git
```
- master branch 에서 확인합니다. 그리고 app 파일로 이동해서 파이썬으로 서버를 가동합니다.
```
$ cd app
$ python -m SimpleHTTPServer 3000
```

## 테스트하기
- 크롬의 개발자 도구를 열고 Application 패널로 이동합니다.
  + 거기엔 offline 체크박스가 있습니다. 그것을 체크하면 노란색 워닝 아이콘을 띄워 오프라인임을 보여줍니다.
- 서비스 워커를 사용해서 오프라인으로 앱을 실행하도록 만들어봅니다.

## 설정하기
- 우선 branch 를 바꾸도록 합니다. 이 버젼은 서비스 워커가 없는 버젼입니다.
```
$ git checkout code-lab
```
- 이제 다시 Application 패널로 돌아가 오프라인을 해제합니다. 페이지를 작동시키면 앱이 실행될 것입니다.
  + 다시 오프라인 모드를 해보면, 인터넷에 열결되어 있지 않다는 메시지가 나올 거라고 예상할 테지만 아닙니다. 오프라인 앱을 실행하고 있을 것입니다.
- 왜냐면 현재 범위(scope)인 localhost:3000 에 아까 서비스 워커를 설치했기 때문입니다.
  + 이 서비스 워커는 이제 앱을 실행할 때마다 자동으로 실행됩니다.
  + 해당 범위에 설치된 서비스 워커는 프로그래밍 방식 혹은 수동으로 삭제하지 않는 한 localhost:3000 에 접근할 때마다 자동으로 시작될 것입니다.
- 이를 해결하려면 개발자 도구의 애플리케이션에서 서비스 워커 -> Unregister 를 누르십시오.
  + 등록을 해제하고 새로고침을 한다면 인터넷에 연결되어 있지 않다는 메시지가 나타납니다.

## 서비스 워커 등록하기
- 이제 앱에 오프라인 지원을 추가해봅니다. 우선 새로운 파일 `sw.js` 를 만듭니다.
- 그리고 `index.html` 에서 <body> 태그 밑에 스크립트를 작성합니다.
  + 서비스 워커의 위치는 매우 중요합니다. 보안상의 이유로 서비스 워커는 같은 디렉토리 또는 하위 디렉토리에 있는 페이지만 제어할 수 있습니다. (즉 서비스 워커 파일을 script 디렉토리에 배치하면 script 디렉토리 또는 아래의 페이지와만 상호 작용할 수 있습니다.)
```HTML
<script>
  if('serviceWorker' in navigator) {
    navigator.serviceWorker
      .register('/sw.js')
      .then(function() {
        console.log("Service Worker Registered");
      })
  }
</script>
```
  + 이 스크립트는 브라우저가 서비스 워커를 지원하는지 확인합니다. 여기선 `sw.js` 파일을 서비스 워커로 등록한 다음 콘솔창에 출력합니다.
- 사이트를 실행하기 전 서비스 워커 칸이 비어 있어야 합니다. 그리고 오프라인을 해제하고 새로고침해봅니다. 서비스 워커가 등록됐습니다.
  + Source 라벨을 클릭하면 등록한 서비스 워커의 소스 코드를 볼 수 있습니다. 현재는 비어 있을 겁니다.

## 사이트의 자산(asset) 설치하기
- 서비스 워커를 등록해 처음으로 사용자가 페이지를 조회하면 설치 이벤트가 트리거됩니다.
  + 이 이벤트는 페이지 자산(asset)을 캐시(cache)하려는 이벤트입니다. `sw.js` 에 코드를 추가합니다.
```javascript
importScripts('/cache-polyfill.js')

self.addEventListener('install', function(e) {
  e.waitUntil(
    caches.open('airhorner').then(function(cache) {
      return cache.addAll([
        '/',
        '/index.html',
        '/index.html?homescreen=1',
        '/?homescreen=1',
        '/styles/main.css',
        '/scripts/main.min.js',
        '/sounds/airhorn.mp3'
      ]);
    })
  );
});
```
  + 모든 브라우저에서 캐시 API 를 지원하진 않아서 첫 행에 폴리필을 추가했습니다.
  + 설치 이벤트 리스너에 캐시 객체를 열고(caches.open) 캐시하려는 자원 목록으로 채웁니다.
  + `addAll` 의 중요한 점은 모두 존재하거나 혹은 아무것도 없다는 것입니다. 만약 파일 중 하나가 없거나 불러오지(fetch) 못했다면 전체 `addAll` 조작은 실패합니다.
- 다음에는 서비스 워커에게 요청을 가로채서 이들 자원 중 하나에 return 하도록 만들고, 캐시 객체를 사용해서 로컬에 저장된 각 자원의 버젼을 return 하도록 만들어봅니다.

## 웹 페이지 요청 가로채기
- 서비스 워커의 강력한 점은 일단 서비스 워커가 페이지를 제어하면, 페이지가 작성한 모든 요청을 가로채고 또 요청에 대한 처리 방법도 결정할 수 있게 된다는 점입니다. 이번에는 서비스 워커에게 네트워크를 검색하지 않고 요청을 가로채 캐시된 버젼의 자산을 return 하도록 만듭니다.
- 우선 이벤트 핸들러를 fetch 이벤트에 연결합니다. 이 이벤트는 만들어진 모든 요청에 대해 트리거됩니다. `sw.js` 의 밑에 코드를 추가합니다.
```javascript
self.addEventListener('fetch', function(event) {
  console.log(event.request.url);
});
```
- 이걸 시험해 봅니다. 개발자 도구의 애플리케이션 패널 가서 새로고침을 한다면 새로운 화면이 보일 것입니다. 서비스 워커에서 상태(status)가 바뀌었습니다.
![새 화면](https://codelabs.developers.google.com/codelabs/offline/img/c7cfb6099e79d5aa.png)
  + 상태에는 활성화를 위해 대기 중인 새로운 서비스 워커가 있습니다. 이 워커는 방금 만든 변경 사항이 있는 새로운 서비스 워커입니다.
  + 그렇지만 현재 설치했던 오래된 서비스 워커가 페이지를 계속 제어하고 있는 상황입니다. `sw.js`를 클릭하면 어떤 서비스 워커가 실행 중인지를 확인할 수 있습니다.
- 이 상태를 바꾸려먼 애플리케이션 패널의 Update on reload 를 체크합니다.
  + 이것을 체크하면 개발자 도구는 페이지를 새로고침할 때마다 항상 서비스 워커를 업데이트합니다. 이는 서비스 워커를 적극적으로 개발할 때 매우 유용합니다.
  + 이제 페이지를 새로고침하면 새 서비스 워커가 설치되었고 요청한 URL 이 콘솔창에 뜰 것입니다.
![새 서비스워커](https://codelabs.developers.google.com/codelabs/offline/img/53c23650b131143a.png)
- 이제 모든 요청을 처리할 건지를 결정합니다. 기본적으로 아무것도 하지 않으면 요청이 네트워크로 전달되고, 응답이 웹 페이지로 return 됩니다.
  + 애플리케이션을 오프라인으로 작동시키려면 캐시에서 요청을 가져와야 합니다.
  + 가져오기(fetch) 이벤트 리스너를 업데이트해서 작성합니다.
```javascript
self.addEventListener('fetch', function(event) {
  console.log(event.request.url);
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});
```
  + `event.respondWith()` 메소드는 브라우저에서 향후 이벤트 결과를 평가하도록 지시합니다.
  + `caches.match(event.request.url)` 은 fetch 이벤트를 트리거한 현재 웹 요청을 취해 캐시에서 일치하는 자원을 찾습니다. 일치 여부는 URL 문자열을 보고 수행합니다.
  + 일치 메소드는 파일이 캐시에서 발견되지 않더라도 해결하는 프로미스를 return 합니다. ( `return response || fetch(event.request)` )간단한 경우 파일을 찾을 수 없으면 네트워크에서 파일을 가져와서(fetch) 브라우저로 return 하기만 하면 됩니다.
- 이외에도 다양한 캐싱 시나리오가 있습니다.
  + 예를 들어 이전에 캐시되지 않은 요청에 대한 모든 응답을 점진적으로 캐시 할 수 있으므로 나중에 캐시에서 모두 반환됩니다.
