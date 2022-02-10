# int(x,radix)

Dates: November 17, 2021
중요도: ★★★★★

- **int(x,radix)** : radix 진수로 표현된 문자열 x를 10진수로 변환 후 반환한다

```python

def solution(n):
    tmp = ''
    while n:
        tmp += str(n % 3)  #tmp는 문자열
        n = n // 3

    answer = **int(tmp, 3) #3진법으로 표기된 tmp 문자열을 10진법으로 변환**
    return answer
```