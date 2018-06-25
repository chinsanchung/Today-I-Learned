# Angular (Udacity)
## Angular ecosystem
- 앵귤러 는 몇몇 다른 컴포넌트들로 구성되어 있습니다.
- `module` 컴포넌트 : 앱의 다른 부분들을 저장한 컨테이너입니다. 앱의 다른 컴포넌트들을 캡슐화합니다.
  + `template` 시스템은 애플리케이션의 HTML 구조를 만들어줍니다. 이것은 앵귤러  쓰는 NBC 프레임워크에서 사용하는 view 파트입니다. 애플리케이션의 보여주는 영역입니다.
    + `Directive` 는 탬플릿의 marker 로 앵귤러 HTML 컴파일러가 DOM element 에 특정 행동을 하려 접근할 때 사용합니다.
  + `controller` 는 view 의 로직과 초기 상태를 설정하는 장소입니다. 앵귤러 컨트롤러는 데이터와 기능을 앱의 탬플릿에 제공하는 기능을 제공합니다.
    + `service` 는 컨트롤러와 관련이 있습니다. view 의 독립적인 로직을 서비스에 넣어 많은 다른 컨트롤러에서 쓸 수 있게 해줍니다.
  + `scope` 는 탬플릿과 컨트롤러를 묶어주는 역할을 합니다.
  + 사용자가 탬플릿과 상호작용하고 데이터를 바꿀 때 스코프는 이러한 변화를 관찰하고 컨트롤러를 업데이트합니다. 반대로 컨트롤러에서 데이터가 바뀌면 탬플릿이 업데이트됩니다.
  + 앵귤러 는 보통 `router` 가 필요 없지만, 만약 복잡한 앱이라면 라우터를 사용해 애풀리케이션 상태를 관리하도록 해주면 좋습니다. 라우터는 url 을 관찰하고 올바른 탬플릿과 컨트롤러를 불러옵니다.
  + 탬플릿과 컨트롤러 : 모든 정보를 컨트롤러에 저장한 후 탬플릿에 전달해 페이지를 띄웁니다.
- 앵귤러 기본 설정
```HTML
<body ng-app>
<!-- 앵귤러의 attribute 인 ng-app 입니다. -->
</body>
<script> /* 앵귤러 주소 */ </script>
```

## Yeoman 설치
- Yeoman 은 프로젝트 스카폴딩을 지원합니다. 전체 프로젝트의 파일 구조를 빠르게 만들어줍니다.
- cmd 창에 입력합니다. `npm install -g grunt-cli bower yo generator-karma generator-angular`
- 그리고 `yo angular udaciMeals` 만 입력합니다. (이것은 앱의 디폴트 모듈이 됩니다.)
  + 그러면 script/controllers/app.js 에 모듈이 생성됩니다.
- 설정 옵션 : grunt-no / sass-no / bootstrap-yes / modules-deselect all
- 앵귤러는 Angular generator 가 있어 Yeoman 설정에 도움이 됩니다.
[generator github](https://github.com/yeoman/generator-angular#angularjs-generator--)

## 앵귤러 모듈
- cmd 창에 `yo angular udaciMeals` 를 입력하면 모듈이 생깁니다.
  + 생성된 모듈에는 첫번째 인수로 모듈 이름이, 두번째로는 앱이 의존하는 모듈들의 이름 배열입니다.
```javascript
//udaciMealsApp 모듈이 생겼음을 말해줍니다. 그리고 a, b 를 의존한다고 알립니다.
angular
  .module('udaciMeals', ['a', 'b']);
//모듈을 얻기 위해서 작성합니다.
angular.module('udaciMeals');
```
- 앵귤러 애플리케이션은 한 개 이상의 모듈을 가질 수 있습니다.
  + 만약 이 앱이 다른 모듈에 의존할 경우, 앱이 작동하려면 해당 모듈이 필요합니다.
  + 그 모듈들의 이름을 두번째 인수에 적습니다.
  + 만약 아무 모듈도 의존하지 않는다면 빈칸으로 적어야 합니다.
- body element 는 애플리케이션을 가지고 있습니다. 그리고 앵귤러에게 해당 앱 모듈과 모든 컴포넌트를 불러오게 해줍니다.
```HTML
<body ng-app="udaciMealsApp">
</body>
```

## 작업
### 탬플릿과 동적 표현
- 모듈은 보여주는 부분입니다.
- {{...}} 괄호 두 번은 앵귤러에서 사용하는 동적 표현입니다. 안에 변수나 단순한 수학 연산 등을 넣습니다. (정적인 표현일 땐 사용하지 않습니다.)
  + 표현 하나당 한 개의 watcher 를 가집니다. scope 객체는 watcher 를 가집니다.
  + {{}} 을 사용한 앵귤러 탬플릿은 컨트롤러로부터 데이터를 얻습니다.
```HTML
<!-- template.html -->
<ul>
  <li>username : {{user.username}}</li>
  <li>password: {{user.password}}</li>
  <li>Favorite color: {{user.favoriteColor}}</li>
</ul>
```
```javascript
//controller.js
function UserController() {
  this.username = 'jin';
  this.password = 'abcdefghijk';
  this.favoriteColor = 'green';
}
```
### 앵귤러 컨트롤러
- 우선 yeoman 을 사용해서 파일을 생성합니다. `yo angular:controller 컨트롤러이름`
  + 그러면 컨트롤러 폴더에 컨트롤러이름 파일이 생성됩니다. (이름을 menu 로 합니다.)
  + 또한 index.html 에 script 로 컨트롤러가 추가됩니다.
- 대다수의 컨트롤러는 앵귤러 모듈 안에 들어있습니다.
```javascript
// 컨트롤러이름.js
angular.module('udaciMealsApp')
//controller('이름Ctrl', 함수) 를 모듈에 처가합니다.
  .controller('MenuCtrl', function () {
  //프로퍼티를 추가합니다.
    this.id = 'strawberry-pudding';
    this.name = 'Strawberry Pudding';
    this.img = 'strawberry-pudding.jpg';
    this.rating = '5';
  });
```
  + 컨트롤러는 생성자 함수입니다. 생성자 객체의 프로퍼티는 탬플릿의 변수로 사용됩니다.
  + 컨트롤러는 탬플릿의 초기 상태를 제공합니다. 그리고 정보를 보여줄 표현을 어떻게 쓸지 알려줍니다.
- 탬플릿을 만들어봅니다. `yo angular:view 탬플릿이름` (이름을 menu 로 합니다.)
  + 다음으로 index.html 을 설정합니다.
```HTML
<div id="menu-view" ng-include="'views/menu.html'" ng-controller="MenuCtrl as menu"></div>
```
  + 탬플릿인 이름(menu).html 을 수정합니다.
```HTML
<!-- 아래와 같이 바꿉니다. -->
<div class="row">
  <div class="items-container">
    <div class="col-md-4">
<!-- 여기서의 menu 는 index.html 의 ng-controller 속성(attribute) 에서 왔습니다.
그 속성은 컨트롤러의 인스턴스를 만들고 이름을 menu 로 짓습니다. -->
      <h4>{{menu.name}}</h4>
      <p>Rating: {{menu.rating}}</p>
      <p>Image: {{menu.img}}</p>
    </div>
  </div>
</div>
```

## scope
- 앵귤러에서 데이터와 탬플릿의 사이를 연결해주는 것이 바로 `scope` 입니다. (예 : 웹사이트에서 데이터가 변경됐을 때 컨트롤러에 전달)
- 앵귤러 scope 는 애플리케이션 데이터를 전달해주는 객체입니다. scope 는 탬플릿과 컨트롤러 사이의 다리입니다. 또한 앵귤러의 데이터를 묶어주는 장치이기도 합니다.
  + 데이터는 처음에 컨트롤러 안에서 설정됩니다. 그리고 엔진 컨트롤러 지시문이 탬플릿에 추가됩니다. 그러면 탬플릿은 이제 표현에 대한 컨텍스트를 가집니다.
  + 탬플릿 안의 표현이 바뀌면, 컨트롤러가 업데이트됩니다.
- index.html 에 클래스가 추가됐습니다.
```HTML
<!-- 앵귤러가 이 탬플릿을 컴파일할 때 지시문(MenuCtrl as menu) 를 해결합니다.
그리고 이 element 와 연관된 scope 객체를 만듭니다.
그래서 탬플릿 안의 표현이 나타날 때 그것은 scope 로부터 해결됩니다. -->
<div ng-include="'views/menu.html'" ng-controller="MenuCtrl as menu" class="ng-scope">
</div>
```
### scope 권장사항
- 앵귤러에서는 `$scope` 를 쓰지 않는 것이 좋습니다.
```javascript
//비추천
angular.module('udaciMealsApp')
    .controller('MenuCtrl', function ($scope) {
        $scope.id = 'chipotle-shrimp-wrap';
        $scope.name = 'Chipotle Shrimp Wrap';
        $scope.img = 'chipotle-shrimp-wrap.jpg';
        $scope.rating = 4.2;
    });
//추천
angular.module('udaciMealsApp')
    .controller('MenuCtrl', function () {
        this.id = 'chipotle-shrimp-wrap';
        this.name = 'Chipotle Shrimp Wrap';
        this.img = 'chipotle-shrimp-wrap.jpg';
        this.rating = 4.2;
    });
```
  + 비추천하는 방식으로 $scope 를 사용하면 scope 객체를 직접 수정하게 됩니다.
  + 그럴 경우 탬플릿은 이렇게 변합니다.
```HTML
<div class="container">
    <div class="row" ng-controller="MenuCtrl">
        <div class="items-container">
            <h4>{{name}}</h4>
            <p>{{rating}}</p>
            <p>{{img}}</p>
        </div>
    </div>
</div>
```
  + 이러면 MenuCtrl 은 함수를 호출하고 scope 객체를 직접 수정합니다. (탬플릿에 {{name}} 등이 필요한 이유입니다.)
  + 권장 방식의 컨트롤러는 인스턴스 MenuCtrl as menu 를 만들고, 탬플릿은 {{menu.name}} 등을 가집니다.
- 컨트롤러와 템플릿을 만들 때 컨트롤러에서 `$scope` 를 사용하지 말고 `this` 와 컨트롤러 syntax 를 사용해야합니다.
[controller as syntax](https://toddmotto.com/digging-into-angulars-controller-as-syntax/)

## Directive
- `directive` 는 앵귤러 컴포넌트 중 하나입니다. 지정된 동작을 해당 DOM 요소에 첨부하거나 해당 DOM 요소와 그 자식을 변형하는 HTML 컴파일러입니다

## 의존성 주입(dependency injection)
- 혼자서 모든 자원을 다 다루지 않고, 외부 자원에 의존해 그것들을 제공받습니다.
  + injector 는 모든 정보와 서비스가 어디에 있는지를 알고 있습니다. 정보나 서비스가 필요할 때면 injector 를 호출합니다.
    + 그러면 injector 는 요구사항들을 불러와 호출하는 코드에 요구사항들을 제공합니다. 그러면 코드는 업무를 수행하는데 그 서비스를 사용합니다. (의존성 주입)
- 의존성 주입은 앵귤러에서 굉장히 많이 쓰입니다.

## 서비스
- 서비스는 컨트롤러와 유사합니다. 둘 다 데이터를 애플리케이션에 제공합니다.
- 차이점은 서비스는 단지 하나의 view 만을 위한게 아니라는 점입니다.
  + 서비스는 특정 컨트롤러에 묶여있지는 않지만 어떤 컨트롤러에도 사용할 수 있습니다. 여러 컨트롤러에 같이 데이터와 기능의 자원을 제공합니다.
- 앵귤러 generator 로 서비스를 만듭니다. `yo angular:service 서비스이름` (예시로 foodFinder 로 합니다.)
```javascript
//  **foodFinder.js**
angular.module('udaciMealsApp')
//앵귤러 injector 에 이름 'foodFinder' 하고 함수가 저장됩니다.
  .service('foodFinder', function () {
    this.getMenu = function () {
      return $.get('/menu/menu.json');
    };
  });

// **menu.js**
angular.module('udaciMealsApp')
/* 서비스는 이제 컨트롤러에 인수로써 삽입됐습니다.
이것들을 변수에 저장해야 하므로 함수의 매개변수(menu)로 넣습니다..
items 부분은 lexical scope 로 묶습니다. */
  .controller('MenuCtrl', ['foodFinder', function (menu) {
    var vm = this;
    /*menu 를 얻으려면 foodFinder 의 getMenu 함수를 써야합니다.
    retunr 한 값이 jQuery 형태이므로 .then 을 사용합니다.(data 에 저장됩니다.)*/
    menu.getMenu().then(function (data) {
      vm.items = data;
    });
    this.increment = function (item) {
      item.rating = ((item.rating * 10) + 1) / 10;
    };
    this.decrement = function (item) {
      item.rating = ((item.rating * 10) -1) / 10;
    };
  }]);
```
  + 컨트롤러가 서비스를 호출할 때 앵귤러는 그 서비스의 위치와 어떻게 만드는지를 알고 있습니다. (예시 : foodFinder 서비스는 모든 메뉴 정보를 다룰 것입니다.)
  + menu.js 의 데이터를 JSON 파일로 옮기고 JSON 형태로 바꿉니다. (여기서는 foodFinder 서비스로 menu.json 을 가져옵니다.)
- 서비스를 삽입하는데는 제한이 없습니다. 넣은 만큼 마지막 함수의 매개변수에 삽입한 서비스를 저장할 변수를 각각 지정합니다.

## order manager feature
- 탬플릿, 컨트롤러, 서비스로 `order manager` 를 만들어봅니다.
- 우선 명령을 수행하는 서비스를 만듭니다. `yo angular:service orderManager`
  + 이 서비스는 매일 명령했던 것들을 추적해야 합니다. 그러기 위해서는 간단한 객체를 만들어 매일매일의 항목들을 추적하게 만듭니다.
```javascript
// **ordermanager.js**
angular.module('udaciMealsApp')
  .service('orderManager', function () {
    var selectedDay = 'Monday';
    var orderSelection = {
      Monday: {
        breakfast: '',
        lunch: '',
        dinner: ''
      },
      Tuesday: {
        breakfast: '',
        lunch: '',
        dinner: ''
      },
      Wednesday: {
        breakfast: '',
        lunch: '',
        dinner: ''
      },
      Thursday: {
        breakfast: '',
        lunch: '',
        dinner: ''
      },
      Friday: {
        breakfast: '',
        lunch: '',
        dinner: ''
      }
    };
    //소비자가 위의 값들에 접근하도록 도와주는 함수
    this.getActiveDay = function () {
      return selectedDay;
    };
    this.setActiveDay = function () {
      selectedDay = day;
    };
    //아이템을 선택할 수 있게 도와주는 함수
    this.chooseMenuOption = function (meal, menuItem) {
      orderSelection[selectedDay][meal] = menuItem;
    };
    this.removeMenuOption = function (meal, menuItem) {
      orderSelection[day][menuCategory] = '';
    };

    this.getOrders = function () {
      return orderSelection;
    };
  });
```
  + 그리고 명령을 보여줄 탬플릿과 탬플릿 표현의 처음 상태를 설정할 컨트롤러를 만들어 사용자가 메뉴 아이템을 추적하게 만들어줍니다. `yo angular: view order`, `yo angular:controller order`
  + 탬플릿을 페이지에 추가합니다. `<div ng-include="'views/menu.html'" ng-controller="OrderCtrl as order"></div>`
  + 탬플릿이 데이터를 얻도록 order manager 서비스를 삽입합니다.
```javascript
// **order.js**
angular.module('udaciMealsApp')
  .controller('OrderCtrl', ['orderManager', function (orderManager) {
    this.list = orderManager.setActiveDay(day);
  }]);
```
  + 그리고 탬플릿을 수정합니다.
```HTML
<!-- order.html -->
<div class="row">
  <!-- 매일 리스트를 반복하도록 ng-repeat directive 를 작성합니다. -->
  <div class="col-md-2" ng-repeat="(day, menuCategory) in order.list">
    {{day}}
    <dl>
      <dt>Breakfast</dt>
      <dd>
        {{menuCategory.breakfast}}
      </dd>
      <dt>Lunch</dt>
      <dd>{{menuCategory.lunch}}</dd>
      <dt>Dinner</dt>
      <dd>{{menuCategory.dinner}}</dd>
    </dl>
  </div>
</div>

<!-- menu.html 에 사용자가 메뉴 아이템을 선택할 수 있도록 select 를 작성합니다. -->
<div class="well">
  <select ng-model="item.meal" name="">
    <option value="breakfast">Breakfast</option>
    <option value="lunch">Lunch</option>
    <option value="dinner">Dinner</option>
  </select>
  <button ng-click="menu.chooseItem[item.meal, item.name]">Select Item</button>
</div>
```
  + menu.js 를 수정해서 선택한 아이템에 대한 함수를 작성합니다.
```javascript
// **menu.js**
angular.module('udaciMealsApp')
  .controller('MenuCtrl', ['foodFinder', function (menu) {
    var vm = this;
    this.chooseItem = function (menuCategory, menuItemName) {
      //order manager 에 있는 함수를 사용합니다.
    };
    //반복...
  }])
```
## 라우터와 UI-Router
- yeoman 으로 모듈을 만들 때 angular-route.js 를 만들 수 있지만 그것은 제한적입니다.
- 그래서 커뮤니티에서 만든 라우팅 모듈인 `UI Router` 를 사용합니다. [UI Router github](https://github.com/angular-ui/ui-router)
  + 앵귤러 1에서 설치하는 방법 : `bower install -S angular-ui-router` (-S 는 save 입니다.)
- UI Router 는 내부 컴포넌트를 가진 모듈입니다.
  + 서비스와 비슷하게 UI Router 를 작성합니다.
```javascript
// ** app.js **
/* UI Router 를 udaciMealsApp 에 넣습니다. 참고로 ui-router 가 아니라
ui.router 입니다. */
angular
  .module('udaciMealsApp', ['ui.router']);
```

### 애플리케이션 상태 관리
- 모듈을 만들기 전에 config 메소드를 사용하여 설정 방법을 구성 할 수 있습니다. (config 메소드는 함수를 받습니다.)
```javascript
// ** app.js **
angular
  .moudle('udaciMealsApp', ['ui.router']);
  .config(['$stateProvider', '$urlRouterProvider', function ($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/');
    $stateProvider
      .state('home', {
        url: '/',
        templateUrl: 'views/menu.html',
        controller: 'MenuCtrl as menu'
      });
  }]);
```
- 다음에는 directive UI view 로 index.html 에 경로를 설정해 UI Router 에게 위치를 알려야합니다.
```HTML
<div ui-view></div>
```
### nested view
- UI router 의 강점 중 하나가 nested view 입니다.
- 메인 view 와 같은 이름으로 내부에 삽입합니다.
```javascript
angular
  .moudle('udaciMealsApp', ['ui.router']);
  .config(['$stateProvider', '$urlRouterProvider', function ($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/');
    $stateProvider
      .state('home', {
        url: '/',
        templateUrl: 'views/menu.html',
        controller: 'MenuCtrl as menu'
      });
      .state('item.nestedview이름', {
        url: '/reviews',
        templateUrl: 'views/item-reviews.html'
        //부모 라우트 scope 로부터 데이터를 상속받을 거라서 컨트롤러는 필요없습니다.
      });
  }]);
```
```HTML
<!-- item.html -->
<p><a ui-sref="item.nestedview이름">이름</a> <a ui-sref="item.reviews">Review</a></p>
```
