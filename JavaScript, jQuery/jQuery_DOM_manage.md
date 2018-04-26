# jQuery 문서 객체 선택, 탐색

## 기본 필터 메소드
- jQuery의 선택자를 사용하면 원하는 문서 객체를 대부분 선택할 수 있습니다.
- 기본적으로 지원하지 않는 필터로 문서 객체를 선택할 시 'filter()' 메소드를 사용해야 합니다.
  + filter() 메소드는 '$(selector).filter(selector);' 또는 '$(selector).filter(function() {});' 형태로 사용합니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').filter('.even').css({
      backgroundColor: 'black';
      color: 'white';
    });
  });

  //filter 메소드의 매개변수로 함수를 넣을 경우
  $(document).ready(function () {
    $('h1').filter(function (index) {
      return index % 3 == 0;
    }).css({
      backgroundColor: 'black';
      color: 'white'
    });
  });
</script>
<body>
  <h1>header-0</h1><h1>header-1</h1><h1>header-2</h1><h1>header-3</h1><h1>header-4</h1><h1>header-5</h1>
</body>
```
