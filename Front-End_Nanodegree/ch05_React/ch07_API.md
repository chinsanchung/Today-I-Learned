# Getting Started with the APIs (Udacity)
- 이번 코스에서는 맵 기능을 사용하게 해주는 다양한 구글 맵 API 들을 어떻게 사용하는지 배웁니다.

## 자바스크립트 API Overview
- 맵을 렌더링할 때 가장 기본적으로 제어하는 건 맵 타입입니다. 4개의 생성된 맵 타입이 있습니다.
  + 첫째로 가장 익숙한 로드 맵 타입입니다. 구글 맵의 기본 설정입니다.
  + 둘째로 위성 맵 타입입니다. 그리고 셋째로는 위성 이미지와 로드 맵을 합친 하이브리드입니다.
  + 마지막으로 terrian 입니다. 음영 처리와 다른 물리적인 지형 정보를 강조하는 terrian 정보 가지고 있습니다.
- 이제 코드에 넣어봅니다.
```HTML
<!DOCTYPE html>
<html>
  <head>
    <style>
      html,
      body {
        font-family: Arial, sans-serif;
        height: 100%;
        margin: 0;
        padding: 0;
      }
      #map {
        height: 100%;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
    <script>
      var map;
      function initMap() {
        // Constructor creates a new map - only center and zoom are required.
        map = new google.maps.Map(document.getElementById('map'), {
          center: {lat: 40.7413549, lng: -73.9980244},
          zoom: 13
        });
      }
    </script>

    <script async defer
        src="https://maps.googleapis.com/maps/api/js?key=MYAPIKEY&v=3&callback=initMap">
    </script>
  </body>
</html>
```
- `<script async defer...>` 에서 비동기적으로 자바스크립트를 로드합니다. 자바스크립트 API 가 로드되는 동안 페이지의 나머지 부분이 렌더링되고, API 가 로드되면 콜백 함수가 실행됩니다.
  + 만약 API 함수를 곧바로 사용하지 않는다면 같은 명령을 함수 안에 넣어 사용하려고 할 때 호출하면 됩니다.
  + 비동기적으로 API 를 로딩하면 자바스크립트를 기다릴 필요 없이도 빠르게 페이지를 로드할 수 있습니다.
  + `maps.googleapis.com` 은 자바스크립 형태(form)로 로드하는 마지막 지점입니다.
  + 파라미터 `v=3` 은 사용하고자 하는 API 의 버젼을 나타냅니다.
- `initMap()` 함수는 페이지가 로드 된 후 사용자가 지도와 상호작용하기 전에 발생하는 모든 것을 포함합니다.
  + 안에 새로운 맵 인스턴스를 만들었습니다. ( map = new google.maps.Map~ )
  + 그리고 HTML 의 어디서 맵을 로드할지, 또 세계 지도의 어떤 부분을 보여줄지를 정해야 합니다.
  + center 는 위도(latitude)와 경도(longitude) 를 축약해서 lat, lng 문자열로 적었습니다. zoom 은 숫자로 최대 21까지 확대할 수 있습니다.
