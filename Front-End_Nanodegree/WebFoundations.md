# javascript nanodegree
## responsive web
- 처음 HTML 파일에서 `<meta name="viewport" content="width=device-width, initial-scale=1.0">` 를 적어두면 기기의 화면크기에 따라 유동적으로 화면을 조절하게 됩니다.
- 기기마다 화면이 달라 CSS 의 크기를 상대적 (relative) 로 잡는 것이 좋습니다.
  + `em` : `px` 은 절대적 크기라면 `em` 은 상대적인 크기를 나타냅니다.
  + `width: 100px;` 보다는 `width: 100%` 로 상대적으로 잡아야 반응형 웹이 됩니다..
- 사람의 손가락에 맞춰 클릭할 버튼의 크기는 최소 `min-weight: 48px;`, `min-height: 48px;` 로 맞추는 것이 좋습니다.
  + `a { padding: 1.5em; }` 으로 하면 글자 사이즈의 1.5배로 간격을 잡아줍니다.
- 화면을 구성할 때 작은 화면에서 큰 화면으로 고려하면 좋습니다. 중요한 정보에서 살을 붙여 나가는 방식이고, 처음부터 데이터의 전송이나 사용자의 경험을 따질 수 있어서입니다.

## build up
- 각 기계마다 다른 형태의 CSS 를 적용시키는 것이 좋습니다.

### media query
- 기계마다 스타일시트의 적용범위를 제한하는 간단한 방법 중 하나입니다. true 가 return 되면 해당 스타일이 실행되고, 아니면 스타일이 적용되지 않습니다.
- 방법 1 : 많은 양의 조그만 파일을 가지고, HTTP 요청이 많습니다.
```HTML
<link rel="stylesheet" media="screen and (min-width:500px)" href="style.css">
```
  + 미디어 타입이 screen 이고 최소범위가 500px 일 경우에 true 가 return 됩니다.
- 방법 2 : 요청은 적지만 파일은 큽니다. 중복해서 적용이 가능합니다.
```CSS
@media screen and (min-width: 500px) {
  body { background-color: green; }
}
@media screen and (min-width: 700px) {
  body { font-color: orange; }
}
/* 700px 가 넘으면 배경색과 글자 색이 같이 적용됩니다. */
```
- 방법 3 : 이 방법은 성능상 권장되지 않습니다.
```CSS
@import url("no.css") only screen and (min-width: 500px);
```
- breakpoints
  + 페이지가 레이아웃을 바꾸는 지점을 breakpoint 라고 합니다. 페이지 크기 제한을 어떻게 지정했는지에 따라 여러 개가 있을 수도 있습니다.
  + 가장 작은 사이즈에서부터 적당한 breakpoint 를 찾아나가면 됩니다.
- 복잡한 media query : 미디어 쿼리 를 하나 이상 작성할 수도 있습니다.
```CSS
@media screen and (min-width: 300px;) and (max-width: 500px;) {
  .yes {
    /* 미디어쿼리 가 적용됩니다. */
    opacity : 1;
  }
  .no {
    /* 미디어쿼리 가 적용되지 않습니다. */
    opacity: 0;
  }
}
```

### grid fluid system
- Bootstrap 에서는 grid 레이아웃을 지원합니다. element 의 크기를 1, 2, 3... 사이즈로 지정해놓고 사용합니다. 반응형 웹에서도 적용이 됩니다.

### Flexbox
- 스크린의 크기에 따라 레이아웃을 조절할 수 있는 쉽고 강력한 방법입니다.
```HTML
<style type:"text/css">
.container {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  .box { width: 150px;}
}
/* item 의 순서를 조절할 수도 있습니다. 700px 전에는 아래 순서대로 나열됩니다.또한 크기도 지정가능합니다.*/
@media screen and (min-width: 700px) {
  .dark_blue { width: 100%; order: 3; }
  .light_blue { width: 50%; order: 1; }
  .green { width: 70%; order: 2; }
}
</style>

<body>
  <div class="container">
    <div class="box dark_blue"></div>
    <div class="box light_blue"></div>
    <div class="box green"></div>
  </div>
```

### 반응형 웹 패턴들
- `Column Drop`
```HTML
<style type:"text/css">
.container {
  display: flex;
  flex-wrap: wrap;
}
.box {
  width: 100%;
}
@media screen and (min-width: 450px) {
  .dark_blue {
    width: 25%;
  }
  .light_blue {
    width: 75%;
  }
}
@media screen and (min-width: 550px) {
  .dark_blue, green {
    width: 25%;
  }
  .light_blue {
    width: 50%;
  }
}
</style>

<div class="container">
  <div class="box dark_blue"></div>
  <div class="box light_blue"></div>
  <div class="box green"></div>
</div>
```
- `Mostly Fluid`
```HTML
<style type:"text/css">
.container {
  display: flex;
  flex-wrap: wrap;
}
.box {
  width: 100%;
}
@media screen and (min-width: 450px) {
  .light_blue, .green {
    width: 50%;
  }
}
@media screen and (min-width: 550px) {
  .dark_blue, .light_blue {
    width: 50%;
  }
  .green, .red, .orange {
    width: 33.333333%;
  }
}
@media screen and (min-width: 700px) {
  .container {
    width: 700px;
    margin-left:auto;
    margin-right: auto;
  }
}
</style>
<div class="container">
  <div class="box dark_blue"></div>
  <div class="box light_blue"></div>
  <div class="box green"></div>
  <div class="box red"></div>
  <div class="box orange"></div>
</div>
```
- `Layout Shifter`
```HTML
<style type:"text/css">
  .container {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
  }
  .box {
    width: 100%;
  }
  @media screen and (min-width: 500px) {
    .dark_blue {
      width: 50%;
    }
    #container2 {
      width: 50%
    }
  }
/* 순서는 0부터 시작입니다. -1 -> 0(container2. order 가 없으면 0으로 생각합니다.) -> 1 */
  @media screen and (min-width: 600px) {
    .dark_blue {
      width: 25%;
      order: 1;
    }
    #container2 {
      width: 50%;
    }
    .red {
      width: 25%;
      order: -1;
    }
  }
</style>
<div class="container">
  <div class="box dark_blue"></div>
  <div class="container" id="container2">
    <div class="box light_blue"></div>
    <div class="box green"></div>
  </div>
  <div class="box red"></div>
</div>
```
- `Off Canvas` : 위의 패턴만큼 자주 쓰이지는 않습니다.
```HTML
<script>
var menu = document.querySelector('#menu');
var drawer = document.querySelector('.nav');

menu.addEventListner('click', function (e) {
  drawer.classList.toggle('open');
  e.stopPropagation();
});
</script>

<style>
html, body, main {
  height: 100%;
  width: 100%;
}
nav {
  width: 300px;
  postion: absolute;
  /* drawer 의 off canvas 입니다. 화면에서 벗어나기 위해 -300px 로 지정했습니다. */
  -webkit-transform: translate(-300px, 0);
  /* nov bar 는 왼쪽 -300px 에서 시작합니다. */
  transform: translate(-300px, 0);
  /* drawer 의 애니메이션입니다. */
  transition: transform 0.3s ease;
}
nav.open {
  /* transform 을 리셋합니다. */
  -webkit-transform: translate(0, 0);
  transform: translate(0, 0);
}

@media screen and (min-width: 600px) {
  nav {
    position: relative;
    transform: translate(0, 0);
  }
  body {
    display: flex;
    flex-flow: row nowrap;
  }
  /* main element 에 flex grow 를 하나 더합니다. 더하면 full remaining width 를 보여줍니다. */
  main {
    width: auto;
    flex-grow: 1
  }
}
</style>

<body>
  <nav id="drawer" class="dark_blue"></nav>
  <main class="light_blue">☰</main>
</body>
```

## Optimization (최적화)
### 반응형 이미지
- [구글 개발자 문서](https://developers.google.com/web/fundamentals/design-and-ux/responsive/images)
- 이미지에 대해서 크기를 상대적으로 지정하면 이미지가 오버플로우하는걸 막을 수 있습니다.
```CSS
img, embed, object. video {
  max-width: 100%
}
```
- `srcset` 속성으로 브라우저 기기 특성에 따라 최적의 이미지를 선택하게 해줍니다.
```HTML
<!-- 2x 디스플레이에서는 2x 이미지 사용,
제한된 네트워크일 때는 2x 디스플레이더라도 1x 이미지로 사용 -->
<img src="photo.png" srcset="photo@2x.png 2x">
```
- `picture` element 를 사용해서 이미지를 보조할 수 있습니다.
```HTML
<picture>
  <source media="(min-width: 800px)" srcset="head.jpg, head-2x.jpg 2x">
  <source media="(min-width: 450px)" srcset="head-small.jpg, head-small-2x.jpg 2x">
  <img src="head-fb.jpg" srcset="head-fb-2x.jpg 2x" alt="a head carved out of wood">
</picture>
```

### 반응형 테이블
- `Hidden Columns` : 뷰포인트 화면의 크기가 작을 때 잘린 컬럼들입니다.
```HTML
<!-- 처음에는 shortName 과 final 만 보여주다가 화면이 점점 커지면서
longName 과 각 inning 들이 보이게 됩니다. -->
<style type="text/css">
body {
  margin: 1em;
}
@media screen and (max-width: 400px) {
  .longName {
    display: none;
  }
  .inning {
    display: none;
  }
}
</style>

<tr>
  <td>
    <span class="shortName">TOR</span>
    <span class="longName">Toronto Blue Jays</span>
  </td>
  <!-- <td class="inning"></td>  반복반복 -->
  <td class="final">5</td>
</tr>
```
- `No more Tables` 
  + 뷰포인트 화면이 아무리 작더라도 모든 데이터를 보여주는 방법입니다.
  + 작은 화면에서는 세로로 나열하고, 화면이 커지면 가로로 나열합니다.
  + [예시 코드](https://codepen.io/JohnMav/pen/BoGJNy)
- 스크롤 남기기
```HTML
<style>
/* breakpoint 를 잡기보다는 너비를 그대로 살리고 스크롤하도록 만듭니다. */
  div.contained_table {
    width: 100%;
    overflow-x: auto;
  }
</style>
<div class="contained_table">
  ...
</div>
```

### Fonts
- 이상적인 글 한 줄의 길이는 45~90 character 입니다. 웹 페이지에서는 65개 이내가 적당합니다.
```CSS
@media screen and (max-width: 500px) {
  .font {
    font-size: 14px;
    line-height: 1.25em;
  }
}
```

### minor breakpoints
- 큰 변화보다는 작은 변화(element 에 padding 을 준다던가, font 의 크기를 키운다던가) 를 주는 것도 좋습니다.
