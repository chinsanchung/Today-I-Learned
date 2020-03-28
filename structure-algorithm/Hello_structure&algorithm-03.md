# 프로그머스- 어서와, 자료구조와 알고리즘

### 10강. 양방향 연결 리스트

#### 소개

- 기존의 단방향 링크 연결이 아닌, 양쪽으로 링크를 연결합니다. 따라서 앞으로도, 뒤로도 진행이 가능합니다.
- 장점, 단점
    - 장점: 데이터 원소들을 차례로 방문할 때 앞에서 뒤로, 그리고 뒤에서 앞으로 방문할 수 있습니다. 운영체제 등에서 자주 쓰입니다.
    - 단점: 링크로 인한 메모리 사용량이 늘어납니다. 그리고 원소를 삽입/삭제하는 연산에서 앞, 뒤 모두를 조정해야합니다.


```python
class Node:
    def __init__(self, item):
        self.data = item
        self.prev = None
        self.next = None
```
```javascript
function Node(data) {
    this.data = data;
    this.next = null;
    this.prev = null;
}
function DoubleLinkedList(data) {
    this.head = null;
    this.tail = null;
    this.size = 0;
}
```

- 이번에도 처음과 끝에 dummy node 를 두겠습니다.

```python
class DoubleLinkedList:
    def __init__(self, item):
        self.nodeCount = 0
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None
```

#### 관련 연산

- 리스트 순회, 역순회

```python
# 순회
def traverse(self):
    result = []
    curr = self.head
    while curr.next.next:
        curr = curr.next
        result.append(curr.data)
    return result
# 역순회
def reverse(self):
    result = []
    curr = self.tail
    while curr.prev.prev:
        curr = curr.prev
        result.append(curr.data)
    return result
```

- 원소 삽입

```python
def getAt(self, pos):
    if pos < 0 or pos > self.nodeCount:
        return None
    if pos > self.nodeCount // 2:
        i = 0
        curr = self.tail
        while i < self.nodeCount - pos + 1:
            curr = curr.prev
            i += 1
    else:


def insertAfter(self, prev, newNode):
    next = prev.next
    newNode.prev = prev
    newNode.enxt = next
    prev.next = newNode
    next.prev = newNode
    self.nodeCount += 1
    return True
```

```javascript
// head
DoubleLinkedList.prototype.addAtFirst = function(value) {
    if (this.head === null) {
        this.head = new DoubleLinkedListNode(value);
        this.tail = this.head;
    } else {
        var temp = new DoubleLinkedListNode(value);
        temp.next = this.head;
        this.head.prev = temp;
        this.head = temp;
    }
    this.size++;
}
// tail
DoubleLinkedList.prototype.insertAtTail = function(value) {
    if (this.tail === null) {
        this.tail = new DoubleLinkedList(value);
        this.head = this.tail;
    } else {
        var temp = new DoubleLinkedList(value);
        temp.prev = this.tail;
        this.tail.next = temp;
        this.tail = temp;
    }
    this.size++;
}
```
- 원소 삭제

```javascript
// head
DoubleLinkedList.prototype.deleteAtHead = function() {
    var toReturn = null;
    if (this.head !== null) {
        toReturn = this.head.data;
        if (this.tail === this.head) {
            this.head = null;
            this.tail = null;
        } else {
            this.head = this.head.next;
            this.head.prev = null;
        }
    }
    this.size--;
    return toReturn;
}
// tail
DoubleLinkedList.prototype.deleteAtTail = function() {
    var toReturn = null;
    if (this.tail !== null) {
        toReturn = this.tail.data;
        if (this.tail === this.head) {
            this.head = null;
            this.tail = null;
        } else {
            this.tail = this.tail.prev;
            this.tail.next = null;
        }
    }
    this.size--;
    return toReturn;
}
```



