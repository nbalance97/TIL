# Queue
## Queue란?
- First in First Out의 구조를 가짐.
- Python의 deque를 import해서 사용 가능(popleft, append) <br/>


## collections.deque
- Deque 자료구조를 쉽게 사용할 수 있음.
- appendleft(왼쪽 삽입), append(오른쪽 삽입)
- popleft(왼쪽 뺌), pop(오른쪽 뺌) <br/>




## 관련 문제
<hr>

- [Programmers] 숫자 게임 <br/>
```python

from collections import deque

def solution(A, B):
    answer = 0
    A.sort()
    B.sort()
    B_queue = deque(B)
    
    for a in A:
        while B_queue:
            b = B_queue.popleft()
            if b > a:
                answer += 1
                break
        
        if not B_queue:
            break
        
    
    return answer

```