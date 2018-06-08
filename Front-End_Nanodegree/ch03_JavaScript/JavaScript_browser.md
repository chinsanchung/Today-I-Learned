# 브라우저 호환성 (Udacity)
## 브라우저와 ES6
- ES6 이 나오기 전에 출시한 브라우저에서는 ES6 이나 HTML5 가 지원되질 않습니다.
- 문제는 사용자들이 브라우저를 오류가 나기 전에는 딱히 업데이트를 하질 않는다는 점입니다.

## Polyfill
- `polyfill` (혹은 polyfiller) : 기능을 지원하지 않는 웹 브라우저에도 해당 기능을 구현해주는 코드입니다.
```javascript
/* polyfill 예시.
브라우저가 ES6 을 지원하고 startsWith 메소드를 가지고 있다면 if(~~) polyfill 을 쓸 이유가 없습니다.
 만약 존재한다면 아래의 코드들을 오버라이드 하지 않습니다.
*/
if (!String.prototype.startsWith) {
  String.prototype.startsWith = function (searchString, position) {
    position = position || 0;
    // 전달된 문자열이 this.substr~~ 문자열과 같은지를 true, false 로 return 합니다.
    return this.substr(postion, searchString.length) === searchString;
  };
}
```
- 다양한 종류의 polyfill 이 있습니다. (SVG, Canvas, Web Storage, Video, HTML5 elements, Accessibility, Web Sockets 등)

## Transpiling
### compiler 와 transplier
- compiler 는 소스 코드를 기계를 위한 낮은 레벨 언어로 바꿉니다.
- transplier 는 소스 코드를 타겟 코드로 바꿉니다. 차이점은 소스 코드와 타겟 코드는 같은 등급이라는 점입니다. 그리고 두 코드 모두 사람이 읽을 수 있습니다.
- 따라서 ES6 코드를 transplier 를 통해 ES5 코드로 바꿔 구형 브라우저에도 쓸 수 있게 만들 수 있습니다.

### Babel 사용하기
- 가장 유명한 자바스크립트 transplier 는 Babel 입니다. 바벨은 ES6 -> ES5 , JSX -> 자바스크립트, Flow -> 자바스크립트 로 바꿀 수 있습니다.
- 바벨 웹사이트에서 코드를 붙여넣기해서 바꿀 수 있습니다. 에디터에서 바꾸려면 바벨 플러그인을 사용하면 됩니다. 설치는 node 를 사용합니다.
  + [바벨 ES2015 preset](http://babeljs.io/docs/plugins/preset-es2015/)
  + 설치한 .babelrc 파일에는 어떤 플러그인으로 Transpiling 할지가 저장되어 있습니다.
