# Using the APIs in Practice
## Your Google Developer Project
- 지도 API 의 번호 키 말고도 map loads 라고 부르는 할당 숫자가 있다는걸 알아야 합니다. 이것은 무료로 사용할 수 있습니다.
- 구글 지도를 불러오고, 시작 함수를 사용해 지도 객체를 만들 때마다 지도 look 을 하나만 사용합니다. 자유롭게 실험하고 앱을 개발할 수 있는 충분한 할당량입니다.
- 구글의 개발자 콘솔에서 얼마나 지도를 많이 사용하는지를 볼 수 있습니다.
- API 키의 보안을 강화하기 위해 API 키를 사용할 수 있는 도메인을 허용 목록(whitelist)에 추가 할 수 있습니다.
  + 예를 들어 허용 목록에 aaa.com 을 넣습니다. 그리고 오직 그 사이트만이 키를 사용한 요청을 만들 수 있게 됩니다.
  + 그러면 그 사이트에서 API 키로 요청을 만들면 그것을 허가됩니다. 하지만 bbb.com 같은 다른 도메인에서 요청을 만들려고 하면 에러 메시지를 띄웁니다.
  + 그러면 누구도 자신의 키를 우연히라도 쓰지 못할 겁니다.
- 이러한 보안은 중요합니다. 쿼터(Quota) 절도는 일반적인 경우는 아니지만 보안을 중요시하도록 합시다.
- 대다수의 웹사이트와 어플리케이션은 무료로 주는 할당량에 충분히 만족합니다.
  + 하지만 만약 많은 양의 트래픽을 계속해서 발생시킨다면 공급을 제한해야만 합니다. 그리고 사용량만큼 추가로 요금을 내야 합니다. 공급량은 미리 제한을 정해둬서 초과하지 않게 만들 수 있습니다.
### QPD, QPS
- 일반적으로 서비스를 사용할 때 두 가지 타입의 cap 이 있습니다.
  + QPD(Queries Per Day) : 하루에 만들어지는 총 요청의 숫자입니다.
  + QPS(Queries Per Second) : 1초에 만들어지는 총 요청의 숫자입니다.
- 개발자 문서에 각 웹 서비스에 대한 한계를 적어뒀습니다.
- 만약 `status: "OVER_QUERY_LIMIT"` 라는 코드를 봤다면, 이 코드의 뜻은 QPD 또는 QPS 제한 중 하나에 도달했다는 것입니다.

## Premium Plan 세부사항
- Premium Plan 은 더 많은 양의 할당량을 사용하고 Google 기술 지원팀의 도움을 받을 수 있는 API의 독점적 기능들이 있습니다.
- 내부(internal) 도메인에 API 를 사용한다는 건 공개적으로 사용하지 않는다는 것입니다.

## 개발자 문서
- [구글 지도 개발자 문서](https://developers.google.com/maps/documentation/javascript/tutorial)
- [Geo 개발자 블로그](https://mapsplatform.googleblog.com/)
- [구글 개발자 유투브](https://www.youtube.com/user/GoogleDevelopers/videos)

## 리액트에서 구글 맵 사용하기
1. 방법 1: Map 컴포넌트에서 지도 데이터를 가져오고 App 컴포넌트로 출력하기 [참조 블로그](http://cuneyt.aliustaoglu.biz/en/using-google-maps-in-react-without-custom-libraries/)
```javascript
//Map component
class Map extends Component {
  componentDidMount() {
    const map = new window.google.maps.Map(document.querySelector('#map'), {
      center: { lat: 41.0082, lng: 28.9784 },
      zoom: 8
    });
  }

  render() {
    return (
      <div style={{ width: 500, height: 500 }} id="map" />
    );
  }
}
```
```javascript
//InfoWindow component
import React from 'react';
import { withStyles } from 'material-ui/styles';
import Card, { CardActions, CardContent, CardMedia } from 'material-ui/Card';
import Button from 'material-ui/Button';
import Typography from 'material-ui/Typography';

const styles = {
  card: {
    maxWidth: 345,
  },
  media: {
    height: 0,
    paddingTop: '56.25%',
  },
};

function InfoWindow(props) {
  const { classes } = props;
  return (
    <div>
      <Card className={classes.card}>
        <CardMedia
          className={classes.media}
image="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Bosphorus.jpg/397px-Bosphorus.jpg"
          title="Contemplative Reptile"
        />
        <CardContent>
          <Typography gutterBottom variant="headline" component="h2">
            Istanbul
          </Typography>
          <Typography component="p">
            Istanbul is a major city in Turkey that straddles Europe and Asia across the Bosphorus Strait. Its Old City reflects cultural influences of the many empires that once ruled here.

          </Typography>
        </CardContent>
        <CardActions>
          <Button size="small" color="primary">
            Share
          </Button>
          <Button size="small" color="primary">
            Learn More
          </Button>
        </CardActions>
      </Card>
    </div>
  );
}


export default withStyles(styles)(InfoWindow);
```
```javascript
//App component
createInfoWindow(e, map) {
    const infoWindow = new window.google.maps.InfoWindow({
        content: '<div id="infoWindow" />',
        position: { lat: e.latLng.lat(), lng: e.latLng.lng() }
    })
    infoWindow.addListener('domready', e => {
      render(<InfoWindow />, document.getElementById('infoWindow'))
    })
    infoWindow.open(map)
  }

  render() {
    return (
      <Map
        id="myMap"
        options={{
          center: { lat: 41.0082, lng: 28.9784 },
          zoom: 8
        }}
        onMapLoad={map => {
          const marker = new window.google.maps.Marker({
            position: { lat: 41.0082, lng: 28.9784 },
            map: map,
            title: 'Hello Istanbul!'
          });
          marker.addListener('click', e => {
            this.createInfoWindow(e, map)
          })
        }}
      />
    );
  }
```
