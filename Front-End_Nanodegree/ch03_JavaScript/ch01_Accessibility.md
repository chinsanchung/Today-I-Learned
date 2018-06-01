# Accessibility (Udacity)
- Accessibility(접근가능성)은 유저가 웹 사이트에 쉽게 접근하고 사용가능하도록 만들어주는 목적입니다.
  + 사이트의 콘텐츠를 누구나 접근할 수 있게 만들어야 합니다.
  + 페이지의 기능들은 누구나 조작할 수 있어야 합니다.
- 인지(시각, 청각) 문제, 신체의 불편 등의 문제가 있어도 웹 콘텐츠에 접근하도록 도와야 합니다.
- 접근가능한 웹 페이지를 위해 다음 장에서 `Focus`, `Semantics`, `Styling` 을 다룰 것입니다.

## WCAG 2.0
- 웹에서 정한 접근가능성 표준 가이드라인이 있습니다.
  + [Web Content Accessiblity Guidelines 2.0](https://www.w3.org/TR/WCAG20/)
  + [Web Aim Checklist for WCAG 2.0](https://webaim.org/standards/wcag/checklist)
- WCAG 는 4가지 원칙으로 구성됩니다.
  + Perceivable : 인지가 가능해야 합니다. (예 : 시각이 약한 사람도 접근할 수 있어야 합니다.)
  + Operable : UI 컴포넌트와 콘텐츠를 탐색할 수 있어야 합니다. (예 : 터치나 마우스를 쓰지 못하는 사람은 hover 기능을 사용하지 못합니다.)
  + Understandable : 모든 사용자들은 기능을 이해하고 혼란을 피해야 합니다.
  + Robust : 다양한 사용자들이 콘텐츠에 충분히 사용할 수 있어야 합니다.

# Focus
- `Focus` 는 페이지 어디에서 키보트 이벤트를 위치시킬지를 결정합니다. (예 : 화살표키)
- `Tab Order` : `Tab` 키로 포커스를 앞으로, `Shift + Tab` 으로 뒤로 이동시킵니다.
- HTML 의 input, button, select 는 implicitly focusable 입니다.
  + 이들에게는 기본적으로 `Tab Order` 와 키보드 이벤트 핸들링이 탑재되어 있습니다.
- 이미지, 글자들은 implicitly focusable 이 아닙니다. 따라서 `Tab Order` 를 쓸 수 없습니다.
