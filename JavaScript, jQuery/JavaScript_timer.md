# JavaScript 로 만든 타이머
- `setInterval()` 메소드와 `clearInterval()` 메소드로 만들었습니다.
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <section class="timer-section">
    <p class="timer"></p>
    <button onclick="stop()">Stop time</button>
  </section>

  <script>
  let second = 0;
  let minute = 0;
  let timeCheck = setInterval(timer, 1000);

  function timer () {
    second += 1;

    if (second === 60) {
      minute += 1;
      second = 0;
    }
    let time = `${minute}:${second}`;
    document.querySelector('.timer').innerHTML = time;
  }
  function stop () {
    clearInterval(timeCheck);
  }


  </script>
</body>
</html>

```
