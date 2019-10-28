# Orchestrating animations in Flutter

한 애니메이션이 종료되면 다음 애니메이션을 진행하고 등을 어떻게 해야할까.
Interval을 사용한다!

모든 애니메이션이 총 3초안에 돌거라고 하자. 일단 AnimationController의 Duration을 3초로 준다.
```dart
final controller = new AnimationController(
  duration: const Duration(seconds: 3),
  vsync: this,
);
```

3개의 애니메이션이 1초씩 순서대로 돈다고 한다면:
```dart
final anim1 = Tween<double>(from: 0, end: 1).animate(
    CurvedAnimation(
        parent: controller,
        // 0% ~ 33%
        curve: Interval(0, 0.33, curve: Curves.ease),
    ),
);

final anim2 = Tween<double>(from: 0, end: 1).animate(
    CurvedAnimation(
        parent: controller,
        // 33% ~ 66%
        curve: Interval(0.33, 0.66, curve: Curves.ease),
    ),
);

final anim3 = Tween<double>(from: 0, end: 1).animate(
    CurvedAnimation(
        parent: controller,
        // 66% ~ 100%
        curve: Interval(0.66, 1.0, curve: Curves.ease),
    ),
);
```

## Reference

https://iirokrankka.com/2018/03/14/orchestrating-multiple-animations-into-visual-enter-animation/
