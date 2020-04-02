# 프로그머스- 어서와, 자료구조와 알고리즘

### 11강. 스택

#### 정의

-   스택은 자료를 보관할 수 있는 선형의 구조를 뜻합니다.
-   하나씩 차곡차곡 데이터 원소를 넣는데(push 연산), 마지막에 넣은 것부터 역순으로 꺼내야(pop 연산) 한다는 제약이 있습니다. 후입선출 LIFO(Last In First Out)이라 부릅니다.
-   스택에서 가능한 연산
    -   size(): 스택 안의 데이터 원소의 수를 구합니다.
    -   isEmpty(): 스택이 비어있는지 판단합니다.
    -   push(x)/: 데이터 원소 x 를 추가합니다.
    -   pop(): 스택 맨 위에 저장된 데이터 원소를 제거, 반환합니다.
    -   peek(): 스택 맨 위에 저장된 데이터 요소를 반환만 합니다.

#### 오류

-   빈 스택에서 데이터 원소를 꺼내려 할 때, stack underflow 라는 오류가 생깁니다.
    -   더 이상 꺼낼 데이터가 없는데 pop 연산을 수행하려 했기 때문입니다.
-   반면, 꽉 찬 스택에 데이터 원소를 넣으면, stack overflow 라는 오류가 생깁니다.

#### 스택의 추상적 자료구조 구현

-   배열

```python
# 빈 스택 초기화
def __init__(self):
    self.data = []
# 연산들
def size(self):
    return len(self.data)
def isEmpty(self):
    return self.size() == 0
def push(self, item):
    self.data.append(item)
def pop(self):
    return self.data.pop()
def peek(self):
    return self.data[-1]
```

-   양방향 연결 리스트

```python
from doublelinkedlist import Node
from doublelinkedlist import DoubleLinkedList

class LinkedListStack:
    def __init__(self):
        self.data = DoubleLinkedList() # 빈 양방향 연결 리스트
    def size(self):
        return self.data.getLength()
    def isEmpty(self):
        return self.size() == 0
    def push(self, item):
        node = Node(item)
        self.data.insertAt(self.size() + 1, node)
    def pop(self):
        return self.data.popAt(self.size())
    def peek(self):
        return self.data.getAt(self.size()).data
```

### 12강. 스택 응용 - 수식의 후위 표기법

#### 중위 표기법, 후위 표기법

-   중위 표기법: 연산자가 피연산자들의 **사이**에 위치합니다.
    > (A + B) _ (C + D)
    > (A + (B - C)) _ D
    > A \* (B - (C + D))
-   후위 표기법: 연산자가 피연산자들의 **뒤**에 위치합니다.
    > A B + C D + _
    > A B C - + D _
    > A B C D + - \*

#### 스택으로 중위 표현식을 후위 표현식으로 바꾸기

### 13강. 스택 응용 - 후위 표기 수식 계산

이번에는 후위 표기법으로 표현된 수식의 값을 계산하는 알고리즘을 만들어봅니다.

#### 계산 예제

-   A B + C D + \*
    -   (A) -> (B) -> A pop, B pop -> (A + B) -> (C) -> (D) -> D pop, C pop -> (C + D) -> (C + D) pop, (A + B) pop -> ((A + B) \* (C + D)) -> 결과 pop 후 리턴
-   정리
    -   후위 표현식을 왼쪽부터 한 글자씩 읽기
    -   피연산자면 스택에 push
    -   연산자를 만나면 pop, (1) 연산자 (2)을 계산하고, 결과를 스택에 push
    -   마지막에 남은 원소 하나를 pop 후 리턴
