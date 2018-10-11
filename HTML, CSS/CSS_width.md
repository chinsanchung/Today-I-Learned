# width
### 메인 app 의 width 맞추기
```CSS
/*평소*/
#app {
  width: device-width;
}
@media screen and (max-width: 480px) {
  #app {
    width: 480px;
  }
}
@media screen and (min-width: 700px) {
  #app {
    width: 700px;
    margin: auto;
  }
}
```
