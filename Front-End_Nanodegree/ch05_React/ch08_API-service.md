# Understanding API Services
## 웹 서비스와 Geocoding 01
- `Geocoding` : 구글 지도 API 의 많은 장소 서비스 중 하나입니다.
- 이러한 서비스의 대부분은 서버 측 (예 : 스크립트로 실행하는 프로세스의 일부 또는 사용자 측 코드의 일부인 클라이언트 측)에서 사용할 수 있다는 점에 유의해야합니다.
  + 이 기능은 각 서비스에서 동일 할 수 있지만 분명히 구문이 약간 다릅니다.
- 서버 측 옵션은 사용자 입력이 필요없이 많은 자료를 처리해야하는 데이터가 이미있는 경우가 이상적입니다.
  + 이런 식으로 웹 서비스 요청을 만들 때 결과는 JSON 이나 XML 형식으로 return 됩니다.
- 하지만 때떄로 데이터를 미리 가지지 못할 때가 있습니다. 사용자가 동적으로 주소, 이메일 등를 입력하는 부분들입니다.
  + 아마도 사용자는 지도에서 살고 있는 장소를 클릭할 것입니다. 그리고 원하는 장소를 찾고자 할 것입니다. 이런 상황이 이상적인 클라이언트 측 서비스입니다.
  + 알아야 할 점은 모든 웹 서비스 요청을 같은 방식으로 만든다는 겁니다. 요구하는 파라미터를 넣고 텍스트 응답을 얻는 것입니다.
- [구글 개발자 블로그: Geocoding API](https://developers.google.com/maps/documentation/geocoding/intro), [구글 개발자 블로그: Geocoding 서비스](https://developers.google.com/maps/documentation/javascript/geocoding), [geocoding 자바스크립트 API 서비스](https://developers.google.com/maps/documentation/javascript/reference/3/)

## 웹 서비스와 Geocoding 02
- 대다수의 매핑은 lat, lng(위도, 경도) 좌표를 중요시하지만, 좌표를 정확히 알기란 어려운 일입니다.
- 구글 지도 API 의 가장 중요한 feature 는 위도와 경도를 오가는 기능입니다. 이것을 `geocoding` 이라고 부릅니다. 주소를 받아 위도, 경도를 가져옵니다.
  + 반대로 `reverse geocoding` 은 위도, 경도로 주소나 장소를 가져옵니다.
### 알아보기
- 개발지 블로그에서 가져온 요청입니다. 이것은
```
https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=YOUR_API_KEY
```
  + 모든 요청은 같은 이름 스페이스에서 시작합니다. `https://maps.googleapis.com/maps/api`
  + 서비스를 명시하고 (`geocode`), 출력 포맷(`json`)을 JSON 이나 XML 중에서 정합니다.
  + 그 다음 입력 파라미터를 지정합니다. 주소와 지도 API 키입니다. 이외에도 컴포넌트 제한(도시나 국가 결과를 제한합니다.), 지역 bias(geocoder 에게 특정 지역이 다른 지역보다 나은 결과를 얻을 수 있다고 알려줍니다.) 등을 추가할 수 있습니다.
```
{
   "results" : [
      {
//주소 컴포넌트는 거리, 마을, 이웃, 국가, 주 등의 정보로 분류됩니다.
일반적으로 이 컴포넌트는 포맷된 주소보다 더 많은 정보를 가집니다.
  그래서 더 많은 정보를 원하면 이 컴포넌트를 해석해야 합니다.
         "address_components" : [...],
//이 주소는 자체적으로 사용할 수 있습니다.
         "formatted_address" : "1600 Amphitheatre Pkwy, Mountain View, CA 94043 미국",
//geometry 는 실제 장소의 lat, lng 과 다른 중요 정보들을 가집니다.
         "geometry" : {
//location 은 찾고자 하는 lat, lng 입니다.
           location: {lat:..., lng:...},
//location_type 은 얻은 포인트에 대해 알려줍니다.
  ROOFTOP 은 정확하게 일치하는 장소를 가졌음을 뜻합니다.
           location_type: "ROOFTOP",
           ...
           },
//place_id 는 전체 국가에서 단일 주소의 세분성까지의
  모든 장소에 대한 또다른 유일 식별자입니다.
  place API 와 함께 사용합니다.
         "place_id" : "ChIJ2eUgeAK6j4ARbn5u_wAGqWA",
         "plus_code" : {
            "compound_code" : "CWC8+W3 미국 California, 마운틴뷰",
            "global_code" : "849VCWC8+W3"
         },
         "types" : [ "street_address" ]
      }
   ],
//status 는 요청이 성공적이었고 다양한 웹 서비스에 쓰인다는걸 알려줍니다.
   OK 는 찾아낸 것이 원하는 것임을 의미합니다.
   Invalid 는 요청이 잘못됐다는 걸 뜻합니다.
   Unknow error 는 보통 서버의 에러고 요청은 성공할 때까지 반복합니다.(예: query limit 에러)
   "status" : "OK"
}
```

## geocoding 요청/응답
- geocoding 요청을 할 때는 key, address 를 입력해야 합니다.
  + 주소는 geocoding 요청에 필요한 유일한 정보입니다. (혹은 `latlng` 파라미터로 역 geocoding 을 하면 됩니다.)
  + 키는 인증에 필요합니다.
  + 참고로 Region 은 선택 파라미터로 결과를 특정 지역으로  BIAS 합니다.

## Status
- 퀴즈
  + ZERO_RESULT 응답은 서비스 요청이 성공적으로 서버에 전달됐고, 인증도 받고 쿼리도 실행했지만 맞는 결과가 없을 경우에 뜹니다. API 키를 전달하고 유효한 요청 형식을 사용하는 한 모든 주소에 지오 코딩을 요청할 수 있습니다.

## 앱에서의 geocoding
- 위에서 사용한 geocoding 데이터를 자바스크립트로 앱에 적용해봅니다. geocoding 클라이언트 측 서비스를 사용하고 사용자 입력이 필요합니다. [Github link 08](https://github.com/udacity/ud864/blob/master/Project_Code_8_GeocodingInTheApp.html)
- 현재 새 집을 원하는 사용자는 검색할 위치를 표시하려 할 수도 있습니다.
  + 방법 중 하나는 관심 포인트(이웃, 주소)에 들어가게 하는 겁니다. 그러면 지도를 중심으로 놓고 주변 장소를 줌합니다.
- 우선 장소를 입력해 줌할 버튼을 만들고 관련 함수를 작성합니다.
```javascript
// This function takes the input value in the find nearby area text input
// locates it, and then zooms into that area. This is so that the user can
// show all listings, then decide to focus on one area of the map.
function zoomToArea() {
  // Initialize the geocoder.
  var geocoder = new google.maps.Geocoder();
  // Get the address or place that the user entered.
  var address = document.getElementById('zoom-to-area-text').value;
  // Make sure the address isn't blank.
  if (address == '') {
    window.alert('You must enter an area, or address.');
  } else {
    // Geocode the address/area entered to get the center. Then, center the map
    // on it and zoom in
    geocoder.geocode(
      { address: address,
        componentRestrictions: {locality: 'New York'}
      }, function(results, status) {
        if (status == google.maps.GeocoderStatus.OK) {
          map.setCenter(results[0].geometry.location);
          map.setZoom(15);
        } else {
          window.alert('We could not find that location - try entering a more' +
              ' specific place.');
        }
      });
  }
}
```

## No Mountain High Enough - Elevation API
- 구글 지도의 Elevation API 는 해당 장소의 높이, 위치, resolution 을 알려줍니다.
- HTTP 요청으로 결과를 알아봅니다.
```
{
  results: [
    {
//이 산의 높이는 6527 미터입니다.
      elevation: 6527.154164,
      location: {
        lat: 30.8681,
        lng: 79.0322
      },
      resolution: 152.70321846454
    }
  ],
  status: "OK"
}
```
- 관련 링크입니다. [구글 개발자 블로그: Elevation API](https://developers.google.com/maps/documentation/elevation/intro), [Elevation API 자바스크립트 서비스](https://developers.google.com/maps/documentation/javascript/elevation)

## Distance Matrix API Part 1
[구글 개발자 블로그: Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix/start), [Distance Matrix API 자바스크립트 서비스](https://developers.google.com/maps/documentation/javascript/distancematrix)
- 전까지 geocoding 을 사용해 도시 내 지역을 위치 정보로 확대/축소 했습니다. 하지만 사용자가 관심있는 특정 영역이 없다면 어떻게 해야 할까요. 사무실까지의 통근 거리에만 관심있다면?
  + 앱에서 사용자에게 위치를 입력할 옵션을 제공하고, 해당 위치에서 일정 거리 내의 부동산 목록만 표시하려 합니다. 어떻게 할까요.
- 그럴 때는 `Distance Matrix API` 를 사용합니다. 구글의 이 API 는 여러 여행지와 목적지 사이의 여행 거리, 소요 시간을 계산합니다.
- HTTP 요청으로 결과를 알아봅니다. url 에 origins(출발지), destinations(목적지)을 입력했습니다.
```
{
   "destination_addresses" : [ "미국 뉴욕" ],
   "origin_addresses" : [ "미국 워싱턴 DC 워싱턴" ],
   "rows" : [
      {
         "elements" : [
            {
               "distance" : {
                  "text" : "225 mi",
                  //기본값은 미터
                  "value" : 361956
               },
               "duration" : {
                  "text" : "3시간 52분",
                  //기본값은 초
                  "value" : 13907
               },
               "status" : "OK"
            }
         ]
      }
   ],
   "status" : "OK"
}
```
  + element 는 origin, destination 페어입니다. 지금은 출발지 하나, 목적지 하나지만 두 개일수도 있습니다.(element 도 두 개가 됩니다.)
- url 에 교통 수단을 선택할 수 있습니다. `json?mode=transit&transit_mode=rail`
  + 혹은 자전거로 highway 를 피하는 방식도 있습니다. `json?mode=bicycling&avoid=highways`

## Distance Matrix API Part 2
- 앱에 검색 form 을 feature 에 넣어 만들어봅니다. [Github link 09](https://github.com/udacity/ud864/blob/master/Project_Code_9_MyCommuteDistanceMatrixAPIPart2.html)
- 사용자 컨트롤 윈도우를 확장해서 걷거나, 자전거를 타거나, 운전하거나, 다른 곳으로 이동하는 등 특정 장소에 도착하기를 원하는 시간(분 단위)을 지정할 수 있도록 합니다.
- select 태그로 이동 시간과 교통 수단을 결정하게 하고, 이동 시간을 계산할 `searchWithinTime` 함수를 만듭니다.
- 참고로 아래의 atLeastOne 변수는 중요합니다. 왜냐면 OK 응답을 getDistanceMatrix 함수로부터 얻는다 해도, 받아 들일 수있는 출퇴근 거리 내에 있는 마커를 찾지 못하면 그것을 사용자에게 알리고 싶어하기 때문에 중요합니다.
```javascript
if (duration <= maxDuration) {
  //the origin [i] should = the markers[i]
  markers[i].setMap(map);
  atLeastOne = true;
  // Create a mini infowindow to open immediately and contain the distance and duration
  //보이는 각 마커에 작은 정보 윈도우를 넣었습니다.
  var infowindow = new google.maps.InfoWindow({
    content: durationText + ' away, ' + distanceText
  });
  //...
}
```

## Directions API
- 구글 지도 API 에는 어떻게 이동할지 그 수단에 대한 정보를 제공하는 기능이 있습니다. 바로 `Directions API` 입니다.
- `Directions API` 는 Distance Matrix API 처럼 여행에 대한 정보를 제공합니다.
  + 하지만 시간만 제공하는 Distance Matrix API 와 다르게, 필요할 경우 A 지점에서 B 지점으로, 심지어 C 지점에서 Z 지점까지 단계별 지침을 제공합니다.
  + 많은 수의 같은 입력을 사용합니다. (travel mode 의 운전, 걷기, 자전거, 대중교통 등 / multiple transit mode 의 버스, 기차 / restrictions 의 톨게이트, 고속도로, 페리 피하기)
  + Directions 는 출발지, 목적지, 경유지(way point)를 문자열로 지정합니다.(이름 혹은 위도-경도)
- `Directions API` 는 일련의 경유지 또는 via 포인트를 사용하여 멀티 포트 directions 을 return 할 수 있습니다.
- 이번에도 url 에 입력해서 알아봅니다.
```
https://maps.googleapis.com/maps/api/directions/json?origin=Disneyland&destination=Universal+Studios+Hollywood4&key=YOUR_API_KEY
```
  + Directions API 는 단일 출발지, 목적지 페어가 필요합니다. 그것들을 위도 경도 좌표나 도시로 입력할 수 있습니다. 현재의 url 요청은 출발지에서 도착지로 가능 운전 directions 를 기본값으로 설정합니다.
- url 에 추가로 입력가능한 것들도 알아봅니다. travel mode 와 다양한 것들을 추가할 수 있습니다.
- `travel_mode` 를 transit 로 하고, `transit_mode` 를 subway 로 하면 지하철로 이동하는 게 됩니다. 그리고 `transit_routing_preferences` 을 지정해 less_walking, less_transfers 등등을 입력할 수 있습니다.
- 이번에는 `travel_mode` 를 bicycling 으로 하고 중간에 경유지를 지정해봅니다.(origin=..&destination=..&waypoints=..)
  + 경유지 파라미터를 사용해서 경로의 중지로 간주되는 추가 목적지를 지정할 수 있습니다. Or I can specify via instead of waypoints.(해석불가)
```
{
  - geocoded_waypoints: [
    {
      geocoder_status: "OK",
      place_id: "...",
      types: [
        "sublocality_level_1",
        "sublocality",
        "political"
      ]
    },
    {
      geocoder_status: "OK",
      place_id: "...",
      types: [
        "premise"
      ]
    }
  ],
  routes: [
    ...
    {
      distance: {text: "...", value: 12108},
      duration: {text: "...", value: 2191}
    },
    end_address: "...",
    end_location: {
      start_location: {lat: ..., lng: ...}
    },
    steps: [
      {
        distance: {...},
        duration: {...},
        end_address: "...",
        end_location: {...},
        start_address: "...",
        start_location: {...},
        steps: [
          {...},
          {...},
          {...}
        ],
        via_waypoint: []
      }
    ],
    overview_polyline: {...},
    summary: "",
    warning: [],
    waypoint_oreder: { }
  ],
  status: "OK"
}
```
  + 밑의 status 가 OK 가 아니라면 위의 route 도 응답에서 볼 수 없습니다.
  + 위의 geocoded_waypoints 를 봅니다. 이것은 출발지, 목적지와 추가 경유지를 가지고 서비스가 해당 위치를 잘 찾는지 확인합니다. 여기선 경유지 없는 결과가 나왔습니다.
  + routes 에는 경유지를 입력하지 않았다면 하나의 legs 만 있을 겁니다. 하지만 경유지를 입력하면 routes 는 여러 legs 로 분할할 겁니다.
  + 각 leg 는 duration 과 distance, 그리고 출발/도착 장소가 있습니다. 그리고 또 하나 혹은 그 이상의 steps 가 있습니다. step 은 가장 작은 명령 단위입니다. 그리고 leg 는 overview polylinke 을 가지고 있습니다. 그것은 지도에 루트를 사용자가 볼 수 있게 만들어줍니다.
  + warning 은 추가했던 설정들이 결과에 없을 때 띄우는 메시지입니다.
- 경유지를 지정할 때 `optimize:true` 를 추가하면 모든 정지 지점을 가장 최적화된 방식으로 처리하는데 도움을 줍니다.
  + 그 말은 경유지의 순서가 원래 요청에서 제공 한 방법과 다른 결과가 나올 수도 있다는 것입니다.

## Route Directions 서비스 보여주기
- directions API 를 앱에 적용해봅니다. 사용자가 통근 시간을 반영한 장소를 등록한 후 앱이 경로를 보여주는 것입니다. [Github link 10](https://github.com/udacity/ud864/blob/master/Project_Code_10_DisplayingRoutesDirectionsService.html)
- 우선 직장 근처의 장소를 찾을 때 나오는 정보 윈도 안의 경로를 보여주는 버튼을 만듭니다. 그리고 이에 대한 함수 `displayDirections` 를 만듭니다.
  + 이 함수는 방향을 계산하고 경로를 지도에 보여줍니다.
  + 새로운 DirectionsService 인스턴스를 만들어 사용합니다. 여기 status 가 OK 면 DirectionsRenderer 의 인스턴스를 만들게 됩니다.
### drivingOptions
- drivingOptions (도로의 교통상황을 보여주는 옵션) 을 추가하려면 drivingOptions 파라미터를 요청에 넣어야 합니다. travel_mode 는 drving 으로 하는게 효과적입니다.
```javascript
drivingOptions: {
  departureTime: new Date(Date.now()),
  trafficModel: "optimistic"
}
```
  + departureTime 은 이동 시간을 계산해주는 옵션을 제공합니다.
  + 또한 최선의 추측, 낙관적 또는 비관적 인 트래픽 모델을 지정할 수 있습니다. 이 모델을 사용하면 트래픽이 더 가벼운 날이나 더 무거운 날의 경로와 소요 시간을 알 수 있습니다.

## Distance Matrix and Directions Specifics
- Distance Matrix API 요청은 travel_mode 뿐만 아니라 출발지와 도착지의 결합으로 만들어집니다. 아래는 요청할 때 살펴볼 점입니다.
  + 선택한 travel_mode 에 관계없이 요청당 최대 25개 출발지 또는 25개 목적지를 전달할 수 있지만 최대 100개의 element 와 elements = 출발지 * 목적지 만 허용됩니다. 예를 들어 10개의 출발지와 10개의 목적지 = 100 개의 elements 가 있습니다. 이 값은 웹 서비스를 사용할 때 Premium Plan 고객의 경우 최대 625개 element 로 증가합니다.
  +  다른 대부분의 서비스는 Rate Limited (초당 쿼리 수는 50을 초과 할 수 없음)이지만 Distance Matrix API는 elements (쿼리가 아닌)의 측면에서 속도가 제한됩니다. 특정 기간 내에 너무 많은 elements 가 요청되면 OVER_QUERY_LIMIT 응답 코드가 return 됩니다.
  + 일부 매개변수는 특정 travel_mode 에서만 유효합니다.
    + 예 1:  travel_mode = 대중교통 이면 AvoidTolls 및 AvoidHighways는 무시됩니다.
    + 예 2: 모든 TransitOptions 는 travel_mode = 대중교통 인 경우에만 유효합니다.
    + 예 3: departureTime 은 DrivingOptions 또는 TransitOptions 매개변수의 일부로 전달할 수 있습니다.
- Directions API 요청은 출발지, 목적지 및 travelMode 도 포함하지만 경유지 또는 via 지점의 선택적 매개변수도 있습니다. 다음은 요청할 때 살펴봐야 할 점입니다.
  + 자바스크립트 API 를 사용하는 클라이언트 측 약도 서비스의 경우 요청 당 최대 8개의 중간 지점을 지정할 수 있습니다. 출발지 및 목적지 외에 요청당 총 10개의 geocoding 된 포인트가 생성됩니다. 이 한도는 프리미엄 플랜 고객의 경우 8 에서 23 까지 증가합니다.
  + 웹 서비스의 경우 API 키와 같은 유효한 식별자를 제공하는 한 요청 당 최대 23개의 경유지를 지정할 수 있습니다. 이 값을 초과하면 응답 코드에 나타납니다.
  + TRANSIT travel_mode 에서는 경유지를 지원하지 않습니다.

## Roads API
- gps 추적기의 정확도가 낮으면 gps 추적이 흔들거리고 이상하게 뜹니다. 이를 해결하려면 Google Maps Roads API 가 필요합니다. [구글 개발자 블로그: Roads API](https://developers.google.com/maps/documentation/roads/intro)
- Roads API 는 흔들거리게 찍힌 GPS 지점을 가장 가능성 높은 길로 스냅합니다.
  + 100개의 흔들거리는 GPS 좌표를 전달하면 Roads API 는 차량이 주행할 가능성이 가장 높은 도로에 스냅했던 데이터를 return 합니다.
  + 추가적으로 포인트를 삽입하는 요청으로 도로 형상(geometry)을 부드럽게 따라가는 경로를 만들 수도 있습니다.
  + 또한 특정 도로 구간에 대한 속도 제한 데이터를 다시 가져올 수 있습니다.
- 다만 Roads API 는 웹 서비스 형태로만 사용가능합니다. 그리고 이 데이터는 오직 프리미엄 플랜 고객들만 쓸 수 있습니다. 그래서 기존의 API 키로는 이 타입의 요청을 하지 못할 겁니다.
### url 요청 연습
- url 로 속도 제한 데이터 요청을 테스트해봅시다.
```
https://roads.googleapis.com/v1/snapToRoads?path=-35.1234|-35.13343,14.12367|...&interpolate=true&key=YOUR_API_KEY
```
- snapToRoads 요청으로 필요한 매개변수를 삽입합니다. 경로, 흔들거리는 GPS 포인트들입니다. 그리고 interpolate 를 true 로 합니다.
```
https://roads.googleapis.com/v1/speedLimits?path=...
```
- 이번에는 속도 제한 요청입니다. 이것은 경로에 따른 포인트들에 대한 속도 제한 데이터입니다.
- Roads API 는 웹 서비스 형태로만 된다는 걸 명심합니다. 자사브크립트 API 에는 이런 서비스가 없습니다.
### Quiz : interpolate
- Interpolating snapToRoads request :
도로 snapToRoads 요청에 최대 100 위도/경도 포인트를 전달할 때(주소가 아닙니다), 여행한 도로에 대한 더 완전하고 명확한 그림을 제공하기 위해 100 위도/경도 포인트를 더 많은 포인트로 전환하도록 선택할 수 있습니다. 그러나 포인트가 거의 없고 넓게 퍼져있으면 interpolation 은 정확한 그림을 제공하지 못할 수도 있습니다. - 데이터가 많을수록 좋습니다.

## Place Autocomplete 01
- 전에 부동산 앱에서 Places 라이브러리를 저장했던 적이 있었습니다. 구글 Places 라이브러리의 기능은 앱이 시설의 경계, 고정된 위치와 같은 정의된 영역 내에 포함된 시설, 지리적 위치 또는 중요한 관심 장소와 같은 장소를 검색할 수 있게합니다.
  + 이걸로 많은 양의 데이터에 장소 데이터에 접근할 수 있습니다.
- 구글 Places 라이브러리는 장소 자동완성 기능을 가지고 있으며 그걸로 사용자가 위치를 입력하는 동안 각 키 입력으로 예상 결과를 되돌릴 수있게합니다. 그걸로 더 빠르고 정확한 검색이 가능해집니다. 게다가 사용자가 타이핑을 끝내기 전에 뭘 입력하는지 알 수 있게 됩니다.
### 앱에 적용하기
- [github link 11](https://github.com/udacity/ud864/blob/master/Project_Code_11_FasterIsBetterPlacesAutocompletePart1.html), [동영상](https://youtu.be/-uHDYCEnifU)
- 우선 script src 에 places 라이브러리를 추가합니다. 이걸로 자동완성과 다른 기능들을 쓸 수 있습니다.
```
https://maps.googleapis.com/maps/api/js?libraries=places,geometry,drawing&key=MYAPIKEY&v=3&callback=initMap
 ```
 - 다음으로 두 개의 새로운 장소 자동완성 인스턴스를 시작 함수와 함께 정의하고 두 입력 박스에 묶습니다.
  + 이것은 사용자가 뭘 입력하는지 예측, 입력 박스 아래의 pic 리스트에서 가장 비슷한 옵션을 제공합니다.
  + 자동 완성을 실행할 텍스트 입력을 지정하는 것 외에도 결과를 바이어스하기위한 위도/경도 영역 인 경계를 추가 할 수 있습니다.
  + 타입 제한도 추가할 수 있습니다. 예를 들어 정확한 주소 또는 사업용 시설만을위한 주소 등의 유형을 제한합니다.
  + 또한 특정 국가에 대한 결과를 제한하기 위해서 컴포넌트 제한도 추가할 수 있습니다.
-  지도 경계를 위해 zoom 을 영역 자동완성으로 바이어스합니다.
- 기존의 geocoder 나 distances matrix 서비스와 비교해봅니다.
  + A 장소에서 우선 zoom 해서 자동완성과 기존 서비스로 검색을 해봅니다.
  + 다음 B 로 지도 경계를 옮겨서 검색을 해보면 자동완성은 새로운 장소를 자동완성하는 반면, 기존 서비스는 A 장소에 대한 것만 검색이 될 것입니다.

## Place Autocomplete 02
- 자동완성 대신 검색 박스로 사용자 검색 경험을 높여보도록 하겠습니다.
- 검색 박스는 자동완성처럼 사용자가 타이핑하는걸 예측합니다.
  + 차이점은 검색 박스는 Google Places API 에서 정의한 장소를 포함할 수 있는 확장된 예측 목록을 지원합니다.
  + 게다가 검색 쿼리를 제안합니다.(예: A 주위의 카페)
  + 하지만 자동완성처럼 검색을 제한하진 못합니다.
### 앱에 적용하기
[Github link 12](https://github.com/udacity/ud864/blob/master/Project_Code_12_FasterIsBetterPlacesAutocompletePart2.html), [강의 링크](https://youtu.be/zNJsj69DnAM)

## Devil in the Details - Places Details
[강의 링크](https://youtu.be/-_1MMBU0EH0)
- places 라이브러리는 많은 데이터베이스를 가지고 있습니다.
- 자동완성과 검색 박스가 return 한 장소들은 place ID 라는 고유 식별자를 가집니다.
  + place ID 를 lat,lng 이나 많은 서비스(distance matrix, directions API 등)들처럼 전달할 수 있음을 알아야 합니다.
- Place ID 는 위치 혹은 장소에 대한 어마어마한 세부 정보들을 얻을 수 있는 키입니다.
  + 검색 박스로 결과를 얻을 때 place ID 를 얻게 됩니다.
- [구글 개발자 블로그: Place ID](https://developers.google.com/places/web-service/place-id), [구글 개발자 블로그: Place details](https://developers.google.com/places/web-service/details)
### 연습
- 장소 정보 웹 서비스에서 place ID 를 사용하여 사용 가능한 세부 정보 유형을 확인하는 예시입니다.
```
https://maps.googleapis.com/maps/api/place/details/json?placeid=ChIJN1t_tDeuEmsRUsoyG83frY4&fields=name,rating,formatted_phone_number&key=YOUR_API_KEY
 ```
 - 전화번호, 운영 시간, 관련 사진, 리뷰, 평가 등등이 나옵니다. 이것을 비즈니스를 의미하는 establishment 라고 부릅니다.
  + 사진들은 사진 레퍼런스 ID 를 가집니다. 사진 레퍼런스 ID 및 wif 를 사용해 사진 레퍼런스 ID 를 사용하는 또 다른 간단한 URL 을 작성할 수 있습니다.
- 이번에는 place photo 요청을 해봅니다.
```
https://maps.googleapis.com/maps/api/place/photo?maxwidth=400&photoreference=...
```
  + 장소 세부사항 요청으로 해당 장소에 대한 사진을 보여줍니다.
- 부동산 앱에 적용해봅니다. [Github link 13](https://github.com/udacity/ud864/blob/master/Project_Code_13_DevilInTheDetailsPlacesDetails.html)
  + 지도 API 를 요청할 때 꼭 Places 라이브러리를 불러야 한다는 걸 명심하셔야 합니다.
- github 링크의 코드 650번에서 에러가 있습니다. `id: place.id` 는 `id: place.place_id`여야 합니다.
```javascript
// Create a marker for each place.
var marker = new google.maps.Marker({
    map: map,
    icon: icon,
    title: place.name,
    position: place.geometry.location,
    id: place.place_id
});
```

## 구글 Places API 의 검색 방법들
### Nearby 검색
- Nearby 검색은 지정 지역 내의 장소를 찾는 가장 간단한 방법입니다. 센터와 검색할 반지름을 지정해서 찾습니다.
- 이 검색은 기본적으로 20개의 장소 목록을, 프리미엄 플랜 라이선스가 있으면 60개의 장소 목록을 반환합니다.
### Text 검색
- Text 검색을 사용하면 사용자 또는 시스템이 텍스트 쿼리를 사용해서 지정할 위치 없이도 장소 검색이 가능합니다. 이 검색방식은 위치 정보를 전달해서 편향될 수도 있습니다.
### Radar 검색
- Rader 검색은 Nearby 검색과 동일한 파라미터를 지정할 수 있고, 반환되는 데이터가 제한적이지만 최대 200개의 결과를 반환합니다.
- 모든 검색에 대해 next_page_token 의 값을 새 검색의 pagetoken 파라미터에 전달하여 다음 결과 집합을 볼 수 있습니다.
  + next_page_token 이 null 이거나 반환되지 않으면 결과는 나오지 않습니다.

- 아래는 필수적인 파라미터, 옵션 파라미터의 목록이며 이것들과 함께 사용할 수 있는 검색 방법들입니다.
![검색 방법 1](https://lh3.googleusercontent.com/qFxkShPh5qNPpNzzmtNKjhUKZuOD3NM6O3Yx9ajCgyxjf8HZkoBv4UzfomkkYhfVo9xWcqpEN76oIp3DSTL6=s0#w=651&h=370)
![검색 방법 2](https://lh3.googleusercontent.com/D87WymWM5s9-MLFYkx965W0orXtP_GiQaejhArKE1Tw71aEK8wRnuOEh2fkAaPxmDaAHXIpU9JQ016B0fs7_=s0#w=651&h=867)
![검색 방법 3](https://lh3.googleusercontent.com/gaqcuIF8lZAs7ApNDUF35LV5PqzySnh-8uZ-7RWgi-saBLli-TzdEmDpEZ7_-3uUWmITSN5DRc9KqHww6mM=s0#w=651&h=390)

## TimeZone API
- 만약 미팅을 베를린에 있는 동료와 잡았다고 합시다. 위치를 기준으로 현재 위치하는 시간대를 찾을 빠른 방법은 어떤 걸까요.
- 구글 지도 API 는 TimeZone API 서비스를 가지고 있습니다.
  + 위도/경도 및 day 타임 스탬프의 형태로 위치를 전달할 수 있습니다. 서비스는 일광 절약 시간제를 적용할지 여부를 서비스에 알려주며 서비스는 해당 좌표에 따라 시간대를 되돌려줍니다.
  + 또한 일광 절약 시간과 협정 세계시(UTC)를 나타내는 UTC 로부터의 해당 시간대의 오프셋으로 인해 해당 시간대가 현재 상쇄되었는지 여부를 알려줍니다.
  + [구글 개발자 블로그: TimeZone API](https://developers.google.com/maps/documentation/timezone/intro)
### 연습
```
https://maps.googleapis.com/maps/api/timezone/json?location=38.2955003,141.4179154&timestamp=1331766000&key=YOUR_API_KEY
```
```
{  
  dstOffset	0
  rawOffset	32400
  status	"OK"
  timeZoneId	"Asia/Tokyo"
  timeZoneName	"Japan Standard Time"
}
```
- 결과는 일본 기준 시간입니다.
  + dstOffset 0 : 현재 일광 절약 시간에 대한 오프셋이 없습니다.
  + rawOffset 32400 : UTC 로부터의 오프셋은 32400초(9시간)입니다.
- 참고로 위도/경도는 고양이가 사람보다 많다는 시로지마 섬입니다. :smiley_cat:

## Geolocation API
- Geolocation API 는 현재 위치을 알게 해주는 서비스입니다. 이 서비스는 Geolocation 이 기본적으로 내장되어 있지 않은 기계, 장치에 유용합니다. Unix, Linux 등으로 작동하는 기계는 현재 위치를 알 필요가 있습니다.
- Geolocation API 는 기계들이 장소에 대한 정보를 얻게 해주는 웹 서비스를 제공합니다.
  + 기계는 장소, 정확한 위도/경도를 얻기 위해 셀 타워나 와이파이 노드에서 정보를 Geolocation API 에 전달할 수 있습니다.
- GPS 가 약하거나 사용이 불가능하고, 혹은 실내에서 정보를 찾고자 할 때에는 최고의 API 입니다.
