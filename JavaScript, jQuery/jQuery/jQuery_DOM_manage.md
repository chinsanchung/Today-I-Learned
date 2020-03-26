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

## 문서 객체 탐색 종료
- 'end()' : 문서 객체 선택을 한 단계 뒤로 돌립니다.
  + filter() 메소드를 제거하기 위해 end() 메소드를 사용합니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').css('background', 'orange').filter('.even').css('color', 'white').end().filter(':odd').css('color', 'red');
  });
 </script>
 ```

## 특정 위치의 문서 객체 선택
- 필터 선택자로 특정 위치에 존재하는 문서 객체를 선택할 수 있습니다.
  + 'eq()' : 특정 위치에 존재하는 문서 객체를 선택합니다.
  + 'first()' : 첫 번째에 위치하는 문서 객체를 선택합니다.
  + 'last()' : 마지막에 위치하는 문서 객체를 선택합니다.

## 문서 객체 추가 선택
- 'add()' : 문서 객체를추가로 선택합니다.
  + 'add()' 메소드를 사용하면 현재 선택한 문서 객체의 범위를 확장할 수 있습니다.
```javascript
<script>
  $(document).ready(function () {
    $('h1').css('background', 'Gray').add('h2').css('float', 'left');
  });
</script>
<body>
  <h1>header-0</h1>
  <h2>header-1</h1>
</body>
```

## 문서 객체의 특징 판별
- 'is()' : 문서 객체의 특징을 판별합니다.
  + 'is()' 메소드는 매개변수로 선택자를 입력합니다.
  + 선택한 객체가 선택자와 일치하는지 판별해 불 자료형을 리턴합니다.

## 특정 태그 선택하기
- 'find()' : 특정 태그를 선택합니다.
  + 'find()' 메소드 XML 문서에서 데이터를 추출하는데 많이 사용합니다.
```javascript
<script>
  //변수를 선언
  let xml = '';
  xml += '<friends>';
  xml += '  <friend>';
  xml += '    <name>정진</name>';
  xml += '    <language>JavaScript</language>';
  xml += '  </friend>';
  xml += '  <friend>';
  xml += '    <name>김철</name>';
  xml += '    <language>Java</language>';
  xml += '  </friend>';
  xml += '  <friend>';
  xml += '    <name>이한</name>';
  xml += '    <language>C#</language>';
  xml += '  </friend>';
  xml += '</friends>';

  // 문서 가져오기
  $(document).ready(function () {
    let xmlDoc = $.parseXML(xml);
    $(xmlDoc).find('friend').each(function (index) {
      let output = '';
      output += '<div>'
      output += ' <h1>' + $(this).find('name').text() + '</h1>'
      output += ' <p>' + $(this).find('language').text() + '</p>';
      output += '</div>';

      document.body.innerHTML += output;
    });
  });
</script>
```

## 특정 태그의 부모 태그를 선택
- 'parant()' : 특정 태그의 부모 태그를 선택합니다.
  + 다양한 문서 객체 탐색 메소드와 조합하는 경우가 많습니다.
```javascript
<body>
  <script>
    $(document).ready(function () {
      //span 태그 부모의 background 스타일 속성을 바꿈
      $('span').parent().css('background', 'blue');
    });
  </script>
  //parent() 메소드와 이벤트 연결
  <script>
    $(document).ready(function () {
      $('button').click(function () {
        //자신의 글자 변경
        $(this).text('비활성');
        //(this).parent()는 클릭한 대상의 부모를 의미함
        $(this).parent().css('background', 'red');
        $(this).parent().find('span').text('활성')
      });
    });
  </script>
  <div>
    <span>비활성</span>
    <button>활성</button>
  </div>
</body>
