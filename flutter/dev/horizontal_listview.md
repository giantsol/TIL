
```dart
ListView.builder(
  scrollDirection: Axis.horizontal,
  itemCount: items.length,
  itemBuilder: (context, index) {
    final item = items[index];
    return ItemView(
      item: item,
    );
  }
)
```

```scrollDirection: Axis.horizontal``` 플래그를 주면 horizontal scrolling listview를 만들 수 있는데,
이대로만 하면 **Horizontal viewport was given unbounded height** 라는 에러가 발생한다.

Horizontal 방향으로는 꽉 채우기때문에 상관 없지만 vertical(i.e. height)로는 얼마나 펼쳐질지 안정해졌다는 뜻!
따라서 어떤 방식으로든 height에 제약을 걸어주면 된다.

