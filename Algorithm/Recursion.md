# Recursion
## 1. needs
1. 종료 조건 <br/>

2. 현재 단계에서 점점 종료조건으로 다가가야 함

## 2. 관련 문제

- 하노이 탑<br/>

``` python 
def hanoii(answer, n, src, temp, dest):
    if n == 1:
        answer.append([src, dest])
    else:
        hanoii(answer, n-1, src, dest, temp)
        answer.append([src, dest])
        hanoii(answer, n-1, temp, src, dest)

def solution(n):
    answer = []
    hanoii(answer, n, 1, 2, 3)
    return answer
```

