# Retrofit2 로 서버연동하기

Created: August 20, 2021 1:36 PM

- **Retrofit2 안드로이드 스튜디오에서 초기설정하기**

[](https://kumgo1d.tistory.com/21)

---

# Get vs Post

> **Get**
> 
- 클라이언트에서 서버로 어떠한 정보를 요청하기 위해 사용
- ex) 게시판의 게시물 조회
- URL 주소 끝에 파라미터로 포함되어 전송되고, 이 부분을 '쿼리 스트링' 이라고 부른다.
- [www.example.com/show?name1=value1&name2=value2](http://www.example.com/show?name1=value1&name2=value2)
- 변수명1=값1&변수명2=값2 형식으로 이어붙인 것
- 서버에서는 name1 과 name2 라는 파라미터 명으로 각각 value1과 value2 의 파라미터 값을 전달 받을 수 있다.

> **Post**
> 
- 클라이언트에서 서버로 리소스를 생성하거나 업데이트하기 위해 데이터를 보낼 때 사용되는 메서드
- ex) 게시판에 게시글을 작성하는 작업
- 전송할 데이터를 HTTP 메시지 body 부분에 담아서 서버로 보낸다
- Get 에서 URL의 파라미터로 보냈던 ? 뒷부분이 body에 담겨 보내진다 생각하면 된다.
- 전송 길이 제한이 따로 없어서 용량이 큰 데이터를 보낼 때 사용.
- GET 처럼 데이터가 외부적으로 드러나는게 아니라 보안이 필요한 부분에 사용됨 (단,데이터를 암호화하지 않으면 body 부분의 데이터도 볼 수 있는건 마찬가지다)
- 멱등이 아니다

참조:

[[네트워크] get 과 post 의 차이](https://noahlogs.tistory.com/35)

---

# Retrofit

-retrofit 한글 문서

[Retrofit](http://devflow.github.io/retrofit-kr/)

- REST API 통신을 위해 구현
    
    -REST 란?
    
    - Respresentational State Transfer 의 약자로 소프트웨어 프로그램 개발의 아키텍처의 한 형식
    - '자원(resource)의 대표(representation)에 의한 상태 전달'
    - 1) 자원: 소프트웨어가 관리하는 모든 것
    - 2) 상태전달: 데이터가 요청되어지는 시점에, 자원의 상태(정보)를 전달하는 것
    - 즉, 자원을 이름으로 구분하고 해당 자원의 상태를 주고 받는 것을 REST 라고 하고, 일반적으로는 HTTP를 통해 CRUD를 실행하는 API를 말한다.

> **장점/단점**
> 
- 빠른 성능
- 간단한 구현
    
    -반복된 작업을 라이브러리로 넘겨서 처리
    
- 가독성
    
    -Annotation  사용으로 직관적인 설계
    
- 동기/비동기의 쉬운 구현

> **Retrofit2 사용하기**
> 

[[안드로이드] Retrofit2 '레트로핏' - 기본 사용법](https://jaejong.tistory.com/33)

> **Annotation '애노테이션'**
> 
- @Path
- @Query
- @QueryMap
- @Body
- @Url

---

## 내 코드

1. **권한 설정**

```java
implementation 'com.squareup.retrofit2:retrofit:2.6.0'
implementation 'com.squareup.retrofit2:converter-gson:2.6.0'
```

```java
<uses-permission android:name="android.permission.INTERNET" />

<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

 **2. 데이터 구조에 맞게 모델 클래스 생성**

```java
package com.google.sample.cloudvision;

import com.google.gson.annotations.SerializedName;

public class Post {
    @SerializedName("userId")  //각각 JSON 객체 매칭 여기서 gson사용
    private int userId;

    @SerializedName("id")
    private int id;

    @SerializedName("title")
    private String title;

    @SerializedName("body")
    private String body;

    public int getUserId(){return userId;}
    public void setUserId(int userId) {this.userId=userId;}

    public int getId() {return id;}
    public void setId(int id) {this.id = id;}

    public String getTitle() {return title;}
    public void setTitle(String title) {this.title = title; }

    public String getBody() {return body;}
    public void setBody(String body) {this.body = body;}
}
```

 **3.  Retrofit 인터페이스 생성**

```java
public interface RetrofitApi {
    //baseUrl 에 연결될 EndPoint(=URI)
    @GET("/posts")

    //Call 객체를 선언해서 HTTP요청을 웹서버로 보낸다
    //Json 데이터를 <>안의 자료형으로 받겠다. Post.class 를 따로 구현한다.
    Call<List<Post>> getData(@Query("userId") String id);

    @FormUrlEncoded //key=value&key=value 와 같은 형태로 데이터를 전달하는 것
    @POST("/posts")
    Call<Post> postData(@FieldMap HashMap<String, Object> param);
}
```

 **4. Retrofit 인스턴스 생성**

- onCreate() 안에 넣어주기

```java
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("http://jsonplaceholder.typicode.com") //주소값 중 변하지 않는 부분 (=baseUrl)
                .addConverterFactory(GsonConverterFactory.create()) //gson은 JSON을 자바 클래스로 바꾸는데 사용
                .build();

        RetrofitApi retrofitApi = retrofit.create(RetrofitApi.class);
        //data를 받아서 queue에 집어 넣는다.
        //retrofit 인터페이스에서 만든 getData 메소드 사용
        //Enqueue 로 비동기 통신실행
        retrofitApi.getData("1").enqueue(new Callback<List<Post>>() {

            public void onResponse(Call<List<Post>> call, Response<List<Post>> response) {
                if (response.isSuccessful()) {
                    List<Post> data = response.body();
                    Log.d("Test", "성공성공");
                    Log.d("Test", data.get(0).getTitle());
                }
            }

            @Override
            public void onFailure(Call<List<Post>> call, Throwable t) {
                t.printStackTrace();
            }
        });
```