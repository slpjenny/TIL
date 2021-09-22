# EOF (End Of File)

Created: August 20, 2021 1:36 PM
Tags: BJ#10951, EOF

### 1. 예외처리

→ `try ~ except`

→ 오류가 나면 어떻게 하라는 명령을 적어두는 문장

- 아무 입력없이 `enter` 입력을 하면, while문이 종료된다.

```python
while True:
    try:
        A, B= map(int,input().split())
        print(A+B)
    except:
        break
```

---

### 2. sys 라이브러리의 readlines() 사용

- **`lines` 는 파일 전체가 들어있는 변수**
- **`sys.stdin.readlines()`** 을 사용하여 파일 입출력시, 파일의 끝까지 가져온다.
- **`line`** **은 첫번째 줄부터 마지막 줄까지 한 줄씩 들어가는 변수**
- 파일내용을 줄 단위로 처리하고 변수 A,B에 담는다.

```python
import sys

lines = sys.stdin.readlines()

for line in lines:
    A, B = map(int, line.split())
    print(A+B)
```