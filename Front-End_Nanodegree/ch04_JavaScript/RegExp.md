# 정규표현식(교재: Learning JavaScript)
정규표현식은 정교한 문자열 매칭 기능을 제공합니다. 이메일 주소, URL, 전화번호처럼 보이는 문자열을 찾으려면 정규표현식에 익숙해져야 합니다. 문자열 매칭 그리고 문자열 교체 작업을 지원합니다.
## 정규식 만들기
```javascript
//간단한 정규식을 만들어봅니다.
const input = "As I was going to Saint Ives";
//단어 going 을 찾을 수 있는 정규식
const re1 = /going/;
//생성자를 사용했지만 위의 /going/ 과 결과는 같습니다.
const re2 = new RegExp('going');
```
일부 특수한 경우를 제외하면 re1 같은 간편한 리터럴 문법을 써야 합니다.
## 정규식 검색
```javascript
//예시: 3 글자 이상 + 대소문자 가리지 않음 + 모두 일치
const re = /\w{3,}/ig;

input.match(re);  //['was', 'going', 'Saint', 'Ives']
input.search(re); //세 글자 이상인 첫 단어의 인덱스 : 5
//정규식 메소드를 사용할 경우
re.exec(input); //처음 일치하는 ['was']
re.exec(input); //exec 는 마지막 위치를 기억합니다. ['going']
re.exec(input); //['Saint']
re.exec(input); //['Ives']
re.exec(input); //null 일치하는 것이 더이상 없습니다.
re.test(input); //true : input 은 세 글자 이상인 단어가 한 개 이상 있습니다.
//변수에 저장하지 않고 써도 괜찮습니다.
input.match(/\w{3,}/ig);
/\w{3,}/ig.test(input);
```
참고로 RegExp.prototype.exec 메소드는 많은 정보를 제공해도 가장 적게 쓰입니다. String.prototype.match, RegExp.prototype.test 를 많이 사용합니다.
## 정규식을 사용한 문자열 교체
```javascript
const input = "As I was going to Saint Ives";
const output = input.replace(/\w{4,}/ig, '****');
console.log(output);  //'As I was **** to **** ****'
```
네 글자 이상인 단어를 ****으로 교체하는 작업을 했습니다.
## 입력 소비
정규식은 단순히 '큰 문자열에서 부분 문자열을 찾는 방법'이라고 생각해선 안됩니다. 보다 나은 개념은 정규식이 *입력 문자열을 소비하는 패턴* 이라고 생각하는 겁니다. 찾아낸 부분 문자열은 그렇게 소비한 결과로 만들어진 부산물입니다.
- 소비 : 정규식은 문자열의 글자들을 탐색할 때 첫 알파벳이 찾으려는 단어의 글자와 일치하지 않는다면 첫 알파벳을 `소비`하고 다음 글자로 넘어갑니다. 이렇게 `소비`하다보면 찾는 단어와 일치하는 알파벳이 나올 것입니다. 일치할 가능성이 있으므로 정규식은 그것을 `소비`하지 않습니다. 이런 식으로 맞는 단어를 찾아낸다면 해당 단어를 `소비`하고 다음 알파벳으로 넘어갑니다.
```
X J A N L I O N A T U R E J X E
```
여기서는 LION 을 찾아내서 소비합니다. 그리고 또한 NATURE 도 찾을 수 있는데 아까 LION 에서 N 을 소비했으므로 단어를 만드는 것은 불가능합니다. 왜냐면 정규식은 이미 소비한 것은 절대 다시 보지 않습니다.(즉 일치하는 것을 찾기 위해 뒤로 되돌아가지 않습니다)
- 정규식이 문자열을 `소비`하는 알고리즘
  1. 문자열 왼쪽에서 오른쪽으로 진행합니다.
  2. 일단 소비한 글자로 다시 돌아오는 일은 없습니다.
  3. 한 번에 한 글자씩 움직이며 일치하는 것이 있는지 확인합니다.
  4. 일치하는 것을 찾으면 해당 글자를 한번에 소비한 후 다음 글자로 진행합니다.(이 경우는 정규식 /g 플래그로 전역 검색할 경우에 해당합니다)
## 대체
문자열에 담은 HTML 페이지에서 외부 자원을 가리키는 <a>, <area>, <link>, <script>, <source>, <meta>를 모두 찾고 싶을 때는 어떻게 할까요. 태그의 대소문자가 통일되지 않을 경우도 있습니다. 이럴 때는 정규식의 `대체`를 사용합니다.
```javascript
const html = 'HTML with <a href="/one">one link</a>, and some javascript' +
  '<script src="stuff.js">';
//이 정규식의 의미는 '텍스트에서 area, a, link, script, source 를 대소문자 가리지 말고 모두 찾으라'입니다.
const matches = html.match(/area|a|link|script|source/ig);
//결과: ["a", "link", "a", "a", "a", "a", "script", "script"]
```
여기서 `|`(파이프)는 대체를 뜻하는 메타 문자, `ig`는 대소문자 구별 없이 + 전체를 검색한다는 뜻입니다. g 플래그가 없다면 일치하는 것 중 첫 번째만 반환합니다.
정규식은 왼쪽에서 오른쪽으로 평가하기에 area 를 a 보다 먼저 썼습니다. 이 이유는 LION, NATURE 사례에서 알 수 있습니다. a 를 먼저 쓴다면 검색 시 a 를 소비하면 area 를 소비하는 게 불가능해지기 때문입니다. 그러니 이렇게 겹치는 것이 있을 때는 더 큰 것을 먼저 씁니다. 작은 것을 먼저 쓰면 큰 것을 절대 찾지 못하게 됩니다.
## HTML 찾기
정규식은 HTML 을 분석할 수 없습니다. 분석하려면 각 부분을 구성 요소로 완전히 분해할 수 있어야 하는데, 정규식은 아주 간단한 언어만 분석할 수 있습니다. 물론 정규식으로 복잡한 언어를 분석하기도 하지만, 정규식의 한계를 이해하고 상황에 따라 더 알맞은 방법을 찾아야 합니다.
정규식을
```javascript
const html = '<br> [!CDATA[[<br>]]]';
const matches = html.match(/<br>/ig);
console.log(matches) //['<br>', '<br>']
```
여기서의 진짜 <br>태그는 하나입니다. 나머지는 글자 데이터입니다. 정규식은 `<p> <a>aa</a> </p>`같은 계층적 구조에 매우 취약합니다. 다시 말하자면 정규식은 HTML 처럼 매우 복잡한 것을 검색하기에는 알맞지 않습니다.
## 문자셋
`문자셋`은 글자 하나를 다른 것으로 `대체`하는 방법을 간단히 줄인 것입니다.
```javascript
const beer99 = "99 bottles of beer on the wall " +
  'take 1 down and pass it around -- ' +
  '98 bottles of beer on the wall.';

//문자열 안의 숫자를 모두 찾습니다.
const matches = beer99.match(/0|1|2|3|4|5|6|7|8|9/g); //["9", "9", "1", "9", "8"]
//문자셋으로 간략히 표현합니다. m2 가 더 좋은 방법입니다.
const m1 = beer99.match(/[0123456789]/g);
const m2 = beer99.match(/[0-9]/g);

//글자와 숫자, 기타 구두점을 찾습니다.(사실 공백만 빼고 다 찾습니다)
const match = beer99.match(/[\-0-9a-z.]/ig) //75글자. ['9', '9', 'b', 'o'...]
//원래 문자열에서 공백만 찾습니다.
const match2 = beer99.match(/[^\-0-9a-z.]/);
/*
[" ", index: 2, input: "99 bottles of beer on the wall take 1 down and pass it around -- 98 bottles of beer on the wall.", groups: undefined]
*/
```
- matches 의 방법은 글자를 찾을 때, 숫자와 글자 모두를 찾을 때 각각 또 다시 만들어야 합니다. 그래서 `문자셋`으로 간편히 표현하는 것입니다.
- match 안의 정규식에서 숫자와 알파벳 순서는 중요치 않습니다.(`/[.a-z0-9\-]/ig`도 가능) 하이픈 `-`은 이스케이프`\`해야합니다. 그러지 않으면 하이픈을 범위 표시 메타문자로 간주합니다.
- match2 안의 정규식은 캐럿`^`을 사용했습니다. 캐럿으로 특정 문자, 또는 범위를 제외합니다.
## 자주 쓰는 문자셋
- 매우 자주 쓰이는 일부 문자셋은 단축 표기가 따로 있습니다. 이들을 클래스라고 부르기도 합니다.
  - `\d` = `[0-9]`:
  - `\D` = `[^0-9]`
  - `\s` = `[ \t\v\n\r]`: 탭, 스페이스, 세로 탭, 줄바꿈을 포함합니다. 공백으로 줄을 맞추는 문자셋입니다.
  - `\S` = `[^ \t\v\n\r]`
  - `\w` = `[a-zA-Z_]`: 하이픈과 마침표는 포함되지 않으므로 이 문자셋으로 도메인 이름이나 CSS 클래스 등을 찾을 수는 없습니다.
  - `\W` = `[^a-zA-Z_]`
위 단축 표기 중에서 가장 자주 쓰는 것은 `\s`입니다.
```javascript
const stuff =
  'hight: 9\n' +
  'medium: 5\n' +
  'low: 2\n';
//탭, 스페이스, 세로 탭, 줄바꿈을 포함 + 숫자는 상관없으며 없어도 된다 + 0-9까지 문자열 안의 숫자 찾기 + 전역 검색
const levels = stuff.match(/:\s*[0-9]/g); // [": 9", ": 5", ": 2"]
```
문자 제외 클래스 `\D`, `\S`, `W`를 사용하면 원치 않는 문자들을 빠르고 효율적으로 제거할 수 있습니다.
  - 예를 들어 전화번호를 데이터베이스에 저장하기 전에 형식을 통일하는 편이 좋습니다. 사람들이 전화번호를 쓰는 방식은 제각기 다르므로 문자셋을 사용해서 10자리 숫자로 통일하는 것이 좋습니다.
```javascript
//예 1. 전화번호
const messyPhone = '(505) 555-1515';
const neatPhone = messyPhone.replace(/\D/g, ''); //5055551515
//예 2. 공백이 아닌 글자가 최소 하나는 있어야 하는 필드에 데이터가 있는지 검사
const field = ' something ';
const valid = /\S/.test(field); //true
```
## 반복
`반복 메타 문자`는 얼마나 많이 일치해야 하는지 지정할 때 사용합니다. 예를 들어 숫자 하나를 찾지 않고 여러 개를 찾을 때 사용해봅니다.
```javascript
const match = beer99.match(/[0-9][0-9][0-9]|[0-9][0-9]|[0-9]/);
//반복 메타 문자
const match02 = beer99.match(/[0-9]+/);
```
  - 이 방식은 세 자리, 두 자리, 첫 자리 숫자를 소비해서 찾습니다. 두 자리 숫자 소비로 인해 세 자리 숫자를 못 찾는 일이 없도록 세 자리 숫자를 먼저 썼습니다.
  - 이 정규식은 두, 세 자리 숫자에서는 쓸 수 있겠지만 4자리, 5자리가 넘으면 사용하기 불편해집니다. 이럴 때는 `반복 메타 문자`를 쓰면 됩니다.
  - 문자셋 다음의 +는 그 앞의 요소가 하나 이상 있어야 한다는 뜻입니다.
`반복 메타 문자`는 그 자체로는 아무 의미도 없습니다. `반복 메타 문자`는 다섯 가지 종류가 있습니다.
  - `{n}` : 정확하게 n 개 반복합니다. 예를 들어 우편번호 숫자처럼 5자리만 필요하면 /d{5}/ 로 하면 됩니다.
  - `{n,}` : 최소한 n 개 이상 반복합니다. 예를 들어 `/\d{5,}/` 는 다섯 자리 이상의 숫자에만 일치합니다.
  - `{n, m}` : n 개 이상, m 개 이하입니다. 예를 들어 `/\d{2, 5}/`는 2, 3, 4, 5개에 일치합니다.
  - `?` : 0개 또는 1개입니다. `{0,1}`과 같습니다. 예를 들어 `/[a-z]\d?/i`는 글자가 있고, 숫자가 없거나 한 개 있는 경우에 일치합니다.
  - `*` : 숫자가 있던 없던 상관하지 않습니다. 클레이니 스타, 클레이니 클로저라고 부르기도 합니다. 예를 들어 `/[a-z]\d*/i`는 글자가 있고, 숫자가 없거나 있는 경우에 일치합니다.
  - `+` : 하나 이상입니다. 예를 들어 `/[a-z]\d+/i`는 글자가 있고, 숫자가 하나 이상 있는 경우에 일치합니다.
## 마침표와 이스케이프
정규식에서 마침표는 줄바꿈 문자를 제외한 모든 문자에 일치하는 메타 문자입니다. 입력이 어떤 문자든 상관하지 않고 소비할 때 주로 사용합니다.
```javascript
//문자열에서 우편번호 5자리만 필요하고 그 이외는 전혀 필요하지 않다고 간주합니다.
const input = 'Address : 333 Main St., Anywhere, NY, 55532. Phone: 555-555-2525.';
const match = input.match(/\d{5}.*/);
/*결과: ["55532. Phone: 555-555-2525.", index: 38, input: "Address : 333 Main St., Anywhere, NY, 55532. Phone: 555-555-2525.", groups: undefined]*/
```
하지만 도메인 이름이나 IP 주소처럼 마침표 자체가 필요할 때도 있습니다. 그 밖에도 아스테리스크나 괄호처럼 정규식 메타 문자를 글자 그대로 찾아야 할 경우도 있습니다. 정규식 특수 문자를 이스케이프해서 일반 문자로 사용하려면 그 앞에 역슬래시를 붙이면 됩니다.
```javascript
const equation = '(2 + 3.5) * 7';
const match = equation.match(/\(\d \+ \d\.\d\) \* \d/);
/*결과: ["(2 + 3.5) * 7", index: 0, input: "(2 + 3.5) * 7", groups: undefined]*/
```
## 진정한 와일드카드
마침표가 줄바꿈을 제외한 모든 문자에 일치하는 거라면, 줄바꿈 문자를 포함한 모든 문자에 일치하는 것은 어떻게 써야 할까요. 가장 널리 쓰이는 것은 `[\s\S]`입니다. 이것은 공백인 모든 문자에 일치하는 동시에, 공백이 아닌 모든 문자에 일치합니다. 한마디로 뭐든 일치합니다.
## 그룹
그룹을 사용하면 하위 표현식을 만들고 단위 하나로 취급할 수 있습니다. 그리고 그 그룹에 일치하는 결과를 나중에도 쓸 수 있도록 캡쳐할 수 있습니다. 결과를 캡쳐하는 것이 기본값이지만 캡쳐하지 않는 그룹도 만들 수 있습니다.
그룹은 괄호로 만듭니다. 캡쳐하지 않는 그룹은 `(?:[subexpression])`형태이고, 여기서 [subexpression]이 일치시키려 하는 패턴입니다.
```javascript
//도메인 이름에서 .com, .org, .edu 만 찾아봅니다.
const text = 'Visit oreilly.com today';
const match = text.match(/[a-z]+(?:\.com|\.org|\.edu)/i);
/*
결과: ["oreilly.com", index: 6, input: "Visit oreilly.com today", groups: undefined]
*/
```
그룹에도 반복을 적용할 수 있습니다. 일반적인 반복은 반복 메타 문자의 바로 왼쪽 문자 하나만 적용되지만, 그룹에서는 그룹 전체를 반복합니다.
```javascript
//http://, https://, //(프로토콜 독립 URL)로 시작하는 URL 을 찾습니다.
const html = '<link rel="stylesheet" href="http://insecure.com/stuff.css">\n' +
  '<link rel="stylesheet" href="https://secure.com/securestuff.css">\n' +
  '<link rel="stylesheet" href="//anything.com/flexible.css">';
const matches = html.match(/(?:https?)?\/\/[a-z][a-z0-9-]+[a-z0-9]+/ig); //["//insecure", "//secure", "//anything"]
```
  - 정규식 시작에는 캡처하지 않는 그룹 `(?:https?)?`가 있습니다. 처음의 물음표는 's 는 옵션이라는 뜻입니다. 일반적으로 반복은 반복 메타 문자의 바로 왼쪽에 있는 문자 하나에 적용됩니다.
  - 두 번째 물음표는 그 왼쪽(https)에 있는 그룹 전체에 적용됩니다. 그러므로 이 패턴은 빈 문자열, http, https 에 일치합니다.
  - 다음에는 이스케이프한 슬래시 두 개 `(\/\/)`가 있습니다.
  - 그 다음 문자 클래스입니다. 도메인 이름에는 글자, 숫자, 하이픈이 들어갈 수 있지만 시작은 글자여야 하며 하이픈으로 끝날 순 없습니다.
  - 이 예제는 완벽하진 않지만 연습을 위해 작성한 것입니다. 완벽한 정규식은 불가능에 가깝고 불필요합니다.
## 소극적 일치, 적극적 일치
정규식 아마추어와 전문가를 기준은 `소극적` 일치와 `적극적` 일치의 차이를 이해하는가입니다. 정규식은 기본적으로 `적극적`입니다. 그 뜻은 검색을 멈추기 전에 일치하는 것을 최대한 많이 찾으려고 한다는 뜻입니다.
```javascript
/* 예: HTML 에서 <i>에서 <strong>으로 바꾸기 */
const input = 'Regex pros know the difference between\n' +
  '<i>greedy</i> and <i>lazy</i> matching.';
input.replace(/<i>(.*)<\/i>/ig, '<strong>$1</strong>');
/* 교체 결과
"Regex pros know the difference between
<strong>greedy</i> and <i>lazy</strong> matching."
*/
```
  - $1 은 .* 그룹에 일치하는 문자열로 바꾸는 것입니다.
  - 이런 결과가 나온 이유를 알아봅니다. 정규식은 일치할 가능성이 있는 동안은 문자를 소비하지 않고 계속 넘어갑니다. 그리고 그 과정을 적극적으로 진행합니다. 여기서는 <i>를 만나면 </i>를 더는 찾을 수 없을 때까지 소비하지 않고 진행합니다. 원래 문자열에는 </i>가 두 개 있으므로, 정규식은 첫 번째 것(greedy</i>)을 무시하고 두 번째 것(lazy</i>)에서 일치한다고 판단합니다.
이 예제를 이번에는 소극적 일치로 바꿔봅니다. 반복 메타 문자 * 를 소극적으로 바꿉니다.
```javascript
input.replace(/<i>(.*?)<\/i>/ig, '<strong>$1</strong>');
/*
"Regex pros know the difference between
<strong>greedy</strong> and <strong>lazy</strong> matching."
*/
```
  - 이제 정규식 엔진은 </i>를 보는 즉시 일치하는 것을 찾았다고 판단합니다. 따라서 </i>를 발견할 때마다 그때까지 찾은 것을 소비하고, 일치하는 범위를 넓히려 하지 않습니다.
반복 메타 문자 `*, +, ?, {n}, {n,}, {n,m}`뒤에는 모두 물음표를 붙일 수 있지만 책의 필자는 `*, +`외에는 물음표를 붙여서 쓰질 않았다고 합니다.
## 역참조
그룹을 사용하면 `역참조`도 사용할 수 있습니다.
```javascript
/*
ABBA 형태의 밴드 이름을 찾습니다. PJJP 등등
서브그룹을 포함해, 정규식의 각 그룹은 숫자를 할당받습니다. 숫자는 맨 왼쪽이 1번으로 오른쪽으로 갈수록 1씩 증가합니다.
역슬래시 뒤에 숫자를 써서 이 그룹을 참조할 수 있습니다.
*/
const promo = 'Opening for XAAX is the dynamic GOOG! At the box office now!';
const bands = promo.match(/([A-Z])(A-Z)\2\1/g);
```
  - 이 정규식을 왼쪽에서 오른쪽으로 읽으면, 그룹이 두 개 있고 그 다음 `\2\1`이 있습니다. 첫 번째 그룹이 X 에 일치하고 두 번째 그룹이 A 에 일치하면 `\2`는 A, `\1`은 X 입니다.
  - 책의 필자가 실무에서 역참조를 사용하는 것은 따옴표의 짝을 맞출 때분이라고 합니다.
HTML 에는 태그의 속성값에 큰따옴표나 작은따옴표를 써야 합니다. 역참조로 쉽게 찾아봅니다.
```javascript
const html = `<img alt='A "Simple" example.'>`  +
  `<img alt="Don't abuse it!">`;
const matches = html.match(/<img alt=(['"]).*?\1/g);
// ["<img alt='A "Simple" example.'", "<img alt="Don't abuse it!""]
//연습용: 적극적 일치
const matches = html.match(/<img alt=(['"]).*\1/g)
//["<img alt='A "Simple" example.'><img alt="Don'"]
```
  - 이 예제는 지나치가 단순합니다. 다른 속성이 alt 속성보다 앞에 있거나, alt 앞에 공백이 두 개 이상이면 이 정규식으로는 아무것도 찾지 못합니다.
  - 이 그룹에서 첫 번째 그룹은 따옴표 뒤에 0개 이상의 문자를 찾습니다.(소극적 일치이므로 두 번째 <img>까지 진행하지 않습니다) 그 다음의 `\1`은 앞에서 찾은 따옴표의 짝입니다.
## 그룹 교체
그룹을 사용하면 문자열 교체도 더 다양한 방법으로 할 수 있습니다.
```javascript
//예 1: a 태그에서 href 속성이 아닌 것들을 제거합니다.
let html = '<a class="nope" href="/yep">Yep</a>';
html = html.replace(/<a .*?(href=".*?").*?>/, '<a $1>')
//html : "<a href="/yep">Yep</a>"
```
  - 역참조와 마찬가지로 모든 그룹은 1로 시작하는 숫자를 할당받습니다. 정규식에서 첫 번째 그룹은 `\1`이고, 교체할 문자열에서는 $1이 첫 번째 그룹에 해당합니다. 이번에도 소극적 일치를 써서 다른 <a> 태그까지 검색이 확장되는 일을 막았습니다.
  - 이 정규식은 href 속성의 값에 큰 따옴표가 아니라 작은따옴표를 쓴 문자열에서는 아무것도 찾지 못합니다.
```javascript
//예 2: class, href 속성만 남기고 나머지는 모두 없애봅니다.
let html = '<a class="yep" href="/yep" id="nope">Yep</a>';
html = html.replace(/<a .*?(class=".*?").*?(href=".*?").*?>/, '<a $2 $1>')
//"<a href="/yep" class="yep">Yep</a>"
```
  - 이 정규식은 class 와 href 의 순서를 바꿔서 href 가 앞, class 가 뒤에 옵니다. 이 정규식은 class 뒤에 href 가 있어야만 동작하고, 속성 값에 작은따옴표를 쓰면 동작하지 않습니다.
$1, $2 등 숫자로 참조하는 것외에도 다른 기호들이 있습니다. 자주 쓰이진 않아도 알아두면 좋습니다.
```javascript
const input = "One two three";
//일치하는 것 앞에 있는 전부를 참조하는 $`
input.replace(/two/, '($`)'); //"One (One ) three"
//일치하는 것 자체인 $&
input.replace(/two/, '($&)'); //"One (two) three"
//일치하는 것 뒤에 있는 전부를 참조하는 $'
input.replace(/two/, "($')"); //"One ( three) three"
//달러 기호 자체를 쓸 때는 $$
input.replace(/two/, "($$)"); //"One ($) three"
```
## 함수를 이용한 교체
함수를 이용하면 아주 복잡한 정규식을 좀 더 단순한 정규식으로 분할할 수 있습니다.
예시로 <a>태그를 정확한 규격에 맞게 바꾸는 작업을 해봅니다. class, id, href 속성 외에는 모두 제거합니다.문제는 허용하는 속성이 항상 있다는 보장도 없고, 모두 있더라도 순서가 뒤죽박죽일 수도 있습니다. 우선 테스트할 HTML 문자열입니다.
```javascript
const html =
  `<a class="foo" href="/foo" id="foo">Foo</a>\n` +
  `<A href="/bar" Class="bar">Bar</a>\n` +
  `<a href="/baz">Baz</a>\n` +
  `<a onclick="javascript:alert('qux')" href="/qux">Qux</a>`;
```
  - 이 문자열을 보면 경우의 수가 너무 많아 정규식만으로는 해결이 어렵습니다. 하지만 <a> 태그를 인식하는 정규식과 <a>태그의 속성 중에서 필요한 것만 남기는 정규식 두 개를 쓰면 됩니다.
<a>태그에서 class, id, href 속성을 제거합니다. 여기선 단순히 정규식만 사용하지 않고 split 메소드를 사용해 한 번에 한 가지 속성만 체크합니다.
```javascript
function sanitizeATag(atag) {
  //태그에서 원하는 부분을 뽑아냅니다.
  const parts = atag.match(/<a\s+(.*?)>(.*?)<\/a>/i);
  //parts[1]은 여는 <a>태그에 들어있는 속성입니다.
  //parts[2]는 <a>와 </a> 사이에 있는 텍스트입니다.
  const attributes = parts[1]
    //속성을 분해합니다.
    .split(/\s+/);
  return '<a ' + attributes
    //class, id, href 속성만 필요합니다.
    .filter(attr => /^(?:class|id|href)[\s=]/i.test(attr))
    //스페이스 한 칸으로 구분해서 합칩니다.
    .join(' ')
    //여는 <a>태그를 완성합니다.
    + '>'
    //텍스트를 추가합니다.
    + parts[2]
    //마지막으로 태그를 닫습니다.
    + '</a>'
}
```
  - 이 함수는 필요 이상으로 길어도 명확이 내용을 이해할 수 있습니다. 이 함수의 정규식으로는 우선 <a>태그의 각 부분을 찾는 정규식, 하나 이상의 공백을 찾아 문자열을 분리하는 정규식, 원하는 속성만 남도록 필터링하는 정규식입니다. 정규식 하나로 이 세 가지 일을 처리하려 했다면 어려웠을 것입니다.
---
sanitizeATag 함수는 <a>태그가 들어있는 HTML 블록에 사용하려고 만들었습니다. <a>태그를 찾는 정규식입니다.
```javascript
html.match(/<a .*?>(.*?)<\/a>/ig);
```
  - 이것을 사용하려면 String.prototype.replace 로 교체할 매개변수를 함수로 넘겨야 합니다.
```javascript
//예시로 콘솔에 출력할 뿐 아무것도 반환하지 않아 결과는 undefined 입니다.
html.replace(/<a .*?>(.*?)<\/a>/ig, function(m, g1, offset) {
  console.log(`<a> tag found at ${offset}. contents: ${g1}`);
});
```
  - String.prototype.replace 에 넘기는 함수는 다음 순서로 매개변수를 받습니다.
    - `m`: 일치하는 문자열 전체. `$&`와 같습니다.
    - `g1`: 일치하는 그룹(일치하는 것이 있다면). 일치하는 것이 여럿이라면 매개변수도 여러 개 받습니다.
    - `offset`: 원래 문자열에서 일치하는 곳의 offset(숫자)
    - 원래 문자열: 거의 사용하지 않습니다.
  - String.prototype.replace 는 함수가 반환하는 값을 써서 원래 문자열을 교체합니다.
---
각 <a>태그를 규격화하는 함수와 HTML 블록에서 <a>태그를 찾는 방법을 합칩니다.
```javascript
html.replace(/<a .*?<\/a>/ig, function(m) {
  return sanitizeATag(m);
});
//더 단순하게 만들어봅니다.
html.replace(/<a .*?<\/a>/ig, sanitizeATag)
```
  - sanitizeATag 함수는 String.prototype.replace 에서 콜백 함수에 넘기는 매개변수를 그대로 받게 만들었으므로, 익명 함수를 제거하고 sanitizeATag 를 직접 써서 단순하게 만들 수도 있습니다.
