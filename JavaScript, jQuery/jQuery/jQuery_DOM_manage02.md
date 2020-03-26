# jQuery 문서 객체 조작

## 클래스 속성 추가
- `addClass()` : 문서 객체의 클래스 속성을 추가합니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').addClass('item');
  });
</script>
<body>
  <h1>header-0</h1>
</body>
```
  + 실행 시 `<h1 class="item">header-0</h1>` 으로 변경됩니다.
```javascript
<script>
  //addClass() 메소드의 콜백함수
  $(document).ready(function () {
    $('h1').addClass(function (index) {
      return 'class' + index;
    });
  });
</script>
```  
  + 실행 시 `<h1 class="class0">header-0</h1>` 으로 변경됩니다.

## 클래스 속성 제거
- `removeClass()` : 문서 객체의 클래스 속성을 제거합니다.
  + removeClass() 메소드의 매개변수에 아무것도 입력하지 않으면 문서 객체의 모든 클래스를 제거합니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').removeClass('select');
  });
</script>
<body>
  <h1 class="item select">header-0</h1>
</body>
```
  + 실행 시 `<h1 class="item">header-0</h1>` 으로 변경됩니다.

## 문서 객체의 속성 검사
- `attr()` : 속성과 관련된 모든 기능을 추가합니다.
```javascript
<script>
  $(document).ready(function () {
    let src = $('img').attr('src');

    alert(src);
  });
</script>
<body>
  <img src="penguins.jpg" />
</body>
```
  + 출력시 src의 내용인 penguins.jpg가 출력됩니다.

## 문서 객체의 속성 추가
- 문서 객체의 속성 추가는 `attr()` 메소드를 사용합니다.
```javascript
//사용 방법
<script>
  //방법 1 : $(selector).attr(name, value);
  $(document).ready(function () {
    $('img').attr('width', 200);
  });

  //방법 2 : $(selector).attr(name, function(index, attr) {});
  $(document).ready(function () {
    $('img').attr('width', function (index) {
      return (index + 1) * 100;
    });
  });

  //방법 3 : $(selector).attr(object);
  $(document).ready(function () {
    $('img').attr({
      width : function (index) {
        return (index + 1) * 100;
      },
      height : 100
    });
  });
</script>
```

## 문서 객체의 속성 제거
- `removeAttr(name)` : 문서 객체의 속성을 제거합니다.
  + 첫 번째 매개변수에 삭제하려는 속성의 이름을 입력합니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').removeAttr('data-index');
  });
</script>
<body>
  <h1 data-index="0">header-0</h1>
</body>
```
  + 출력하면 `<h1>header-0</h1>` 으로 바뀝니다.
- `removeClass()` 메소드와 `removeAttr()` 메소드 모두 클래스 속성을 제거할 수 있습니다. 하지만 `removeAttr()` 는 모든 클래스 속성이 한 번에 제거되는 것과 달리 `removeClass()` 는 여러 개의 클래스 속성 중에서 선택저으로 제거할 수 있습니다.

## 문서 객체의 스타일 검사, 추가
- `css()` : 스타일과 관련된 모든 기능을 수행합니다.
```javascript
<style>
  .first { color : red; }
  .second { color : blue; }
</style>

<script>
  $(document).ready(function () {
    let color = $('h1').css('color');
    //출력하면 첫 번째 스타일 속성이 나옵니다.
    alert(color);
  });
</script>
```
- 스타일을 추가하기 위해선 아래 형태로  메소드를 사용합니다.
```javascript
<script>
  //방법 1 : $(selector).css(name, value);
  $(document).ready(function () {
    $('h1').css('color', 'red');
  });

  //방법 2 : $(selector).css(name, function (index, style) {});
  $(document).ready(function () {
    let color = ['red', 'white', 'purple'];

    //문서 객체의 스타일 변경
    $('h1').css('color', function (index) {
      return color[index];
    });
  });

  //방법 3 : $(selector).css(object);
  $(document).ready(function () {
    let color = ['red', 'white', 'purple'];

    //문서 객체의 스타일 변경
    $('h1').css({
      color : function (index) {
        return color[index];
      },
      backgroundColor : 'black'
    });
  });
</script>
```

## 문서 객체의 내부 검사, 내부 추가
- `html()` : 문서 객체 내부의 글자와 관련된 모든 기능을 수행합니다. (HTML 태그 인식)
  + 첫 번쨰 문서 객체의 내용물을 출력합니다.
- `text()` : 문서 객체 내부의 글자와 관련된 모든 기능을 수행합니다. (HTML태그 인식 안함)
  + 'html()' 메소드와 달리 선택자로 선택한 모든 문서 객체의 글자를 이어서 출력합니다.
- 문서 객체 내부에 내용물을 추가할 때는 아래 메소드를 사용합니다.
```javascript
<script>
  //방법 1 : $(selector).html(value)
  $(document).ready(function () {
    $('div').html('<h1>$().html() Method</h1>');
  });
  //$(selector).text(value);
  $(document).ready(function () {
    $('div').text('<h1>$().html() Method</h1>');
  })
</script>
<body>
  <div></div>
  <div></div>
  <div></div>
</body>
```
  + 출력 시 `<h1>$().html() Method</h1>` 이 세 div 태그 안에 삽입됩니다.

```javascript
<script>
  //방법 2 : 함수를 매개변수로 입력할 경우
  $(document).ready(function () {
    $('div').html(function (index) {
      return '<h1>header-' + index + '</h1>';
    });
  });
</script>
```

```javascript
<script>
  //방법 3 : $(selector).html(function (index, html) {});
  $(document).ready(function () {
    $('h1').html(function (index, html) {
      return '*' + html + '*';
    });
  });
  //$(selector).text(function (index, text) {});
</script>
```

## 문서 객체 제거
- `remove()` : 문서 객체를 제거합니다.
- `empty()` : 문서 객체 내부를 비웁니다.
```javascript
<script>
  $(document).ready(function () {
    //선택한 h1 태그 중 첫 번쨰 문서 객체를 제거합니다.
    $('h1').first().remove();
    //body 태그의 모든 h1 태그가 제거됩니다.
    $('div').empth();
  })
</script>
```
## 문서 객체 생성
### 방법 1
- `$()` : 문서 객체를 생성합니다.
```javascript
<script>
  $(document).ready(function () {
    //문서 객체 생성
    $('<h1></h1>');
    //문서 객체 생성 및 텍스트 노드 추가
    $('<h1></h1>').html('hello');
    //문서 객체 연결
    $('<h1></h1>').html('hello').appendTo('body');
    //문서 객체 생성 2
    $('<h1>hello</h1>').appendTo('body')
  });
</script>
```
### 방법 2
- 텍스트 노드를 갖지 않는 문서 객체 생성법입니다.
```javascript
  $(document).ready(function () {
    //$() 메소드로 문서 객체 생성 후 attr() 메소드로 속성을 입력
    $('<img />').attr('src', 'penguins.jpg').appendTo('body');
    //attr() 메소드 없이 생성하기
    $('<img />', {
      src: 'penguins.jpg',
      width: 350,
      height: 250
    }).appendTo('body');
  });
```

## 문서 객체 삽입
- 모든 메소드는 아래와 같이 사용됩니다.
  + '$(selector).~~~(content, content, ...)'
  + '$(selector).~~~(function (index) {});'
- 첫 번쨰의 content 내용에는 문자열이나 jQuery 문서 객체도 들어갈 수 있습니다.
### 방법 1
- '$(a).appendTo(b)' : a를 b의 뒷부분에 추가합니다.
- '$(a).prependTo(b)' : a를 b의 앞부분에 추가합니다.
- '$(a).insertAfter(b)' : a를 b의 뒤에 추가합니다.
- '$(a).insertBefore(b)' : a를 b의 앞에 추가합니다.
### 방법 2
- '$(a).append(b)' : b를 a의 뒷부분에 추가합니다.
- '$(a).prepend(b)' : b를 a의 앞부분에 추가합니다.
- '$(a).after(b)' : b를 a의 뒤에 추가합니다.
- '$(a).before(b)' : b를 a의 앞에 추가합니다.

## 문서 객체 이동
```javascript
<script>
  $(document).ready(function () {
    //appendTo() 메소드를 이용한 첫 번쨰 문서 객체 이동
    $('img').first().appendTo('body');
  });
</script>
```

## 문서 객체 복사
- `clone()` : 문서 객체를 복사합니다.
- `clone()` 메소드는 아래와 같이 사용합니다.
  + `$(selector).clone()`
  + `$(selector).clone(Boolean dataAndEvents)`
  + `$(selector).clone(Boolean dataAndEvents, Boolean deepDataAndEvents)`
```javascript
  $(document).ready(function () {
    //h1 태그를 선택해 복제하여 삽입합니다.
    $('div').append($('h1').clone());
  });
```
