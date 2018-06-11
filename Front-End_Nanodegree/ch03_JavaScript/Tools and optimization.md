# 각종 도구와 최적화 (Udacity)
## Gulp
- gulp 는 자바스크립트의 빌드 자동화 툴입니다.
- [tistory gulp 소개](http://programmingsummaries.tistory.com/356)
- 전역 설치하기 `npm install -g gulp`
  + 개인적으로 연습할 때는 해당 프로젝트 폴더에 설치했습니다. `npm install gulp` (최신버젼)
- 연습용 gulpfile.js 입니다.
```javascript
var gulp = require('gulp');
gulp.task('default', function () {
  console.log('test');
});
```
  + cmd 에서 gulp 를 입력하면 test 가 나올 것입니다.

### SASS
- SASS 는 CSS attribute(속성) 의 값을 자바스크립트의 변수처럼 활용합니다. 이를 통해 반복적인 작업을 간소화합니다.
- [sass 공식사이트](https://www.npmjs.com/package/gulp-sass)
- 설치하기 : `npm install gulp-sass --save-dev`
- SASS 폴더를 만들고 CSS 파일을 SCSS 확장자로 바꿉니다.
  + SCSS 초기 연습. gulpfile.js 입니다.
```javascript
'use strict';

var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
```
- SCSS 파일은 아래처럼 바뀝니다.
```CSS
@mixin aaa {
  margin-left: 10px;
}
.btn {
  @include aaa;
}
.list {
  @include aaa;
}
```

### AutoPrefixer
- AutoPrefixer 플러그인은 vendor-prefixed CSS 속성을 자동으로 추가해 줍니다.
-
```javascript
'use strict';

var gulp = require('gulp');
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');

gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass().on('error', sass.logError));
    .pipe(autoprefixer({
      browsers : ['last 2 versions']
    }))
    .pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
```

### Eslint
- eslint 는 에러 체크를 하는 `linter` 중에서 자바스크립트에서 쓰는 `linter` 입니다.
- 설치하기 : `npm install -g eslint`
- 그리고 `npm init` 후에 `eslint --init` 을 실행합니다.
- 원하는 규칙(구글, 에어비엔비 등) 을 고르면 알아서 설치해줍니다.
- 보통은 gulp 를 이용해서 eslint 를 사용합니다. `npm install --save-dev gulp-eslint`
  + gulpfile.js 입니다.
```javascript
var gulp = require('gulp');
var eslint = require('gulp-eslint');

gulp.task('lint', function() {
  return gulp.src(['**/*.{js,jsx}','!node_modules/**'])
    .pipe(eslint())
    .pipe(eslint.format())
    .pipe(eslint.failAfterError());
});

gulp.task('watch', function() {
	gulp.watch('**/*.{js,jsx}', ['lint']);
});

gulp.task('default', ['watch'], function () {
});
```
  + gulp 를 cmd 에 입력하면 eslint 를 실행하고 결과를 알려줍니다.

## 최적화
- 개발과 생산을 나눠서 제작하면 효율성이 올라갑니다.
- 해당 프로젝트 내부에 dist 폴더를 생성해 거기에 소스에서 변경된 파일을 복사, 저장할 것입니다.
- 그리고 gulpfile.js 에서 dist 경로를 설정합니다.
```javascript
//작성 후 cmd 에 gulp styles 를 실행하면 dist 폴더에 CSS 파일을 복사 후 저장합니다.
gulp.task('styles', function () {
  gulp.src('sass/**/*.scss')
    .pipe(sass({
      outputStyle: 'compressed'
    }).on('error', sass.logError))
    .pipe(autoprefixer({
      browsers : ['last 2 versions']
    }))
    .pipe(gulp.dest('dist/css'))
    .pipe(browserSync.stream());
});
//index.html 을 gulp 를 사용해서 복사, dist 폴더에 넣습니다.
gulp.task('copy-html', function () {
  gulp.src('./index.html')
    .pipe(gulp.dest('./dist'));
});
//여기서는 모든 img 안의 파일들을 잡고(gulp.src) dist 폴더에 복사, 저장합니다.
gulp.task('copy-images', function () {
  gulp.src('img/*')
    .pipe(gulp.dest('dist/img'));
});

gulp.task('default', ['copy-html', 'copy-images', 'styles', 'lint'], function () {
  /* gulp.watch 로 파일이 추가 혹은 삭제됐음을 감지해서 브라우저를 재시작합니다.
  gulp.watch(folder, [action]) : 특정 폴더에서 어떤 action 을 재시작하게 합니다.
  */
  gulp.watch('sass/**/*.scss', ['styles']);
  gulp.watch('js/**/*.js', ['lint']);
  gulp.watch('/index.html', ['copy-html']);

  browserSync.init({
    server: './dist'
  });
});
```
  + 자동으로 index.html 을 reload 하기
```javascript
gulp.watch('./build/index.html')
  .on('change', browserSync.reload)
```

### CSS 연결
- SASS 를 사용합니다. `.pipe(sass({outputStyle: 'compressed'}))` 로 sass pipe 를 수정하면 멋진 압축 파일을 만들어줍니다.

### 자바스크립트 연결
- cmd 에 gulp concat 을 설치하고 변수로 지정합니다. `npm install gulp-concat`
```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');
var browserSync = require('browser-sync').create();
var eslint = require('gulp-eslint');
var jasmine = require('gulp-jasmine-phantom');
var concat = require('gulp-concat');
```
- 하단 부분입니다.
```javascript
gulp.task('scripts', function () {
  gulp.src('js/**/*.js')
  //파일을 합쳐서 인수로 지정한 이름으로 파일을 생성합니다.
    .pipe(concat('all.js'))
    .pipe(qulp.dest('dist/js'));
});

gulp.task('scripts-dist', function () {
  gulp.src('js/**/*.js')
    .pipe(concat('all.js'))
    .pipe(qulp.dest('dist/js'));
})
```
- 마지막으로 index.html 에서 script 의 src 를 all.js 로 바꿉니다.

### 축소
- 연결 후에는 자바스크립트의 파일 사이즈를 줄이기 위해 축소를 해야합니다.
- 현재 가장 유명한 축소기는 `uglifyJS` 입니다. 종종 무겁긴 해도 안전한 최적화를 보장합니다.
- 이로 인해 script 와 script-disk 는 다른 양상을 띄게 됩니다.
```javascript
//script-dist 에 추가합니다.
gulp.task('scripts-dist', function () {
  gulp.src('js/**/*.js')
    .pipe(concat('all.js'))
    .pipe(uglify())
    .pipe(qulp.dest('dist/js'));
})
```
  + 그리고 cmd 에서 `gulp scripts-dist` 를 실행합니다. 그러면 자바스크립트는 완벽하게 연결과 축소를 실행합니다.
- 참고로 압축 방법에는 `Gzip` 도 있습니다. Gzip 과 축소를 비교하는 설명입니다. (https://code.i-harness.com/ko/q/c50cf)

### 제작 task 설정하기
- 사이트의 제작 준비 버전을 제작하기 위해 전체 실시간 편집 및 시청을 건너 뛰고 대신 스크립트 배포 작업을 포함 할 수 있습니다.
```javascript
gulp.task('dist', [
  'copy-html',
  'copy-images',
  'styles',
  'lint',
  'script-dist'
])
```
- 위 코드를 작성한 후에 cmd 에서 `gulp script-dist` 을 실행합니다.
  + 만약 gulp 가 조금 오래 걸리고 브라우저를 열지 않고도 존재한다면, 모든 설정이 완료되어 dist 폴더에는 생산 준비가 완료됐음을 의미합니다.

### BABEL 을 이용한 최적화
- BABEL 은 ES6 을 사용해서 자바스크립트를 최적화하는 도구입니다.

## 자바스크립트 소스 맵
- 소스 맵은 브라우저가 현재 라인 번호를 검색하고 디버깅할 때 올바른 소스 파일을 열도록 도와줍니다. 소스 맵은 처리된 파일과 원본 파일을 연관시키는 파일입니다.
- 설정하기
  + `gulp-sourcemaps` 플러그인을 cmd 에 설치합니다.
  + 아래처럼 설정합니다.
```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.task('scripts-dist', function () {
  gulp.src('js/**/*.js');
  //우선 소스 파일을 다른 파이프로 전송하기 전에 init 을 실행합니다.
    .pipe(sourcemaps.init())
    .pipe(concat('all.js'))
    .pipe(uglify())
  /* 모든 플러그인과 파이프가 적용되었지만, 대상 지정 전에 소스 맵을 인라인하지 않기 위해
  write() 로 선택적인 위치 파라미터를 파이프합니다. */
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist/js'));
});
```
- 소스 맵은 자바스크리브 이외에도 타입스크립트, 커피스크립트, ES6, JSX 등을 지원합니다.

## 이미지 압축
- `gulp-imagemin` : 손실없이 이미지 파일을 압축해줍니다.
  + 무손실 : 파일 크기가 작아지더라도 시각적인 변경이 없습니다.
  + 플러그인을 잡은 후에는 새로운 이미지 작업 사이에 파이프를 추가, imagemin() 을 호출합니다. 모든 이미지를 가져와 안전한 최적화를 수행합니다.
- 손실 압축 : 원본의 근접한 값을 다시 만들 수 있을 뿐입니다. 참고로 `PNG 양자화` 는 이미지를 256 이하의 컬러 8비트 PNG 로 바꿉니다.
  + `npm install imagemin-pngquant` 로 설치합니다.
  + 아래 내용을 작성합니다.
```javascript
gulp.task('default', function () {
  return gulp.src('src/images/*')
    .pipe(imagemin({
      progressive : true,
      use : [pngquant()]
    }))
    .pipe(gulp.dest('dist/images'));
});
```
- 작은 이미지는 더 공격적으로 손실 압축을 할 수 있습니다.
  + 그 중에서 SVG 는 XML 형식으로 파일 크기를 늘리거나 품질을 저하하지 않으면서 무한대로 확장이 가능합니다.
