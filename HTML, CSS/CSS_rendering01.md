# CSS 렌더링 with 코드스피츠76
## 그래픽 시스템
한 마디로 말하자면 점 찍는 방법입니다.
1. 원시적인 형태는 고정된 숫자로 표현했습니다. 문제는 고정값으로 만든 그래픽 시스템은 다양한 환경에 적응을 못합니다.
2. 스크린 사이즈, 크롬 사이즈(닫기버튼, 최소화 UI), 상속은 메타포를 사용합니다. : %, left, block, inline, float
  - 부모나 화면 등 특정 대상을 기준으로 비율을 나타낸 것입니다. 예를 들어 %는 실제 화면에 그려질 때 환경을 인식해서 숫자로 바꿔주는 것입니다. 이것은 공식이자 함수입니다. 숫자로 쓰는 것이 빠르지만 환경에 적응하기 위해 공식을 적고 그것으로 숫자를 계산하는 것입니다.
  - 메타포는 따라서 함수라고 부를 수 있습니다.
3. 컴포넌트: HTML 이 예입니다. 그리고 각 HTML 체계를 묶은 것이 프레임워크입니다.
4. 프레임워크
(서양인들은 상대적인 관점으로 생각합니다. 자식은 자식이 아니라 손자 입장에서는 부모인 것처럼 자식이라는 단어도 개념에 따라 의미가 달라집니다.)

## 렌더링 시스템
렌더링은 어떠한 대상을 자신이 원하는 모습으로 다시 그려내는 것입니다. 여기서는 그림을 표현하기 위한 정보를 나타냅니다.
렌더링은 보통 2단계를 통합니다.
  - 첫째로는 박스를 찾는 것(geometry calculate)입니다. 어떻게 영역이 나눠지는 지를 찾습니다.
  - 두 번째로 색칠(fragment fill)입니다. 픽셀로 표현하지 않습니다. 픽셀이란 단어는 중립적이지 않습니다.

## normal flow
### 포지션

position : 속성들의 너비를 계산하는 계산식입니다. 여기서 static, relative 만 normal flow 가 적용됩니다. absolute, fixed, inherit 는 적용되지 않습니다.
- `normal flow`는 계산 공식이 2가지 있습니다.
  - block formatting contexts
  - inline formatting contexts
  - relative positioning 은 포지션 모델에서 정의하기에 bfc, ifc 를 위주로 봅니다.
![rendering01_1](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/rendering01_1.jpg)
### 블록
블록은 부모의 가로 길이를 가득 채운 한 줄입니다. 블록은 x 는 언제나 부모의 width 입니다. 그래서 블록은 다음 블록에서의 y 자리만 고려하면 됩니다.(두 번째 블록 요소는 첫 번째 블록이 끝나는 지점이라고 계산합니다)
블록은 내용의 height 가 곧 블록의 height 가 됩니다.
- 예시
```HTML
<div style="width:500px; background:red">&nbsp;</div>
<div style="background:blue">&nbsp;</div>
```
![rendering01_2](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/rendering01_2.jpg)
  - 위의 블록은 비록 너비를 지정하기에 절반만 붉은 색으로 칠해집니다. 그래서 남은 흰 공간은 비었다고 생각하기 쉽지만, 블록은 부모의 너비를 전부 차지하기에 빈 공간 끝까지가 븕은색 div 인 것입니다. 그래서 파란색 div 가 다음 줄에서 실행됩니다.
```HTML
<div style="width:200px background:red">aaaaaaaaaaaaaaaaaaa</div>
<div style="width:200px">
  aaaaaaaa
  aaaaaaaa
  aaaaaaaa
</div>
```
  - a 를 붙여놓으면 div 를 뚥고 나옵니다. 반대로 띄어쓰기로 하면 뚥고 나가지 않고 아래로 내려갑니다. 왜냐면 스페이스나 엔터같은 공백 문자를 삽입하는 순간 안의 내용을 인라인으로 판단하기 때문입니다.
  - 공백 문자가 없는 글자라도 워드 브레이크라는 것을 주면 인라인으로 처리합니다만 글자 하나하나를 다 지오메트리로 잡아서 영역이 늘어나므로 매우 느려집니다.
### 인라인
인라인은 자신의 콘텐츠 내용만큼 너비를 차지합니다. 그 다음 인라인은 전 인라인이 끝나는 x 자리로 결정합니다. 그러나 인라인들의 너비 합이 부모의 너비를 넘어서면 다음 줄로 이동합니다.(현재 줄의 인라인 중에서 가장 큰 인라인의 line-height 를 기준으로 y 자리를 계산해서 다음 줄로 넘어가는 것입니다.)
인라인을 넣다가 블록을 넣는 순간 인라인 옆이 아닌, 새로운 줄에서 블록을 시작합니다.
### 인라인 + 블록
```HTML
<div style="width:500px">
  hello
  <span>
    world
    <div style="background:red">&nbsp;</div>
  </span>
  !!
</div>
```
![rendering01_3](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/rendering01_3.jpg)
- DOM 포함 관계와 렌더링의 포함 관계는 다릅니다. 브라우저 눈에는 블록, 인라인, 블록으로만 보일 뿐입니다.
```HTML
<div style="width:500px">
  **
  <span>
    hello
    <span>world
      <div style="background:red">&npsp;</div>
    </span>
    !!
    <div style="background:blue">&nbsp;</div>
  </span>
  **
</div>
<!--
결과
** hello world
븕은 색 한 줄
!!
파란색 한 줄
**
-->
```
### relative positioning
```HTML
<div style="width:500px">
  **
  <span>
    hello
    <span style="position:relative;top:50px">world
      <div style="background:red">&nbsp;</div>
    </span>
    !!
    <div style="background:blue">&nbsp;</div>
  </span>
  **
</div>
```
![rendering01_4](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/rendering01_4.jpg)
relative 는 static 으로 그린 후 relative 로 위치를 이동시킨 포지션입니다.
만약 static 과 relative 가 섞여 있다면, 무조건 relative 가 static 위로 뜹니다.
relative 때문에 그림의 너비, 높이가 바뀌질 않습니다. 미리 static 으로 그림을 다 그린 다음에 포지션을 relative 하게 옮겨주는 것입니다.
HTML 의 모든 값을은 기본으로 static 포지션입니다.

## float
새로운 블록, normal flow 너버로 float, text, 인라인 가드, 라인 박스로 그립니다. float 와 overflow 는 익스플로러와 호환할 수 있는 유일한 수단이기에 많이 사용합니다.
### 블록 + float
```HTML
<div style="width:500px">
  <div style="height:50px;background:red"></div>
  <div style="width:200px;height:150px;float:left;background:green"></div>
  <div style="height:50px;background:skyblue"></div>
</div>
```
![float001](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float001.jpg)
float 가 나오면 새로운 블록 영역이 생깁니다. float 는 추가적인 블록 박스를 만드는 역할을 합니다.
```HTML
<div style="width:500px">
  <div style="height:50px;background:red"></div>
  <div style="width:200px;height:150px;float:left;background:green"></div>
  hello
  <div style="height:50px;background:skyblue">world</div>
  !!
</div>
```
![float002](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float002.jpg)
인라인 영역 글자들이 있지만 float 는 인라인 가드 역할이기에 float 영역에선 글자가 나오질 않습니다. 블록은 상관없으므로 스카이블루 블록 위에는 글자가 올 수 있습니다.
참고로 초록색을 투명하게 해서 보이는 것인데 스카이블루 div 는 끝까지 그려진 것입니다. 인라인만 가드된 것이니 착각하면 안됩니다.
### 라인 박스 + 인라인 가드
```HTML
<style media="screen">
  .left {
    background-color:green;
    float:left;
  }
  .right {
    background-color: red;
    float:right;
  }
</style>
<div style="width:500px">
  <div class="left" style="width:200px;height:150px;">1</div>
  <div class="right" style="width:50px;height:150px;">2</div>
  <div class="right" style="width:50px;height:100px;">3</div>
  <div class="left" style="width:150px;height:50px;">4</div>
  <div class="right" style="width:150px;height:70px;">5</div>
  <div class="left" style="width:150px;height:50px;">6</div>
  <div class="left" style="width:150px;height:50px;">7</div>
  <div style="height:30px;background:grey">abc</div>
  <div style="height:30px;background:grey">abc1 abc2 abc3 abc4 abc5 abc6 abc7 abc 8</div>
</div>
```
라인 박스는 오직 float 만 신경씁니다.
1. 첫째로 왼쪽 끝에 박스를 그립니다. 왼쪽은 초록, 오른쪽은 붉은색입니다.
2. 두번째 라인 박스에서는 첫째로 한 영역을 제외합니다. 첫째가 float 로 블록 영역을 차지해버렸기 때문입니다. 그래서 첫째 초록을 제외한 영역이 라인 박스입니다.
3. 1과 2만큼 줄어든 라인 박스에서 3을 그립니다. 줄어든 3만큼 라인박스를 다시 정합니다.
4. 4번도 라인 박스를 새로 줄여서 차지합니다.
5. 5번은 남아있는 라인 박스에 150 너비를 넣는 것은 불가능합니다. 그래서 현 라인 박스는 더 이상 쓰지 않고 그것을 기준으로 근처의 남은 빈 공간을 기준으로 새로운 라인 박스를 만듭니다.
6. 5번이 남긴 라인 박스로는 6을 채우지 못합니다. 다시 밑의 새 공간을 라인 박스로 잡습니다.
7. 6의 베이스라인을 기준으로 다시 밑의 공간으로 라인 박스를 잡아 7을 채웁니다.
8. 블록 박스는 맨 위의 남은 공간을 차지합니다. 왜냐면 8번째 자리긴 해도 유일한 normal flow 요소이므로 첫번째 블록 위치에 그려집니다. 그리고 안의 글자도 인라인은 블록 위에 덮어도 되므로 그려집니다.
![float003](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float003.jpg)
9. 만약 인라인 글이 길다면 어떻게 될까요. 남은 빈 공간을 차지합니다. 7번의 left로 글자가 오지 않는 이유는 float:left 의 왼쪽은 죽은 공간으로 쳐서입니다.
![float004](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float004.jpg)
- 글자들(abc2  abc5  7 아래, abc8위)이 라인 박스의 베이스라인입니다. 이걸로 다음 번 라인 박스를 계산할 수 있습니다.

## 오버플로우
오버플로우는 visible, hidden, scroll, inherit, auto 가 있습니다.
  - auto(기본값): 내부의 크기가 커지만 부모의 크기도 커집니다.
  - visible: 보일 때까지 커집니다. 일반적인 브라우저는 auto 가 visible 입니다.
  - scroll: 내 지오메트리 영역을 벗어나는 콘텐츠가 있으면 스크롤을 만듭니다.
  - inherit : 내 지오메트리 영역을 벗어나는 콘텐츠가 있으면 아예 잘라버립니다.
### 오버플로우-X, -Y
이 오버플로우는 x 축, y축 을 따로 관리하는 것입니다. 이것은 현재 draft 됐습니다. 새로 나온 것들과 충돌해서 이를 해결하기 전까지 빠진 것을 뜻합니다.
이것도 visible, hidden, scroll, clip, auto 가 있습니다.
---
그런데 오버플로우가 hidden, scroll 일 때만 float 와 관계가 있습니다. 이 때만 새로운 bfc 를 즉시 생성합니다. 이 때 첫 라인 박스 경계를 이용해서 bfc 를 만듭니다.
  - bfc 를 만들 때 부모 영역을 기준으로 너비를 계산하는데 라인 박스의 경계를 기준으로 계산합니다.
```HTML
<div style="width:500px">
  <div class="left" style="width:200px;height:150px;">1</div>
  <div class="right" style="width:50px;height:150px;">2</div>
  <div class="right" style="width:50px;height:100px;">3</div>
  <div class="left" style="width:150px;height:50px;">4</div>
  <div class="right" style="width:150px;height:70px;">5</div>
  <div class="left" style="width:150px;height:50px;">6</div>
  <div class="left" style="width:150px;height:50px;">7</div>
    <div style="height:30px;background:grey">abc1 abc2 abc3 abc4 abc5 abc6 abc7 abc 8</div>
  <div style="height:30px;overflow:hidden;background:grey">a</div>
  <div style="height:15px;overflow:hidden;background:orange">b</div>
  <div style="height:30px;background:black">c</div>
  <div style="height:30px;overflow:hidden;background:orangered">d</div>
  <div style="height:20px;overflow:hidden;background:purple">e</div>
  <div style="height:30px;background:black">f</div>
  <div style="overflow:hidden;background:skyblue">g</div>
  <div style="height:30px;background:black">h</div>
  <div style="height:30px;overflow:hidden;background:violet">i</div>
  <div style="height:30px;background:black">j</div>
</div>
```
- a 는 저번의 abc 가 한 줄을 다 차지했던 것과 달리 빈 공간 조금만 차지합니다.
- b 역시 오버플로우 히든으로 인해 새로운 bfc 영역을 만들고 라인 박스를 기준으로 너비를 정합니다.
- c 는 오버플로우 히든이 없습니다. 그래서 전의 블록 끝을 기준으로 30 높이만큼 차지합니다. 이것 때문에 b 는 c 높이만큼의 영도 차지하는 블록이 됩니다.
- d 는 오버플로우 히든으로 전의 bfc 아래를 기준으로 새로운 bfc 를 생성합니다.
- e 는 d 아래의 조그만 공간에 만들어야 하는데 높이가 20이 안되므로 공간이 없어 그리지 못합니다. 하지만 bfc 영역은 20만큼 제대로 만듭니다.
- f 는 오버플로우 없이 높이 30만 가지고 있습니다. 위의 e 의 블록은 이제 f 영역을 가진 bfc 가 됐습니다.
- g 는 더 이상 그릴 곳이 없어 skyblue 는 없습니다. 그래도 밑의 h 만큼 bfc 영역은 가집니다.
- 이제 더이상 라인 박스는 끝났으므로 i 는 제대로 그려진 bfc 영역을 가지게 됩니다.

강의의 그림
![float005_1](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float005_1.jpg)
작성한 그림
![float005_2](https://github.com/chinsanchung/Today-I-Learned/blob/master/HTML%2C%20CSS/image/float005_2.jpg)

참고 : 원래 콘텐츠가 커서 bfc 박스가 밀려날 때 visible 속성이면 늘어납니다. 그런데 라인 박스 때문에 인라인이 밀려서 커지는 것은 늘어나지 않습니다.
