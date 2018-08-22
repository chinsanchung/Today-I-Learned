# 리액트 컴포넌트 안에서의 함수 실행
***리액트에서는 함수를 실행할 때 뒤에 괄호를 작성하면 여러 번 실행이 됩니다.***
```javascript
//함수
filterList(e) {
  console.log(e.target.value);
}

```
```javascript
//올바른 예시
<span className="filterButton" value="cafes" onClick={this.filterList}>
  {this.state.filter === 'cafes' ?
    <img className="icon" alt="Cafe lists button" src="https://png.icons8.com/ios/40/2c3e50/tea-cup-filled.png" />
    : <img className="icon" alt="Cafe lists button" src="https://png.icons8.com/ios/40/2c3e50/tea-cup.png" />
  }
</span>

//잘못된 예시
<span className="filterButton" value="cafes" onClick={this.filterList(value)}>
  {this.state.filter === 'cafes' ?
    <img className="icon" alt="Cafe lists button" src="https://png.icons8.com/ios/40/2c3e50/tea-cup-filled.png" />
    : <img className="icon" alt="Cafe lists button" src="https://png.icons8.com/ios/40/2c3e50/tea-cup.png" />
  }
</span>
```
- 잘못된 예시로 할 경우 클릭을 하지 않았는데도 앱이 실행되면 저절로 여러 번 반복해서 클릭 이벤트를 수행하게 됩니다. 위의 경우 함수를 처음에 한 번, 그리고 앱의 데이터를 불러올 때마다 각각 출력해 총 8번을 실행했습니다.
- 따라서 이벤트 함수를 연결하려면 뒤에 괄호를 빼고 작성해야 합니다.
- 테스트를 위해 아래처럼 작성했었는데 이 경우도 a를 여러번 실행하는 문제가 발생합니다.
```javascript
<span className="aaa" onClick={console.log('a')}
```
## 매개변수가 있는 함수를 실행할 경우
- 매개변수가 있는 함수를 실행할 경우에는 익명 함수 형태로 필요한 인수를 전달합니다.
```javascript
//함수
filterList(e) {
  console.log(e);
}
//render() 안에서의 html
<img className="icon" onClick={() => this.filterList('cafes')} alt="Cafe lists button" src="https://png.icons8.com/ios/40/2c3e50/tea-cup.png" />
```
