#jQuery each() 메소드

## 기본
- 'each()' : 매개변수로 입력한 함수로 객체나 배열의 요소를 검사하는 메소드입니다.
  + '$.each(object, function (index, item) {})'
  + '$(selector).each(function(index, item) {})'
```javascript
$(document).ready(function () {
  let array = [
    { name: 'a', color: 'blue'},
    { name: 'b', color: 'red'}
  ];

  $.each(array, function (index, item) {
    let output = '';
    output += '<a href="' + item.link + '">';
    output += '  <h1>' + item.name + '</h1>';
    output += '</a>';
    //body 태그 뒷부분에 넣어서 출력
    document.body.innerHTML += output;
  });
});
```

## 배열 관리
- each() 메소드의 매개변수로 입력하는 함수는 첫 번째 매개변수에 index를 가지며, 두 번째 매개변수에는 각각의 item을 가집니다.
```javascript
<html>
  <head>
    <meta charset="utf-8">
    <title>each</title>
    <script src="https://code.jquery.com/jquery-3.3.1.js"></script>
    <style>
      .high-light-0 { background: blue; }
      .high-light-1 { background: green; }
      .high-light-2 { background: yellow; }
      .high-light-3 { background: red; }
      .high-light-4 { background: purple; }
    </style>
    <script>
      $(document).ready(function () {
        $('h1').each(function (index, item) {
          //매개변수 자리에 item 대신 this 키워드를 주로 사용합니다.
          $(this).addClass('high-light-' + index);
        });
      });
    </script>
  </head>
  <body>
    <h1>item-0</h1>
    <h1>item-1</h1>
    <h1>item-2</h1>
    <h1>item-3</h1>
    <h1>item-4</h1>
  </body>
</html>
```
  + 출력하면 <h1>태그의 각 item에 다른 css가 적용됩니다.
