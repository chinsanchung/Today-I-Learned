# Angular (Udacity)
## Angular ecosystem
- 앵귤러 는 몇몇 다른 컴포넌트들로 구성되어 있습니다.
- `module` 컴포넌트 : 앱의 다른 부분들을 저장한 컨테이너입니다.
  + `template` 시스템은 애플리케이션의 HTML 구조를 만들어줍니다. 이것은 앵귤러  쓰는 NBC 프레임워크에서 사용하는 view 파트입니다.
    + `Directive` 는 탬플릿의 marker 로 앵귤러 HTML 컴파일러가 DOM element 에 특정 행동을 하려 접근할 때 사용합니다.
  + `controller` 는 view 의 로직과 초기 상태를 설정하는 장소입니다.
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
