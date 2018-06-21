# 프레임워크 소개
## 싱글 페이지 앱
### MVC
- Model : 사용자가 보고 싶어하는 모든 정보들에 대한 데이터입니다.
- View : 사용자가 화면에 직접 보고 또 상호작용하는 부분입니다. 페이지의 모양과 행동을 다루는 HTML, CSS, 자바스크립트입니다.
- Controller : Model 과 View 를 연결해줍니다. 서로간의 데이터를 전달해줍니다. 모든 로직들을 가지고 있습니다.
### 특화된 프레임워크
- 어떤 프레임워크는 사용자만의 커스텀 element 를 만들어줍니다.
  + 커스텀 element 는 아직 사양이 안정적이진 않습니다.
  + Angular 나 Ember 는 커스텀 HTML element 를 만들 기술을 제공합니다.
```javascript
//예시 : 블로그 템플릿을 만든다면 <user-bio> element 를 씁니다.
<user-bio>
  <h3>RRR</h3>
  <img src="11.jpg">
  <p>aaa</p>
</user-bio>
//일반적인 코드
<div>
  <h3>RRR</h3>
  <img src="11.jpg">
  <p>aaa</p>
</div>
```
- DOM 업데이트하기 : 앱의 데이터가 바뀌면 템플릿(그리고 DOM)은 바뀌어야만 합니다.
  + 다만 DOM 삽입과 조작은 매우 느립니다.
  + 일부 프레임워크는 메모리에 가상 DOM 을 제공해서 그 문제를 완화합니다. 그 후 메모리 구조에 필요한 업데이트, 삭제 작업을 진행합니다.
  + 그 다음 가상 DOM 을 실제 DOM 으로 변환하고 기존 콘텐츠를 교체합니다.
- 프레임워크 중에서는 풀 스텍 프레임워크도 존재합니다. Meteor 는 웹 소켓으로 클라이언트와 서버간 연결을 유지하고 데이터 전송을 다룹니다.
[Meteor 사이트](https://www.meteor.com/)

## 프레임워크 소스
### 템플릿 (backbone.js 사용)
- 탬플릿은 기본적인 구조와 콘텐츠를 제공할 시작점을 만들어줍니다.
```HTML
<script type="text/template" id="menu-template">
  <td><%= menuItem.name %></td>
  <td><%= menuItem.rating %></td>
  <td><%= menuItem.calories %></td>
</script>
```
  + `<%= %>` 이것을 `delimiter` 라고 부릅니다. delimiter 는 자바스크립트 변수가 될 HTML 을 구분하기 위한 기호입니다.
  + 이러한 특정 문자들의 시퀸스는 underscore 에 대한 기본값입니다.
```javascript
//underscore.js 입니다. 여기서 위의 delimiter 를 볼 수 있습니다.
_.templateSettings = {
  evaluate    : /<%([\s\S]+?)%>/g,
  interpolate : /<%=([\s\S]+?)%>/g,
  escape      : /<%-([\s\S]+?)%>/g
};
```
  + evaluate, interpolate, escape 모두 다른 시작 delimiter 를 가지고 있습니다. 이것에는 의미가 있습니다.
```javascript
/* app.js 입니다.
$(~~) 탬플릿을 .html() 으로 문자열로 바꿉니다. 그리고
_.template() underscore 의 탬플릿 함수에 전달합니다.
 */
template: _.template($('#menuItem-template').html(), {variable: 'menuItem'})
```
  + 여기서 탬플릿은 DOM 에서 가져옵니다. 그리고 이 HTML 내용은 문자열로 return 됩니다.
  + 이 탬플릿 문자열은 underscore 의 탬플릿 함수로 전송됩니다.
  + 탬플릿 함수의 결과를 보면 템플릿 함수 자신을 return 함을 알 수 있습니다. 이 함수는 자바스크립트 생성자 함수를 사용합니다.
- 탬플릿 기호
  + `<%= %>` : 탬플릿 함수는 `<%= %>` 를 사용해 값을 삽입합니다. 참고로 '<%= =%>' 가 아닙니다.
  + `<% %>` : `<% %>` 로 자바스크립트 코드를 실행합니다.
  + `<%- %>` : 값을 삽입하고 HTML-escape 처리하려면 `<%- %>` 를 씁니다.

### 생성자 함수
- 생성자 함수는 리터럴 함수에 비해 읽을 수 없고, 작업하기 어렵고, 느리다는 단점이 있습니다.
  + 하지만 생성자 함수는 런타임에 함수를 동적으로 생성 할 수 있습니다.
- underscore 탬플릿 함수의 일은 함수를 생성하는 것입니다.
  + 생성자 함수 포멧으로 만듭니다.
```javascript
//리터럴 함수
var adder1 = function (num1, num2) { return num1 + num2; };
/*생성자 함수. 마지막 파라미터는 함수의 body 고
그 앞에는 함수의 파라미터가 됩니다. */
var adder2 = new Function("num1", "num2", "return num1 + num2");

//이 함수는 인수 하나에 새로운 함수를 return 합니다.
function make(adjective) {
  return new function('noun', "return noun[0].toUpperCase() + noun.slice[1] + ' is" + adjective + "!'");
}

var isFun = make('fun');
isFun('biking'); //결과 : 'Biking is fun!'
```
```javascript
//quiz :  numLetters('a')(4) yields 'aaaa'
var numLetters = function (letter) {
  return new Function('times',
    "if (times < 0) return ''; \
    var result = ''; \
    times = Math.round(times); \
    while(times--) { result += '" + letter + "';} \
    return result;"
  );
};
```

### 탬플릿 변수와 자바스크립트 with
- 탬플릿 delimiter 안에는 첫 클래스 변수를 사용합니다.
```HTML
<!-- index.html -->
<script type="text/template" id="menuItem-template">
    <td><a href="#item/<%= menuItem.id %>"><%= menuItem.name %></a></td>
    <td><%= menuItem.rating %></td>
    <td><%= menuItem.calories %></td>
    <td>
        <button class="select-item">Select Item</button>
    </td>
</script>
```
  + 여기서 데이터는 menuItem 객체에서 온 데이터입니다.
  + menuItem 객체에 접근할 수 있는 프로퍼티를 사용하는 이유는 설정에서 그것을 전달했기 때문입니다.
```javascript
//app.js
template: _.template($('#menuItem-template').html(), {variable: 'menuItem'}),
render: function(id) {
  this.$el.html(this.template(this.model.attributes));
  return this;
},
//~~~
```
- 설정 객체는 생성자 함수를 만드는 데 사용됩니다. 이 기능이 없다면 backbone 은 자바스크립트의 `with` 블록을 사용합니다.
  + 블록이 있는 A 는 블록 내에 있는 명령문의 범위 체인을 확장합니다. `with `블록은 권장되지 않으며 엄격한 모드에서는 허용이 안됩니다.
  + backbone 탬플릿을 만들 때 데이터를 객체의 프로퍼티로 접근하고 객체 이름을 탬플릿 설정으로 전달해야 합니다.
[with : 모질라 블로그](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)
[underscore.js 설명 페이지](https://underscorejs.org/#template)

### addEventListener
- `addEventListener` 메소드는 DOM API 중에서 매우 중요합니다. 브라우저의 이벤트 시스템 수신하게 해줍니다.
```javascript
target.addEventListener(type, listener);
```
  + target 은 문서 객체든 윈도우 객체든 XHR 객체도 괜찮습니다.
  + type 에는 click, animationend, keyup 등이 있습니다.
  + listener 는 이벤트리스너 인터페이스 또는 함수를 구현하는 모든 것들을 사용합니다.
- 이벤트 객체 : 브라우저는 리스너 함수를 호출할 때 이벤트 객체를 전달합니다.
  + 이벤트 객체를 사용해서 발생한 이벤트에 대한 정보를 수집합니다.
```javascript
document.addEventListener('keydown', function(eventObject) {
  if (eventObject.keyCode == 27) {
    //실행할 조건
  }
})
```
- 커스텀 이벤트 : 직접 type 를 만들 수 있습니다.
```javascript
var myCustomeEvent = new CustomEvent('partyTime', {timeToParty: true, partyYear: 1999});

document.addEventListener('partyTime', function(e) {
  if (e.partyYear) {
    console.log("파티한 연도는 " + e.partyYear + '!');
  }
});
//커스텀 이벤트를 발생시킵니다.
document.dispatchEvent(myCustomeEvent);
```

### backbone 이벤트 설정하기
