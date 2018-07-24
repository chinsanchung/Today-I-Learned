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
### 실습
- 이제는 단일 마커와 윈도우를 삭제하고 다수의 정보들을 지도에 띄워 봅니다. [github link](https://github.com/udacity/ud864/blob/master/Project_Code_3_WindowShoppingPart1.html)
- 개인 프로젝트에서는 데이터베이스를 사용해 사이트에 정보를 전달해주는게 좋습니다. 쉽게 수많은 데이터를 맵에 렌더링할 다양한 API 가 있습니다. [Layer](https://developers.google.com/maps/documentation/javascript/layers), [Data Layer](https://developers.google.com/maps/documentation/javascript/datalayer), [Fusion Tables Layer](https://developers.google.com/maps/documentation/javascript/fusiontableslayer)
- 빈 전역 배열 markers 를 만들고 for 반복문으로 각 위치에 대한 마커를 만들었습니다. 그리고 markers 배열 안에 새로 만든 marker 인스턴스를 보관해 조직화한 상태로 유지합니다.
- 그리고 마커를 클릭하면 정보 윈도우가 뜨고 마커에 대한 콘텐츠가 나오도록 이벤트 리스너를 추가했습니다. 새로 populateInfoWindow 함수를 만들어서 첫 인수 this 에 클릭한 marker 를 전달하게 만듭니다.
- 정보 윈도우는 largeInfowindow 변수에서 시작합니다. populateInfoWindow 함수는 클릭해서 marker 를 열면 그와 관련된 정보를 채워넣습니다.
- 그 다음 처음의 zoom 영역 박의 목록을 가지고 사용자가 원하는 것에 맞게 지도의 경계를 조절해봅니다.
  + `LatLngBounds` 인스턴스를 만들어 남서쪽과 북동쪽 코너를 잡습니다.
  + 그리고 아까 만든 모든 마커의 경계를 확장합니다. `bounds.extend(marker.position)`
- 마지막으로 지도에 아까의 경계에 맞추도록 만듭니다. `map.fitBounds(bounds)`
- 완성입니다. 이제 이걸로 커스텀 아이콘으로 마커를 만들거나 이미지, 링크, 정보들을 정보 윈도우에 넣기 등 다양하게 만들 수 있습니다.

## 윈도우 쇼핑 파트 2
- [github link](https://github.com/udacity/ud864/blob/master/Project_Code_4_WindowShoppingPart2.html)
- 굳이 모든 리스트를 한번에 보일 필요는 없을지도 모릅니다. 버튼을 클릭해서 보여주거나 숨기게 선택하도록 만들수 있습니다.
  + input 태그로 Show Listings 와 Hide Listings 버튼을 만듭니다. 그리고 그걸 감싼 <div class="options-box"> 태그에 대한 CSS 도 작성합니다.
  + 각 버튼에 대한 이벤트 리스너를 만들어 클릭했을 때 함수를 실행하게 합니다.
  + 우선 showListings 함수를 만들어 markers 배열으로 확장하는 반복문을 만듭니다. map 을 설정하고 마커의 경계를 확장, 지도의 경계를 맞춥니다.
  + 다음은 hideListings 함수입니다. 각 마커를 null 로 설정합니다. 이것은 마커를 지우지 않고 숨기기만 합니다.

## Being Stylish
- 지도의 스타일을 바꿀 수 있습니다. 변화시킬 수 있는 것들을 features 라고 부르는데 물, 도로 타입, 땅 타입, 장소 포인트, 라벨들이 있습니다.
- [구글 개발자 블로: 스타일 맵](https://developers.google.com/maps/documentation/javascript/styling), [스타일 맵 예시](https://snazzymaps.com/), [MapTypeStyle inteface](https://developers.google.com/maps/documentation/javascript/reference/3/map#MapTypeStyle)
### 실습
- 직접 스타일로 지도를 꾸며 봅시다. [github link](https://github.com/udacity/ud864/blob/master/Project_Code_5_BeingStylish.html)
- 스타일 맵은 색으로 꾸미고 지도를 바꾸는 두 컨셉을 사용합니다.
- styles 배열 안 객체의 `featureType` 은 지도에 타겟팅할 수 있는 지리적 element 입니다. 도로, 공원, 물 등등.
  + `elementType` 은 바꾸고 싶은 feature 에 대한 것입니다. 지도 자체, 라벨, 라벨 아웃라이너 텍스트 등입니다.
  + `stylers` 배열은 색이고 지도 features 에 적용할 수 있는 눈에 보이는(가시적인) 프로퍼티입니다.
- 지도 features 와 stylers 는 styles 배열으로 결합됩니다.
  + styles 는 계단식입니다.
- map 객체의 옵션에 styles 를 넣었습니다. 그리고 `mapTypeControl` 파라미터는 사용자가 지도 타입을 4가지 중에 하나로 바꾸도록 해줍니다.(로드맵, 위성, terrain 등) 여기선 기본으로 하기 위해 false 값을 줬습니다.
```javascript
map = new google.maps.Map(document.querySelector('#map'), {
  center: {lat: 1 lng: -1},
  zoom: 11,
  styles: styles,
  mapTypeControl: false
})
```
- 새로운 마커를 만드는 함수로 기본아이콘, 하이라이트 아이콘을 만듭니다.
  + 색으로 markerImage 변수를 만들어 마커의 색, 크기까지 지정합니다.
- 마커를 만드는 for 반복문에서 마커에 대한 프로퍼티로 icon 을 주는데 거기에 우선 기본아이콘으로 지정합니다.
- 두 이벤트를 만드는데, 우선 mouseover 이벤트는 아이콘을 하이라이트 아이콘으로 바꿉니다. 그리고 mouseout 이벤트로 다시 기본 아이콘으로 바꿔줍니다.

## 정적 지도 및 스트리트 뷰 이미지
- [github link](https://github.com/udacity/ud864/blob/master/Project_Code_6_StaticMapsAndStreetViewImagery.html)
### 뷰 파노라마
- 뷰 파노라마를 이해하려면 우선 어떤 장소를 봐야 할지 알아야 합니다. 그리고 뷰의 포인트가 필요합니다.
  + 좌우 뷰 포인트를 확인하는건 heading, 상하 뷰 포인트를 확인하는건 pitch 라고 부릅니다.
  + 이러한 것들을 각 포인트마다 동적으로 정의를 해야 합니다.
- 이와 관련된 코드를 작성해 봅니다.
### 실습
- 우선 전에 만들었던 `populateInfoWindow` 함수에 코드를 추가합니다.
  + 그 전에 변수를 하나 추가합니다. 새로운 `StreetViewStatus` 객체를 만드는 겁니다. 이 서비스는 마커의 가장 가까운 위치에서 파노라마 이미지를 보여주게 합니다. 그리고 카메라 포인트를 heading, pitch 중 어떤 방식으로 할지 찾아야 합니다.
```javascript
var streetViewService = new google.maps.StreetViewService();
```
- 이제 시장의 정확한 랜드마크 위치를 가진 파노라마 객체를 만들 수 있습니다.
  + 하지만 정확한 위치에 대한 이미지가 없다면, 미리 radius 를 50 미터로 정의하고 그걸로 StreetView 이미지를 찾습니다.
- 다음은 `streetViewService.getPanoramaByLocation` 함수를 사용해 마커의 포지션과 radius 를 전달하고 또한 위에서 만든 `getStreetView` 함수를 호출합니다.
  + `getStreetView` 함수는 근처의 `nearStreetViewLocation` 변수를 가져와서 가장 가까운 StreetView 위치와 마커 위치를 기반으로 heading 을 계산합니다.
```javascript
var nearStreetViewLocation = data.location.latLng;
var heading = google.maps.geometry.spherical.computeHeading(
  nearStreetViewLocation, marker.position);
```
  + 이것을 하기 위해선 구글 맵 geometry 라이브러리를 써야 합니다. API 를 불러올 때 라이브러리 프로퍼티를 사용해 geometry 라이브러리를 불러오게 합니다.
- 올바른 heading 을 작성하기 위해  `nearStreetViewLocation` 에서 건물을 볼 수 있도록 `computeHeading` 함수를 사용합니다.
- 변수 `panoramaOptions` 를 설정할 때 포지션을 nearStreetViewLocation 으로 하고, 사용할 뷰 포인트를 heading 과 pitch: 30 으로 합니다. 그러면 건물을 약간 위에서 바라보게 됩니다.
