# SchedulerBinding#addPostFrameCallback

`Widget#build()` 함수에서 이번 loop가 다 끝난 후 어떤 로직을 실행시키고 싶었는데,
안드로이드에서는 다음 loop에서 실행시킬때 `Handler#post()`를 사용한다.
Flutter에서는 유사한게 `SchedulerBinding#addPostFrameCallback` 인 듯 하다.

근데 안드에는 `Handler#postDelayed()`도 있어서 특정 딜레이를 줄 수도 있는데,
Flutter의 `SchedulerBinding`에는 그건 없는거같고 (아마), 그냥 `Future.delayed()`쓰면 되는듯?