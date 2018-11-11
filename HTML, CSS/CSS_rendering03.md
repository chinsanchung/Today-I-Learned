# CSS 렌더링 with 코드스피츠76 3강
## CSSOM & VENDOR PREFIX
CSS 는 브라우저마다 지원 성향이 다르므로 VENDER PREFIX 가 붙는게 일반적입니다. 새로운 기능마다 붙이고 안정적이 되면 뺍니다.
  - VENDER PREFIX 를 빼면 작동하고 붙으면 작동하지 않습니다. 붙이고 떼는 여부도 브라우저마다 다릅니다.
CSS 를 객체화해서 바꿀 수 있게 한게 CSSOM 입니다. DOM 과 같은 느낌입니다.

## CSS OBJECT MODEL
```HTML
<!-- 실체..style 태그 -->
<head>
<style id="s">.test {background: #ff0}</style>
</head>
<script type="text/javascript">
//el 은 실체가 아닙니다. 이것이 가져온 객체(style 태그)가 실체입니다.
  const el = document.querySelector('#s');
//여기서 CSS 스타일 시트라는 객체를 가져옵니다.
  const sheet = el.sheet;
//sheet 안에 있는 CSS 내부 역할들 리스트를 가져옵니다.
  const rules = sheet.cssRules;
//rules 중 하나만 가져옵니다. rules[0]=background:#ff0
  const rule = rules[0];
//rule 안에는 selectorText, style 객체가 들어있습니다.
  console.log(rule.selectorText);
  console.log(rule.style.background);
</script>
```
- 실체는 DOM 안에 감춰져 있고, 이것을 DOM 으로 포장해서 HTML 문서 안에 넣을 수 있습니다.
- 스타일 시트를 가져오려면 요소 안에 있는 CSS 스타일 시트라는 객체를 가져와야 합니다.
- 내부 역할 리스트는 유사배열입니다. 유사배열은 length 만 있고 다른 배열 메소드는 안됩니다.
- 한 개의 룰 안에는 type, selectortext 속성, style 객체가 들어있습니다.
  - 스타일 객체는 DOM 요소에 있는 것과 같습니다.
  - selectortext 는 스타일 태그 안의 '.text'입니다.
### insertRule
```HTML
<style id="s">.test {background: #ff0}</style>
<div class="red">red</div>
<div class="blue">blue</div>
<script>
//css 를 추가하려면 시트에다 추가하는 의뢰를 해야 합니다.
const sheet = el.sheet;
const rules = sheet.cssRules;

document.querySelector('.red').onclick= _=> {
  sheet.insertRule('.red{background:red}', rules.length);
  sheet.insertRule('.blue{background:blue}', rules.length);
}
console.log(Array.from(rules).map(v => v.cssText).join('\n'));
</script>
```
- CSS 를 추가할 때 rules 가 아닌 시트에다 해야 합니다. `insertRule`을 사용합니다.
  - rules.length 는 가장 마지막 인덱스를 의미합니다. 예를 들어 배열 인덱스는 0,1,2 일 때 length 는 3이므로 마지막에 넣는 것입니다. *CSS 에서 순서는 중요합니다.* 위에서 시트 객체 인덱스 1에서 width:100px 를 해도 인덱스 2에서 width:200px 를 하면 200이 됩니다.
- CSS 는 cssText 라는 innerHTML 과 비슷한 것이 있습니다. 이것을 이용해서 시트에 내용이 추가됐는지 출력합니다.
- document 에 속해있는 시트의 내용이 바뀌면 document 는 리페인트를 실시합니다. 상황에 따라 리플로우까지 처리해서 리렌더링하는 효과도 있습니다.
### deleteRule
```HTML
<script>
//blue 를 제거합니다.
document.querySelector('.blue').onclick= _=> {
  sheet.deleteRule(rules.length - 1);
}
</script>
```
이 방식으로 사용하면 CSS 객체 모델 하나만 조절해서 스타일을 바꿀 수 있어서 성능 저하도 없고 빠르며, HTML 에는 태그 하나만 정의하면 됩니다. 세계적으로 유명한 사이트들은 DOM 을 건드리지 않고, 클래스나 DOM 구조에 맞게 CSSOM 만을 조종해서 스타일을 바꿔주는 경우가 많습니다.
  - 인라인 스타일을 바꾸는 것과 비교가 안됩니다. 스타일을 직접 건들기 때문입니다.
## compatibility library
### 프레임워크 만들기
CSS 전체를 안정적으로 통제하는 프레임워크를 만들어봅니다.
1. 우선 vender prefix 를 확인합니다. 그런데 이것은 실행중에 그 속성을 확인해야만 합니다.(공식을 미리 만들 수 없습니다. 이게 이 브라우저 버젼에서 먹히나 일일이 확인)
2. 브라우저마다 지원하지 않는 속성이나 값이 있습니다. 이것도 확인해야 합니다.
3. 계층구조를 최적화합니다. 수많은 스타일시트가 있다면 각 CSS 룰의 객체들을 읽어서 계산하는 과정으로 인해 브라우저가 느려집니다. 그래서 CSS 객체 모델을 사용해 하나의 객체로 통합하고 나머지를 꺼버리면 성능을 높일 수 있습니다.(sheet.disabled = true) 만약 찾으려는 키가 없다면 아예 건드리지 않는 '우아한 실패'를 사용합니다.
만들려는  프레임워크의 클래스는 style(CSSStyleDeclare)->rule(CSSRule)x n->CSS(StyleSheet)입니다. 가장 의존성이 없는 것부터 만드는 게 쉽습니다.
#### style 클래스 만들기
1. getKey()
```javascript
const Style = (_ => {
  //진짜 속성과 가짜 속성을 묶어주는 맵입니다.
  const prop = new Map;
  //배열로 가집니다.
  const prefix = 'webkit,moz,ms,chrome,o,khtml'.split(',')
  //BASE 한테 해당 속성이 있는지 없는지 물어봅니다.
  const NONE = Symbol();
  const BASE = document.body.style;
  /*브라우저가 지원하는 진짜 키를 받을 수 있어야 합니다.
  없으면 NONE 을 반환합니다.
  */
  const getKey = key => {
//캐시(맵) 안에 표준 이름이 들어 있으면 그 캐시 안의 진짜 이름으로 반환합니다.
    if(prop.has(key)) return prop.get(key);
//키(속성)가 body 에 있다면 이 시점에 캐시(맵)로 잡습니다.
    if(key in BASE) prop.set(key, key);
    else if(!prefix.some(v => {
/*prefix 를 붙인 속성은 존재하는가? 예:webkitBackground..
webkit + B + ackground
*/
      const newKey = v + key[0].toUpperCase() + key.substr(1);
//원래 속성은 없는데 webkitBackground 는 body 에 있는가?
      if(newKey in BASE) {
//key 의 진짜 이름은 webkitBackground 였어. 이것을 캐시에 저장
        prop.set(key, newKey);
        key = newKey;
        return true;
      } else return false;
    })){
//그 키는 NONE..그 키는 없다고 해버림. 지원불가 위에 true 면 여긴 작동 x
      prop.set(key, NONE);
//prefix 로도 안되면 없는 키
      key = NONE;
    }
    return key;
  };
})();
```
getKey 는 vender prefix 를 런타임에 조사해서 진짜 이름을 얻는 방법입니다.
- key 의 리턴 3가지: return prop.get(key), key = newKey -> return true, key = NONE -> return key
2. 클래스 만들기
```javascript
const Style = (_ => {
  //...
  const getKey = {
    /*...*/
    return key;
  }  
  return class {
//rule 의 style 객체를 잡아옵니다
    constructor(style) { this._style = style; }
//키를 가져오려면 반드시 getKey 를 통해서 진짜 이름을 얻어야 합니다.
    get(key) {
      key = getKey(key);
      //키가 없다면 null 을 리턴
      if(key === NONE) return null;
      return this._style[key];
    }
    set(key, val) {
      key = getKey(key);
      if(key !== NONE) this._style[key] = val;
      return this;
    }
  };
})();
```
3. 연습
```javascript
const el = document.querySelector("#s");
//...
const rule = rules[0];
const style = new Style(rule.style);
style.set('borderRadius', '20px').set('boxShadow', '0 0 0 10px red');
```
#### rule 만들기
1. rule 만들기
룰은 원래 있던 룰 객체를 통으로 묶어버리도록 하겠습니다.
```javascript
const Rule = class {
  constructor(rule) {
    this._rule = rule;
    this._style = new Style(rule.style);
  }
  get(key) {
    return this._style.get(key);
  }
  set(key, val) {
    this._style.set(key, val);
    return this;
  }
}
```
2. 연습
```javascript
//...
//const rule = rules[0] 을 쓰지 않습니다.
const rule = new Rule(rules[0]);
rule.set('borderRadius', '20px').set('boxShadow', '0 0 0 10px red');
```
#### sheet 만들기
rule 을 더하거나 지우는 것을 부드럽게 하는 것이 주요 기능입니다.
```javascript
const Sheet = class {
  constructor(sheet) {
    this._sheet = sheet;
    this._rules = new Map;
  }
  add(selector) {
    const index = this._sheet.cssRules.length;
    //빈 룰을 만들어 진짜 스타일시트(index)안에 넣습니다.
    this._sheet.insertRule(`${selector}{}`, index);
    //진짜 cssRule 입니다.
    const cssRule = this._sheet.cssRules[index];
    const rule = new Rule(cssRule);
    this._rules.set(selector, rule);
    return rule;
  }
  remove(selector) {
//안들어있으면 할일 없이 리턴
    if(!this._rules.contains(selector)) return;
/*위 new Rule(cssRule) 안의 cssRule 이 _rule 안에 들어있습니다.
  이 식은 진짜 CSS 의 룰을 꺼내는 것입니다.
*/
    const rule = this._rules.get(selector)._rule;
//루프돌 때마다 cssRule 과 rule._rule 을 비교
    Array.from(this._sheet.cssRules).some((cssRule, index) => {
      if(cssRule === rule._rule) {
        this._sheet.deleteRule(index);
        return true;
      }
    })
  }
  get(selector) { return this._rules.get(selector); }
}
```
2. 연습
```HTML
<head>
  <style></style>
</head>
<div class="test">test</div>
<script>
  const sheet = new Sheet(document.styleSheets[1]);
  sheet.add('body').set('background', '#foo');
  sheet.add('.test').set('cssText',`
    width:200px;
    border:1px solid #fff;
    color:#000;
    background:#fff;
    `
  );
</script>
```
### keyframes
```CSS
@keyframes  size {
  from{width:0px;}
  to  {width: 100px;}
}
```
위의 CSSOM 과 다른 점은 안의 선택자가 DOM 선택자가 아니라 keyframes 선택자(from, to 등)라는 것입니다. 이 선택자들을 어떻게 다뤄야 할까요.
```javascript
const Sheet = class {
  //앞에서의 Sheet 의 add 에 추가합니다.
  add(selector) {
    const index = this._sheet.cssRules.length;
    this._sheet.insertRule(`${selector}{}`, index);
    const cssRule = this._sheet.cssRules[index];
    let rule;
    if(selector.startsWith('@keyframes')) {
      rule = new KeyFramesRule(cssRule);
    } else {
      rule = new Rule(cssRule);
    }
    this._rules.set(selector, rule);
    return rule;
  }
}
//새로 추가한 KeyFramesRule 입니다.
const KeyFramesRule = class {
  constructor(rule) {
    this._keyframe = rule;
    this._rules = new Map;
  }
  add(selector) {
    const index = this._sheet.cssRules.length;
    this._keyframe.appendRule(`${selector}{}`);
    const cssRule = this._keyframe.cssRules[index];
    const rule = new Rule(cssRule);
    this._rules.set(selector, rule);
    return rule;
  }
  remove(selector) {
    if(!this._rules.contains(selector)) return;
    const rule = this._rules.get(selector)._rule;
    Array.from(this._keyframe.cssRules).some((cssRule, index) => {
      if(cssRule === rule._rule) {
        this._keyframe.deleteRule(index);
        return true;
      }
    });
  }
};
```
2. 연습
```HTML
<style>
  .test {
    background-color: #f00;
    animation: size 1s infinite alternate;
  }
</style>
<div class="test">test</test>
<script>
const sheet = new Sheet(document.styleSheets[0]);
const size = sheet.add('@keyframes size');
size.add("from").set("width", "0");
size.add("to").set("width", "500px");
</script>
```
## typed CSSOM
차세대 CSS 객체 모델입니다. 구글이 현재 독자적인 기능들을 마음대로 넣는 상황입니다.
### 특징
`$('#someDiv').style.height=getRandomInt()+'px';` 기존의 방식입니다. 이것에서 기능 따로 문자열 따로 하는 것도 계산이라 구글은 새로 바꿨습니다.
```javascript
const h = $('#someDiv').styleMap.get('height');
$('#otherDiv').styleMap.set('height', h);
//예시2 구글이 만든 방식
el.styleMap.set('opacity', CSS.number(0.5));
//500px 라는 문자열이 아니라 숫자 500을 넣을 수 있게 됐습니다.
el.styleMap.set('height', CSS.px(500));
```
이외에도 다양한 기능들을 추가했습니다. 크롬만 지원하는 브라우저라면 이런 기능들을 써도 무방합니다.
