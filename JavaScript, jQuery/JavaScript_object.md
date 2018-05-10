# JavaScript 객체
## 개요
- 객체는 자바스크립트의 데이터 구조로서 어떠한 형태의 데이터라도 저장할 수 있습니다. 그리고 key 를 이용해 데이터의 정보를 얻을 수 있습니다.
- object 안에는 property 가 있습니다. 'property = key + value'
  + property 외에 메소드도 들어갈 수 있습니다.
- 객체에도 끝맺음에는 세미콜론을 사용하는 것이 좋습니다.
- 객체는 brace {} 로 감쌉니다.
- 메소드를 사용할 경우 property 의 이름을 쓸 떄 변수의 이름을 앞에 작성해줘야 key 임을 인식합니다.
```javascript
let abc = {
  name : 'abcde'
  cal : function calculate() {
    //name 이 abc 의 property 임을 증명합니다.
    let num = abc.name.length();
  }
}
```
```javascript
let pets = {
  let name = 'cat',
  let color = 'black',
  let eyeColor = ['blue', 'yellow'],
  let weight = '4kg'
};
```
- 리터럴 표기법
```javascript
let pets = {
  name : 'cat',
  color : 'black',
  eyeColor = ['blue', 'yellow'],
  weight : '4kg',
  sleep : function areYouSleeping () { return 'zzz'; }
};

pets["eyeColor"] //return : blue, yellow
pets.eyeColor //return : blue, yellow
pets.sleep();  //return : zzz
```
  + eyeColor property 를 불러오는 방식 중 'pets["eyeColor"]' 을 '브리켓 표기법' 이라고 합니다. (property 이름에 쌍따옴표를 붙여야 합니다.)
  + 그리고 'pets.eyeColor' 로 불러오는 방식은 '점 표기법' 입니다.
- property 의 이름(아래의 규칙들은 변수의 이름을 정할 때도 적용됩니다.)
  + 첫 글자에 숫자를 넣어선 안됩니다. 넣을 경우 브리켓으론 가능하지만, 점 표기법으로는 출력할 수 없습니다.
  + 중간에 띄어쓰기나 하이픈( - ) 을 넣으면 점 표기법으로 출력이 안됩니다.
  + 여러 글자일 경우 캐멀 표기법을 권장합니다. (예 : firstChild)

## 문제 예시 01
```javascript
/*
 * Programming Quiz: Facebook Friends (7-5)
 */

// your code goes here
let facebookProfile = {
    name : 'jin',
    friends : 7,
    messages : ['from han', 'erollment', 'tax bills'],
    postMessage : function postMessage(message) {
        facebookProfile.messages.push(message);
        return facebookProfile.messages;
    },
    deleteMessage : function deleteMessage(index) {
        facebookProfile.messages.splice(index, 1);
        return facebookProfile.messages;
    },
    addFriend : function addFriend() {
        facebookProfile.friends += 1;
        return facebookProfile.friends;
    },
    removeFriend : function removeFriend() {
        facebookProfile.friends -= 1;
        return facebookProfile.friends;
    }
};
```
