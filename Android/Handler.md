# Handler

### Android 의 UI 작업은 Main Thread에서

- 다른 스레드에서 UI 처리를 해야할 때, **해당 스레드와 메인 스레드를 연결**해주는 것이 `Handler` 이다.
- Main Thread 에서 UI 작업이 이루어지지만, 시간이 오래 걸리는 작업을 하면 안된다. UI 처리가 늦어지면 당연히 어플 구동 속도가 느려진다.<br><br>


### Handler 는 어떻게 사용할까?

일반적으로 UI 갱신을 위해 사용한다. **반복적으로 UI 를 갱신**하는 역할을 한다.

1. 다른 스레드에게 메시지를 전달하려면, **수신 대상 스레드에서 생성한 handler의 post/sendMessage 함수를 사용**한다.
2. 수신 대상 스레드의 Message Queue에 message나 runnable이 저장된다

    → Message Queue는 handler가 전달하는 message를 보관하는 FIFO방식의 Queue 이다.

3. handler 가 넣어놓은 message나 runnable 을 Looper가 차례로 Message Queue에서 빼내어 **MainThread에서 실행시킨다.** <br><br>

**주요 함수**

- `Handler.sendMessage(Message msg)`: Message 객체를 message queue에 전달하는 역할
- `Handler.sendEmptyMessage(int wht)`: Message의 wht 필드를 전달하는 함수
- `post()`: post()는 Message 객체가 아닌 **Runnable 객체**를 Message queue에 전달한다. ex) Handler.post(new Runnable())

---

## Example

- Main Thread 에서 Handler 를 생성

```java
// Creates a new Handler
        final Handler handler
                = new Handler();
```

- Work Thread 에서 post() 로 실행시키기

```java
// Call the post() method, passing in a new Runnable.
        // The post() method processes code without a delay,
        // so the code in the Runnable
        // will run almost immediately.
        handler.post(new Runnable() {
            @Override

            public void run()
            {
							//여기에 UI 작업 코드 작성

							//반복해서 UI갱신할건데, 그 패터을 작성(몇 초에한번?)
							handler.postDelayed(this, 1000);
						}
				});
```

- 즉, Main Thread의 Main Handler 를 통해 run()을 실행한다.
---
### Reference
https://velog.io/@jaeyunn_15/Android-Handler