# React Native 의 기초, 설정
### 정의
리액트 네이티브는 네이티브 웹 어플리케이션 빌드를 도와주는 UI 라이브러리입니다.
이것의 특징은 마지막에 컴파일링할 때 HTML, CSS 로 되는게 아니라 iOS/android 네이티브 코드로 실행이 된다는 것입니다.

- 실행 : 먼저 자바스크립트와 JSX 를 이용해서 작성하고 그 후 자바스크립트가 iOS 용 Swift, android 용 java 로 변환해주는 것입니다.

리액트 네이티브는 기존의 리액트처럼 `import React, { Component } from 'react'` 를 사용하고, 추가로 `import { Text, View } from 'react-native'` 을 사용합니다.
- 웹에서 div, span 이 있는 것처럼 리액트 네이티브는 view, text 같은 전용 요소가 있고 이것들을 네이티브 코드(Swift, java)로 변환합니다. 그리고 리액트 네이티브의 컴포넌트는 기본으로 제공하는 것 외에 다양한 api 컴포넌트를 활용해 개발할 수 있습니다.

리액트 네이티브의 장점은 우선 자바스크립트로 사용한다는 점, 그리고 커뮤니티가 크고 많은 회사들이 사용하고, iOS 와 android 둘 다 사용할 수 있다는 것입니다. 다만 각 운영체제에 맞추기 위해 코드 작성이 리액트보다 엄격해야 합니다. 리액트 네이티브는 오타가 있다면 경과 화면을 띄워 어디가 틀렸는지를 알려줍니다.
즉 리액트 네이티브는 각 기능에 대해서 엄격한 규칙을 준수해야 한다는 것입니다.

리액트 네이티브의 레이아웃은 CSS 의 flexbox 를 사용합니다. 이 방식은 iOS, android 전용 언어, 도구로 레이아웃을 짜는 것보다 훨씬 편리한 방법입니다.
```javascript
//flexbox 예시
export default class App extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text>Open up App.js to start working on your App!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  },
});
```
- 여기서 styles 라는 객체를 만들었습니다. 위의 View 에서 만든 컨테이너를 붙여서 사용합니다.

### Expo
`Expo` 는 불편한 xcode(iOS) 나 안드로이드 스튜디오 없이도 리액트 네이티브 앱을 만들게 해줍니다. 즉 xcode 없이도 iOS 를 위한 시뮬레이터, 그리고 안드로이드 스튜디오를 위한 시뮬레이터를 관리해줍니다. (Expo 가 없을 때는 xcode, 안드로이드 스튜디오 로 작업을 별도로 각각 했어야 했습니다.) 다만 미리 xcode 와 안드로이드 스튜디오를 컴퓨터에 설치해둬야 합니다.
그리고 `Expo client` 를 다운받으면 앱 테스트를 편리하게 할 수 있습니다.
Expo 는 유저가 직접 업데이트를 받는 과정(이 과정은 험난합니다. 새 버전을 내면 승인과정을 거쳐야하는데 그 버전에서 버그가 있다면 골치아플 것입니다.)을 생략하고, 제작자가 원할 때마다 앱을 알아서 업데이트하도록 만들어줍니다. Expo client 에 푸시를 하면 앱이 아닌 서버의 코드를 업데이트해서 승인과정을 생략하는 것입니다.
- 설치하기 : 설치와 프로젝트 생성은 [공식 페이지 링크](https://expo.io/learn)를 참고합니다. expo client 를 설치하고 `expo init 프로젝트이름` 으로 생성 후 `expo start` 로 실행합니다.
- 실행 : 실행하면 qr 코드가 뜹니다. 폰에 설치한 Expo client 로 찍으면 시작합니다. 기본적으로 라이브 리로딩으로 설정돼있어서 코드를 고치면 저절로 새로고침을 합니다. 만약 리액트 네이티브의 기능인 핫 리로딩으로 한다면 수정한 코드를 저장하고 변경한 그 코드만을 골라서 리로드하게 해줍니다.(라이브 리로딩처럼 전체 페이지를 새로고침하지 않습니다.)
### 기능들
#### StatusBar
`StatusBar`는 맨 위의 핸드폰 표시들을 바꿀 수 있게 해줍니다. 우선 리액트 네이티브에서 `StatusBar` 를 가져온 후 컴포넌트 형태로 사용합니다.
```javascript
import { StatusBar } from 'react-native';
<StatusBar barStyle="light-content" /> //흰색 글씨로 바꿉니다.
<StatusBar barStyle="light-content" /> //기존의 검정 글씨로 바꿉니다.
<StatusBar hidden={true} /> //상태바를 숨깁니다.
```
#### flexbox
리액트 네이티브는 디자인할 때 CSS 가 아니라 flexbox 를 사용합니다. flexbox 의 언어는 기존의 CSS 와는 약간 다릅니다. (예 : background-color 는 CSS, backgroundColor 는 flexbox) 보면 캐멀 표기법으로 프로퍼티를 작성하고 그 값을 문자열로 작성하는 것을 알 수 있습니다.(자바스크립트처럼 작성합니다.)
- flex : flex 를 우선 부모가 `flex : 1` 로 정의하고, 자식 뷰 컴포넌트에서 flex 를 1, 2, 3 으로 나눠서 확장합니다.
- 리액트 네이티브에서의 flexDirection 은 디폴트로 column 입니다. flexDirection column 으로 할 경우 세로 방향으로 뷰 컴포넌트를 확장합니다.
- 부모 뷰 컴포넌트에서 justifyContent 속성을 정의하면 자식 뷰 컴포넌트의 배치를 변경할 수 있습니다.
  - justifyContent 의 속성으로는 flex-start, center, flex-end, space-around, space-between 등이 있습니다.
- alignItems : 자식 뷰 컴포넌트의 보조 축(secondary axis) 의 정렬 기준을 정합니다. 값으로는 flex-start, center, flex-end stretch 가 있습니다. 예를 들어서 `alignItems: 'center'`이고 `flexDirection: 'column'`이면 세로 방향이 주축(primary axis)이고 가로 방향이 보조 축이 됩니다. 이럴 경우 가로 방향에 대해서 가운데 정렬을 하게 됩니다.
  - 자식 뷰 컴포넌트에서는 `alignSelf` 를 사용해서 부모에서의 `alignItems` 를 이어받아 설정할 수 있습니다.
- flexbox 에서는 CSS 처럼 축약해서 상하좌우를 한번에 입력할 수 없습니다. 일일히 각각 지정해서 입력해야 합니다.

### expo 의 기능들
#### LinearGradient (expo)
`LinearGradient` 은 여러 색을 한번에 합쳐서 보여줍니다. expo 로부터 가져와서 사용합니다. `LinearGradient` 는 2개 이상의 색을 넣을 수 있습니다.
```javascript
import { LinearGradient } from 'expo';

<LinearGradient colors={["#00C6FB", "#005BEA"]} style={styles.container}>
</LinearGradient>

const styles = StyleSheet.create({
  container: {
    flex: 1
  }
})
```
#### vector-icons (expo)
expo 에서 지원하는 이미지 아이콘입니다. [vector-icons 페이지](https://expo.github.io/vector-icons/)
예를 들어 `Ionicons` 를 사용해봅니다.
```javascript
import { Ionicons } from "@expo/vector-icons"
<View style={styles.upper}>
  <Ionicons color="white" size={144} name="ios-rainy" />
</View>
const styles = StyleSheet.create({
  upper: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center",
    backgroundColor: "transparent"
  }
})
```
