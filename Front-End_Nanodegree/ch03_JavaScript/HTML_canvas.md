# HTML canvas (Udacity)
## Basic
HTML5에 추가된 <canvas> 요소는 자바스크립트에서 스크립트를 통해 그래픽을 그리는 데 사용됩니다. 그래프, 사진, 애니메이션, 실기간 비디오, 렌더링에도 쓰입니다.
- [W3School Canvas 레퍼런스](https://www.w3schools.com/tags/ref_canvas.asp)
- [MDN 소개 페이지](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas)
### 이미지 로딩, 저장
```HTML
<body>
  <canvas id="c" width="300" height="300"></canvas>

  <script>
    let c = document.querySelector('#c');
    let ctx = c.getContext('2d');

    let image = new Image();
    image.onload = function () {
      console.log('loaded image');
      ctx.drawImage(image, 0, 0, c.width, c.height);
      //이미지 저장에 쓰이는 코드입니다. toDataURL 은 사진의 텍스트 문자열을 만듭니다.
      let savedImage = c.toDataURL();
      //새 창에 이미지를 띄웁니다.
      window.open(savedImage);
    }
    image.src = '링크.jpg'
  </script>
</body>
```
  + `drawImage` 는 캔버스 객체에서 제공합니다. 매개변수로는 컨텍스트에 그릴 element, x축, y축, 너비, 높이 입니다.
  + 위 toDataURL 은 실행되지 않을 것입니다. 보여줄 이미지를 직접적으로 호스팅하지 않아서입니다. (Uncaught SecurityError: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported.)
  + 마우스 우클릭 없이 이미지를 저장하려 서버를 만들어야 합니다.
- `createImageData()` : 새로운 빈 이미지데이터 객체를 만듭니다.
- `getImageData()` : 지정된 사각형의 픽셀 데이터를 캔버스에 복사하는 이미지데이터 객체를 return 합니다.
- `putImageData()` : 지정된 이미지데이터 객체의 데이터를 다시 캔버스에 넣습니다.

### 로컬 웹 서버
- 파이썬 :
  1. Open the terminal (Mac and Linux) or command prompt (Windows).
  2. cd to a directory where you've saved an HTML file. For example, cd ~/Documents/mysite/. (Mac and Linux: here's more on cd if you aren't familiar with the terminal. Windows: here's more info on cd for you).
  3. Run python --version. If Python is installed, you'll see "Python X.Y.Z". The "X" will be 2 or 3, indicating Python 2 or 3. If nothing shows up or the command produces an error, I recommend that you download Python.
  4. If you have Python 2, run python -m SimpleHTTPServer 8000. If you have Python 3, run python -m http.server 8000.
  5.  Navigate your browser to http://localhost:8000/. If there is a file called index.html in the directory where you ran the command from step 4, then it should automatically show up! If not, you should see the files in that directory listed. Click on an HTML file and watch it load! Congrats! You've got a server running.
- 노드 :
  1. Run node --version from the terminal or command line. If nothing shows up or you get an error, install node
  2. Run npm install -g http-server.
  3. Serve files with http-server ~/Documents/mysite -p 8000 (replace ~/Documents/mysite with the path to your project's directory!).
  4. Navigate your browser to http://localhost:8000 to test!

## 사각형 그리기
```javascript
var c = document.querySelector("#c");
var ctx = c.getContext("2d");
//사각형의 배여색을 정합니다.
ctx.fillStyle = "blue";
// Start at (0,0) and draw a 50px x 50px blue rectangle.
ctx.fillRect(0,0,50,50);
// Start at (0,0) and clear a 25px x 25px rectangle.
ctx.clearRect(0,0,25,25);
```
- 모든 캔버스를 지우려면 `ctx.clearRect(0, 0, c.width, c.height);` 를 실행합니다.

## Path
```javascript
var c = document.querySelector("#c");
var ctx = c.getContext("2d");
// Start at (0,0) and draw a 50px x 50px blue rectangle.
ctx.fillRect(100,100,100,100);
// Start at (0,0) and clear a 25px x 25px rectangle.
ctx.clearRect(50,50,50,50);

ctx.beginPath();
//그릴 위치를 정합니다. x축 y축
ctx.moveTo(10, 10);
ctx.lineTo(50, 50);
ctx.lineTo(50, 10);
ctx.lineTo(10, 10);
//둘 중 하나를 합니다.
ctx.fill(); //채워진(검은색) 삼각형
ctx.stroke(); //채워지지 않은(하얀색) 삼각형
```
- `fill()` : 현재 드로잉을 채웁니다.
- `stroke()` : 정의했던 경로에 실제로 그립니다.
- `beginPath()` : 경로를 시작하거나 현재 경로를 새로고침합니다.
- `moveTo` : 라인을 만들지 않고 캔버스의 특정 위치로 경로를 이동합니다.
- `lineTo()` : 새로운 포인트를 더하고 거기서부터 특정 위치까지 선을 긋습니다.

## 움직이는 물체
- `scale(x, y)` : x 하고 y 값만큼 곱합니다. (x 축에서 x 배, y 축에서 y 배)
- `translate(x, y)` : 모든 그리기 명령들을 가로는 x 만큼 수직, 세로는 y 만큼 수평 이동합니다. (x 픽셀 뒤의 모든 elements 를 오른쪽으로 이동, 40 픽셀 아래로 이동)
- `ctx.rotate(x)` : 중심부의 특정 라디안( 각도 * (Math.PI/180) )만큼 회전시킵니다.
- 참고로 먼저 물체를 `scale` 하고 나서 `rotate` 한 후에 `translate` 을 마지막으로 해야합니다.
- `transform()` : 현재의 변형 매트릭스를 변경합니다.

## 캔버스 상테 저장 및 복원
- 모든 캔버스 객체는 드로잉 states 의 스택을 포함합니다. (스택 : 새로운 항목을 한 쪽에서만 푸시할 수 있는 데이터 구조, Last In First Out 구조를 가짐)
```javascript
//버전 1
let c = document.querySelector("#c");
let ctx = c.getContext("2d");

ctx.fillStyle = "blue";
ctx.fillRect(0,0,50,50);

ctx.fillStyle = "green"
ctx.fillRect(100,100,10,10);

ctx.fillStyle = "blue";
ctx.fillRect(200,10,20,20);

//버전 2.
let c = document.querySelector("#c");
let ctx = c.getContext("2d");

ctx.fillStyle = "blue";
ctx.fillRect(0,0,50,50);

// Save state with blue fill
ctx.save();
ctx.fillStyle = "green";
ctx.fillRect(100,100,10,10);
// Restore to blue fill
ctx.restore();

ctx.fillRect(200,10,20,20);
```
- 캔버스가 저장할 수 있는 것들 : 현 위치, strokeStyle, fillStyle, font, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation, textAlign, textBaseline, The current clipping region

## 색깔
```javascript
//여기에 위치시켜야 합니다.
ctx.strokeStyle = '#33CC33';

ctx.strokeRect(50, 50, 100, 100);
ctx.beginPath();
ctx.moveTo(75, 75);
ctx.lineTo(125, 125);
ctx.lineTo(125, 75);
//해당 도형을 채우는 색
ctx.fillStyle= "blue";
/* 스트로크도형의 선색깔. 그러나 여기서 선언하면 실행이 안됩니다.
strokeRect 로 사각형을 그린 다음에 색을 지정했기 때문입니다.
*/
//strokeStyle = "#33CC33";
```

## 글자 그리기
```javascript
ctx.strokeText("hello", 50, 10);
//폰트 색을 지정할 수도 있습니다.
ctx.strokeStyle = '#33CC33';
```
```javascript
ctx.font = '36pt Impact';
ctx.textAlign = "center";

ctx.fillStyle="white";
ctx.fillText("canvas meme", c.width / 2, 40);

ctx.strokeStyle = 'black';
ctx.lineWidth = 3;
ctx.strokeText("canvas meme", c.width / 2, 40);
```
- `fillText()` : 채워진 글자를 캔버스에 그립니다.
- `strokeText()` : 채워지지 않은 글자를 캔버스에 그립니다.
- `measureText()` : 지정된 글자의 너비를 포함한 객체를 return 합니다.

## 비디오
- `requestAnimationFrame` 을 사용해서 비디오를 캔버스에 재생합니다.
- 이것 뿐만 아니라 키보드, 마우스, 오디오 등의 인풋을 다룰 프로세스도 필요합니다.
  + game 반복문은 계속해서 앱이나 게임이 사용중일 때 실행되는 이벤트 단계입니다. `requestAnimationFrame` 은  앱을 적극적으로 보면서 초당 60 프레임에 가까운 앱 실행을 보장한다는 점에서 대부분의 어려운 작업을 처리합니다.
- game 반복문 불러오기
```javascript
function draw() {
  requestAnimationFrame(draw);
  processInput();
  moveObjectsAndEnemies();
  drawAllTheThings();
}
```
- 키보드 인풋 작성하기 : `Kibo` 를 사용합니다.
```javascript
var key = new Kibo();
key.down(['up', 'w'], function() {
  //캔버스에 작업
});
key.up(['enter', 'q'], function() {
  //캔버스에 작업
})
```
- 마우스 인풋 작성하기 : `click` 과 `mouseDown` 이벤트를 사용합니다. 다만 클릭한 캔버스의 위치를 파악해야 합니다. 마우스 클릭 시 `clientX` 와 `clientY` 위치를 return 하는데 이를 여기서 offsetLeft 와 offsetTop 을 빼냅니다.
```javascript
var c = document.querySelector("canvas");

function handleMouseClick(evt) {
        x = evt.clientX - c.offsetLeft;
        y = evt.clientY - c.offsetTop;
        console.log("x,y:"+x+","+y);
}
c.addEventListener("click", handleMouseClick, false);
```

## 애니메이션 기본
한 장면을 그리려면 캔버스를 비우고(`clearRect`)->캔버스 상태를 저장->애니매이션 도형 그리기->캔버스 상태를 복원 이라는 단계를 밟습니다.
- 정해진 시간마다 특정 함수를 불러 애니메이션을 만드는 방법과 사용자가 키보드나 마우스 이벤트로 작동하는 애니메이션을 만드는 방법 두 가지가 있습니다. [MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas/Tutorial/Basic_animations)에서 태양계, 시계, 파노라마 사진 애니메이션 예시 코드를 볼 수 있습니다.
