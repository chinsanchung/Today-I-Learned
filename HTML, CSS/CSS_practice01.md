# practice01 : CSS 로 버튼, 라디오, 블록 엘리먼트 연습
### CSS
```CSS
#main {
/*
  마진을 여기서 오토로 잡으면 좌우를 띄우고 가운데에 화면을 정렬합니다.
*/
  margin: auto;
/*
부모인 여기서 너비를 지정하면 깔끔하게
고정된 형태로 출력이 가능합니다.
*/
  width: 50%;
}
#top {
  font-size: 1.7em;
  background-color: green;
  text-align: center;
}
#side {
  background-color: orangered;
  text-align: center;
}
#contents {
  padding-left: 1.1em;
  background-color: orange;
}
h1, p {
/* 네거티브 마진 */
  margin-top: -0.5em;
}
.icon {
  display: inline-block;
  border-style: solid;
  border-width: 2px 10px 4px 20px;
  border-color: blue;
  border-radius: 50%;
  color: white;
}
.test {
  margin-top: 1.5em;
}
.hidden {
/* translate3d(x축, y축, z축) */
  transform: translate3d(0, -15em, 0);
/* 이동하는데 걸리는 시간 */
  transition: transform 0.2s;
}
.show {
  display: block;
/* 숨기는 쪽에서 translate3d 을 걸었기 때문에 보이는 쪽은 지정하지 않아도 됩니다. */
/* 시간을 지정하지 않으면 클릭하자마자 바로 나타나게 됩니다. */
  transition: transform 0.2s;
  background-color: yellow;
}
```

### JavaScript
```javascript
/*
  아이콘 클래스가 여러개라서 이 아이콘 변수는 여러 엘리먼트를 가지고 있습니다.
  그래서 이벤트리스너 연결이 불가능합니다.
  이미지를 감싸는 span 에 온클릭으로 함수를 각각 연결하면 실행됩니다.
*/
  const icon = document.querySelectorAll(".icon");
  const show = document.querySelector(".test");

/*
  바 애니메이션 위아래 실험. 앞에 다른 클래스가 있어야 여기서 삭제를 하더라도
  해당 클래스의 엘리먼트임을 인식합니다.
*/
  _deleteSide = () => {
    show.classList.remove('show')
    show.classList.add('hidden');
  }
  _showSide = () => {
    show.classList.remove('hidden')
    show.classList.add('show');
  }
  _clickIcon = () => {
    console.log('icon')
  }
```

### HTML
```HTML
<div id="main">
  <div id="top"><strong>HEADER</strong></div>
  <div id="contents">
    <article>
      <h1>test</h1>
      <p>test</p>
    </article>
  </div>
  <div id="side">
  <!--
    data 속성을 자바스크립트에서 dataset 으로 뽑아내려 했지만,
    클래스로 지정해서 여러 개이므로 배열형태로만 각각 뽑아낼 수 있습니다.
    그래서 만약 span 을 id 로 고유하게 한다면 각 data 를 뽑아낼 수 있을 듯합니다.
 -->
    <span class="icon" data-value=1 onclick="_clickIcon()">
      <img src="https://png.icons8.com/material-rounded/34/2c3e50/summary-list.png">
    </span>
    <span class="icon" data-value=2 onclick="_clickIcon()">
      <img src="https://png.icons8.com/material-rounded/34/2c3e50/summary-list.png">
    </span>
    <span class="icon" data-value=3 onclick="_clickIcon()">
      <img src="https://png.icons8.com/material-rounded/34/2c3e50/summary-list.png">
    </span>
  </div>
  <button onclick="_deleteSide()">hidden</button>
  <button onclick="_showSide()">show</button>
  <div class="test show">
    <h1>show</h1>
  </div>
</div>
```
