# Ember 프레임워크 (Udacity)
## 설치
- 엠버 앱을 만들기 전에 Ember-CLI 를 설치해야 합니다. `npm install -g ember-cli`
  + 설치 후 제대로 됐는지 `npm info ok` 로 확인합니다.
  + 오류가 있다면 해당 페이지에서 확인합니다. [Ember CLI 홈페이지](https://ember-cli.com/user-guide/#getting-started)
- 설치된 Ember-CLI 를 인증하기 위해서 `ember -v` 를 실행합니다.
- 새로운 앱 생성하기 : `ember --help new` 는 새로운 앱을 만들떄 사용하는 키워드를 알려줍니다.
  + `ember new 이름` 으로 설치합니다. (앱 이름을 udaciMeals 로 했습니다.) 그러면 폴더를 생성해 파일을 만들어줍니다.
- 앰버는 서버를 설치하거나 제 3자 코드를 작성할 걱정이 없습니다. 왜냐면 앰버 CLI 은 서버를 내장하고 있기 떄문입니다.
  + 그래도 서버는 프로젝트 내부에서 실행해야 하므로 `ember serve` 를 실행해 서버를 엽니다.

## 앰버 앱의 파일 구조
- 앱에서 중요한 폴더는 public, dist, config, app 입니다.
- `public` 폴더에 font files 나 이미지 등의 정적인 asset 들을 넣습니다.
- 웹에 사이트를 보낼 준비가 된다면 엠버는 모든 것을 함께 만들고 묶어서 `dist` 폴더에 넣을 수 있습니다.
- `config` 폴더는 프로젝트의 환경을 기준으로 프로젝트를 변경해야 하는 경우 프로젝트의 구성을 변경하는 코드를 추가하는 곳입니다.
- `app` 폴더에는 대다수 중요한 파일들이 있습니다.
  + 앰버는 라우트에 매우 의존하므로 `router.js` 는 다른 폴더 안에 숨겨져 있지 않습니다.
  + `index.html` 은 전체 페이지에 대한 탬플릿이 있습니다. 하지만 헤더, body, footer 를 가지고 있어 app/templates 에서 작업할 수 있습니다.
  + `app.js` 는 프로젝트의 메인 setup 파일입니다.
  + `template` 폴더는 앰버에게 있어 매우 중요합니다. 어떤 HTML 이라도 작성한 것들은 이 폴더에 저장됩니다.
  + `Styles` 폴더는 프로젝트의 어떤 커스텀 CSS 라도 입력할 수 있습니다.
  + `route` 폴더에는 개별적인 라우트 파일이 저장되는 곳입니다. (추가적인 라우팅 정보를 가집니다.)
  + `Models` 는 앱 데이터에 대한 스키마 파일을 가지고 있습니다. 만약 추가로 함수를 탬플릿에 넣고 싶다면 helper 파일을 만들고 이 폴더에 위치시킵니다.
  + 앰버에서는 컨트롤러는 컴포넌트와 매우 유사하며 향후 버젼에서는 실제로 단계적으로 제거되고 있습니다. (그러니 컨트롤러로 작업하진 않습니다.)
  + 앰버는 custom HTML element 를 쉽게 만들 수 있습니다. 그것들은 `components` 폴더에 들어갑니다. 컴포넌트의 HTML 파일은 templates/components 에 들어가고, 함수는 components 폴더에 들어갑니다.
## 라우트, 라우터, 탬플릿
### 라우팅
- 앰버 앱을 불러올 때 라우터는 라우트 목록에서 URL 을 매칭하는 일을 합니다. 라우트를 찾으면 라우터는 해당 라우트의 경로 파일을 불러오고 연결된 템플릿을 렌더링합니다.
- 베이스 라우트(/)는 애플리케이션 탬플릿을 불러왔고, 앰버가 관리하는 라우트 목록들도 추가할 수 있습니다. [menu 라우트(/menu)로 menu template 를 불러올 수 있습니다.]
  + 여기서 메뉴 라우트와 메뉴 탬플릿의 menu 는 서로 다릅니다. 라우트의 menu 는 URL 과 일치하는 것이고, 탬플릿에서의 menu 는 그저 탬플릿 이름을 나타냅니다.
- 또한 개별적인 항목의 탬플릿도 생성할 수 있습니다.
  + /item/strawberry-pudding 라우트는 item 탬플릿을 사용합니다.
  + 하지만 모든 메뉴 하나하나에 라우트를 만드는건 좋지 않습니다. 앰버를 사용하면 URL 매칭을 동적으로 할 수 있습니다.
- `/item/:item_name` 라우트는 동적인 segment 인 :item_name 과 일치하는 것을 매칭합니다.
  + 라우터는 동적 segment 를 잡아 item 라우트에 전달합니다. 그 다음 item 탬플릿이 라우트 파일의 데이터로 렌더링됩니다.
### 라우트와 탬플릿 생성하기
- 해당 폴더에 직접 만들지 말고 앰버 CLI 로 만들어봅니다. generate 는 g 로 줄일 수 있습니다. `ember generate route menu` (여기선 menu URL 과 매칭되는 라우트를 만듭니다.)
  + 생성한 menu 라우트는 menu.js 라우트 파일을 불러오고, 다음으로 menu.hbs 탬플릿을 불러옵니다.
- router.js 에서 라우트 경로를 수정할 수도 있습니다.
```javascript
//router.js
Router.map(function () {
  this.route('bricks', {path: '/legos'});
});
```
  + legos 경로는 bricks 라우트 파일과 bricks 탬플릿을 사용합니다.

### menu 항목 보기(Viewing)
- 각각의 메뉴 항목들을 보려면 동적 segment 를 경로에 설정해줘야 합니다.
```javascript
//router.js
Router.map(function() {
  this.route('menu');
//item 라우트의 경로를 동적 segment 로 지정합니다.
  this.route('item', {path: 'item/:item_name'});
});
```
### Nested Routes
- 앰버의 내장 라우트는 복잡한 애플리케이션을 쉽게 만들도록 도와줍니다.
- 앰버 탬플릿은 컨테이너라고 볼 수 있습니다. 베이스 컨테이너는 애플리케이션의 탬플릿이고 모든 다른 탬플릿들을 가지고 있습니다.
site-wide HTML 이 여기에 있습니다. (header, navigation, footer)
  + menu 탬플릿은 애플리케이션 탬플릿 안에 있습니다. 또한 item 탬플릿도 애플리케이션 탬플릿의 안에 있습니다.
- 만약 nutrition 탬플릿을 item 탬플릿 안에 포함되거나 내장하려면 어떻게 해야 할까요. 우선 라우터를 만듭니다. `ember g route item/nutrition`
  + 내장 라우트는 그 위 라우트에서 함수 형태로 생성됩니다.
```javascript
this.route('item', {path: 'item/:item_name'}, function() {
  this.route('nutrition');
});
```

## Handlebars
- Handlebars 탬플릿은 {{  }} 같은 형태의 HTML 블록입니다.(예: {{brick_color}} 는 "brick_color" 가 "brick_color" 변수의 콘텐츠를 출력한다는 뜻입니다.)
- Handlebars 는 앰버 밖에서도 사용할 수 있는 탬플릿 언어입니다.
```HTML
<div class="brick-container"></div>
<script id="brick-template" type="text/x-Handlebars-template">
  <div class="brick">
    <h1>{{name}}</h1>
    <div class="desc">
      {{description}}
    </div>
  </div>
</script>
```
- 아래의 자바스크립트 코드는 탬플릿을 얻고, 컴파일 후 데이터와 함께 호출합니다. 그 다음 brick-container element 를 결과 HTML 과 함께 업데이트합니다.
```javascript
// get template from HTML
var brickContainer = document.querySelector( '#brick-container' );
var brickTemplate = document.querySelector( '#brick-template' );
// compile source template into a template function
var template = Handlebars.compile( brickTemplate.innerHTML );
// the app's data
var context = {name: 'Red Brick', description: 'A colored brick that can be used to...'};
// build the HTML template with the supplied data
var html = template(context);
// fill the page with content
brickContainer.innerHTML = html;
```
