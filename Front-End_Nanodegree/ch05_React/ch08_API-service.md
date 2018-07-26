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
