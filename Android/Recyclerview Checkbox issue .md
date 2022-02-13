# Recyclerview Checkbox issue

Created: August 20, 2021 1:36 PM

> **Recyclerview는 아이템이 재활용된다.**
> 

→ 아이템들이 스크롤이 생길때까지 늘어나면, 스크롤을 다시 올렸을 때 체크박스가 초기화되는 문제가 생긴다.

---

## CompoundButton

-A button with two states, checked and unchecked. When the button is pressed or clicked, the state changes automatically.

- **setOnCheckedChangeListener (void)**

```java
setOnCheckedChangeListener(CompoundButton.OnCheckedChangeListener listener)
```

    -Register a callback to be invoked when the checked state of this button changes.

    -사용자가 체크박스를 체크/해제 했을때 반응하는 이벤트 처리

- **setChecked**

```java
public void setChecked (boolean checked)
```

    -버튼의 체크상태를 변경한다.

[](https://developer.android.com/reference/android/widget/CompoundButton)

---

- todo_object.java
    
    ```java
    //체크박스 체크 상태
        boolean isSelected;
    
    //getter,setter
        public boolean isSelected() {return isSelected; }
        public void setSelected(boolean selected) { isSelected = selected; }
    ```
    
- todoAdapter.java
    
    ```java
    static class ViewHolder extends RecyclerView.ViewHolder {
            static TextView item_title;
            static TextView item_time;
            //...
            static CheckBox checkBox;
    
            public ViewHolder(View itemView, OnToDoItemClickListener listener) {
                super(itemView);
    
                item_title = itemView.findViewById(R.id.item_title);
                item_time = itemView.findViewById(R.id.item_time);
                //...
    
                checkBox=itemView.findViewById(R.id.checkbox);
    ```
    
    ```java
    public void onBindViewHolder(@NonNull todoAdapter.ViewHolder holder, int position) {
            final todo_object item = items.get(position);
    
            //checkbox 상태변화있을시 불리는 리스너 초기화 **얘가 젤 중요
            holder.checkBox.setOnCheckedChangeListener(null);
    
            //현재 item의 체크 상태 여부를 읽어 체크박스값을 초기화해준다.
            holder.checkBox.setChecked(item.isSelected());
    
    				//체크상태가 달라질 때 불러오는 리스너
            //사용자가 체크할 값에 대해 체크 이벤트를 달아서 setSelected를 해줌으로써 체크를 할 수 있게 한다.
            holder.checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
                @Override
                public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
                    
                    item.setSelected(isChecked);
                }
            });
    ```