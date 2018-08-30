# gulp 와 babel, ESLint 설정하기 (From [Learning JavaScript])
[Learning JavaScript] 의 2장 '자바스크립트 개발 도구'에서 가져온 설명입니다.
## 폴더 설정
우선 프로젝트 루트를 아래처럼 설정합니다.
```
.git
.gitignore
package.json
node_module(폴더)
es6(폴더)
dist(폴더)
public/(폴더)
  es6/(폴더)
  dist/(폴더)
```
- 서버 쪽 코드는 루트의 es6 디렉토리에 저장하고 브라우저 코드는 public/es6 디렉토리에 저장합니다. 브라우저에서 보내는 자바스크립트는 원래 공개된 것이므로 이런 식으로 저장하는 프로젝트는 많습니다.
- distribution 의 약자인 dist 디렉토리에 변환한 ES5 코드를 저장합니다.

## 걸프 설정
- 걸프는 반복적인 개발 작업을 자동화하는 빌드 도구입니다.
우선 전역으로 걸프를 설치한 후 (`npm install -g gulp`), 프로젝트 안에 로컬 걸프를 설치합니다. `npm install --save-dev gulp`
그 다음 gulpfile.js 를 만듭니다.
```javascript
const gulp = require('gulp');
//걸프 의존성 작성하기
gulp.task('default', function() {
  //걸프 작업 작성하기
})
```
명령을 내리려면 명령 창에 gulp 라 치면 됩니다.
## 바벨
- 바벨은 ES5 를 ES6 으로 바꾸는 트랜스컴파일러로, 그 외에도 ES6 과 리액트 그리고 ES7 등을 지원하는 범용 트랜스컴파일러가 됐습니다.
우선 ES6 프리셋을 설치합니다.
```
npm install --save-dev babel-preset-es2015
```
그 다음 프로젝트 루트에 .babelrc 파일을 만듭니다. 프로젝트에서 바벨을 사용할 때 이 파일이 있다면 ES6 을 사용한다는 것을 인식하게 됩니다.
```
{ "presets": ["es2015"] }
```

## 바벨을 걸프와 함께 사용하기
es6 과 public/es6 에 있는 코드를 es5 코드로 변환해 dist 와 public/dist 에 저장합니다. 우선 `npm install --save-dev gulp-babel`로 gulp-babel 패키지를 설치하고 gulpfile.js 를 수정합니다.
```javascript
const gulp = require('gulp');
const babel = require('gulp-babel');

gulp.tast('default', function() {
  //노드 소스
  gulp.src('es6/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest("dist"));
  //브라우저 소스
  gulp.src('public/es6/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest("dist"));
});
```
걸프는 파이프라인으로 작업을 처리합니다.
우선 변환할 파일을 지정하는데 ** 은 서브 디렉토리를 포함한 모든 디렉토리를 뜻하는 와일드카드입니다. 이 소스 필터는 es6 에 있는 모든 .js 파일을 선택합니다.
그 다음 이 소스 파일을 바벨에 파이프로 연결합니다. 바벨은 ES6 을 ES5 코드로 변환합니다.
마지막으로 컴파일된 ES5 코드를 dist 디렉토리에 저장합니다. 걸프는 소스 파일 이름과 디렉토리 구조를 그대로 유지합니다.

이제 es6 에 ES6 문법이 적용된 test.js 파일을 만들고 gulp 명령을 내려봅니다. 그러면 dist 디렉토리에 같은 이름의 파일이 생깁니다. 그것을 실행하면 그대로 실행되는 것을 확인할 수 있습니다.
마지막으로, dist 와 public/dist 디렉토리를 .gitignore 파일에 추가합니다. ES5 소스를 추적할 필요는 없기 때문입니다.

## ESLint
- ESLint 는 자주 하는 실수를 피하고 더 나은 프로그래머가 되도록 돕는 린트 프로그램입니다.
우선 `npm install -g eslint` 로 설치합니다. 그 다음에는 설정 파일 .eslintrc 를 만들어야 합니다.
프로젝트 루트에서 `eslint --init` 을 실행하고 몇 가지 질문에 답해야 합니다.
  - ECMAScript 6 기능을 사용합니까? Y
  - ES6 모듈을 사용합니까? Y
  - 어디서 코드를 실행합니까? NODE
  - CommonJS 를 사용합니까? N(CommonJS 는 ES6 스타일 모듈의 일종입니다. 지금은 쓰지 않습니다.)
  - JSX 를 사용합니까? N
  - 들여쓰기를 어떤 식으로 하겠습니까? 탭/스페이스
  - 문자열에 어떤 따옴표를 사용합니까? '/"
  - 줄 끝 문자에 무엇을 사용합니까? 윈도우면 윈도우/리눅스나 맥이면 유닉스
  - 세미콜론을 필수호 하긴 원합니까? Y
  - 설정 파일 형식은 무엇으로 하겠습니까? 자바스크립트

설정이 끝나면 .eslintrc 파일이 생성됩니다.
ESLint 를 사용하는 방법은 여러 가지입니다. `eslint es6/test.js` 처럼 직접 실행하거나, 혹은 에디터에 통합하거나, gulpfile 에 추가해도 됩니다. 에디터 통합은 편리해도 각 에디터, 운영체제마다 전부 방법이 달라 직접 찾아야 합니다. 에디터에 통합을 하든 안하든 gulpfile 에는 ESLint 를 꼭 추가하길 권합니다. 결국 빌드할 때마다 걸프를 실행하기에 gulpfile 에서 코드를 체크하는 것이 좋습니다.
이제 `npm install --save-dev gulp-eslint`를 실행합니다. 그리고 gulpfile.js 를 수정합니다.
```javascript
const gulp = require('gulp');
const babel = require('gulp-babel');
const eslint = require('gulp-eslint');

gulp.tast('default', function() {
  //ESLint 를 실행
  gulp.src(["es6/**/*.js", "public/es6/**/*.js"])
    .pipe(eslint())
    .pipe(eslint.format());
    //노드 소스
    gulp.src('es6/**/*.js')
      .pipe(babel())
      .pipe(gulp.dest("dist"));
    //브라우저 소스
    gulp.src('public/es6/**/*.js')
      .pipe(babel())
      .pipe(gulp.dest("dist"));
})
```

ESLint 는 무엇을 실수로 지적할지 정할 수 있습니다. 마지막 쉼표 규칙을 끄거나 항상 쓰거나 등등 설정할 수 있습니다.
참고로 console.log 를 ESLint 는 엉성한 습관이라 여기고 구식 브라우저에서는 문제를 일으키기도 해서 경고를 띄울 수 있습니다. 공부 목적이라면 console.log 경고를 꺼도 됩니다.
quotes 규칙도 끄는 편이 좋습니다.
```javascript
{
  "rules": {
    /* ... */
  }

}
```
