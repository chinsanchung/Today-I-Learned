# CSS 렌더링 with 코드스피츠76 2강
## 박스 모델
### 박스 사이징
```HTML
<style media="screen">
  div {
  width: 200px;
  height: 200px;
  padding: 10px;
  border: 10px solid #000;
  display: inline-block;
  }
  .aaa {
  background-color: red;
  }
  .bbb {
  background-color: blue;
  box-sizing: border-box;
  }
</style>
<div class="aaa"></div><div class="bbb"></div>
```
![sizing001](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing001.jpg)

aaa 는 콘텐츠 박스 사이즈 200*200에서 패딩 10, 보더 10을 상하좌우 더해서 총 240*240이 됐습니다.
  - 콘텐츠 박스는 CSS 의 기본값입니다. `box-sizing:border-box`을 주지 않으면 콘텐츠 박스의 내부를 보존할 수 있습니다.
bbb 는 `box-sizing:border-box`를 줬습니다. 그러면 콘텐츠 박스 안에 보더, 패딩을 우겨넣습니다. 그래서 bbb 의 콘텐츠 박스는 180*180입니다.
  - 일정 사이즈라는 제약사항에 넣어야 할 필요가 있을 때 사용합니다.
### 보더 & 백그라운드
```CSS
div {
width: 200px;
height: 200px;
border: 10px dashed rgba(0,0,0,0.5);
display: inline-block;
}
```
![sizing002](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing002.jpg)

도형을 보면 보더까지도 배경색을 칠했음을 알 수 있습니다. 그림을 그려주는 fragment 영역은 border-box 까지임을 알 수 있습니다.

## 박스 쉐도우
이번엔 bbb 에 박스 쉐도우를 줘봅니다.
```CSS
.bbb {
background-color: blue;
box-shadow: 0 0 0 10px rgba(255,255,0,0.5);
}
```
![sizing003](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing003.jpg)

현재 박스 쉐도우에 x 0, y 0, 블러 0, 두께 10 입니다. 그래서 보더 10px 과 같은 의미가 됐습니다.
사진을 보면 이것은 지오매트리에는 전혀 영향을 주지 않습니다. 레이아웃에 영향을 주지 않고 색칠만 한 것입니다.
포지션 absolute 와 fixed 만 z-index 로 순서를 가진다고 생각하는데 인라인 요소도 순서를 가집니다.
### 박스 쉐도우 & relative
```CSS
.aaa {
background-color: red;
position: relative;
}
```
![sizing004](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing004.jpg)

같은 도형인데 relative 만 줬을 뿐인데도 바뀝니다. 그리고 나서 꺼내고 relative 가 다시 그린 것입니다.
  - relative 는 normal flow 를 그린 다음에 위로 뜬 상태로 상대 위치를 계산하고 다시 그립니다.
### 박스 쉐도우 inset
```CSS
.bbb {
background-color: blue;
box-shadow:inset 0 0 0 10px #aa0;
}
```
![sizing005](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing005.jpg)

이제는 박스 바깥쪽이 아니라 보더 박스 안쪽으로 박스 쉐도우를 그립니다.(패딩하고 같은 위치에서 그리는 것입니다.)
### 박스 쉐도우 sandwich
```CSS
.bbb {
background-color: blue;
box-shadow:0 0 0 10px purple, inset 0 0 0 10px #aa0;
}
```
![sizing006](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing006.jpg)

박스 쉐도우의 대단한 점은 콤마로 몇 개의 그림을 그릴 수 있다는 것입니다. 이걸로 총 3개의 보더를 그렸습니다.
### 박스 쉐도우 여러 레이어
```CSS
.bbb {
background-color: blue;
box-shadow:
  0 0 0 10px #aa0,
  0 0 0 20px #0a0,
  inset 0 0 0 10px #aa0,
  inset 0 0 0 20px #0a0;
}
```
![sizing007](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing007.jpg)

참고로 20은 눈에는 10처럼 보일 것입니다. 겹쳐지는 것일 뿐 확장하는게 아닙니다. 그런데 10, 20 이렇게 썼으니 덮어씌워지진 않을까요? 걱정할 필요 없습니다. 스택처럼 쌓이는 방식이기 때문입니다.
그래서 선언의 반대 순서로 채워집니다.(가장 마지막에 선언한 것부터 채워지는 겁니다.)만약 원래 순서대로 그려진다면 가려지게 될 것입니다.
### 박스 쉐도우 & border-radius
```CSS
.bbb {
background-color: blue;
box-shadow:
  0 0 0 10px #aa0,
  0 0 0 20px #0a0,
  inset 0 0 0 10px #aa0,
  inset 0 0 0 20px #0a0;
border-radius: 50%
}
```
![sizing008](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing008.jpg)

이걸 사용해서 당구공에 음영을 넣어서 그린다던가 하는 방식도 표현할 수 있습니다.
### 박스 쉐도우 애니메이션
```CSS
@keyframes ani {
  from{
    transform: rotate(0);
    box-shadow:
      0 0 0 0px #aa0,
      0 0 0 0px #0a0,
      inset 0 0 0 0px #aa0,
      inset 0 0 0 0px #0a0;
  }
  to {
    transform: rotate(360deg);
    box-shadow:
      0 0 0 10px #aa0,
      0 0 0 20px #0a0,
      inset 0 0 0 10px #aa0,
      inset 0 0 0 20px #0a0;
  }
}
/* 도형 */
.bbb {
background-color: blue;
border-radius: 50%;
animation: ani 0.4s infinite
}
```

## 아웃라인
### stiched
스티치는 많이 쓰이는 기법입니다. 아웃라인과 보더를 사용합니다.
```HTML
<style>
.stiched {
  background-color: black;
  border-radius: 15px;
  outline: 10px solid rgba(0, 255, 0, 0.5);
  border: 1px dashed #fff;
  color: #fff;
  box-shadow: 0 0 0 10px red;
}
</style>
<div class="stiched">stiched</div>
```
![sizing009](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing009.jpg)

현재 아웃라인과 박스 쉐도우가 10픽셀로 겹치고 있습니다.
  - 우선 아웃라인은 border-radius 를 따라가지 않고 직선으로 그렸습니다.
  - 박스 쉐도우는 border-radius 를 따라 원형으로 그립니다. 박스 쉐도우는 아웃라인의 초록과 섞여서 다른 색이 됐습니다.
  - 보더는 보더 라인에다 그려집니다.

## 오프셋
보통 CSS 명령을 추상적으로 내립니다. 그래도 브라우저가 지오매트리 계산을 끝내고 fixed 영역 숫자를 구하는데 이 숫자들을 오프셋이라고 합니다.
오프셋은 이미 계산한 것들이므로 변경이 불가능합니다. 그리고 브라우저가 실제 계산값은 본인은 모릅니다.
브라우저는 일일이 요소들을 계산하면 부담스러우므로 한번에 계산하려 하는데 이렇게 한번에 모아서 계산하려는 걸 프레임이라 합니다.

그런데 요소에 오프셋을 요청하면 즉시 재계산을 해버립니다. 레이아웃을 다 그린 다음에 오프셋을 요청해야 합니다. 안그러면 다시 계산을 하므로 느려집니다.
### offset parent
오프셋을 계산하려면 우선 어디가 기준점인지를 파악하는게 중요합니다. 이 기준점이 바로 오프셋 부모입니다. 오프셋 부모는 DOM 부모가 아니라는 것을 알아야 합니다.
  - 특히 absolute 포지션에서는 DOM 부모를 아예 무시합니다.
- 오프셋 부모가 null 인 경우
  - ROOT, HTML, BODY
  - position: fixed..이것은 chrome 을 기준으로 그립니다.
  - out of DOM tree..createElement 로 만드는 경우가 이렇습니다. append 로 넣으면 오프셋이 생깁니다.
- 오프셋 부모를 찾는 방법
  - 부모의 position:fixed => null
  - 부모의 position: static 이 아닌 경우(absolute, relative) => ok
  - BODY => ok
  - TD, TH, TABLE => ok
  - 여기서 오케이는 부모, 부모의 부모로 오프셋을 찾을 때까지 올라간다는 뜻입니다.
예를 들어 자신의 포지션이 absolute 면 그림을 그리는 기준점은 내 부모들 중에서 포지션이 absolute 혹은 relative 인 것이 바로 기준점인 것입니다.
  - relative 가 static 에서 위치를 조정하는 개념인데 일반적으로 static 으로 그림을 그리는 도중 자연스럽게 안에 absolute 를 넣기 위한 컨테이너로 사용합니다.
### 오프셋 값
offsetLeft, offsetTop/offsetWidth, offsetHeight
offsetScrollTop,offsetScrollLeft, offsetScrollWidth, offsetScrollHeight
ios, 안드로이드에서는 offsetScrollWidth, offsetScrollHeight 가 진짜 페이지 크기입니다. offsetWidth, offsetHeight 는 화면의 크기일 뿐입니다.
### absolute
```HTML
<style>
.main {
  margin: 100px;
  width: 200px;
  height: 200px;
  background-color: yellow;
}
.red {
  width: 100px;
  height: 100px;
  position: absolute;
  background-color: red;
}
.blue {
  width: 100px;
  height: 100px;
  position: absolute;
  background-color: blue;
  left: 0;
}
</style>
<div class="main">
  <div class="red"></div><div class="blue"></div>
</div>
```
![sizing010](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing010.jpg)

파랑은 absolute 이고 left 값을 가졌으므로 오프셋을 찾습니다. 그런데 노랑은 static 부모라서 offset 을 가질 자격이 없습니다. 그래서 파랑은 BODY 를 부모로 여기고 위치를 잡습니다.
  - 그런데 left 만 지정하고 top 은 지정하지 않았습니다. 그러면 top 은 부모인 노랑을 기준으로 정합니다.
빨강은 absolute 인데도 기본값이므로 static 과 같은 위치를 가지고 있습니다.
```CSS
.blue {
  width: 100px;
  height: 100px;
  position: absolute;
  background-color: blue;
  left: 0;
}
```
![sizing010_2](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing010_2.jpg)

파랑에 top 도 줘봤습니다. 이제 오프셋을 부모를 찾아서 그것을 기준으로 위치를 정합니다.
- 포지션 `absolute`에서의 left, top, bottom, right 는 오프셋 부모로부터의 거리를 정합니다.
참고로 포지션 `static`은 normal flow 를 기준으로 그리므로 left, top, bottom, right 를 무시합니다. 그리고 포지션 `relative`에서의 이 값들은 normal flow 로 그린 이후의 거리를 계산합니다.
- 만약 `absolute`인데 float 를 준다면 어떻게 될까요. float 는 normal flow 일 때만 성립하므로(새로운 bfc 를 만들어야 하므로) `absolute`이면 float 를 무시합니다.
### position:relative 의 오프셋
```HTML
<style>
  .in {
    display: inline-block;
    width: 100px;
    height: 100px;
    border: 1px solid black;
  }
  .abs {
    width: 50px;
    height: 50px;
    position: absolute;
    left: 40px;
    top: 40px;
    background: #00f;
  }
</style>
<div class="in"></div>
<div class="in"></div>
<div class="in"></div>
<div class="in" style="position:relative;">
  <div class="abs"></div>
</div>
<div class="in"></div>
<div class="in"></div>
<div class="in"></div>
<div class="in" style="position:relative;">
  <div class="abs"></div>
</div>
<div class="in"></div>
```
![sizing011](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/sizing011.jpg)

클래스 abs 는 `absolute`로 기존의 in(`static`)으로 한다면 in 을 무시하고 BODY 를 오프셋으로 삼았을 것입니다. 그것을 막기 위해 바로 위 부모에 `relative`를 줬습니다.
`static`안에 `absolute`를 주고 싶으면 `relative`로 감싸야 한다는 개념은 굉장히 지주 쓰입니다.
- 참고 : TD, TH, TABLE => ok 라고 위에 했었습니다. 하지만 td 안에 다시 `relative`를 감싸야만 `absolute`를 제대로 사용할 수 있습니다. 그냥 td 안에 `absolute`를 넣으면 안됩니다.
