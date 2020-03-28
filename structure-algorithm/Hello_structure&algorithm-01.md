# 프로그머스- 어서와, 자료구조와 알고리즘

### 1강. 소개

#### 배워야 하는 이유

- 자료구조: 데이터를 활용해 특정 연산을 할 수 있는 구조.
- 알고리즘: 어떤 문제를 해결하기 위한 절차, 방법, 명령어의 집합입니다. 프로그래밍적으로는 "주어진 문제의 해결을 위한 자료구조와 연산 방법에 대한 선택"
    - 해결하고자 하는 문제에 따라 최적의 해법은 다릅니다. 올바른 선택을 위해선 자료구조를 이해해야 합니다.

### 2강. 선형 배열(Linear Array)

#### 소개

- 선형 배열은 데이터들이 일렬로 늘어선 형태를 뜻합니다. 다른 언어에서는 Array(배열)이라 부르고, 파이선에서는 list(리스트)라는 데이터형이 있습니다.

#### 리스트

- 리스트
    - 원소들을 순서대로 늘어놓은 것입니다.
    - 각 원소에는 번호(index)가 붙게 됩니다.
    - 동일 원소만을 허용하는 배열과 달리, 어떤 타입이라도 가능합니다.
- 리스트 연산
    - 리스트 길이와 관계없이 빠르게 실행: 원소 덧붙이기 `.append()`, 끝에서 꺼내기 `.pop()`은 무조건 리스트 끝에서 실행하기에 계산이 오래 걸리지 않습니다.
        - 이를 빅오 연산에서는 상수 시간에 할 수 있는 일 `O(1)`이라 표현합니다.
    - 리스트 길이에 비례해서 시간이 걸리는 연산: 원소 삽입 `.insert()`, 원소 삭제 `.del()`은 리스트가 길면 길수록 처리가 오래 걸립니다. (삽입, 삭제 후 기존의 원소들을 당기거나 뒤로 미는 과정이 필요합니다.)
        - 빅오에서는 선형 시간이라 부르며 `O(n)`이라 표현합니다.
    - 그 외 원소 탐색하기 `.index()`

```python
names = ['bob', 'cat', 'spam', 'programmers']
names[1] # cat
names[-2] # spam
# 리스트 길이와 무관한 연산
names.append('new') # ['bob', 'cat', 'spam', 'programmers', 'new']
names.pop() # 'new' 출력, ['bob', 'cat', 'spam', 'programmers'] 으로 변화
# 길이와 관련된 연산
numbers = [20, 37, 58, 72, 91]
numbers.insert(3, 65) # (index 3, 65 삽입) -> [20, 37, 58, 65, 72, 91]
del(numbers[2]) # index 2 의 원소 삭제
# numbers.pop(2) 와 차이점이 있습니다.
# 그 외
names.index('spam') # 2
```

### 3강. 정렬, 탐색

#### 정렬(sort)

- 데이터를 정해진 기준에 따라 늘어놓는 작업입니다.
- 파이썬 리스트는 정렬 기능을 제공합니다.
    - `sorted()`: 내장 함수로, 정렬된 새 리스트를 반환합니다.
    - `sort()`: 리스트의 메소드로, 기존에 주어진 리스트를 정렬합니다.

```python
L = [5,7,3,1,2]
L2 = sorted(L)
L2_reverse = sorted(L, reverse=True)
L.sort() # 반대: L.sort(reverse=True)
```

- 문자열의 경우, 사전의 순서대로 정렬합니다.
    - 문자열 길이로 순서를 정하려면, 정렬의 key 를 지정하면 가능합니다.

```python
L = ['abcd', 'xyz', 'spam']
sorted(L, key=lambda x: len(x)) # ['xyz', 'abcd', 'spam']
# key 사용하는 예 02
O = [{'name': 'John', 'score': 83}, {'name': 'Paul', 'score': 92}]
O.sort(O, key=lambda x: x['score'], reverse=True) # 점수 높은 순으로 정렬
```

#### 탐색(search)

##### 탐색 알고리즘 01 - 선형 탐색(Linear Search)

- 선형 탐색은 앞에서부터 차례대로 특정 값을 찾는 방법입니다. 리스트가 순서대로 정렬이 되어있든, 아니든 상관없이 차례대로 값을 찾습니다.
- 즉, **리스트의 길이에 비례해서 시간이 소요되는** `O(n)`입니다. (찾는 값이 없거나 끝에 있을 경우, 리스트 전부를 다 찾아야합니다.)

```python
# 아래 리스트에서 6을 찾기
L = [3, 8, 2, 7, 6, 10, 9]
# 선형 탐색
def linear_search(L, x):
    i = 0
    while i < len(L) and L[i] != x:
        i += 1
    if i < len(L):
        return i
    else:
        return -1
linear_search(L, 3) # 0
```

##### 탐색 알고리즘 02 - 이진 탐색

- 탐색하려는 리스트가 이미 정렬되어 있을 때 사용합니다. (크기 순으로 정렬되어 있는 성질을 이용하기 때문입니다.)
- 예시: [1, 2, 3, 7, 8, 12, 15, 18, 20]에서 12를 찾으시오.
    - 원래는 1에서부터 시작하지만, 중간인 8 이전을 지우고 8 ~ 20 범위로 좁힙니다. 그 후 8 ~ 20 의 중간인 15의 뒤를 지우고, 8, 12, 15로 좁힌 후 중간값은 12를 찾게 됩니다.
- 즉, **한 번 비교가 일어날 때마다 검색할 리스트를 절반으로 줄이는** `O(log n)`입니다.

```python
def solution(L, x):
    if x in L:
        lower = 0
        upper = len(L) - 1
        index = 0
        while lower <= upper:
            middle = (lower + upper) // 2
            if L[middle] == x:
                return middle
            elif L[middle] < x:
                lower = middle
            elif L[middle] > x:
                upper = middle
    else:
        return -1
```

### 4강. 재귀 알고리즘 - 기초

- 재귀 알고리즘(Recursive Algorithms)는 일반적으로 재귀 함수로 구현됩니다.
- 재귀 함수: 하나의 함수에서 **자신을 다시 호출**해서 작업을 수행합니다. 많은 문제를 재귀적으로 해결할 수 있습니다.
    - 재귀 함수 호출의 종결 조건은 매우 중요하니 주의해야 합니다.

```python
# 1 부터 n 까지 모든 자연수의 합
def sum(n):
    if n <= 1:
        return n
    else:
        return n + sum(n - 1)
```

#### 재귀 알고리즘의 효율

```python
def sum(n):
    s = 0
    while n >= 0:
        s += n
        n -= 1
    return s
# 효율성으로는 오히려 아래 함수가 더 좋습니다. O(1)이 걸립니다.
def sum2(n):
    return n * (n + 1) // 2
```

- 재귀 함수의 반대로 반복 버젼이 있습니다.
    - 둘의 복잡도는 모두 `O(n)`으로 같습니다.
    - 다만, 효율적인 측면에서는 반복 버젼이 더 좋습니다. 재귀 함수는 n 이 증가할때마다 함수를 호출하고 리턴하는 부가 작업이 필요하기 때문입니다.
- 효율성이 떨어지는(시간이 오래걸리는) 재귀 함수를 사용하는 이유는, 사람의 생각하는 방식을 코드로 직접 옮길 수 있다는 장점이 있기 때문입니다.

```python
# 추가 예제. n 팩토리얼(n!)을 구하는 공식
def what(n):
    if n <= 1:
        return 1
    else:
        return n * what(n - 1)
# 추가 예제. 피보나치 순열
# def solution(x):
#     if x < 2:
#         return x
#     first = 0
#     second = 1
#     for i in range(2, x + 1):
#         third = first + second
#         first = second
#         second = third
#     return third

def solution(x):
    if x < 2 :
        return x
    else:
        return solution(x - 1) + solution(x - 2)
```


### 5강. 재귀 알고리즘 - 응용

#### 문제 풀기

- 조합의 수 계산: n 개의 서로 다른 원소에서 m 개를 택하는 경우의 수

```python
from math import factorial

def combi(n, m):
    return factorial(n) / (factorial(m) * factorial(n - m))

def combi2(n, m):
    if n == m:
        return 1
    elif m == 0:
        return 1
    else:
        return combi2(n - 1, m) + combi2(n - 1, m - 1)
```

- 하나의 재귀 알고리즘이 주어질 때, 이것을 재귀적이지 않은 (non-recursive) 방법으로 동일하게 풀어내는 알고리즘이 존재한다는 것을 수학적으로 증명할 수 있습니다. 보통은 순환문 (loop) 을 이용해서 정해진 연산을 반복함으로써 문제의 답을 구하는데, 따라서 반복적 (iterative) 알고리즘이라고 부르기도 합니다. 일반적으로, 주어진 문제에 대해서 반복적인 알고리즘이 재귀적인 알고리즘보다 문제 풀이의 (시간적) 효율이 높습니다.
- 그럼에도 불구하고, 재귀 알고리즘이 가지는 특성 때문에 자료 구조를 이용하는 알고리즘에는 매우 직관적으로 적용할 수 있는 경우가 많습니다.