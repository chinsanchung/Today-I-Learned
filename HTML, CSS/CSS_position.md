# 위치 속성
## position
- `static` : 상대 위치로 좌표를 설정합니다.
- `relative` : 초기 위치에서 상하좌우로 조절한 위치로 좌표를 설정합니다.
- `absolute` : 절대 위치로 좌표를 설정합니다.
- `fixed` : 화면을 기준으로 절대 위치로 좌표를 설정합니다.

## z-index
`z-index` 혹성으로 지정한 태그의 순서를 바꿀 수 있습니다. 숫자가 클수록 앞에 위치합니다.

## overflow
`overflow` 속성은 내부의 요소가 부모의 범위를 벗어날 때 요소를 처리할 방법을 지정합니다.
- hidden : 영역을 벗어나는 부분을 감춥니다.
- scroll : 영역을 벗어나는 부븐을 스크롤로 만듭니다.
  - 특정 방향으로만 스크롤을 생성하고 싶을 때는 overflow-x 혹은 overflow-y 속성을 사용합니다.

## float
레이아웃을 잡을 때 사용합니다.
- left : 태그를 왼쪽에 붙입니다.
- right : 태그를 오른쪽에 붙입니다.
