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
        src="https://maps.googleapis.com/maps/api/js?key=MYAPIKEY&v=3&callback=initMap"
    >
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

## 마커를 만들기
- 부동산 리스트 사이트같은 애플리케이션을 만들기 위해 필요한 것들을 잠시 점검해 봅시다. 우선 지도에 위치를 지정해야 합니다.
- lat 과 lng 로 목록을 매핑해봅니다. (이 과정의 뒷부분에 주소에서 좌표를 얻는 법을 배울 것입니다.)
  + 먼저 지도 위에 포인트 하나를 만듭니다.
```HTML
<script>
  let map;
  function initMap() {
    map = new google.maps.Map(document.querySelector('#map'), {
      center: {lat:40.7413549, lng:-73.99802439999996},
      zoom: 13
    });
    let tribeca = {lat: 40.7413549, lng: -74.0089934};
    let marker = new google.map.Marker({
      position: tribeca,
      map: map,
      title: 'First Marker'
    });
  }
</script>
```
  + 이 시작 함수 안에 lat, lng 문자열 객체를 만듭니다
  + tribeca 포인트를 지도에 띄우려면 어떻게 할까요. `marker` 라 부르는 인증된 아이콘을 사용합니다.
- `marker` 는 버튼을 클릭하기 혹은 시작 함수 작업시 만들 수 있는 객체입니다.
  + `google.maps.Marker` 객체를 사용해 새로운 마커 인스턴스를 만들었습니다. 마커에 대해 알아야 할 점은 오직 마커가 나오는 지도, 그리고 나올 위치입니다. 그리고 타이틀을 만들었습니다. 마커 위에 나타날 것입니다.
- 마커를 드래그해서 움직일 수 있게 만들 수도 있습니다. 애니메이션을 지정해 지도에 표시되는게 아닌 떨어지는 마커를 만드는 것도 가능합니다.
- 다음에 마커에 다양한 정보를 보여주게 만들어봅니다.

## 윈도우 쇼핑 파트 1
- 마커가 보여주는 정보 윈도우는 마커가 있거나 혹은 없는 위치에 나오는 팝업 디스플레이입니다.
  + 정보 윈도우는 위치를 설명하는 텍스트나 이미지를 제공하는데 적합합니다.
  + 마커와 비슷한 방법으로 만들 수 있습니다. 객체와 몇 파라미터를 사용해봅니다.
- 새로운 윈도우 인스턴스를 만드려면 집어넣을 콘텐츠를 작성해야 합니다. 문자열 혹은 미리 정의한 element 를 사용합니다.
```HTML
<script>
  function initMap() {
    //...
    let infowindow = new google.maps.InfoWindow({
      content: 'Do you fell like an infowindow, floating through the wind,' + ' ready to start again?'
    });
    marker.addListener('click', function() {
      infowindow.open(map, marker);
    });
  }
</script>
```
  + 정보 윈도우는 마커와 다르게 자동으로 열리지 않습니다. 언제 열어야 할지를 지정해줘야 합니다. 이벤트 리스너를 추가해 마커를 클릭할 때 정보 윈도우가 뜨도록 만들었습니다.
  + 이벤트 리스너 안에 window.open 메소드를 사용했는데 만약 마커를 전달하지 않는다면, 정보 윈도우에 포지션 프로퍼티를 줘서 열 수 있게 만들어야 합니다.
- 이제는 단일 마커와 윈도우를 삭제하고 다수의 정보들을 지도에 띄워 봅니다.
  + 개인 프로젝트에서는 데이터베이스를 사용해 사이트에 정보를 전달해주는게 좋습니다. 쉽게 수많은 데이터를 맵에 렌더링할 다양한 API 가 있습니다. [Layer](https://developers.google.com/maps/documentation/javascript/layers), [Data Layer](https://developers.google.com/maps/documentation/javascript/datalayer), [Fusion Tables Layer](https://developers.google.com/maps/documentation/javascript/fusiontableslayer)
```javascript
function initMap() {
  var markers = [];
  function initMap() {
    //map = ...
    let locations = [
      {title: 'Park Ave Penthouse', location: {lat: 40.7713024, lng: -73.963293}},
      {title: 'Chelsea Loft', location: {lat: 40.7444883, lng: -73.9949465}},
      {title: 'Union Square Open Floor Plan', location: {lat: 40.7347062, lng: -73.9895759}}
    ];
  }
}
```
  + 빈 전역 배열 markers 를 만들고 for 반복문으로 각 위치에 대한 마커를 만들었습니다. 그리고 markers 배열 안에 새로 만든 marker 인스턴스를 보관해 조직화한 상태로 유지합니다.
  + 그리고 마커를 클릭하면 정보 윈도우가 뜨고 마커에 대한 콘텐츠가 나오도록 이벤트 리스너를 추가했습니다. 새로 populateInfoWindow 함수를 만들어서 첫 인수 this 에 클릭한 marker 를 전달하게 만듭니다.
  + 정보 윈도우는 largeInfowindow 변수에서 시작합니다. populateInfoWindow 함수는 클릭해서 marker 를 열면 그와 관련된 정보를 채워넣습니다.
  + 그 다음 처음의 zoom 영역 박의 목록을 가지고 사용자가 원하는 것에 맞게 지도의 경계를 조절해봅니다.
    + `LatLngBounds` 인스턴스를 만들어 남서쪽과 북동쪽 코너를 잡습니다.
    + 그리고 아까 만든 모든 마커의 경계를 확장합니다. `bounds.extend(marker.position)`
  + 마지막으로 지도에 아까의 경계에 맞추도록 만듭니다. `map.fitBounds(bounds)`
- 완성입니다. 이제 이걸로 커스텀 아이콘으로 마커를 만들거나 이미지, 링크, 정보들을 정보 윈도우에 넣기 등 다양하게 만들 수 있습니다.
