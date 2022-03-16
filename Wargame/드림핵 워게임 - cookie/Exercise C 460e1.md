# Exercise: Cookie 🍪

---

## 문제 파일 코드 먼저 열어보기

- 사용자가 ‘admin’ 일때 FLAG 값을 password 로 하고 있다.

```python
users = {
    'guest': 'guest',
    'admin': FLAG
}
```

- username == “admin” 일 경우 FLAG 값을 출력한다.
- **이때 username 은 요청에 포함된 쿠키에 의해 결정되어 문제 발생**

```python
def index():
    username = request.cookies.get('username', None)
			# 이용자가 전송한 쿠키의 username 입력값을 가져옴
    if username:
        return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')
    return render_template('index.html')
```

## 사이트 내에서 개발자도구 이용

- **쿠키는 클라이언트의 요청에 포함되므로, 임의로 조작 가능**
    1. guest - guest 로 로그인했을때 
    
    ![스크린샷 2022-03-16 오후 6.13.14.png](Exercise%20C%20460e1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.13.14.png)
    
    1. **개발자 도구에서 쿠키에 존재하는 username 을 “admin” 으로 조작**
        
        ![스크린샷 2022-03-16 오후 6.12.03.png](Exercise%20C%20460e1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.12.03.png)
        
    2. FLAG 값 획득
        
        ![스크린샷 2022-03-16 오후 6.13.00.png](Exercise%20C%20460e1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.13.00.png)