# 파이썬 기초 (Udacity)
## Use Function
### 파이썬 실행
- 파이썬의 IDLE 를 열면 우선 콘솔창이 뜹니다. 그리고 새 파일을 누릅니다. 그리고 아래의 내용을 작성합니다.
```python
import time
import webbrowser
time.sleep(10)
webbrowser.open("http://google.co.kr")
```
  + 파일을 실행하면 10초 후에 페이지가 뜹니다. (sleep 함수는 초단위입니다.)
- 파이썬에서는 함수를 바로 실행할 수 없습니다. 위에 `import time` 이런 식으로 모듈들을 먼저 가져와야만 합니다.
- 다음은 반복문으로 작성합니다.
```python
import time
import webbrowser

#반복문에서 사용할 변수를 선언합니다.
total_breaks = 3
break_count = 0

#콘솔창에 현재 시간을 띄웁니다.
print("This program started on " + time.ctime())
while(break_count < total_breaks) :
  time.sleep(10)
  webbrowser.open("http://google.co.kr")
  break_count = break_count + 1
```
  + 파이썬의 주석은 # 를 씁니다. 여러 줄일 때는 ''' 을 사용합니다.
- 현재 이 파일에서는 webbrowser 파일과 그 안에는 open 함수가 있습니다. 또한 파이썬 기본 라이브러리인 time 이 있고 time 의 함수인 ctime 과 sleep 이 있습니다. 그리고 여기에는 `Abstraction` 이 숨어 있습니다.
- `Abstraction` 은 쓰고자 하는 프로그램에 초점을 두게 해주고 함수들을 쓸 수 있게 만들어줍니다.

### 파일을 추출해서 이름 바꾸는 프로그램
- 아래의 내용을 작성합니다.
```python
import os

def rename_files() :
  #1. get file names from a folder
  file_list = os.listdir(r"C:\Users\jin\Desktop\prank")
  print(file_list)
  #2. for each file, rename filename
  for file_name in file_list :
    #파일 이름에 숫자가 있는 걸 삭제합니다. 그리고 이름을 숫자없는 이름으로 바꿉니다.
    os.rename(file_name, file_name.translate(None, "0123456789"))
rename_files()
```
  + `os.listdir` 은 특정 위치의 파일 리스트를 추출하는 역할을 합니다. 그것을 변수에 저장합니다.
  + 경로 처음에 r(rawpack) 을 쓴 이유는 윈도우 OS 에서 이 문자열을 다른 형식으로 해석하지 말라고 쓴 것입니다.
  + `os.rename(경로, 새 이름)` 으로 이름을 바꿔줍니다.
- rename 메소드 인수로 이름에서 숫자를 지우는 메소드를 넣어줍니다..
  + `string.translate(table[, deletechars])` 메소드입니다. deletechars 는 변경할 대상입니다. 문자를 삭제할 경우 table 인수를 None 으로 설정합니다. (3 버젼에서는 양식이 바뀌었습니다.)
- 이것을 실행하면 오류가 뜹니다. 이유는 경로가 대상 폴더의 상위 폴더로 되어 있어서입니다. 이것을 바꿉니다.
```python
#오류를 알기 위해 위치를 찾습니다.
saved_path = os.getcwd()
print("현재 위치는" + saved_path)
#오류를 수정합니다.
os.chdir(r"C:\Users\jin\Desktop\prank")
```
```python
import os

def rename_files() :
  #1. get file names from a folder
  file_list = os.listdir(r"C:\Users\jin\Desktop\prank")
  print(file_list)
  saved_path = os.getcwd()
  #os 에서 파일 위치를 지정합니다.
  os.chdir(r"C:\Users\jin\Desktop\prank")
  #2. for each file, rename filename
  for file_name in file_list :
    os.rename(file_name, file_name.translate(None, "0123456789"))
  #파일 경로를 saved_path 로 바꿉니다.
  os.chdir(saved_path)
rename_files()
```

- 만약 없는 파일의 이름을 바꾸거나, 이미 있는 이름으로 바꾸면 `Exception` 에러가 발생합니다.

### os
- 파이썬은 내부에 os 라는 모듈이 있습니다. 안에는 여러 메소드들이 있습니다. (listdir, rename)
  + 예- `listdir` : 해당 디렉토리의 모든 파일의 리스트를 보여줍니다.

  
