# Bottom Navigation

Created: August 20, 2021 1:36 PM

<aside>
1️⃣ **gradle: app 부분에 필요한 외부 라이브러리 추가**

</aside>

```java
//상단,하단 바 만들기
    implementation 'com.google.android.material:material:1.0.0'
```

<aside>
2️⃣ **res 폴더 안에 'menu' 폴더 생성 → 하단 바 아이콘,색상,개수 지정해주기**

</aside>

```java
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/tab1"
        app:showAsAction="always"
        android:enabled="true"
        android:icon="@drawable/one_line"
        android:title="달력"/>

    <item
        android:id="@+id/tab2"
        app:showAsAction="always"
        android:enabled="true"
        android:icon="@drawable/one_line"
        android:title="할 일"/>

    <item
        android:id="@+id/tab3"
        app:showAsAction="always"
        android:enabled="true"
        android:icon="@drawable/one_line"
        android:title="루틴"/>

</menu>
```

<aside>
3️⃣ **원하는 xml 레이아웃의 위치에 bottomNavigation 태그 넣어주기**

</aside>

```java
<com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottom_navigation"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:itemIconTint="@color/black"
        app:itemTextColor="#079E90"
        app:menu="@menu/menu_bottom" />
```

<aside>
4️⃣ 하단 바 각각의 아이콘 클릭시, 원하는 페이지 나오도록하기. (나는 view pager와 연동시킴)

</aside>

```java
BottomNavigationView bottomNavigationView=findViewById(R.id.bottom_navigation);
        bottomNavigationView.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch (item.getItemId()){
                    case R.id.tab1:
                        pager.setCurrentItem(0);
                        return true;

                    case R.id.tab2:
                        pager.setCurrentItem(1);
                        return true;

                    case R.id.tab3:
                        pager.setCurrentItem(2);
                        return true;
                }
                return false;
            }
        });
```