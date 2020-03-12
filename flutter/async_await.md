# Async await

아앍!!! Dart 언어는 기본적으로 비동기 처리를 위해 async/await 문법을 제공하는데 이게 여간 불편한게 아니다!
문법 자체는 정말 쉽기 편하다..

```dart
void reactFor(int feeling) async {
  final reaction = await getReaction(feeling)
  showReaction(reaction)
}
```

요로케 비동기적인 함수도 쭉 써내려갈 수 있어서 편하긴한데.. 문제는 만약 ```reactFor```라는 함수가 연속으로 두 번 불리게 될 경우
바로 이전에 불렸던 await를 취소할 수가 없다는 것이다!! 즉 저 함수의 lifecycle을 우리가 통제하지 못한다 ㅠㅠ!!

그래서 코틀린에서는 자체적으로 ```Coroutine```이라는걸 만들어서 async/await의 문법의 편리성도 그대로 쓰면서 그 함수들의 lifecycle도 관리할 수 있도록 해줬다.
Dart에서는 그런게 없으니.. 일일이 다 rx로 통일하기도 되게 귀찮고..! rx도 처음 배울 땐 너무 좋다! 했는데 coroutine 접하고 나서는 얘도 2순위이다!
