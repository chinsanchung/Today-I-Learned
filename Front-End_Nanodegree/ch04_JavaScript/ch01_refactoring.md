# changing expectations (Udacity)
## 클로저와 이벤트 리스너
```javascript

let nums = [1, 2, 3];
/*실패한 코드. 이유는 num 의 값이 계속해서 변화하기 때문입니다.
아래의 내부함수 alert 는 동일한 num 변수를 가리킵니다. (루프의 끝인 3)
익명의 함수가 클릭 이벤트에서 호출될 때마다 함수는 같은 num(3) 만들 참조합니다. */
for (let i = 0; i < nums.length; i++) {
  let num = nums[i];

  let elem = document.createElement('div');
  elem.textContent = num;

  elem.addEventListener('click', function () {
    alert(num);
  });

  document.body.appendChild(elem);
}

//클로저 사용. 이벤트 리스너를 추가하는 정확한 순간에 num 값을 가질 내부 스코프를 만듭니다.
for (let i = 0; i < nums.length; i++) {
  let num = nums[i];

  let elem = document.createElement('div');
  elem.textContent = num;

  elem.addEventListener('click', function (numCopy) {
    return function () {
      alert(numCopy);
    };
  });
  document.body.appendChild(elem);
}

```
- `function (numCopy) { }` 은 외부 함수입니다. 여기서 num 을 전달해서 바로 호출합니다.
  + 외부 함수에서 numCopy 는 num 의 복사본입니다. num 값을 numCopy 에 저장한 것입니다.
  + 외부 함수는 내부 함수를 이벤트 리스너에 return 합니다. 내부 함수는 numCopy 에 접근할 수 있습니다.
  + numCopy 는 밖에서 num 이 바뀌더라도 절대 바뀌지 않습니다.
  + 클릭하면 return 된 내부 함수를 실행, numCopy 를 경고로 출력합니다.

## Model View Octopus
- `View` : 유저가 보고 상호작용하는 것입니다. DOM elements, input elements, button, image 등이 있습니다.
- `Model` : 모든 데이터가 저장된 곳입니다. (유저로부터의 데이터 + 서버의 데이터)
- `Octopus` : `View` 와 `Model` 을 연결합니다. 그래서 애플리케이션을 만들 필요가 있을 때의 걱정을 덜어줍니다. 둘을 연결하면서도 각각 분리된 상태로 유지시키고 변경사항도 허용합니다.
  + `View` 나 `Model` 어느 한 쪽을 바꾸더라도 다른 쪽에 영향을 주지 않습니다. 둘은 각각 분리되어 있어야 복잡한 코드가 만들어지지 않습니다.
  + `View` 와 `Model` 은 직접 얘기하지 않습니다. `Octopus` 를 통해서만 영향을 줍니다.
  + 예 : 데이터를 DOM 에 연결시켜주는 함수
- [피자 앱으로 알아보는 MVO](https://github.com/udacity/ud989-pizzamvo)
  + `View` : app.js 에서 init 메소드는 세팅하는 역할, render 메소드는 뷰를 업데이트하는 역할입니다.
 ```javascript
 $(function(){
     var model = {
         init: function() {
             if (!localStorage.notes) {
                 localStorage.notes = JSON.stringify([]);
             }
         },
         add: function(obj) {
             var data = JSON.parse(localStorage.notes);
             data.push(obj);
             localStorage.notes = JSON.stringify(data);
         },
         getAllNotes: function() {
             return JSON.parse(localStorage.notes);
         }
     };
     var octopus = {
         addNewNote: function(noteStr) {
             model.add({
                 content: noteStr
             });
             view.render();
         },

         getNotes: function() {
             return model.getAllNotes();
         },

         init: function() {
             model.init();
             view.init();
         }
     };
     var view = {
         init: function() {
             this.noteList = $('#notes');
             var newNoteForm = $('#new-note-form');
             var newNoteContent = $('#new-note-content');
             newNoteForm.submit(function(e){
                 octopus.addNewNote(newNoteContent.val());
                 newNoteContent.val('');
                 e.preventDefault();
             });
             view.render();
         },
         render: function(){
             var htmlStr = '';
             octopus.getNotes().forEach(function(note){
                 htmlStr += '<li class="note">'+
                         note.content +
                     '</li>';
             });
             this.noteList.html( htmlStr );
         }
     };
     octopus.init();
 });
 ```
