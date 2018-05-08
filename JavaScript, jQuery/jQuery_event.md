# jQuery 이벤트
## 이벤트 연결과 제거
### 연결
- 'on()' : 이벤트를 연결합니다. 다음 두 가지 형태로 사용합니다.
  + '$(selector).on(eventName, function (event) { })'
```javascript
$(document).ready(function () {
  //이벤트 연결
  $('h1').on('click', function () {
    //this : 이벤트가 발생한 객체
    $(this).html(function (index, html) {
      return html + '+';
    });
  });
});
```
  + '$(selector).on(object)'
```javascript
$(document).ready(function () {
  $('h1').on('click', function () {
    $(this).html(function (index, html) {
      return html + '+';
    });
  });
  $('h1').on({
    mouseenter: function () { $(this).addClass('속성') };
    mouseleave: function () { $(this).removeClass('속성') };
  });
});
```
- 간단한 형식의 이벤트(blur, resize, focus, scroll, click, load, error, ready 등) 은 아래의 방식을 사용합니다.
  + '$(selector).method(function (event) { });'
- 'one()' : 이벤트를 한 번만 연결합니다. 연결 후 제거하는 형식과 차이점이 없습니다.
### 제거
- 'off()' : 이벤트를 제거합니다. 아래의 형태로 사용합니다.
  + '$(selector).off()'
  + '$(selector).off(eventName)'
  + '$(selector).off(eventName, function () { })'
- 기본 이벤트를 제거, 이벤트 전달을 제거하는 메소드도 있습니다.
  + 'preventDefault()' : 기본 이벤트를 제거합니다.
```javascript
$(document).ready(function () {
//a 태그의 기본 이벤트를 제거합니다.
  $('a').click(function (event) {
    $(this).css('background-color', 'blue');
    event.preventDefault();
  });
});
```
  + 'stopPropagation()' : 이벤트 전달을 제거합니다.
```javascript
$('a').click(function (event) {
  $(this).css('background-color', 'blue');
  event.stopPropagation();
  event.preventDefault();
});
```
- 'return false' 로 stopPropagation() 메소드와 preventDefault() 메소드를 둘 다 적용할 수 있습니다.
```javascript
$('a').click(function (event) {
  $(this).css('background-color', 'blue');
  return false;
})
```

## 이벤트 강제 발생
- 'trigger()' : 이벤트를 강제로 발생시킵니다.
  + '$(selector).trigger(eventName)'
```javascript
$(document).ready(function () {
  //이벤트 연결
  $('h1').click(function () {
    $(this).html(function (index, html) {
      return html + '*';
    });
  });
  //1초마다 함수를 실행
  setInterval(function () {
    $('h1').last().trigger('click');
  });
});
```
  + '$(selector).trigger(eventName, data)' : data 는 배열으로 넣습니다.
```javascript
$(document).ready(function () {
  $('h1').click(function (event, data1, data2) {
    alert(data1 + ':' + data2);
  });
  $('h1').eq(1).trigger('click', [273, 52]);
});
```

## 윈도우 이벤트
### 무한 스크롤
```javascript
<script>
$(document).ready(function () {
  //스크롤 이벤트 발생
  $(window).scroll(function () {
    //필요한 변수를 구합니다.
    let scrollHeight = $(window).scrollTop() + $(window).height();
      //상위 개념이 애매한 태그는 document 객체에 이벤트를 연결합니다.
    let documentHeight = $(document).height();

    //스크롤의 높이와 문서의 높이가 같을 때
    if (scrollHeight == documentHeight) {
      for (let i = 0; i < 10; i++) {
        //$(a).appendTo(b) : b에 a를 추가합니다.
        $('<h1>Infinity Scroll</h1>').appendTo('body');
      }
    }
  });
});
//내부의 공간을 채웁니다.
$(document).ready(function () {
  for(let i = 0; i < 20; i++) {
    $('<h1>Infinity Scroll</h1>').appendTo('body');
  }
});
</script>
```

## 입력 양식 이벤트
- 'submit()' : submit 버튼을 누르면 발생합니다. form 태그에서 발생하므로 form 객체에 이 메소드를 연결합니다.
```javascript
$(document).ready(function () {
  $('#form 태그의 id').submit(function (event) {
    //입력 양식의 value 를 가져옵니다.
    let name = $('#input 태그의 id').val();
    let password = $('#input 태그의 id').val();

    alert(name +  ':' + password);
  })
})
```
