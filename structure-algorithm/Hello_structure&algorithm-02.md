# 프로그머스- 어서와, 자료구조와 알고리즘

### 6강. 알고리즘의 복잡도

#### 개념

- 알고리즘의 복잡도: 프로그램의 이해도가 아닌, 문제를 푸는데 얼만큼의 자원을 요구하는가를 표현할 때 알고리즘의 복잡도라는 말을 사용합니다.
- 자원
    - 시간 복잡도: 문제(데이터)의 크기와 이를 해결하는 데 걸리는 시간 사이의 관계
    - 공간 복잡도: 문제의 크기와 이를 해결하는 데 필요한 메모리 공간 사이의 관계

#### 시간 복잡도

- 구분
    - 평균 시간 복잡도: 임의의 입력 패턴을 가정했을 때 소요되는 시간의 평균
    - 최악 시간 복잡도: 가장 긴 시간을 소요하게 만드는 입력에 따라 소요되는 시간

#### Big O notation

- 점근 표기법의 하나로, 어떤 함수의 증가 양상을 다른 함수와 비교해서 표현합니다. 알고리즘의 복잡도를 표현할 때 흔히 사용합니다.
- O(log n), O(n), O(n^2), O(2^n) 등으로 표기합니다.
- 입력의 크기가 중요하지, 계수는 그다지 중요하지 않습니다.
- 예시: 입력의 크기가 n 일 경우
    - O(log n): 입력의 크기의 로그에 비례하는 시간이 소요됩니다.
    - O(n): 입력의 크기에 비례하는 시간이 소요됩니다.

#### 복잡도 예시들

##### 선형 시간 알고리즘 O(n)

- 예시: n 개의 무작위로 나열된 수에서 최댓값을 찾기
    - 무작위 나열이기에, 하나하나 끝까지 살펴보기 전까지는 값을 알 수 없습니다.
    - 그래서 Average case `O(n)`, Worst case `O(n)` 인 n 에 비례하는 선형 시간이 걸립니다.

##### 로그 시간 알고리즘 O(log n)

- 예시: n 개의 크기 순으로 정렬된 수에서, 특정 값을 찾기
    - n 이 커질수록 n 보다 log n 이 훨씬 작습니다. 그래서 O(log n) 복잡도로 문제를 풀 수 있다면 효율이 좋은 알고리즘이라 할 수 있습니다.

##### 이차 시간 알고리즘 O(n^2)

- 예시: 삽입 정렬(insertion sort)
    - 처음 주어진 배열이 정렬이 되어있다면, 순서를 바꿀 필요 없이 살펴보기만 하면 됩니다. 즉, Best case 로 `O(n)` 시간이 걸립니다.
    - 만약 역순으로 정렬되어있다면, Worst case `O(n^2)` 시간이 걸립니다.

##### 보다 낮은 복잡도를 가진 정렬 알고리즘

- 예시: 병합 정렬(merge sort) O(n * log n)
    - 정렬할 데이터를 반씩 나누어 각각 정렬합니다. `O(log n)` -> 정렬한 데이터를 두 묶음씩 합칩니다. `O(n)` -> 전체적으로는 `O(n * log n)`만큼 시간이 걸립니다.

### 7강. 연결 리스트(Linked Lists) 01

- 연결 리스트는 링크로 연결되어 있으므로, 가운데를 끊어 하나를 삭제하거나 다른 원소를 삽입하는 것이 선형 배열보다 빠르게 처리할 수 있습니다.
    - 원소의 삽입, 삭제가 빈번히 일어나는 경우에 많이 사용합니다. (예: 운영체제)
- 단점은, 선형 배열에 비해 데이터 구조 표현에 필요한 저장 공간(메모리) 소요가 큽니다. 또한 특정한 원소를 찾을 때 선형 배열보다 오래 걸립니다.

#### 추상적 자료구조

- 내부 구조를 숨기고, 밖에서 보이는 것들인 데이터, 연산만을 제공하는 구조입니다.
    - 데이터: 정수, 문자열, 레코드 등
    - 연산: 삽입, 삭제, 순회 또는 정렬, 탐색

#### 기본적인 연결 리스트

- 각 데이터와 링크의 묶음을 노드라 부릅니다.
    - 노드 내의 데이터는 문자열, 레코드, 다른 연결 리스트 등 다양한 형식이 있습니다.
- 앞에 있는 노드가 뒤에 있는 노드를 링크(next)로 연결해 가리키는 형태입니다.
- 알면 좋은 것을
    - 리스트의 맨 앞 Head 를 알아야만 리스트를 찾기 쉬워집니다. 
    - 또한 리스트의 맨 끝 Tail 을 알아야 새로운 노드를 추가할 때 낭비를 줄일 수 있습니다.(처음에서부터 하나씩 연결고리를 찾을 가능성)
    - 노드가 몇 개 있는지 기록하는 것도 좋습니다.
- 노드, 연결 리스트를 클래스로 만들기

```python
class Node:
    def __init__(self, item):
        self.data = item
        self.next = None
# 비어있는 연결 리스트
class LinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = None
        self.tail = None
```

```javascript
function Node(data) {
    this.head = null;
    this.size = 0;
}
```

#### 배열 vs 연결 리스트

|분류|배열|연결 리스트|
|저장 공간|연속한 위치|임의의 위치|
|특정 원소 지칭|인덱스만 알면 가능 `O(1)`|선형 탐색과 유사 `O(n)`|

#### 연결 리스트에 연산을 넣어보기

- 연산 1. 특정 원소를 참조하기

```python
# pos 번째의 노드를 찾기
def getAt(self, pos):
    if pos <= 0 or pos > self.nodeCount:
        return None
    i = 1
    curr = self.head
    while i < pos:
        curr = curr.next
        i += 1
    return curr
```
```javascript
Node.prototype.insert = function (value) {
    if (this.head === null) this.head = new Node(value)
    else {
        var temp = this.head;
        this.head = new Node(value);
        this.head.next = temp;
    }
    this.size++;
}
```

- 리스트를 순회하며 노드를 방문하기

```python
def traverse(self):
    answer = []
    curr = self.head
    while curr != None:
        answer.append(curr.data)
        curr = curr.next
    return answer
```

- 검색

```javascript
Node.prototype.find = function(value) {
    var curr = this.head;
    while (curr.next) {
        if (curr.data === value) return true;
        curr = curr.next;
    }
    return false;
}
```

### 8강. 연결 리스트(Linked Lists) 02

#### 노드 삽입, 노드 삭제, 리스트 합치기

- 노드 삽입
    - 예시로 3번 자리에 새로 넣을 경우, 3번 이후는 +1로 한 칸씩 밀리고, 결국 pos - 1(prev) 과 pos(next) 사이에 노드를 넣습니다.
    - 새 노드를 next 노드와 연결시킨 후, prev 노드를 새 노드와 연결시킵니다. `prev.next -> newNode <- prev.next` 마지막으로 nodeCount += 1 을 수행합니다. 중간의 경우 `O(n)` 복잡도를 가집니다.
    - 주의사항
        - 삽입하려는 위치가 리스트 처음: prev 없는 상황으로, Head 조정이 필요합니다. `O(1)` 복잡도를 가집니다. (상수시간)
        - 삽입하려는 위치가 리스트 끝: Tail 조정이 필요합니다. 맨 앞에서부터 찾아갈 필요가 없습니다. `O(1)` 복잡도를 가집니다. (상수시간)
        - 빈 리스트에 삽입: `if pos == 1`, `if pos == self.nodeCount + 1` 두 조건을 모두 충족하여 실행합니다.

```python
# def insertAt(self, pos, newNode):
#     prev = self.getAt(pos - 1)
#     # 순서대로 해야만 합니다.
#     newNode.next = prev.next
#     prev.next = newNode
#     self.nodeCount += 1
def insertAt(self, pos, newNode):
    if pos < 1 or pos > self.nodeCount + 1:
        return False

    if pos == 1:
        newNode.next = self.head
        self.head = newNode
    else:
        if pos == self.nodeCount + 1:
            prev = self.tail
        else:
            prev = self.getAt(pos - 1)
        newNode.next = prev.next
        prev.next = newNode
    if pos == self.nodeCount + 1:
        self.tail = newNode
    
    self.nodeCount += 1
    return True
```

- 노드 삭제
    - 우선 대상 curr 의 prev 노드를 찾고 prev.next 를  curr.next 와 연결합니다. 그리고 데이터를 꺼내 리턴하고, nodeCount -= 1 을 수행합니다. `O(n)` 복잡도를 가집니다.
    - 주의사항
        - 맨 앞의 노드를 삭제: prev 가 없기에, Head 조정이 필요합니다. `O(1)` 복잡도를 가집니다.
        - 맨 뒤의 노드를 삭제: Tail 조정이 필요합니다. nodeCount 계산을 위해 prev 노드를 찾아야하므로, `O(n)` 복잡도를 가집니다.
        - 유일한(마지막) 노드를 삭제

```javascript
Node.prototype.remove = function (value) {
    var curr = this.head;
    if (curr.data === value) {
        this.head = curr.next;
        this.size--;
    } else {
        var prev = curr;
        while (curr.next) {
            if (curr.data === value) {
                prev.next = curr.next;
                prev = curr;
                curr = curr.next;
                break;
            }
            prev = curr;
            curr = curr.next;
        }
        if (curr.data === value) {
            prev.next = null;
        }
        this.size--;
    }
}
Node.prototype.deleteAtHead = function() {
    var toReturn = null;
    if (this.head !== null) {
        toReturn = this.head.data;
        this.head = this.head.next;
        this.size--;
    }
    return toReturn;
}
```

- 두 리스트 합치기
    - `L1.concat(L2)` 형식으로 concat 메소드를 사용합니다.
   
```python
def concat(self, L):
    self.tail.next = L.head
    # L.tail 이 None 일 가능성 고려하기
    if L.tail != None:
        self.tail = L.tail
    self.nodeCount += L.nodeCount
```

### 9강. 연결 리스트(Linked Lists) 03

- 연결 리스트 활용 예시
    - 아이폰 홈을 더블 클릭해서 나오는 앱의 리스트
- 연결 리스트의 최대 장점은 **삽입과 삭제가 유연하다**는 것입니다.

#### 연결 리스트 코드 변경하기

- dummy 노드를 추가해서 삽입, 삭제를 보조합니다.
    - 그러려면, 기존의 연산도 조금 수정해야 합니다.

```python
class LinkedList:
    def __init__(self):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = None
        self.head.next = self.tail
```

- 리스트 순회

```python
def traverse(self):
    result = []
    curr = self.head
    while curr.next:
        curr = curr.next
        result.append(curr.data)
    return result
```

- 특정 원소 검색

```python
def getAt(self, pos):
    if pos < 0 or pos > self.nodeCount:
        return None
    i = 0
    curr = self.head
    while i < pos:
        curr = curr.next
        i += 1
    return curr
```

- 노드 삽입

```python
def insertAfter(self, prev, newNode):
    newNode.next = prev.next
    if prev.next is None:
        self.tail = newNode
    prev.next = newNode
    self.nodeCount += 1
    return True
# 기존의 insertAt 수정
def insertAt(self, pos, newNode):
    if pos < 1 or pos > self.nodeCount + 1:
        return False
    if pos != 1 and pos == self.nodeCount + 1:
        prev = self.tail
    else:
        prev = self.getAt(pos - 1)
    return self.insertAfter(prev, nextNode)
```

- 두 리스트 연결

```python
def concat(self, L):
    self.tail.next = L.head.next
    if L.tail:
        self.tail = L.tail
    self.nodeCount += L.nodeCount
```
